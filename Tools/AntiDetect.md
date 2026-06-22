---
type: tool
category: antidetect
created: 2026-06-01
---

# Anti-Detect Stack

## Components
| Tool | Purpose | Cost |
|------|---------|------|
| **CapSolver** | Captcha solving | $0.4 / 1k |
| **GPM** | Browser automation daemon | Port `:19995` |
| **NetNut** | Residential proxy network | $8 / GB |
| **Twitter via browser** | Session cookie auth (not xurl OAuth) | — |

## Twitter Auth
- Twitter access = **browser session cookie**, NOT xurl OAuth
- xurl tool exists but OAuth flow broken for @sanonihongo
- Workflow: `browser_navigate` → login manually → extract session cookie

## Setup Locations
- GPM daemon: `localhost:19995` (default config)
- CapSolver API key: `/home/node/.capsolver-key`
- NetNut API key: `/home/node/.netnut-key`
- Twitter session cookie: extract from logged-in browser, store locally

## Common Pitfalls
- xurl OAuth cho Twitter fails silently — không retry, switch sang browser ngay
- GPM port conflicts nếu có nhiều daemons — check `ss -ltnp | grep 19995`
- CapSolver credits: 1K credits = $0.4, theo dõi usage
