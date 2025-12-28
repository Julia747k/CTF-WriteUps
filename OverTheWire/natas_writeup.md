#  <a href="https://overthewire.org/wargames/natas/">OverTheWire Natas Writeup</a>

Natas is a web-based wargame by OverTheWire that focuses on fundamental server-side web 
security concepts. Each level presents a vulnerable website where the goal is to discover 
and exploit weaknesses in order to retrieve the password for the next level. Through hands-on
challenges, Natas introduces common web vulnerabilities such as authentication flaws, 
input validation issues, and information disclosure, making it an excellent starting point for
learning practical web exploitation techniques.Natas is a web-based wargame by OverTheWire that
focuses on fundamental server-side web security concepts. Each level presents a vulnerable website
where the goal is to discover and exploit weaknesses in order to retrieve the password for the next
level. Through hands-on challenges, Natas introduces common web vulnerabilities such as authentication
flaws, input validation issues, and information disclosure, making it an excellent starting point for
learning practical web exploitation techniques. 


---

## Level 0

**Problem description:**  

The objective of this level is to find the password for the next level.
The only hint provided is that the password is somewhere on this page.

**Solution:**  


By using the browser’s Inspect Element feature, we can examine the HTML source code of the webpage.
Inside the `<div>` element with the id content, there is an HTML comment containing the password.
This comment is not visible on the rendered page, but it can be seen when viewing the page source.


<img width="599" height="283" alt="html with flag" src="https://github.com/user-attachments/assets/b646d3a2-ddca-4883-bb94-6268d73f7ff9" />


<details>
<summary><strong>🚩 Flag:<strong></summary>

`0nzCigAq7t2iALyvU9xcHlYN4MlkIwlq`

</details>

---
