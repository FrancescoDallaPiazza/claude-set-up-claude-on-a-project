# Notes

## CLAUDE.md: what's in, what's out

**In:** a one-line description, the three npm scripts, the conventions Claude would otherwise guess wrong (CommonJS, `node:test` + supertest rather than Jest, data access only through `db/store.js`, the JSON error shape, no new dependencies), and a short architecture map showing where new code goes.

**Out:** anything Claude can read straight from the code (the full route list, the ESLint rules, seed users), setup steps from the README, and anything from `.env`. Those either go stale or waste context, and secrets never belong in a committed file.

## Permission rules

- **allow:** `npm test`, `npm run lint`, `git status`, `git diff`. They're read-only or safe and run constantly, so prompting for them is just noise.
- **ask:** `git push` and `npm install`. They reach outside the machine or change dependencies, so I want to confirm each one.
- **deny:** reading `.env` / `.env.*`, force-pushes, and `rm -rf`.

Without the deny rules, Claude could read real secrets from `.env` into the conversation (and from there into logs or commits), overwrite shared history on the remote with a force-push, or delete files recursively without me noticing in time.

## Verification

In a fresh session, `/memory` lists `./CLAUDE.md` as loaded and `/permissions` shows the allow / ask / deny rules above. Asking "How do I run the tests here?" gets `npm test` back without any extra explanation.
