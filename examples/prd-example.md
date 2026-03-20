# PRD Example: User Authentication Feature

This example demonstrates how to use the **Product Requirements Document Skill** to define a feature from scratch.

---

## 🧩 Input

```md
You are using the "product-requirements-document" skill.

Feature:
Build a user authentication system for a web application.

Context:
- Users should be able to register and log in
- The app is a simple e-commerce platform
- No preferred tech stack
```

---

## ❓ Clarification Phase (AI Questions)

```md
1. Who are the target users?
2. Should authentication support social login (Google, GitHub)?
3. Is email verification required?
4. Should password reset functionality be included?
5. Any constraints on security (e.g., JWT, session-based)?
6. Is this part of an existing system or a new one?
```

---

## ✅ Clarified Answers

```md
1. General users (customers of the e-commerce app)
2. No, only email/password for now
3. Yes, email verification is required
4. Yes, include password reset
5. Use JWT-based authentication
6. This is part of an existing Node.js backend
```

---

## ⚠️ Note on Clarification Phase

In this example, additional context is already provided in the input.  
Because of that, the AI may skip the clarification phase and proceed directly.

If you want the AI to **ask clarification questions first (recommended)**:
- Remove or reduce the provided context
- Keep the input more ambiguous

This allows the skill to follow its intended workflow:
**Clarification → Planning → Execution**

---

## 🧩 Output: Generated PRD

# 📄 Product Requirements Document

**Feature:** User Authentication System
**Product:** E-Commerce Web Application
**Status:** Draft
**Last Updated:** 2026-03-20

---

## 1. Overview

| Field                 | Detail                                                                                                                                                                                                                           |
| --------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Feature Name**      | User Authentication System                                                                                                                                                                                                       |
| **Summary**           | A secure, standard auth flow covering registration, login, email verification, password reset, and session management for both customers and admins.                                                                             |
| **Problem Statement** | Without authentication, users cannot maintain a persistent identity, access order history, save addresses, or manage the store. The platform has no way to distinguish between anonymous visitors, paying customers, and admins. |
| **Goals**             | Allow customers to create accounts and log in securely; allow admins to access a protected dashboard; protect all user-specific resources behind an authenticated session.                                                       |

---

## 2. Users & Use Cases

### Target Users

| User         | Description                                                                       |
| ------------ | --------------------------------------------------------------------------------- |
| **Customer** | Shoppers who register to place orders, track purchases, and manage their profile. |
| **Admin**    | Store managers who log in to manage products, orders, and users.                  |

### User Stories

- As a **new customer**, I want to register with my email and password, so that I can place orders and track my purchases.
- As a **returning customer**, I want to log in with my credentials, so that I can access my account and order history.
- As a **customer**, I want to verify my email address, so that my account is confirmed and secure.
- As a **customer**, I want to reset my forgotten password via email, so that I can regain access to my account.
- As an **admin**, I want to log in with elevated credentials, so that I can access the store management dashboard.
- As any **authenticated user**, I want to log out, so that my session is terminated on shared devices.

---

## 3. Scope

### In Scope

- Customer registration (email + password)
- Customer and admin login
- Email verification after registration
- Forgot password / password reset via email link
- Authenticated session management (JWT + refresh tokens)
- Logout (single device)
- Role-based access control: `customer` vs `admin`

### Out of Scope

- OAuth / social login (Google, GitHub) — can be added as Phase 2
- Phone / OTP authentication
- Multi-factor authentication (MFA)
- Multi-device session management / revoke all sessions
- Admin user creation UI (admins seeded directly in DB for now)
- Account deletion

---

## 4. Functional Requirements

