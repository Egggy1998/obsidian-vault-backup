# Architecture Decision: Agency Agent Router (2026-06-02)

**Status:** ✅ APPROVED + EXECUTED
**Date:** 2026-06-02
**Author:** Babyboo (Hermes Agent) per directive từ anh Mi béo (Egggy)
**Scope:** Routing architecture cho Agent37/Hermes

## Context

Trước đây có **2 hệ routing chạy song song** gây miss info + linh tinh:
- **AGENTS.md SA workers** (8 agents: planner/writer/designer/poster/engager/analyst/researcher/video)
- **Agency Agent Router** (184 built-in agents tại `/home/node/.openclaw/agency-agents/`)

Conflict: cùng 1 task có 2 cách route → dễ chọn nhầm, miss thông tin, trả lời lung tung.

## Decision

**BỎ 3-tier Hub-Worker (8 SA) hoàn toàn. CHUYỂN sang single-tier: Main Agent (Babyboo) → Agency Agent Router → 184 specialist agents.**

### Routing mechanism

```bash
# CLI preferred
cd /home/node/.openclaw && python3 route_agent.py <agent-id> "<task message>"

# Alt
openclaw agent --agent <agent-id> --message "<task>" --local
```

**Source of truth:**
- `/home/node/.openclaw/agency-routing.json` — 184 task→agent routes
- `/home/node/.openclaw/agency-agents/` — mỗi agent có SOUL.md + IDENTITY.md + AGENTS.md

## 8 SA → Agency mapping (DEPRECATED)

| Cũ (SA) | Mới (Agency) |
|---------|--------------|
| SA-Planner | `ad-creative-strategist` / `content-creator` |
| SA-Writer | `content-creator` / `technical-writer` |
| SA-Designer | `image-prompt-engineer` / `visual-storyteller` |
| SA-Poster | `social-media-strategist` |
| SA-Engager | `twitter-engager` / `instagram-curator` / `reddit-community-builder` |
| SA-Analyst | `analytics-reporter` / `finance-tracker` |
| SA-Researcher | `growth-hacker` / `analytics-reporter` |
| SA-Video | `video-optimization-specialist` / `visual-storyteller` |

## 184 agents by category

| Category | Count | Key agents |
|----------|-------|-----------|
| general | 74 | account-strategist, brand-guardian, product-manager, seo-specialist, ui-designer, mcp-builder, wechat-official-account-manager, zhihu-strategist... |
| engineering | 35 | ai-engineer, backend-architect, frontend-developer, software-architect, security-engineer, solidity-smart-contract-engineer, voice-ai-integration-engineer... |
| social_media | 11 | twitter-engager, instagram-curator, tiktok-strategist, douyin-strategist, bilibili-content-strategist, linkedin-content-creator, reddit-community-builder... |
| devops | 9 | devops-automator, automation-governance-architect, workflow-architect, workflow-optimizer, infrastructure-maintainer, sre-site-reliability-engineer... |
| analytics | 8 | analytics-reporter, data-engineer, finance-tracker, report-distribution-agent, data-consolidation-agent, experiment-tracker... |
| gaming | 8 | game-designer, unity-architect, godot-gameplay-scripter, roblox-experience-designer, unreal-world-builder... |
| marketing | 7 | growth-hacker, paid-media-auditor, paid-social-strategist, ppc-campaign-strategist, programmatic-display-buyer, carousel-growth-engine... |
| legal_compliance | 7 | compliance-auditor, legal-document-review, blockchain-security-auditor, accessibility-auditor... |
| sales | 6 | sales-outreach, sales-coach, sales-engineer, salesforce-architect... |
| media | 6 | video-optimization-specialist, visual-storyteller, image-prompt-engineer, short-video-editing-coach... |
| management | 4 | agents-orchestrator, chief-of-staff, senior-project-manager, project-shepherd |
| content | 3 | content-creator, ad-creative-strategist, technical-writer |
| hr | 3 | hr-onboarding, recruitment-specialist, anthropologist |
| mobile | 2 | mobile-app-builder, app-store-optimizer |
| web3 | 1 | zk-steward |

## When to route vs tự làm

**Route qua `agency-agent-router` skill khi:**
- Mọi task có agent phù hợp trong 184 routes
- Multi-agent coordination → dùng `agents-orchestrator` hoặc `chief-of-staff`
- Ads/KPI reporting → MCP push `pnl-kpi-dashboard.vercel.app/api/mcp` trực tiếp

**Tự làm (không route) khi:**
- Tra cứu file / check system status
- Câu hỏi thông tin nhanh
- Check status cron/job
- Forward tin nhắn
- Health check nhỏ

## Execution Log

- 2026-06-02: AGENTS.md rewritten (Hub-Worker section removed, Agency Router section added)
- 2026-06-02: 8 SA worker skills marked DEPRECATED in `/home/node/.hermes/skills/workers/*/SKILL.md`
- 2026-06-02: This doc saved to Obsidian + pushed to GitHub (Egggy1998/quang-obsidian-vault, main)

## Lesson Learned

**Conflict between 2 routing systems = silent failure mode.** Khi anh Mi béo bảo "bỏ sa hub", phải execute migration đến cùng (rewrite + mark DEPRECATED + commit), KHÔNG dừng giữa chừng trả lời câu hỏi ngoài luồng.
