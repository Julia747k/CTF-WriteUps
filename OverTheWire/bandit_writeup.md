#  <a href="https://overthewire.org/wargames/bandit/">OverTheWire Bandit Writeup</a>

*Writeups on the first problems are short since I already have experience with the basic command line.*

---

## Level 0

**Problem description:**  

> The goal of this level is for you to log into the game using SSH. The host to which you need to connect 
is bandit.labs.overthewire.org, on port 2220. The username is bandit0 and the password is bandit0. Once 
logged in, go to the Level 1 page to find out how to beat Level 1.

**Solution:**  
The solution to this problem is logging into the game using SSH on port **2220** with the 
username **bandit0**. This is the first stepping stone to completing the rest of the problems.

**Command used:**  

```bash
ssh -p 2220 bandit0@bandit.labs.overthewire.org
```

---

## Level 0 → Level 1

**Problem description:**  

>The password for the next level is stored in a file called readme located in the home directory. 
>Use this password to log into bandit1 using SSH. Whenever you find a password for a level, 
>use SSH (on port 2220) to log into that level and continue the game.



**Solution:**  
We are told that there is a **readme** file in the home directory that contains the password.

```bash
cat readme
```

<details>
<summary><strong>🚩 Flag:<strong></summary>

`ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If`

</details>


---

## Level 2

**Problem description:**  

> The password for the next level is stored in a file called - located in the home directory


**Solution:**  
In Unix, **-** means “read from stdin instead of a file,” so **cat -** will not try to open 
a file literally named -. The problem here is how to open a file with a dashed filename. 
There are multiple solutions. The one I used is:

```bash
cat ./-
```

This can also be used: 

```
cat < -
```

<details>
<summary><strong>🚩 Flag:<strong></summary>

`263JGJPfgU6LtdEvgfWU1XP5yac29mFx`

</details>


---

## Level 3

**Problem description:** 

> The password for the next level is stored in a file called --spaces in this filename-- 
> located in the home directory

**Solution:**  
This problem builds on the last one but adds spaces in the filename.
To open a file with spaces in its name, we can use quotes around the filename:

```bash
cat "./--spaces in this filename--"
```

<details>
<summary><strong>🚩 Flag:<strong></summary>

`MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx`

</details>

---

## Level 4

**Problem description:** 

> The password for the next level is stored in a hidden file in the inhere directory.

**Solution:** 
First, I used “cd inhere” to go into the inhere directory.

```bash
cd inhere
```

Then I used:

```bash
ls -a
```  

to view hidden files.

Then I read the file called ...Hiding-From-You by using:

```bash
cat ...Hiding-From-You
```


<details>
<summary><strong>🚩 Flag:<strong></summary>

`2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ`

</details> 


---

## Level 5

**Problem description:** 

> The password for the next level is stored in the only human-readable file in the inhere directory. 
> Tip: if your terminal is messed up, try the “reset” command.

**Solution:**  
The problem says the flag is in the only human-readable file in the inhere directory.
First, we go into the directory:
cd inhere
Then we check which file is ASCII text. This can be done manually, 
but it is easier to check the entire directory at once using:

```bash
file ./*
```

We can see that **file07** is **ASCII text**, so we check it with:

```bash
cat ./-file07
```

<details>
<summary><strong>🚩 Flag:<strong></summary>

`4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw`

</details> 


---

## Level 6

**Problem description:** 

> *he password for the next level is stored in a file somewhere under the inhere directory 
> and has all of the following properties:
>
>   human-readable
>   1033 bytes in size
>   not executable

**Solution:**  
We have to find an **ASCII file** that is **1033** bytes in size and does not have the executable bit.
We could check every folder manually using **ls -la**, but it is easier to narrow it down by searching 
for a file with the exact size that is not executable:

```bash
find . -type f -size 1033c ! -executable
cat ./maybehere07/.file2
```

<details>
<summary><strong>🚩 Flag:<strong></summary>

`HWasnPhtq9AVKe0dmk45nxy20cvUa6EG`

</details> 


---

## Level 7

**Problem description:** 

> The password for the next level is stored somewhere on the server and has all of the
>following properties:
>
>    owned by user bandit7
>    owned by group bandit6
>    33 bytes in size

**Solution:**  
Again, we use find, but this time with more filters: 
-type (file type), 
-size (size in bytes, using c), 
-group (group owner), 
-user (file owner).

We also redirect stderr(2) to /dev/null to remove the “Permission denied” errors:

```bash
find /* -type f -size 33c -group bandit6 -user bandit7 2>/dev/null
```