| ID    | Requirement        | Description                                                                                                                              |
| ----- | ------------------ | ---------------------------------------------------------------------------------------------------------------------------------------- |
| FR-01 | User Registration  | User submits email, password, and full name. System creates an unverified account and sends a verification email.                        |
| FR-02 | Email Verification | User clicks the link in the verification email. Account status changes to `verified`. Unverified users cannot log in.                    |
| FR-03 | User Login         | User submits email + password. System validates credentials, checks account is verified, and returns a JWT access token + refresh token. |
| FR-04 | Role Assignment    | On registration, all users get the `customer` role. Admin role is assigned manually at the DB level.                                     |
| FR-05 | Protected Routes   | Any endpoint requiring authentication must validate the JWT. Admin endpoints must additionally check for `admin` role.                   |
| FR-06 | Forgot Password    | User submits their email. If it exists, a time-limited reset link (valid 1 hour) is sent.                                                |
| FR-07 | Password Reset     | User submits a new password using the token from the reset link. Old password is invalidated. Reset token is single-use.                 |
| FR-08 | Token Refresh      | Client submits a valid refresh token to obtain a new access token without re-authentication.                                             |
| FR-09 | Logout             | Revoke the user's current refresh token. Subsequent refresh attempts are rejected.                                                       |
| FR-10 | Password Hashing   | All passwords are hashed with bcrypt (cost factor ≥ 12) before storage. Plaintext passwords are never stored or logged.                  |

---

## 5. Edge Cases

- **Duplicate email registration:** Return a `409 Conflict` without revealing whether the account exists (prevent user enumeration).
- **Login with unverified email:** Return `403 Forbidden` with a clear message prompting email verification. Offer resend verification.
- **Expired verification link:** User can request a new verification email from the login screen.
- **Expired or used reset token:** Return `400 Bad Request`. Do not silently fail.
- **Login with wrong password (brute force):** Rate-limit login attempts to 5 per minute per IP. Lock account for 15 minutes after 10 consecutive failures.
- **Refresh token reuse (rotation attack):** If a previously revoked refresh token is used, invalidate all active tokens for that user and require re-login.
- **Case sensitivity in email:** Normalize emails to lowercase before storing and comparing.
- **Password complexity:** Enforce minimum 8 characters with at least 1 uppercase, 1 number, and 1 special character.

---

## 6. UX / Flow Description

### Registration Flow
1. User fills in full name, email, and password on `/register`.
2. Client validates input before submission.
3. On success, user sees a "Check your email to verify your account" message.
4. User clicks link in email → redirected to `/verify-email?token=...`.
5. System verifies token → account activated → user redirected to `/login`.

### Login Flow
1. User fills in email and password on `/login`.
2. On success, access token is stored in memory (JS variable); refresh token stored in `HttpOnly` cookie.
3. User is redirected to `/dashboard` (admin) or `/` (customer).

### Forgot Password Flow
1. User clicks "Forgot password?" on `/login` → lands on `/forgot-password`.
2. Submits email → receives "If this email exists, you'll get a reset link" response (always same message).
3. Clicks link in email → redirected to `/reset-password?token=...`.
4. Submits new password → success message → redirected to `/login`.

### Logout Flow
1. User clicks "Log out".
2. Client calls `POST /api/v1/auth/logout` with refresh token cookie.
3. Server revokes refresh token.
4. Client clears access token from memory → redirected to `/login`.

---

## 7. Technical Design

### Recommended Stack

> Since no stack was specified, the following is recommended for a simple, maintainable e-commerce app.

| Layer                | Technology                                | Rationale                                                                 |
| -------------------- | ----------------------------------------- | ------------------------------------------------------------------------- |
| **Frontend**         | Next.js 15 (App Router)                   | Full-stack React, server components, easy route protection via middleware |
| **Backend**          | Next.js API Routes / Node.js Express      | Co-located or standalone depending on team preference                     |
| **Database**         | PostgreSQL (via Supabase or raw)          | Relational, strong for e-commerce data integrity                          |
| **ORM**              | Prisma                                    | Type-safe, excellent PostgreSQL support                                   |
| **Auth tokens**      | JWT (access) + opaque refresh token in DB | Standard, stateless access + revocable refresh                            |
| **Email**            | Resend or Nodemailer + SendGrid           | Simple transactional email for verification and reset                     |
| **Password hashing** | bcrypt                                    | Industry standard                                                         |

