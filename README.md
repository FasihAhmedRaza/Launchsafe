# LaunchSafe

Five security skills for AI coding agents. They help you find and fix the mistakes that cause most breaches in apps built with AI tools, before you launch.

Works with Claude Code, Cursor, Codex, Windsurf and any agent that supports the SKILL.md format.

## Install

Install everything:

```
npx skills add FasihAhmedRaza/launchsafe
```

Install one skill:

```
npx skills add FasihAhmedRaza/launchsafe --skill secretguard
```

## The five skills

1. secretguard: finds leaked API keys, passwords and tokens, and moves them to environment variables.
2. datasafe: maps where users' personal data goes and stops it leaking into logs, third party tools and API responses.
3. prelaunch: pre deploy checklist covering env vars, debug code, error messages, security headers, rate limits, CORS and database settings.
4. deepcheck: deep audit of login, permissions, payments and input handling.
5. hackercheck: reviews your app like an attacker would and fixes what it finds.

## How to use

Run them in this order, one at a time. Just talk to your agent:

1. "Run secretguard on this project"
2. "Run datasafe on this project"
3. "Run prelaunch before I deploy"
4. "Run deepcheck, this app has Stripe payments and custom login"
5. "Run hackercheck"

The agent will also pick the right skill by itself when you say things like "is my app ready to launch" or "are my API keys safe".

Every skill ends with a clear report of what it found and what it changed. Read it. That list is the best way to learn your app's weak points.

Run hackercheck again after every major new feature, because new code means new attack surface.

## Important notes

These skills catch common mistakes. They do not replace a professional security review. If your app handles real money or sensitive data at scale, get a human review before launch.

If a secret was ever committed to git, rotate it right away. Deleting it from the code does not remove it from history.

## Contributing

Pull requests are welcome. Keep skills short, practical and written in plain language. Each skill needs a clear name and a description that says when to use it.

## Credits

The checks follow practices from open source tools and guides: Gitleaks, Bearer, Trail of Bits security skills and the OWASP Top 10.

## License

MIT. See LICENSE.

Built by Fasih Ahmed Raza
