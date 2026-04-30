# Authentication State Desynchronization (2FA Bypass)

While testing authentication flows, I wanted to see what happens when a user enables 2FA during an active session.

I logged in normally, captured the access token, then enabled 2FA via the /toggle2fa endpoint without completing OTP verification.

I expected the token to be invalidated or restricted — but it wasn’t.

I reused the same token on protected endpoints like /auth/me and profile update, and the request returned HTTP 200 OK with valid user data.

This shows the backend is not linking token validity with the current authentication state (2FA enabled but not verified).

If an attacker gets a token before 2FA is enabled, they can continue using it even after 2FA enforcement.

In this case, the token expiry was around 15 minutes, so exploitation depends on token theft within that window — but still weakens the purpose of 2FA.

Fix:
- Invalidate all active tokens when 2FA is enabled
- Bind token validation to 2FA verification status