---

### Data Model

```sql
-- Users table
CREATE TABLE users (
  id            UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  email         VARCHAR(255) UNIQUE NOT NULL,
  password_hash VARCHAR(255) NOT NULL,
  full_name     VARCHAR(255) NOT NULL,
  role          VARCHAR(20) NOT NULL DEFAULT 'customer', -- 'customer' | 'admin'
  is_verified   BOOLEAN NOT NULL DEFAULT FALSE,
  created_at    TIMESTAMPTZ NOT NULL DEFAULT NOW(),
  updated_at    TIMESTAMPTZ NOT NULL DEFAULT NOW()
);

-- Refresh tokens (for revocation tracking)
CREATE TABLE refresh_tokens (
  id         UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id    UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
  token_hash VARCHAR(255) UNIQUE NOT NULL,
  expires_at TIMESTAMPTZ NOT NULL,
  revoked    BOOLEAN NOT NULL DEFAULT FALSE,
  created_at TIMESTAMPTZ NOT NULL DEFAULT NOW()
);

-- Email verification tokens
CREATE TABLE verification_tokens (
  id         UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id    UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
  token_hash VARCHAR(255) UNIQUE NOT NULL,
  type       VARCHAR(30) NOT NULL, -- 'email_verification' | 'password_reset'
  expires_at TIMESTAMPTZ NOT NULL,
  used       BOOLEAN NOT NULL DEFAULT FALSE,
  created_at TIMESTAMPTZ NOT NULL DEFAULT NOW()
);
```

---

### API Design

| Method | Endpoint                           | Auth                   | Description                     |
| ------ | ---------------------------------- | ---------------------- | ------------------------------- |
| `POST` | `/api/v1/auth/register`            | None                   | Register a new customer account |
| `POST` | `/api/v1/auth/verify-email`        | None                   | Verify email using token        |
| `POST` | `/api/v1/auth/resend-verification` | None                   | Resend verification email       |
| `POST` | `/api/v1/auth/login`               | None                   | Login and receive tokens        |
| `POST` | `/api/v1/auth/logout`              | Refresh token (cookie) | Revoke refresh token            |
| `POST` | `/api/v1/auth/refresh`             | Refresh token (cookie) | Issue new access token          |
| `POST` | `/api/v1/auth/forgot-password`     | None                   | Request password reset email    |
| `POST` | `/api/v1/auth/reset-password`      | None                   | Reset password with token       |
| `GET`  | `/api/v1/auth/me`                  | Bearer JWT             | Get current user info           |

---

#### Request / Response Examples

**POST `/api/v1/auth/register`**
```json
// Request
{
  "email": "jane@example.com",
  "password": "Secure@123",
  "full_name": "Jane Doe"
}

// Response 201
{
  "message": "Registration successful. Please check your email to verify your account."
}

// Response 409
{
  "error": "An account with this email already exists."
}
```

**POST `/api/v1/auth/login`**
```json
// Request
{
  "email": "jane@example.com",
  "password": "Secure@123"
}

// Response 200
// Refresh token set as HttpOnly cookie: Set-Cookie: refresh_token=...; HttpOnly; Secure; SameSite=Strict
{
  "access_token": "<jwt>",
  "user": {
    "id": "uuid",
    "email": "jane@example.com",
    "full_name": "Jane Doe",
    "role": "customer"
  }
}

// Response 401
{
  "error": "Invalid email or password."
}

// Response 403
{
  "error": "Please verify your email address before logging in."
}
```

**POST `/api/v1/auth/refresh`**
```json
// Request: No body — refresh_token read from HttpOnly cookie

// Response 200
{
  "access_token": "<new_jwt>"
}

// Response 401
{
  "error": "Invalid or expired refresh token."
}
```

---

### Key Technical Decisions