<details>
<summary><strong>🚩 Flag:<strong></summary>

`morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj`

</details> 


---

## Level 8

**Problem description:** 

> The password for the next level is stored in the file data.txt next to the word millionth


**Solution:** 
I’m assuming this is a pretty big file, so I start by using stat on data.txt to confirm that.

```bash
stat data.txt
```

Stat confirms this, so we probably want to use grep.
We know the password is stored next to the word millionth, so using grep with 
"millionth" as the pattern should work:

```bash
grep "millionth" data.txt
```


<details>
<summary><strong>🚩 Flag:<strong></summary>

`dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc`

</details> 


---

## Level 9

**Problem description:** 

> The password for the next level is stored in the file data.txt and is the only line of text that occurs only once



**Solution:**  
The “Helpful Reading Material” points to piping and redirection, so I’m assuming 
they want us to pipe the text to uniq.
It says the only line that is unique, so we remove all other lines that are not unique.
First, we need to sort the file, since uniq only works on neighboring lines. 
Then we use uniq -u to show only the unique line:

```bash
sort data.txt | uniq -u
```


<details>
<summary><strong>🚩 Flag:<strong></summary>

`4CKMh1JI91bUIZZPXDqGanal4xvAg0JM`

</details> 


---

## Level 10

**Problem description:** 

> The password for the next level is stored in the file data.txt in one of the few human-readable strings, 
> preceded by several ‘=’ characters.

**Solution:**  
At first, I assumed data.txt was human-readable and tried using grep: 

```bash
grep -w "==" data.txt
```

This assumption was wrong, it’s a binary file.

We need to extract printable strings first and then grep the output:

```bash
strings data.txt | grep -w "======"
```


<details>
<summary><strong>🚩 Flag:<strong></summary>

`FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey`

</details> 


---

## Level 11

**Problem description:** 

>The password for the next level is stored in the file data.txt, which contains base64 encoded data


**Solution:**  
Like the problem states, the content of data.txt is encrypted in **Base64**.
We can decode it using the **base64** command with the **-d (decode)** flag:

```bash
base64 -d data.txt
```

<details>
<summary><strong>🚩 Flag:<strong></summary>

`dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr`

</details> 


--- 

## Level 12

**Problem description:** 

> The password for the next level is stored in the file data.txt, where all lowercase (a-z) 
> and uppercase (A-Z) letters have been rotated by 13 positions

**Solution:**  

Since it is a ROT13-style shift, we have to shift all the characters by 13 positions.
This can be solved by using the tr command, which replaces characters with the mapped ones.

```bash
cat data.txt | tr "A-Za-z" "N-ZA-Mn-za-m"
```

<details>
<summary><strong>🚩 Flag:<strong></summary>

`7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4`

</details> 


--- 

## Level 13

**Problem description:** 

> The password for the next level is stored in the file data.txt, which is a hexdump of a file 
> that has been repeatedly compressed. For this level it may be useful to create a directory under 
> /tmp in which you can work. Use mkdir with a hard to guess directory name. Or better, use the 
> command “mktemp -d”. Then copy the datafile using cp, and rename it using mv (read the manpages!)

**Solution:** 
Creating a directory under /tmp: 

```bash
mktemp -d
cp data.txt /tmp/<tempdir>
cd /tmp/<tempdir>
```

First we reverse the hexdump by using the xxd command with the -r flag to convert it back
to a normal binary file:

```bash
xxd -r data.txt > data
```

Then using `file` command we check the type of file, which shows us that it is a
gzip compressed file:

```bash
file data
```

We can extract it using the gunzip command, but first we have to rename it to data.gz:

```bash
mv data data.gz
gunzip data.gz
```

The file still contains unreadable text, so we check the file type again:

```bash
file data
```

The file is compressed with bzip2, so we can decompress it using bunzip2:

```bash
mv data data.bz2
bunzip2 data.bz2
```
We then have to use gunzip and bunzip2 once more. After that, the file turns out to be a
POSIX tar archive, which can be extracted with tar -xf:

```bash 
mv data data.tar
tar -xf data.tar
```
We now get a data5.bin file that is still a POSIX tar archive, so we extract it again:

```bash
tar -xf data5.bin
```

This gives us a data6.bin file. By checking it with the file command, we see that it is
compressed again, so we have to use bunzip2 and gunzip one more time. Finally we can read
the file using cat to get the password.

<details>
<summary><strong>🚩 Flag:<strong></summary>

`FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn`

