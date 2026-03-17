# Broken Access Control (Authorization Bypass)
A web security flaw where the application fails to properly enforce access restrictions, allowing unauthorized users to access functionality or data that should be restricted.

## Vulnerability
Administrative or sensitive endpoints are accessible without proper server-side authorization checks.

The server may rely on client-supplied parameters or session state to enforce privileges.

Any user, including unauthenticated or low-privileged users, can perform actions reserved for administrators or other users.

Example Attack:
An endpoint `/api/v1/admin/settings/update` accepts `JSON` input:
```json
{
  "email": "user@example.com",
  "is_admin": 1
}
```

The server does not verify that the requester is an admin. A non-admin user can update settings, view restricted data, or perform admin-only operations. Broken access control can also be exploited to access internal endpoints, generate privileged resources, or enumerate sensitive information.

## Impact
- Unauthorized access to administrative functionality or sensitive data.
- Can be chained with other vulnerabilities (e.g., command injection, data exfiltration, privilege escalation).

## Mitigations
- Implement role-based access control (RBAC) and validate user roles against protected actions.
- Never rely on client-supplied parameters to determine privileges.
