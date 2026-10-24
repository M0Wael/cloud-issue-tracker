# API Test Plan

| Test ID | Request | Expected Result | Actual Result | Pass/Fail |
|---|---|---|---|---|
| T01 | GET /api/issues | 200; JSON array returned. | 200; JSON array of issues returned. | Pass |
| T02 | POST valid title, description, priority | 201; complete created issue returned. | 201; issue returned with server-generated id, status Open and current createdAt. | Pass |
| T03 | POST missing title | 400; VALIDATION_ERROR. | 400; VALIDATION_ERROR: Title must contain between 3 and 100 characters. | Pass |
| T04 | POST invalid priority = Urgent | 400; VALIDATION_ERROR. | 400; VALIDATION_ERROR: Priority must be Low, Medium or High. | Pass |
| T05 | PATCH existing issue status = Resolved | 200; returned status is Resolved. | 200; complete issue returned with status Resolved. | Pass |
| T06 | PATCH status = Deleted | 400; VALIDATION_ERROR. | 400; VALIDATION_ERROR: Status must be Open or Resolved. | Pass |
| T07 | PATCH unknown issue ID | 404; ISSUE_NOT_FOUND. | 404; ISSUE_NOT_FOUND in standard error shape. | Pass |
| T08 | GET when no issues exist | 200; empty JSON array. | 200; empty array [] returned. | Pass |
| T09 | Send unexpected extra server-owned fields | Server ignores client id/status/createdAt. | 201; id is a new UUID, status is Open and createdAt is now. | Pass |
| T10 | Create without JSON (text/plain) | 415; UNSUPPORTED_MEDIA_TYPE. | 415; UNSUPPORTED_MEDIA_TYPE: Content-Type must be application/json. | Pass |
| T11 | Create with malformed JSON | 400; VALIDATION_ERROR. | 400; VALIDATION_ERROR: Request body is not valid JSON. | Pass |
| T12 | Unknown route (PATCH /api/fake-url) | 404; ROUTE_NOT_FOUND. | 404; ROUTE_NOT_FOUND: Route not found. | Pass |
