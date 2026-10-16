---
name: deepcheck
description: Deep security audit of the risky parts of an app, meaning login and permissions, payments, and input handling. Use when the app has Stripe or other payments, custom auth, user roles, file uploads, or complex server logic, or when the user asks for a serious security review.
---

# DeepCheck

Goal: audit the critical paths where one bug costs real money or exposes real users.

## Before you start

Identify which of these exist in the app: authentication, roles, payments, file uploads, user generated content, raw database queries. Audit only what exists, but be thorough on those parts.

## Authentication and authorization

- Every protected route and API endpoint has auth middleware on the server.
- No IDOR. An endpoint must never take a user ID or object ID from the client and return data without checking the caller owns it.
- Password reset tokens are random, single use, expire within 15 minutes and are tied to one user.
- JWTs use a strong secret, short expiry and a way to revoke on logout.
- Sessions or tokens are invalidated after a password change.

## Payments

- The server calculates price, quantity, tax and discounts. Never trust values from the client.
- Check whether an attacker could change price, quantity or discount in the request body.
- Webhook signatures from Stripe, Razorpay or similar are verified, and events are handled once only.
- Paid access is granted only after the payment status is confirmed server side.

## Input handling

- No raw SQL built from user input. Use parameterized queries or the ORM properly.
- No user input rendered as HTML without sanitizing. Check dangerouslySetInnerHTML, innerHTML and template raw output.
- File uploads: validate type on the server, limit size, rename files, and do not serve uploads from a place that can execute them.
- Check redirects, URL fetching features and anything that passes user input to a shell or file path.

## Safety rules

- Test by reading code and running the app locally only. Never attack a live production system or other people's services.
- Fix issues in small, clearly described changes so the user can review them.

## Final report

For every issue give: what it is, where it is (file and line), how an attacker would use it, how bad it is (Critical, High, Medium, Low) and the exact fix. Order by severity. If nothing is found in an area, say what you checked.
