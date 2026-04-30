Title: Race Condition in Registration Leading to Duplicate Account Creation

Vulnerability Type:
Race Condition / Data Integrity Issue

Summary:
The application allows multiple accounts to be created with the same email due to lack of atomic enforcement of uniqueness during concurrent requests.

Technical Analysis:
Email uniqueness validation occurs before insertion but is not enforced at the database level. Concurrent requests bypass this check, resulting in duplicate records.

Steps to Reproduce:
1. Capture registration request
2. Send two identical requests simultaneously
3. Observe both requests succeed with different userIds

Impact:
- Duplicate user identities
- Inconsistent authentication and password reset behavior
- Data integrity issues in downstream systems

Severity:
Medium–High (depends on business logic)

Remediation:
- Enforce unique constraints at database level
- Use transactions or locking for registration