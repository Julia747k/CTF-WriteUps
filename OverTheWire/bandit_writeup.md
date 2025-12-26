# OverTheWire Bandit Writeup 

> *Writeups on the first problems are short since I already have experience with the basic command line.*

---

## Level 0

**Problem description:**  
Log into the game using SSH.
![Level 0](https://github.com/Julia747k/CTF-WriteUps/tree/main/OverTheWire/Bpic/Level0.png)

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
Find the password stored in a file.

**Solution:**  
We are told that there is a **readme** file in the home directory that contains the password.

```bash
cat readme
```

<details>
<summary><strong>🚩Flag:<strong></summary>

`ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If`

</details>


---

## Level 2

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
<summary><strong>🚩Flag:<strong></summary>

`263JGJPfgU6LtdEvgfWU1XP5yac29mFx`

</details>


---

## Level 3

**Solution:**  
This problem builds on the last one but adds spaces in the filename.
To open a file with spaces in its name, we can use quotes around the filename:

```bash
cat "./--spaces in this filename--"
```

<details>
<summary><strong>🚩Flag:<strong></summary>

`MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx`

</details>

---

## Level 4

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
<summary><strong>🚩Flag:<strong></summary>

`2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ`

</details> 


---

## Level 5

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
<summary><strong>🚩Flag:<strong></summary>

`4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw`

</details> 


---

## Level 6

**Solution:**  
We have to find an **ASCII file** that is **1033** bytes in size and does not have the executable bit.
We could check every folder manually using **ls -la**, but it is easier to narrow it down by searching 
for a file with the exact size that is not executable:

```bash
find . -type f -size 1033c ! -executable
cat ./maybehere07/.file2
```

<details>
<summary><strong>🚩Flag:<strong></summary>

`HWasnPhtq9AVKe0dmk45nxy20cvUa6EG`

</details> 


---

## Level 7

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
<summary><strong>🚩Flag:<strong></summary>

`morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj`

</details> 


---

## Level 8

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
<summary><strong>🚩Flag:<strong></summary>

`dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc`

</details> 


---

## Level 9

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
<summary><strong>🚩Flag:<strong></summary>

`4CKMh1JI91bUIZZPXDqGanal4xvAg0JM`

</details> 


---

## Level 10

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
<summary><strong>🚩Flag:<strong></summary>

`FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey`

</details> 


---

## Level 11

**Solution:**  
Like the problem states, the content of data.txt is encrypted in **Base64**.
We can decode it using the **base64** command with the **-d (decode)** flag:

```bash
base64 -d data.txt
```

<details>
<summary><strong>🚩Flag:<strong></summary>

`dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr`

</details> 
