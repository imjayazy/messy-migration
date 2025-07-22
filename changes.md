# CHANGES.md

## Issues Identified in Legacy Code

- Passwords were stored and checked in plaintext, which is a major security flaw.
- SQL injection risk due to use of raw string formatting in SQL queries.
- Database connection (`sqlite3.connect`) was opened globally with `check_same_thread=False`, which is unsafe and prone to concurrency issues.
- API returned inconsistent and non-JSON responses (`str()` for some endpoints).
- Missing HTTP status codes — most responses were `200 OK` even for errors.
- No validation of incoming JSON data or required fields.
- Debugging `print()` statements left in production code.
- Code was harder to maintain and modify due to duplicated logic and poor naming.

## Changes Made

- Passwords are now securely hashed on creation and checked with `check_password_hash` on login.
- All SQL queries use parameterized queries (`?`) to prevent SQL injection.
- Removed global database connection and created a helper function to open & close connections per request.
- All API responses are now proper JSON with clear keys and messages.
- All endpoints now return appropriate HTTP status codes: `201 Created`, `400 Bad Request`, `404 Not Found`, `401 Unauthorized`, etc.
- Added validation for required fields in `POST` and `PUT` requests.
- Removed debugging print statements.
- Improved code readability: better variable names, consistent style, DRY (Don't Repeat Yourself) principle applied.

## Why These Changes Are Important

- Protects sensitive user data and improves security.
- Prevents malicious SQL injection attacks.
- Ensures correctness and safety of database operations.
- Makes the API consistent, easier for clients to consume, and adheres to REST best practices.
- Code is easier to read, maintain, and extend in the future.

## Assumptions & Trade-offs

- Did not switch to SQLAlchemy ORM for this assignment, despite its advantages:
  - The assignment discourages over-engineering.
  - Current scope is small, and sqlite3 suffices.
  - Retained sqlite3 for simplicity and compatibility with existing `users.db`.


## AI Usage

I used **ChatGPT** to help draft this `CHANGES.md` and refine the language for clarity and conciseness.
Check the correct way to hash and verify passwords.
Review my HTTP status code choices.

## With More Time

- Add unit tests for all endpoints.
- Add input schema validation using `marshmallow` or `pydantic`.
- Improve error logging.
- Switch to SQLAlchemy ORM and add migrations.
- Implement JWT-based authentication & rate limiting for enhanced security.
