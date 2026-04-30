Title: Improper API Validation Leading to False Success Responses

Vulnerability Type:
Business Logic / Input Validation Failure

Summary:
The API returns success responses for invalid or non-existent resource IDs and does not properly handle malformed input.

Technical Analysis:
Backend does not verify whether the operation actually succeeded before returning a success response. Some malformed inputs also cause request timeouts.

Steps to Reproduce:
1. Send request to /notifications/read/:id with invalid ID
2. Observe success response despite non-existent resource
3. Send malformed input → observe hanging request

Impact:
- Misleading system state
- Inconsistent API behavior
- Potential resource exhaustion

Severity:
Medium

Remediation:
- Validate resource existence
- Return correct HTTP status codes
- Handle malformed input safely