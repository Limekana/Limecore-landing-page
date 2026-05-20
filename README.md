# Limecore

Independent software studio landing page. Single `index.html`, deployed via Vercel from `Limekana/Limecore-landing-page` on GitHub.

## TODO before HN / Show HN launch

- [ ] **Migrate to a real domain** — currently on `limecore.vercel.app`. For HN, r/selfhosted, X build-in-public distribution, swap to `limecore.dev` / `.studio` / `.fi`. Prerequisite for the public launch push.
- [x] ~~Add LimeLog hero screenshot~~ — Lock in + Phases shipped.
- [x] ~~Add StudyDesk hero screenshot~~ — Grades + Pomodoro shipped.
- [ ] Open-source LimeLog and Nexus repos; wire real GitHub URLs into the card link slots.
- [ ] Flip on F-Droid badges as submissions are accepted (LimeLog, StudyDesk, Nexus).
- [ ] Ship `/blog` and replace the `Blog ↗` placeholder href. First post: Realtime cross-app sync architecture.
- [ ] Replace footer X and Email placeholders with real handle / address.

## Stack

- Plain HTML/CSS/JS — no framework, no build step.
- Hosted via Vercel (static), auto-deploys from `main`.
- Screenshots in repo root: `Nexus_HQ_*_Main.png` (priority), `Portfolio.png` / `Weekly_review.png` / `What_if.png` (secondary), `Limelog_*.png`, `StudyDesk_*.png`.
- Lightbox: click any screenshot to enlarge. ESC or click outside to close.
