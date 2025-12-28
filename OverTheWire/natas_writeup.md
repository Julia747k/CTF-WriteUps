#  <a href="https://overthewire.org/wargames/natas/">OverTheWire Natas Writeup</a>

This is a writeup for Natas, a web-based wargame by OverTheWire that focuses on basic server-side web security. 
Each level presents a vulnerable webpage where the goal is to obtain the password for the next level.


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



