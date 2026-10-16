---
name: prelaunch
description: Runs a full pre deploy checklist on an app and fixes what fails. Covers env vars, debug code, error handling, security headers, rate limiting, CORS and database security. Use when the user says deploy, go live, ship, production ready, or asks if the app is ready to launch.
---

# PreLaunch

Goal: make sure nothing embarrassing or dangerous goes live. Run every check, mark it Pass or Fail, and fix failures.

## Before you start

Detect the framework and hosting target (Vercel, Netlify, Render, AWS, VPS, Docker). Apply the fixes in the idiom of that stack, for example helmet for Express or the headers config for Next.js.

## Checklist

1. Environment variables. Every required variable is referenced properly. The app refuses to start with a clear message if a critical one (database URL, API keys, auth secret) is missing.
2. Debug code. Remove debug console.log calls, commented out code blocks, TODO or FIXME notes about security, hardcoded test credentials and test only routes such as /test, /debug, /seed and any admin backdoor. Debug mode defaults to OFF.
3. Error handling. Clients never see stack traces, query details, file paths or server info. Return a generic message plus a correlation ID. Full details go to server logs only.
4. Security headers. Set X-Content-Type-Options nosniff, X-Frame-Options DENY, Strict Transport Security with a one year max age, a Content Security Policy that limits scripts to your own domain and trusted sources, and a sensible Referrer Policy.
5. Rate limiting. Login, signup, password reset and OTP routes must be limited. Minimum: 5 attempts per minute per IP on login and 3 per hour on password reset.
6. CORS. Do not allow all origins unless the API is truly public. Restrict to the real frontend domain.
7. Database. TLS on in production, no default credentials, no database port open to the internet without authentication.
8. Basics. HTTPS enforced, cookies secure, dependencies free of known critical vulnerabilities (run npm audit or the stack equivalent), and a working health check that reveals nothing sensitive.

## Safety rules

- Do not change business logic while cleaning up. Only remove code that is clearly debug or dead.
- If a fix needs hosting settings the user must change, list the exact steps instead of guessing.

## Final report

Give a table with columns: Check, Pass or Fail, What you changed. Finish with a short Ready or Not ready verdict and the top three things still blocking launch, if any.
