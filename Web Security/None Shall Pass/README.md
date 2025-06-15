## None Shall Pass
## Difficulty: Medium
## Tools: Browser (Firefox), [CyberChef](https://cyberchef.org/), [Insomnia](https://insomnia.rest/download) (Or any other [tools](https://www.guru99.com/top-6-api-testing-tool.html) for [API testing](https://testsigma.com/guides/api-testing/))
## Description: 
Deep inside Juche Jaguar’s intranet runs a custom token‐based gateway protecting their most sensitive files at ```/secret```. We got our hands on a low‐privilege account (```user:pass = agent:spudpotato```) - use it to request an access token, then find a way to trick the gateway into granting you full admin rights and pull down the hidden intelligence (the flag) from ```/secret```. Good luck, Operative.
## Solve:
- Go to the website and log in using the provided credentials. It will display a token on the screen. We see that the token is so long that it overflows the page, so it's easy to copy it from ```Inspect```
![login](login.jpg)
![inspect](inspect.jpg)
```Token: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyIjoiYWdlbnQiLCJyb2xlIjoidXNlciIsImlhdCI6MTc0OTkxNTg0NCwiZXhwIjoxNzQ5OTE5NDQ0fQ.dxCE5yFgndQUyf5IDlnD5QJxHJqLMFhOtmA9XEz0nj8```
- Now let's see what this token contains. The token is consisted of 3 parts separated by '.', which tells us this is a [JWT](https://www.geeksforgeeks.org/json-web-token-jwt/). We paste the first two parts of the token into CyberChef and decode using base64 to get the payloads
```{"alg":"HS256","typ":"JWT"}```
```{"user":"agent","role":"user","iat":1749915844,"exp":1749919444}```
- The first block identifies the type of data and algorithm for [digital signature](https://www.geeksforgeeks.org/computer-networks/digital-signatures-certificates/), and the second block signifies the identity of the logged in user. The third part of the token is the digital signature signed using a key and HS256 algorithm, as indicated by the first part. There's no reason to decode this.
- To construct a new JWT that would hopefully give us admin access, we alter the second payload, specifically ```"role":"user"``` into ```"role":"admin"```, then use CyberChef to encode the payload again.Reuse the original first and third part of the JWT. Since we don't have the private key, we can't generate our own digital signature for the new payload. We can only hope that the website has weak verification method. The new JWT would be: ```eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyIjoiYWdlbnQiLCJyb2xlIjoiYWRtaW4iLCJpYXQiOjE3NDk5MTU4NDQsImV4cCI6MTc0OTkxOTQ0NH0.dxCE5yFgndQUyf5IDlnD5QJxHJqLMFhOtmA9XEz0nj8```
- Use Insomnia to send a GET request to ```URL/secret``` with Header ```Authorization: Bearer <Token>```. Voila! The site does have weak verification. We've got the flag!
![flag](flag.jpg)