| Decision                 | Choice                                                         | Rationale                                                                                 |
| ------------------------ | -------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| **Token storage**        | Access token in memory; refresh token in `HttpOnly` cookie     | Mitigates XSS (no access token in localStorage) and CSRF (SameSite cookie)                |
| **Refresh token format** | Random opaque token, hash stored in DB                         | Enables revocation; hash prevents DB exposure from granting access                        |
| **Token rotation**       | Issue new refresh token on each refresh                        | Limits refresh token reuse window; detect replay attacks                                  |
| **Email enumeration**    | Identical response for all forgot-password / register attempts | Prevents probing which emails are registered                                              |
| **Rate limiting**        | 5 req/min on login, 3 req/15min on forgot-password             | Protects against brute force and email bombing                                            |
| **Admin seeding**        | No admin registration endpoint                                 | Prevents privilege escalation; admins seeded at the DB or via a protected internal script |

---

## 8. Non-Functional Requirements

| Category        | Requirement                                                                                      |
| --------------- | ------------------------------------------------------------------------------------------------ |
| **Performance** | Login and token refresh endpoints respond in < 300ms under normal load                           |
| **Security**    | Passwords hashed with bcrypt (cost 12+); tokens never logged; HTTPS enforced in production       |
| **Scalability** | Stateless JWT access tokens allow horizontal scaling of API servers without shared session state |
| **Reliability** | Auth endpoints target 99.9% uptime; email delivery failures should not block login               |
| **Compliance**  | Passwords never stored in plaintext; email addresses stored with consent                         |

---

## 9. Success Metrics

| Metric                         | Target                                                            |
| ------------------------------ | ----------------------------------------------------------------- |
| Registration completion rate   | ≥ 80% of users who start registration complete email verification |
| Login success rate             | ≥ 99% of valid login attempts succeed within 300ms                |
| Password reset completion rate | ≥ 70% of reset emails result in a completed password change       |
| Auth-related error rate        | < 0.5% of auth requests result in unexpected 5xx errors           |
| Token refresh failure rate     | < 0.1% of valid refresh attempts fail                             |

---

## 10. Testing Considerations

### Unit Tests
- Password hashing and comparison
- JWT generation and validation logic
- Token expiry calculation

### Integration Tests
- `POST /register` — success, duplicate email, weak password
- `POST /verify-email` — valid token, expired token, already-used token
- `POST /login` — success, wrong password, unverified account, locked account
- `POST /refresh` — valid cookie, expired token, revoked token, reuse detection
- `POST /logout` — success, missing cookie
- `POST /forgot-password` — known email, unknown email (same response)
- `POST /reset-password` — valid token, expired, already used

### Security Tests
- Verify refresh token is `HttpOnly` and not accessible via `document.cookie`
- Verify access token is not stored in `localStorage` or `sessionStorage`
- Verify brute force lockout triggers after threshold
- Verify plaintext password never appears in logs or API responses

---

## 11. Implementation Checklist

### Backend
- [ ] `users` table migration
- [ ] `refresh_tokens` table migration
- [ ] `verification_tokens` table migration
- [ ] Register endpoint + bcrypt hashing
- [ ] Verification email + token endpoint
- [ ] Login endpoint + JWT issuance
- [ ] Refresh token endpoint with rotation
- [ ] Logout endpoint with revocation
- [ ] Forgot password endpoint
- [ ] Reset password endpoint
- [ ] Auth middleware (JWT validation)
- [ ] Role guard middleware (admin check)
- [ ] Rate limiter on auth routes

### Frontend
- [ ] `/register` page + form validation
- [ ] `/login` page
- [ ] `/verify-email` handler page
- [ ] `/forgot-password` page
- [ ] `/reset-password` page
- [ ] Access token management in memory
- [ ] Axios/fetch interceptor for token refresh
- [ ] Route protection via middleware (Next.js) or guards

### Quality
- [ ] Unit tests written for auth logic
- [ ] Integration tests covering all auth endpoints
- [ ] Security checklist reviewed
- [ ] README / API docs updated