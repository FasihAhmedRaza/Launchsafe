---
name: hackercheck
description: Reviews an app the way an attacker would, looking for ID manipulation, login bypass, privilege escalation, feature abuse, injection, exposed internals and business logic flaws. Use after building new features, before launch, or when the user asks to try to hack or break their own app.
---

# HackerCheck

Goal: think like an attacker, find the easiest ways in, and fix them. Data theft and unauthorized access come first, abuse and logic flaws second.

## Before you start

List the app's routes, roles and data objects. Rank them by what an attacker would want most: user data, money, admin access.

## Attack paths to check

1. ID manipulation. Can I read or change another user's data by editing an ID in the URL or request body? Check every endpoint with a user ID, order ID or document ID for an ownership check.
2. Login bypass. Does any endpoint work without a token? Are expired or malformed tokens rejected? Are there default admin accounts or leftover test users?
3. Privilege escalation. Can a normal user reach admin routes by guessing URLs or editing their role in a token or request? Role checks must happen on the server, not by hiding buttons.
4. Feature abuse. Check limits on signup, messaging, uploads, API calls, search, promo codes and referrals. Look for mass account creation, spam, storage filling and infinite coupon use.
5. Content injection. Put script tags and SQL characters into every text field, search box, filename and login form. Check how the app stores and displays them.
6. Internal exposure. Look for an exposed database admin panel, .env or .git reachable by URL, secrets in error messages, public Swagger or OpenAPI docs that should be private, and health endpoints that leak system info.
7. Business logic. If payments exist: negative amounts, stacking discounts, restarting free trials, referring yourself, replaying requests, race conditions on balances or stock.

## Safety rules

- Work from the code and a local copy of the app. Do not probe live sites or services the user does not own.
- Never keep or print real user data found during testing.

## Final report

For every finding give: what the attacker does, how much damage it could cause, severity, and the fix you applied or recommend. Start with the most damaging. End with one line saying when to run this again: after any major new feature.
