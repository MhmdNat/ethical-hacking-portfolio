# SQL Injection 
It is when  web application improperly handles user-controlled input, allowing an attacker to manipulate `SQL` queries executed by the backend database.

## Vulnerability Root Cause
SQL Injection happens because:
- User input is directly concatenated into SQL queries
- No input sanitization or parameterized queries are used
- The application trusts client input
- Example of vulnerable logic:
```sql
SELECT * FROM users WHERE username = '<user_input>';
```

## Impact
The improper parametrization and sanitization of user-controlled input may allow:
- Read confidential data (users, credentials, tokens)
- Modify or delete data
- Achieve remote code execution in some configurations


## Types of `SQL` Injections
SQL Injection vulnerabilities can be categorized into the following types:
- In-band SQL Injection  
  - Error-based SQLi  
  - Union-based SQLi  

- Blind SQL Injection  
  - Boolean-based SQLi  
  - Time-based SQLi  

## How to Identify `SQLi`
SQL Injection can often be identified through:
- Database error messages returned in responses
- Changes in application behavior when injecting logical conditions
- Time delays caused by injected sleep functions

Common test inputs include:
```sql
' OR '1'='1
'--
" OR 1=1#
```

## Mitigations
To prevent SQL Injection:
- Use prepared statements and parameterized queries
- Avoid constructing dynamic SQL queries with user input
- Implement proper input validation and allowlists
- Enforce least-privilege permissions for database accounts
- Disable verbose SQL error messages in production environments
