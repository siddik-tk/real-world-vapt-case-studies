Title: OTP Verification Bypass via Pre-Verification Token Abuse

Vulnerability Type:
Broken Authentication / Improper Authorization

Summary:
The application issues a fully functional access token immediately after user registration, without enforcing OTP verification status. This allows attackers to access protected endpoints before completing verification.

Technical Analysis:
During the registration process, the backend returns an access token prior to OTP validation. This token is accepted by authorization middleware without checking whether the account has completed verification.

This results in a state desynchronization between authentication (token issued) and verification (OTP pending).

Steps to Reproduce:
1. Register a new account via POST /auth/register
2. Capture the access token from the response
3. Do NOT complete OTP verification
4. Use the token to access protected endpoints:
   - GET /auth/me
   - PATCH /profile/update
   - PATCH /auth/toggle2fa
5. Observe successful responses (HTTP 200) and state changes

Impact:
- Bypass of OTP verification mechanism
- Unauthorized access to protected user functionality
- Ability to modify account data without completing identity validation
- Weakens trust model for systems relying on verified users

In real-world scenarios, this can lead to:
- Abuse of onboarding flows
- Fraudulent account usage
- Circumvention of identity verification controls

Severity:
High (Authentication control bypass)

Why this matters:
OTP verification is intended as a security boundary. Allowing full access before verification breaks this boundary and enables unauthorized state transitions.

Remediation:
- Issue restricted or temporary tokens before OTP verification
- Enforce verification status in authorization middleware
- Block access to sensitive endpoints until verification is complete