</details> 


--- 

## Level 14

**Problem description:** 

>The password for the next level is stored in /etc/bandit_pass/bandit14 and can only be read by user bandit14. 
>For this level, you don’t get the next password, but you get a private SSH key that can be used 
>to log into the next level. Look at the commands that logged you into previous bandit levels, and find out how to use 
>the key for this level.

**Solution:**  

To log in using the provided SSH private key, we need to use the -i flag with ssh.
Since the key exists inside the Bandit level environment, it must be copied to our 
local machine before it can be used.
Once the key is available locally, we can attempt to log in using:

```bash
ssh -i sshkey.private -p 2220 bandit14@bandit.labs.overthewire.org
```

At first, this results in the following warning:

```bash
WARNING: UNPROTECTED PRIVATE KEY FILE! 
```  

SSH requires private keys to be readable only by the owner for security reasons.
This can be fixed by restricting the file permissions: 

```bash 
chmod 600 sshkey.private
```
After correcting the permissions, running the SSH command again successfully 
logs us into the bandit14 account.
Once logged in, the password for the next level can be read from:

```bash
cat /etc/bandit_pass/bandit14
```
<details>
<summary><strong>🚩 Flag:<strong></summary>

`MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS`

</details> 


--- 


## Level 15

**Problem description:** 

>The password for the next level can be retrieved by submitting the password of 
> the current level to port 30000 on localhost.


**Solution:**  

Commands used: 
```bash
nc 
```

At first i thought we were just supposed to ssh intp the localhost and submit 
the password, which was wrong.

Then i tried netcat, since then if the localhost is listening i could chat with it
and send "sumbit" the password, which worked, the command used: 

```bash
nc localhost 30000
```

<details>
<summary><strong>🚩 Flag:<strong></summary>

`8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo`

</details> 

## Level 16

**Problem description:** 

> The password for the next level can be retrieved by submitting the password of the
> current level to port 30001 on localhost using SSL/TLS encryption.
>
> Helpful note: Getting “DONE”, “RENEGOTIATING” or “KEYUPDATE”? Read the “CONNECTED"
> COMMANDS” section in the manpage.


**Solution:**  

Commands used: 
```bash
openssl
```

Using openssl to connect to localhost at port 30001 with s_client (TLS client used to talk directly to the server) worked: 
```bash
openssl s_client -connect localhost:30001
```

<details>
<summary><strong>🚩 Flag:<strong></summary>

`kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx`

</details> 


## Level 17

**Problem description:** 

>The credentials for the next level can be retrieved by submitting the password of
>the current level to a port on localhost in the range 31000 to 32000. First find
>out which of these ports have a server listening on them. Then find out which of
>those speak SSL/TLS and which don’t. There is only 1 server that will give the
>next credentials, the others will simply send back to you whatever you send to it.


**Solution:**  

Commands used: 
```bash
nmap
openssl 
s_client
chmod 
```

The problem here is that we have a range of ports and only one will give us the 
next credantials, to solve that we can use nmap which is a command that can be 
used to scan ports:

```bash
nmap -sV -T4 -p 31000-32000 localhost
```
-T5 increases the speed of the scan 
-p specifies the port(range) 
-sV identifies the version of the service 


From it we got a list of open ports:

| PORT      | STATE    | SERVICE     |
|-----------|----------|-------------|
| 31046/tcp | open     | echo        |
| 31518/tcp | open     | ssl/echo    |
| 31691/tcp | open     | echo        |
| 31790/tcp | open     | ssl/unknown |
| 31960/tcp | open     | echo        |


And this message: 

```bash 
1 service unrecognized despite returning data. If you know the service/version,
please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port31790-TCP:V=7.94SVN%T=SSL%I=7%D=12/27%Time=694FB34A%P=x86_64-pc-lin
SF:ux-gnu%r(GenericLines,32,"Wrong!\x20Please\x20enter\x20the\x20correct\x
SF:20current\x20password\.\n")%r(GetRequest,32,"Wrong!\x20Please\x20enter\
SF:x20the\x20correct\x20current\x20password\.\n")%r(HTTPOptions,32,"Wrong!
SF:\x20Please\x20enter\x20the\x20correct\x20current\x20password\.\n")%r(RT
SF:SPRequest,32,"Wrong!\x20Please\x20enter\x20the\x20correct\x20current\x2
SF:0password\.\n")%r(Help,32,"Wrong!\x20Please\x20enter\x20the\x20correct\
SF:x20current\x20password\.\n")%r(FourOhFourRequest,32,"Wrong!\x20Please\x
SF:20enter\x20the\x20correct\x20current\x20password\.\n")%r(LPDString,32,"
SF:Wrong!\x20Please\x20enter\x20the\x20correct\x20current\x20password\.\n"
SF:)%r(SIPOptions,32,"Wrong!\x20Please\x20enter\x20the\x20correct\x20curre
SF:nt\x20password\.\n");
```
From which we can see that port 31790 is asking for the a password `Wrong!\x20Please\x20enter\x20the\x20correct\x SF:20current\x20password\.\n`
which seems promising, firsty only using openssl like we did in the last level
didnt work, i just got `KEYUPDATE`. By reaserching s_client i realised that without using `-quiet` the server reads all the extre bytes and becouse of that the password comparison failes.  

