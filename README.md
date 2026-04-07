# GHL Email Ecosystem — Architecture Guide

Live documentation for the GoHighLevel email platform architecture, covering frontend apps, backend services, workers, PubSub topics, data models, and developer onboarding.

## Live URL

**https://email-ecosystem-docs.vercel.app**

Hosted on Vercel free tier. Auto-deploys on every push to `main`.

## What's Inside

| Section | Covers |
|---------|--------|
| System Overview | Frontend layer, backend layer, external services, data stores |
| Repositories | All repos with paths and roles |
| Architecture Diagram | Full visual flow from browser → API → PubSub → workers → data stores |
| Frontend Deep Dive | 4 apps (email-home, email-builder, email-preview, default-emails) — routes, stores, MF config |
| Backend Deep Dive | 16 controllers, 9 services, sibling apps (email-ai, tracking-link, countdown-timer) |
| Workers | 10+ PubSub workers with subscriptions, processing logic, side effects |
| PubSub Topic Map | 13 topics with publishers, subscribers, message types |
| Data Layer | 20+ MongoDB collections, ClickHouse tables, Redis, GCS |
| Flow Diagrams (8) | SVG service-level diagrams for broadcast campaign, sequence, AB test, template save, unsubscribe, workflow integration, stats export, tracking links |
| External Service Map | 17 services called via Istio mesh |
| Environment & Config | Backend + frontend env vars, key config files |
| Developer Setup | Step-by-step onboarding: clone, auth, install, run, verify, common gotchas |

## Flow Diagrams

The doc includes 8 interactive SVG diagrams showing cross-service data flow:

1. **Broadcast Campaign** — 10-step flow across 6 layers with resend loop
2. **Sequence Drip Campaign** — Cloud Tasks delayed execution → Conversations send
3. **A/B Test Campaign** — Audience split, per-variation stats, winner selection
4. **Template Save & Snapshot** — Builder → MongoDB → PubSub → snapshot worker → GCS
5. **Unsubscribe/Resubscribe** — Recipient click → Conversations API → PubSub webhook
6. **Workflow Integration** — Two-stage pipeline (integration worker → precompute worker)
7. **Stats Export** — Async job: PubSub → worker → ClickHouse/Mongo → CSV to GCS
8. **Tracking Link Lifecycle** — Click redirect → PubSub → ClickHouse event log

## Updating

```bash
# Edit the doc
vim index.html

# Push — Vercel auto-deploys
git add -A && git commit -m "update docs" && git push
```

## Repo Setup Notes

This repo uses a personal GitHub account (`kumar8317`) with a dedicated SSH key, separate from the company GitHub (`kumar-ankit-dev`).

| Account | SSH Host | Key |
|---------|----------|-----|
| Company (`kumar-ankit-dev`) | `github.com` (default) | `~/.ssh/id_ed25519` |
| Personal (`kumar8317`) | `github.com-personal` | `~/.ssh/id_ed25519_personal` |

The git remote for this repo uses the `github.com-personal` host alias, so pushes route through the personal key automatically. All other repos continue using the company key by default.

To switch `gh` CLI for personal operations:

```bash
gh auth switch --user kumar8317     # switch to personal
# ... do personal stuff ...
gh auth switch --user kumar-ankit-dev  # switch back to company
```
