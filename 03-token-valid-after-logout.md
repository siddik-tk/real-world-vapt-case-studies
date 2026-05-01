Title: Access Token Remains Valid After Logout

Vulnerability Type:
Session Management Failure

Summary:
Access tokens are not invalidated on logout, allowing continued use until expiration.

Technical Analysis:
Logout does not revoke or blacklist issued tokens. The backend continues to accept tokens even after session termination.

Steps to Reproduce:
1. Login and capture token
2. Perform logout
3. Reuse token on:
   - GET /auth/me
   - GET /users/me
4. Observe valid responses

Impact:
- Session hijacking risk
- Unauthorized access after logout
- Weak session termination controls

Severity:
High

Remediation:
- Implement token invalidation/blacklisting
- Track active sessions server-side
