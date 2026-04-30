# API Logic Issue – Success Response for Invalid Input

While testing API endpoints, I tried accessing resources using invalid and non-existent IDs.

I also tested how the API behaves with malformed input.

I expected the API to return proper error responses like 400 or 404 — but it didn’t.

For the endpoint /notifications/read/:id:
- When using an invalid or non-existent ID, the API still returned success ("marked as read")
- In some cases, malformed input caused the request to hang until timeout

Authorization checks were working correctly, so this is not an IDOR.

The issue is that the backend is not validating whether the operation actually succeeded.

This leads to:
- Misleading success responses
- Inconsistent API behavior
- Potential resource exhaustion due to hanging requests

Fix:
- Validate resource existence before returning success
- Return proper HTTP status codes (400, 404)
- Handle malformed input gracefully