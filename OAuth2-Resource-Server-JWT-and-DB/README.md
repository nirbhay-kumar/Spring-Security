## Spring-Boot-2.1-OAuth2-Authorization-Server-and-Resource-Server-JWT-and-MySQL

[Youtube video link](https://youtu.be/l9chhjL7Kuk)

### Step to create Database
1. Create MySQL Database on AWS RDS.
2. Open **CloudShell**<br>
    [cloudshell-user@ip-10-0-35-48 ~]$ mysql -h userservice.ck6tkrmiljhm.us-east-2.rds.amazonaws.com -P 3306 -u root -p 
3. All script given in SQL-SCRIPT.txt
                
                MySQL [USERSERVICE]> select * from USER;
                +----+------+----------------+--------------------------------------------------------------+
                | ID | NAME | EMAIL_ID       | PASSWORD                                                     |
                +----+------+----------------+--------------------------------------------------------------+
                |  1 | John | john@gmail.com | $2a$10$jbIi/RIYNm5xAW9M7IaE5.WPw6BZgD8wcpkZUg0jm8RHPtdfDcMgm |
                |  2 | Mike | mike@gmail.com | $2a$10$jbIi/RIYNm5xAW9M7IaE5.WPw6BZgD8wcpkZUg0jm8RHPtdfDcMgm |
                +----+------+----------------+--------------------------------------------------------------+
                2 rows in set (0.00 sec)

                MySQL [USERSERVICE]> select * from ROLE
                    -> ;
                +----+---------------+
                | ID | ROLE_NAME     |
                +----+---------------+
                |  1 | ADMINISTRATOR |
                |  2 | AUDITOR       |
                +----+---------------+
                2 rows in set (0.00 sec)

                MySQL [USERSERVICE]> select * from PERMISSION;
                +----+-----------------+
                | ID | PERMISSION_NAME |
                +----+-----------------+
                |  1 | CREATE_NOTE     |
                |  2 | EDIT_NOTE       |
                |  3 | DELETE_NOTE     |
                |  4 | VIEW_ALL_NOTE   |
                |  5 | VIEW_NOTE       |
                +----+-----------------+
                5 rows in set (0.00 sec)

                MySQL [USERSERVICE]> select * from ASSIGN_USER_TO_ROLE;
                +----+---------+---------+
                | ID | USER_ID | ROLE_ID |
                +----+---------+---------+
                |  1 |       1 |       1 |
                |  2 |       2 |       2 |
                +----+---------+---------+
                2 rows in set (0.00 sec)

                MySQL [USERSERVICE]> select * from ASSIGN_PERMISSION_TO_ROLE;
                +----+---------------+---------+
                | ID | PERMISSION_ID | ROLE_ID |
                +----+---------------+---------+
                |  1 |             1 |       1 |
                |  2 |             2 |       1 |
                |  3 |             3 |       1 |
                |  4 |             4 |       1 |
                |  5 |             5 |       1 |
                |  6 |             4 |       2 |
                |  7 |             5 |       2 |
                +----+---------------+---------+
                7 rows in set (0.00 sec)
4. **CURL:**
```JSON
  curl -X POST \
  http://localhost:9090/oauth/token \
  -H 'authorization: Basic dGFsazJhbWFyZXN3YXJhbjp0YWxrMmFtYXJlc3dhcmFuQDEyMw==' \
  -H 'cache-control: no-cache' \
  -H 'content-type: application/x-www-form-urlencoded' \
  -d 'grant_type=password&password=password&username=mike%40gmail.com'
  ```
