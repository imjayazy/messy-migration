# CHANGES.md

## Overview

This document outlines the changes I made to improve the legacy user management API.  
The goal was to refactor and improve code quality, maintain functionality, and make minimal, pragmatic changes.  

---

## Major Issues Identified

- Raw SQL queries with no ORM abstraction, making code less maintainable.
- No password hashing (plain text password storage — a critical security risk).
- Missing validation for required fields in some endpoints.
- Inconsistent error handling.
- Return values not always proper JSON or correct HTTP status codes.
- Hard-coded DB connections without separation of concerns.

---

## Changes Made

 **Code Organization**
- Moved all DB operations to use SQLAlchemy ORM for better maintainability and readability.
- Defined a `User` model with proper schema mapping.
- Added `to_dict()` method in `User` model for JSON serialization.

 **Security Improvements**
- Added password hashing for user creation and password checks for login using `werkzeug.security`.
- Ensured email uniqueness by database constraint.

 **Best Practices**
- Standardized all endpoints to return `jsonify` responses with proper HTTP status codes.
- Improved error messages and exception handling.
- Used parameterized queries or ORM (eliminates SQL injection risk).

**Documentation**
- Documented changes and trade-offs in this file.
- Requirements updated to include `Flask-SQLAlchemy`.

---

## Assumptions / Trade-offs

- Kept the same database file (`users.db`) for simplicity.
- Did not implement user authentication tokens (out of scope).
- Kept responses minimal and focused on functionality.

---

## AI Usage

I used **ChatGPT** to assist with:
- Drafting this `CHANGES.md` file.
- Suggesting best practices for SQLAlchemy usage and password hashing.

All code was reviewed and manually tested after suggestions.

---

## What I Would Do With More Time

- Add automated unit tests for all endpoints.
- Implement token-based authentication for login.
- Use environment variables for sensitive configuration (like DB path).
- Add pagination and sorting on `/users` endpoint.

---

## Updated Requirements

Added the following to `requirements.txt`:
