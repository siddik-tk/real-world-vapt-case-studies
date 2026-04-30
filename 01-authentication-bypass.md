Title: 2FA Bypass via Authentication State Desynchronization

Vulnerability Type:
Broken Authentication / Session Management

Summary:
After enabling 2FA, previously issued access tokens remain valid and are not re-evaluated against the updated authentication state. This allows continued access to protected endpoints without completing 2FA verification.

Technical Analysis:
The application does not bind token validity to the current authentication state. When 2FA is enabled mid-session, existing tokens are not invalidated or restricted, resulting in a desynchronization between session state and security controls.

Steps to Reproduce:
1. Login and capture access token
2. Enable 2FA via /auth/toggle2fa (do not complete verification)
3. Reuse the original token on:
   - GET /auth/me
   - PATCH /profile/update
4. Observe successful responses (HTTP 200)

Impact:
- Bypass of 2FA enforcement
- Continued access using pre-2FA tokens
- Weakens account security against token compromise

Severity:
High

Remediation:
- Invalidate all active tokens when enabling 2FA
- Enforce 2FA verification status during token validation