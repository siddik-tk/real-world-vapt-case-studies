# P1 Information Disclosure via Debug Page

While exploring public endpoints, I interacted with the HR assistant interface without authentication.

I tried triggering unexpected input to see how the system handles errors.

The application returned an HTTP 500 error with a full backend stack trace.

The response exposed internal details such as application structure and configuration data.

This indicates debug or verbose error handling is enabled in production.

Such information can help attackers understand the backend and plan targeted attacks.

This issue was reported under a VDP and validated as P1.

Fix:
- Disable debug mode in production
- Replace detailed error messages with generic responses
- Log errors internally instead of exposing them to users