# Access to Protected Endpoints Before OTP Verification (Unverified Token Abuse)

This was one of the first things I tested while exploring the authentication flow — specifically what happens between registration and OTP verification.

I registered a new user using /auth/register and noticed that an access token was immediately returned in the response.

Out of curiosity, I tried using that token before completing OTP verification.

I expected the API to block access until verification was completed — but it didn’t.

I used the same token on multiple endpoints:
- GET /auth/me
- PATCH /profile/update
- PATCH /auth/toggle2fa

All of them returned HTTP 200 OK and performed the action successfully, even though the account was still unverified.

This shows that the backend is issuing fully usable tokens before OTP verification and not enforcing verification state across protected endpoints.

From a logic perspective:
- Registration and verification are treated as separate steps
- But access control does not depend on verification status

Impact:
- Unverified users can access and modify account data
- Security features like 2FA can be enabled before verification
- Breaks the purpose of OTP verification as a gatekeeping control

Severity is moderate — user is still operating within their own account, but this weakens the intended authentication flow and could lead to misuse depending on application logic.

Fix:
- Restrict access to protected endpoints until OTP verification is completed
- Issue limited-scope tokens before verification (or delay token issuance)
- Enforce verification state during authorization checks