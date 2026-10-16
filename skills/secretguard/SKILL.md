---
name: secretguard
description: Finds and fixes leaked secrets such as API keys, passwords, tokens and database URLs before an app goes live. Use when the user says deploy, launch, publish, push to GitHub, or asks whether their keys are safe.
---

# SecretGuard

Goal: make sure no secret lives in the code, the browser bundle, the logs or git history.

## Before you start

1. Detect the stack (Next.js, React, Vite, Express, Django, Flutter, and so on) and the services used (Supabase, Firebase, Stripe, OpenAI, AWS, Twilio, SendGrid).
2. Search the whole repo, including config files, comments, tests, scripts, Dockerfiles and CI files. Look for long random strings, anything near the words key, secret, token, password, URI or DSN, and common prefixes like sk_live, sk_test, AKIA, ghp_, eyJ.
3. Do not print real secret values in your report. Show only the first 4 characters and the file path.

## What to check and fix

1. Move every secret into environment variables. No secret may exist as a string in source code, config or comments.
2. Service rules:
   - Supabase: the anon key is fine in the browser only if Row Level Security is ON for every table. The service role key must never reach client code.
   - Firebase: the web config is public by design, but security rules must not be open to everyone.
   - Stripe: the publishable key can go in the browser. The secret key and webhook secret stay on the server.
   - Database URLs, JWT secrets, OAuth client secrets, SMTP passwords and third party API keys: server only.
3. Browser exposure: any variable starting with NEXT_PUBLIC_, REACT_APP_, VITE_ or EXPO_PUBLIC_ ships to users. Make sure none of them holds a private value. Also check that mobile apps and built bundles do not contain private keys.
4. Make sure .env and similar files are in .gitignore. Create .env.example with every variable name and a fake placeholder value.
5. Check console logs, error handlers and API responses so they never print or return secrets, tokens or connection strings.
6. Add startup validation so the app refuses to boot when a required variable is missing.
7. Git history: if a secret was ever committed, deleting it now is not enough. Tell the user to rotate that key immediately, then optionally clean history with git filter repo or BFG. Add a short rotation note to the README.

## Safety rules

- Never invent or rotate real credentials yourself. Ask the user to do it in the provider dashboard.
- Do not break the app. After moving values, confirm the code still reads them from the environment.

## Final report

Give a table with columns: Secret type, File and line, Risk, What you did, Needs user action. End with a list of keys the user must rotate.