By using 

```bash
openssl s_client -connect localhost:31790 -quiet
```

and the flag from last level. We recieve a RSA key that we now can save on the 
local system, change privileges to 400 by using chmod, so that ssh accepts it, we 
can ssh into level 18.


<details>
<summary><strong>🚩 Flag:<strong></summary>

Not really a flag haha! 

```
ssh -i sshkey.private -p 2220 bandit17@bandit.labs.overthewire.org
```

</details> 

## Level 18 

**Problem description:** 

> There are 2 files in the homedirectory: passwords.old and passwords.new. The 
> password for the next level is in passwords.new and is the only line that has 
> been changed between passwords.old and passwords.new


**Solution:**  

Commands used: 
```bash
ls
diff 
```

This level is straightforward. The diff command compares the contents of two text
files line by line. Lines marked with `<` come from the first file, while lines marked with `>` show how they differ in the second file.

```bash
diff passwords.old passwords.new
```

<details>
<summary><strong>🚩 Flag:<strong></summary>

`x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO`

</details> 

## Level 19

**Problem description:** 

> The password for the next level is stored in a file readme in the homedirectory.
> Unfortunately, someone has modified .bashrc to log you out when you log in with
> SSH.



**Solution:**  

Commands used: 

```bash
ssh 
cat
```

To avoid triggering .bashrc, run the cat command directly over SSH instead of
starting an interactive shell:

```bash
ssh -p 2220 bandit18@bandit.labs.overthewire.org cat readme
```

<details>
<summary><strong>🚩 Flag:<strong></summary>

`cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8`

</details> 


## Level 20

**Problem description:** 

> To gain access to the next level, you should use the setuid binary in the
> homedirectory. Execute it without arguments to find out how to use it. The password
> for this level can be found in the usual place (/etc/bandit_pass), after you have
> used the setuid binary.


**Solution:**  

Commands used: 
```bash
cat
```

The setuid binary `bandit20-do` has the owner set to bandit20, which means the code
inside the setuid binary is executed with bandit20 privileges. After testing
different commands as arguments, it appears that permission is granted to use the
cat command. Using it allows reading the password from the
/etc/bandit_pass/bandit20 file

```bash
./bandit20-do cat /etc/bandit_pass/bandit20
```

<details>
<summary><strong>🚩 Flag:<strong></summary>

`0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO`

</details> 

## Level 20

**Problem description:** 

> There is a setuid binary in the homedirectory that does the following: it 
> makes a connection to localhost on the port you specify as a commandline argument. 
> It then reads a line of text from the connection and compares it to the password 
> in the previous level (bandit20). If the password is correct, it will transmit 
> the password for the next level (bandit21).


**Solution:**  

The key detail is that the binary works as a client, so a listener must be run 
first. 

Commands used: 
```bash
tmux 
nc
```

`tmux` is used to handle both sides of the connection, then CTRL+b % to split
it into planes.


```bash 
tmux
``` 

In the first pane, start a listener on a local port: 

```bash
nc -l localhost 2000
```

In the second pane, execute the setuid binary and point it at that port:

```bash
./suconnect 2000
```

Once the connection is made, send the bandit20 password through the nc listener.
If correct, the binary responds with the password for the next level.

<details>
<summary><strong>🚩 Flag:<strong></summary>

`EeoULMCra2q0dSkYj561DX7s1CpBuOBt`

</details> 

## Level 21

**Problem description:** 

> A program is running automatically at regular intervals from cron, the time-based 
> job scheduler. Look in /etc/cron.d/ for the configuration and see what command is 
> being executed.


**Solution:**  

Commands used: 
```bash

```


```bash

```



<details>
<summary><strong>🚩 Flag:<strong></summary>

``

</details> 
