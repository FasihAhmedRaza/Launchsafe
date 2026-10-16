---
name: datasafe
description: Maps how users' personal data (emails, phones, passwords, addresses, payment info) moves through an app and fixes leaks into logs, third party tools and API responses. Use when the user asks about privacy, GDPR, user data, logging, analytics, or before launching an app that collects personal data.
---

# DataSafe

Goal: know exactly where personal data enters, where it goes and where it ends up, then close every leak.

## Before you start

Detect the stack and list the third party services in use (analytics, error tracking, payments, email, AI APIs, auth providers).

## What to check and fix

1. Collection points. Find every form, signup, checkout, profile and API route that collects emails, phones, passwords, names, addresses, birth dates, payment info, IP addresses or device info. Trace where each value goes next.
2. Logs. Check every console.log, logger, print and error handler. If any of them output personal data or tokens, replace the value with [REDACTED] or remove the log.
3. Third party services. For every SDK or API call, list the exact user data it receives. Remove fields the service does not need. Pay special attention to error trackers, analytics and AI prompts that may contain user text.
4. Passwords. They must be hashed with bcrypt, argon2 or scrypt. Never plaintext, never MD5 or plain SHA256. Never logged, returned in a response or sent anywhere except the hashing function.
5. Cookies and storage. Sensitive cookies need httpOnly, secure and sameSite. Do not keep personal data or long lived tokens in localStorage.
6. API responses. Return only the fields the client needs. No password hashes, no internal IDs, no other users' data. Use explicit field lists, not whole database rows.
7. Database. Check that sensitive fields are not stored in more places than needed, and that backups and exports do not leak data.
8. Deletion. Make sure users can delete their account and data. If there is no flow, add a basic one that deletes or anonymizes personal data.

## Safety rules

- Never print real user data in your report. Use fake examples.
- Do not remove logs that are needed for security events, only the personal data inside them.

## Final report

Give a data map table with columns: Data type, Where collected, Where stored, Sent to which service, Problem found, Fix applied. End with anything that needs a legal or policy decision from the user, such as a privacy policy.
