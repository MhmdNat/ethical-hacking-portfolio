# Insecure Direct Object Reference
A web security flaw where an application exposes identifiers (such as IDs or file names) and fails to enforce proper authorization checks.

## Vulnerability
- The application trusts client-controlled input to reference internal objects 
- The server does not check if the logged-in user is authorized to view requested data
- Can allow unauthorized access to data belonging to other users

## Example Attack
- An attacker modifies the exposed object identifier
- If authorization steps are missing, the server might return other users private data to the attacker
- This access can be used to leak sensitive information or chained into further compromise

## Mitigations
- Perform server side authorization checks on object access
- Verify ownership of requested recources before returning data
- Reject unauthorized access
