Title: Sensitive Information Disclosure via Flask Debug Mode Enabled in Production

Vulnerability Type:
Information Disclosure / Misconfiguration

Summary:
The application was running with Flask DEBUG mode enabled in production, exposing sensitive internal data including SECRET_KEY and JWT signing material.

Technical Analysis:
Improper deployment configuration allowed debug traceback pages to be publicly accessible. These revealed:
- Application file paths
- Environment variables
- SECRET_KEY
- JWT signing keys

Steps to Reproduce:
1. Access unauthenticated endpoint
2. Trigger error via malformed input
3. Observe full debug traceback in response

Impact:
- Exposure of cryptographic secrets
- Potential session/token forgery
- Risk of full account compromise including admin access

Severity:
Critical (P1 – validated)

Remediation:
- Disable debug mode in production
- Rotate all exposed secrets immediately
- Implement generic error handling