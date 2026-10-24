# Lab 3 - Cloud Issue Tracker Architecture

## 1. Application Goal
The Cloud Issue Tracker is a three-layer web application that separates the browser user interface, the REST API business logic, and the persistent data layer so issues can be created, viewed, and updated reliably.

## 2. Functional Scope
- Create an issue.
- View all issues.
- Change an issue status.

## 3. Request Flow

### CREATE ISSUE
User -> Frontend form -> POST /api/issues -> API validation -> Data layer -> 201 + JSON -> Render created issue
- The user fills in the form (title, description, priority) and clicks Submit.
- The frontend sends POST /api/issues with the form data as JSON.
- The API validates the data (title present, length limits, valid priority). If invalid, it stops and returns code 400 and an error message.
- The data layer stores the issue, gives it an id, createdAt timestamp, and status "Open".
- The API replies 201 Created and the new issue as JSON.
- The frontend adds the new issue to the list on screen.

### VIEW ISSUES
Page load -> GET /api/issues -> API -> Data layer -> 200 + JSON array -> Render list
- The page loads and the frontend sends GET /api/issues.
- The API asks the data layer for every stored issue.
- The API replies 200 OK and the JSON array (empty array if none).
- The frontend renders one card per issue.

### CHANGE STATUS
Button -> PATCH /api/issues/{id}/status -> API validation -> Data layer -> 200 + updated JSON -> Re-render issue
- The user clicks a status button on one issue.
- The frontend sends PATCH /api/issues/{id}/status with a JSON body such as {"status": "Resolved"}.
- The API validates that the status is an allowed value (otherwise 400 Bad Request) and that the issue {id} exists (otherwise 404 Not Found).
- The data layer updates only the status field of that issue.
- The API replies 200 OK and the full updated issue as JSON.

## 4. Trust Boundaries

```text
USER-CONTROLLED SIDE                 APPLICATION-CONTROLLED SIDE
Browser input ---> HTTP request ---> REST API ---> Data
      ^                                 |
      |----------- JSON response -------|
```

- **User-controlled side:** The browser, form fields, buttons, and every HTTP request leaving the browser (including curl or Postman). Every HTTP request is untrusted until validated.
- **Application-controlled side:** The REST API and the data layer. Only the server codebase runs here, and data is reachable only through the API.
- **Boundary rules:**
  1. The API validates every request. Frontend checks are only for convenience, never for security.
  2. The browser never talks to the data layer directly.
  3. The API returns only what the frontend needs, and never leaks internal errors or stack traces.
  4. The frontend shows issue text as plain text, never as HTML.
