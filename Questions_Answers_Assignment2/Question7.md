
7. **Decode the following JWT and explain its contents:**
   - Token: `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJqb2huLmRvZUBleGFtcGxlLmNvbSIsInJvbGUiOiJBRE1JTiIsInVzZXJfaWQiOiJjZGY4M2QzZi0zNzQ5LTRjZGQtOTRlYS1hNTVjZmMwNDhkMGYiLCJleHAiOjE3MTc2MTY4MjAuMjIwNzA5fQ.ANS8PgUiwPCmOvnZLYTCy_5WzLyhCDOx8aF4xu-Kaz8`
   - Use [jwt.io](https://jwt.io/) to decode and explain the contents.

## Answer:
Decoded payload using jwt.io:
```
{
  "sub": "john.doe@example.com",
  "role": "ADMIN",
  "user_id": "cdf83d3f-3749-4cdd-94ea-a55cfc048d0f",
  "exp": 1717616820.220709
}
```

Explanation of the Decoded Token :
sub: Subject of the token, usually the user email or ID (john.doe@example.com).
role: The role of the user (ADMIN).
user_id: Unique identifier for the user (cdf83d3f-3749-4cdd-94ea-a55cfc048d0f).
exp: Expiration time of the token (in Unix timestamp format).

   [Return back to answer.md](/answer.md)