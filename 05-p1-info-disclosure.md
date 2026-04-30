# P1 Information Disclosure — Flask DEBUG=True in Production

While testing the HackwithIndia VDP target, I came across an HR assistant 
interface that didn't require authentication to access.

I sent some unexpected input to see how the app handles errors — and instead 
of a generic 500 page, I got a full Flask debug traceback dumped in the response.

The trace exposed file paths, internal module structure, environment config 
variables, the app's SECRET_KEY, and JWT signing key material. All of it. 
In plain text. On a live production domain.

Flask's debug mode is meant for local development. Someone deployed it to 
production and left it on.

The SECRET_KEY and JWT material meant session cookies and tokens could be 
forged — admin impersonation included. I reported it immediately.

Validated as P1 on Bugcrowd (HackwithIndia VDP). Reported January 23, 
resolved February 3.

Fix: DEBUG=False in production. Rotate exposed secrets. Generic error pages 
facing users, verbose logs staying server-side.