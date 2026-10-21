## Brandon's words

> i've heard that there are security vulnerabilities found today in Node.js. I know some of our projects use that. I was told - if there's anything in node.js earlier than v22.23.4, v24.22.1, or v26.12.1, we need to update. If we deploy with docker, we need to rebuild the image. If we're still on node.js 18 or 20, we have to migrate.
>
> Please check through my main projects and diagnose if we need updates

> that link I sent may be a trap anyway - but the point is check all vulnerabilities released in the last day. Some major stuff out there

> ok let's get going on the fixes

## Context (Claude's diagnosis, not Brandon's words)

`.nvmrc` pins Node 24.18.0, which every CI and release job reads, below the Oct 21 fixed 24.22.1. `npm audit` shows vite 8.3.0–8.3.2 (launch-editor advisories published Oct 20). No Docker. `@modelcontextprotocol/server` is 2.3.1 (patched for the Oct 20 MCP advisories), but `incur@0.6.1` bundles `@modelcontextprotocol/server@2.0.0-alpha.4`; incur's latest (0.7.1) still pins that alpha.

## Plan

- `.nvmrc`: `24.18.0` → `24.22.1`.
- `npm audit fix` (non-breaking; lockfile only) with `--before=2026-10-19`, moving vite to a patched release.
- Left out, named for Brandon: raising rea's _supported_ Node floor (`engines`, installer, doctor, setup, 18 README translations, the CI job that deliberately tests 22.19.0) — a product decision, not a patch; and incur's bundled MCP alpha, which has no upstream fix.
- Proof: `npm run deps:check`, `npm run check:fast`, `npm run test:fast`, `npm audit` clean.
