# Limecore

Independent software studio landing page. Single `index.html`, deployed via Vercel from `Limekana/Limecore-landing-page` on GitHub.

## TODO before HN / Show HN launch

- [ ] **Migrate to a real domain** — currently on `limecore.vercel.app`. For HN, r/selfhosted, X build-in-public distribution, swap to `limecore.dev` / `.studio` / `.fi`. Prerequisite for the public launch push.
- [x] ~~Add LimeLog hero screenshot~~ — Lock in + Phases shipped.
- [x] ~~Add StudyDesk hero screenshot~~ — Grades + Pomodoro shipped.
- [x] ~~Open-source LimeLog and Nexus repos; wire real GitHub URLs into the card link slots.~~ — live and linked.
- [ ] Flip on F-Droid badges as submissions are accepted (LimeLog, StudyDesk, Nexus).
- [x] ~~Ship `/blog`~~ — live at `/blog/`, first post "Vibe coding through high school" at `/blog/vibe-coding-through-high-school/`. (Cross-app sync architecture post still pending as post 002.)
- [x] ~~Replace footer X and Email placeholders with real handle / address.~~ — `x.com/l1m3core` and `l1m3core@gmail.com` live across landing + blog.

## Stack

- Plain HTML/CSS/JS — no framework, no build step.
- Hosted via Vercel (static), auto-deploys from `main`.
- Screenshots in repo root: `Nexus_HQ_*_Main.png` (priority), `Portfolio.png` / `Weekly_review.png` / `What_if.png` (secondary), `Limelog_*.png`, `StudyDesk_*.png`.
- Lightbox: click any screenshot to enlarge. ESC or click outside to close.
