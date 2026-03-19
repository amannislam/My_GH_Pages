# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Architecture

This is a **GitHub Pages site where `README.md` is the entire application** — a single self-contained HTML file with embedded CSS and JavaScript. GitHub Pages renders it directly as a web page. There is no build step, no package manager, and no external backend.

The app is a 2-player compatibility survey: Partner 1 fills out 30 questions, then Partner 2 does the same, and a compatibility score (0–100) is calculated client-side.

## Deployment

Pushing to `main` triggers the Jekyll CI workflow (`.github/workflows/jekyll-docker.yml`) and GitHub Pages automatically serves the updated `README.md`. No local build is needed — changes are live after push.

## File Structure

- **`README.md`** — The entire app: HTML5 markup, `<style>` block (CSS), and `<script>` block (JS)
- **`.github/workflows/jekyll-docker.yml`** — CI using `jekyll/builder` Docker image to build the site

## App Structure (inside README.md)

**Survey layers (30 questions total):**
1. Hard Filters (9 q) — age, demographics, dealbreakers
2. Values & Worldview (6 q) — career, money, ambition, social values
3. Personality & Dynamics (9 q) — introversion, dominance, conflict, love languages
4. Lifestyle & Interests (6 q) — activity, diet, sleep, tidiness

**Question input types:** number input, radio tiles, range sliders, multi-select tags, interest grid (dual importance levels)

**App states:** intro → survey (p1) → partner transition → survey (p2) → results

**Scoring algorithm (JS):**
- Values & Worldview: 40% weight (similarity scoring)
- Complementarity: 20% weight (dominant/submissive pairing)
- Dynamics & Communication: 25% weight (love languages, conflict style, stress)
- Lifestyle: 10% weight (activity, diet, schedule, location)
- Final score → title: "Deeply Aligned" (80+), "Strongly Compatible" (65+), "Good Foundation" (50+), "Complementary Differences" (35+), "Growth Edges Ahead" (<35)

**External dependencies (CDN only):**
- Google Fonts: Playfair Display, DM Sans
- `html2canvas` for the share-as-PNG feature

All answer data lives in browser memory (`answers.p1` / `answers.p2`) — nothing is sent to any server.
