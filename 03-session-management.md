# Access Token Remains Valid After Logout

While testing session handling, I wanted to verify whether logout properly invalidates access tokens.

I logged in, captured the token, then called the logout endpoint.

I expected the token to be invalidated immediately — but it wasn’t.

I reused the same token on endpoints like /auth/me and /users/me, and the response returned HTTP 200 OK with valid user data.

This shows that logout is not invalidating the token server-side.

The token remains valid until expiry, meaning a stolen token can still be used even after logout.

This weakens session security and allows continued access after user logout.

Fix:
- Invalidate tokens on logout (server-side)
- Maintain token blacklist or session tracking
- Use shorter token lifetimes with refresh logic