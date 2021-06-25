## Spring-Boot-2.1-OAuth2-Authorization-Server-and-Resource-Server-JWT-and-MySQL

[Youtube video link](https://youtu.be/l9chhjL7Kuk)

### Step to create Database (authserver)
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
```curl
  curl -X POST \
  http://localhost:9090/oauth/token \
  -H 'authorization: Basic dGFsazJhbWFyZXN3YXJhbjp0YWxrMmFtYXJlc3dhcmFuQDEyMw==' \
  -H 'cache-control: no-cache' \
  -H 'content-type: application/x-www-form-urlencoded' \
  -d 'grant_type=password&password=password&username=mike%40gmail.com'
  ```

```json
{
    "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyX25hbWUiOiJtaWtlQGdtYWlsLmNvbSIsInNjb3BlIjpbInJlYWQiLCJ3cml0ZSJdLCJuYW1lIjoiTWlrZSIsImlkIjoiMiIsImV4cCI6MTYyNDYyOTI2MCwidXNlck5hbWUiOiJtaWtlQGdtYWlsLmNvbSIsImF1dGhvcml0aWVzIjpbIlJPTEVfVklFV19OT1RFIiwiUk9MRV9WSUVXX0FMTF9OT1RFIl0sImp0aSI6IjU1YjZjMTJhLTdkMmUtNDE4Ni1hZTQ1LTU0ZjdmZGJlOTg3NCIsImNsaWVudF9pZCI6InRhbGsyYW1hcmVzd2FyYW4ifQ.CXDZ6oq97AfA9e3j_XKn4EWm4GIpkqP7Eq5TxVUNw_4-pED0T5aljLyh7ILWtKoxEnVUYT6RYmxLzewELsujSZhDQW4oCn9qYqpkRS_Eo8kK_jLQWLI8pDWLq3IqdPqnIaGxrsuu2uriSBjGGQjBy2O6g100T-gSWaqYOcm16TaEFKcohCBQpk_FCXXrF-QFbJiShmR0U7z7bXM7ZDzoT81FBHkmX438ohti1Vu-mY9syB2LG-FRo9ckwADGSUmdDAREYvaLwF7RMT455xFWEy1JKrwFfREXOlyFoCO-g5muS_G1gBpHpWIYLvRjIbZRBmLPAeI6NbblBPEuTfwKeA",
    "token_type": "bearer",
    "refresh_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyX25hbWUiOiJtaWtlQGdtYWlsLmNvbSIsInNjb3BlIjpbInJlYWQiLCJ3cml0ZSJdLCJhdGkiOiI1NWI2YzEyYS03ZDJlLTQxODYtYWU0NS01NGY3ZmRiZTk4NzQiLCJuYW1lIjoiTWlrZSIsImlkIjoiMiIsImV4cCI6MTYyNDY0MzY2MCwidXNlck5hbWUiOiJtaWtlQGdtYWlsLmNvbSIsImF1dGhvcml0aWVzIjpbIlJPTEVfVklFV19OT1RFIiwiUk9MRV9WSUVXX0FMTF9OT1RFIl0sImp0aSI6IjNhODdmYTdjLTcwY2ItNDRlZi05NDExLTQ5ZmE2NGY2YjNiZCIsImNsaWVudF9pZCI6InRhbGsyYW1hcmVzd2FyYW4ifQ.Q7x9ka2hoi4TLvpzENykBO5UIk5DOWAHzeFkjwE-lyREKsac8lkIVsoGKthxpu_30OZJ7V66C93GccPLLL8OgvqBSfQEjMd3sp_URlB5WAmKztAHzd0_Znbvby7HEri1Eb3WIVNNnPaFNkVuarGdF5adr9zgY1HydG6mJXMmt68HEPpg2HYv19wPSeKM30bRuC5GTtMN2pV_AqPrLUOu1ZlPssxOs2opiipbKbT-V3VwUsEMTCs4NPPp4NInQhEhmh6hxkVRni0RwCjiNnxNcSXL8Y9yCvkTwwOApyuIrtNTNhKaxFcCHL68lHAD7quMvEfNtJrdwWFRprT25gH8yg",
    "expires_in": 3599,
    "scope": "read write",
    "id": "2",
    "name": "Mike",
    "userName": "mike@gmail.com",
    "jti": "55b6c12a-7d2e-4186-ae45-54f7fdbe9874"
}
```

### Tesr Resourceserver:

        curl -X POST http://localhost:9191/note -H 'authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyX25hbWUiOiJqb2huQGdtYWlsLmNvbSIsInNjb3BlIjpbInJlYWQiLCJ3cml0ZSJdLCJuYW1lIjoiSm9obiIsImlkIjoiMSIsImV4cCI6MTYyNDYzMjYwOSwidXNlck5hbWUiOiJqb2huQGdtYWlsLmNvbSIsImF1dGhvcml0aWVzIjpbIlJPTEVfQ1JFQVRFX05PVEUiLCJST0xFX1ZJRVdfTk9URSIsIlJPTEVfRURJVF9OT1RFIiwiUk9MRV9ERUxFVEVfTk9URSIsIlJPTEVfVklFV19BTExfTk9URSJdLCJqdGkiOiI5MzM0MmJmNC04NjBmLTQ1ODYtYTQyYS04MThiYTU4ZWVlMDciLCJjbGllbnRfaWQiOiJ0YWxrMmFtYXJlc3dhcmFuIn0.E2gs9t3ni1x-nBjdQo0J_zSS2C5cVzEi182ri8wYNy3zrD42BKrYxN53zpGEIy48hzGd0R3ZPzSyyo6pk3oqOTkU3MX4JbESimhsumTRXYf-Ed695EBf65XDUVyW9sRkxFJlpR35qSWJGkCtnfujdUuc8qyaF0y4KmWHG6ffSoVILl41kSm2Ral1kwtLakg0XbbLegi0f1tEvsaDlDWk3sSCOwZMsCkjtjjok9S4sowKvEOYljOXwVSb6Mi--EtCYB0fupc6Olv57VzyJVtkl_8I-yozMOoz0kODVWCq1_o-uiEX6mVYjmX_DWpxbpeq8AMLP0aoSmCriDB0ZUw_8A'

        curl -X GET http://localhost:9191/note
        curl -X PUT http://localhost:9191/note
        curl -X DELETE http://localhost:9191/note
        curl -X GET http://localhost:9191/noteById
