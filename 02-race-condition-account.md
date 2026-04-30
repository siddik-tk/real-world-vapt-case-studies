# Race Condition in Account Creation (Duplicate Email Registration)

While testing the registration flow, I wanted to check how the system handles concurrent requests.

I suspected email uniqueness might not be strictly enforced.

I captured a valid registration request and sent two identical requests in parallel using Burp Repeater.

I expected one request to fail due to duplicate email — but both succeeded.

Both responses returned success, and two accounts were created with the same email but different userId values.

This indicates the uniqueness check is happening before insertion but not enforced atomically at the database level.

This can lead to:
- Duplicate identities
- Confusion in login or password reset flows
- Data integrity issues in downstream systems

Fix:
- Enforce unique constraints at the database level
- Use atomic operations or transaction locking