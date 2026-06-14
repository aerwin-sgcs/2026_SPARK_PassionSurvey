# Spark — Passion Discovery Lesson · Handoff Document
**For use at the start of a new Claude session**
**Last updated: June 2026**

---

## What this project is

A complete 30-minute digital lesson for gifted students in a standard classroom. The goal is to help students identify and ignite their passion areas using a survey-based elimination tool, a card game, and a micro-commitment activity.

The lesson has 4 phases:
1. **Hook** (5 min) — "lost in time" warm-up question
2. **Survey** (12 min) — 50-domain rule-out survey on student devices
3. **Card game** (8 min) — 3 game options to narrow sparks to 1–3
4. **Ignite** (5 min) — micro-commitment and public statement

---

## Files produced in this project

### 1. Main student app — `passion_survey_v4.html` (82kb)
The primary deliverable. A self-contained HTML file hosted on GitHub Pages.

**Features:**
- 50 domains across 8 colour-coded categories
- 4 rating buttons per domain: ✗ (no thanks) / ~ (maybe) / ★ (interested) / ★★ (most interested)
- Hard block at 3 × ★ and 3 × ★★ — buttons lock once limit reached
- Slot tracker bar showing fills (dark bar below hero section)
- Sticky progress bar showing rated count
- Floating shortlist bar at bottom showing selected sparks as student rates
- Results screen with two labelled groups:
  - ★★ "Your most interested areas" — strongest sparks, pursue first
  - ★ "Your next interests" — worth exploring after top areas
- Copy to clipboard button (plain text)
- Download as PDF button (opens print dialog)
- Phase 4 — Ignite page (slides in from right, slides back out)

**Phase 4 page contains:**
- Step 1: Spark sentence textarea (pre-fills with top ★★ domain if survey completed)
- Step 2: Micro-action selector (Watch / Ask / Make / Read) — tap to choose
- Step 3: Say it aloud instructions + teacher script
- Step 4: Close the loop teacher script
- Generate commitment card button → produces copy + PDF download

**Deployment:**
- Hosted on teacher's GitHub Pages account
- URL format: `[username].github.io/[repository-name]`
- Students open URL in Safari on iPad — works perfectly
- DO NOT deliver as a file attachment in Schoology, Google Classroom, or any LMS — file viewers sandbox JavaScript and no buttons work

**Known working delivery method:**
- GitHub Pages URL → pasted into Google Classroom as a LINK (not file) → students tap in Safari

### 2. Printable survey — `passion_survey.pdf`
- A4, 2 pages
- Name/class/date line at top
- ✗ / ~ / ★ legend in colour
- All 50 domains in 8 colour-coded categories
- Footer reminder telling students what to do after finishing
- For students without devices or as backup

### 3. Interest card deck — `interest_card_deck.pdf`
- A4, cards print 4 across
- 50 domain cards, colour-coded by category
- Instructions page at top (how to prep, cut, 3 game summaries)
- Print on card stock (160–200gsm) for best durability

### 4. Teacher guide (interactive widget in Claude)
- Full word-for-word scripts for all 4 phases
- Timing for each step
- Warnings for specific gifted student pitfalls
- Collapsible sections per phase

---

## Technical notes for future coding sessions

### Language and approach
- Pure HTML/CSS/JS — single self-contained file, no frameworks, no dependencies
- No external requests (fonts use system font stack: `-apple-system, BlinkMacSystemFont, sans-serif`)
- All 50 domain rows rendered as **static HTML** (not built by JS) — critical for Safari compatibility
- Button interactions use `classList.add/remove/toggle` exclusively — never `className` string manipulation or regex
- Phase 4 overlay uses `transform: translateX(100%)` / `translateX(0%)` + `pointer-events:none/auto` — never `display:none/block` on fixed full-screen elements (causes Safari iPad touch blocking bug)

### The bugs we fixed and why
| Bug | Cause | Fix |
|-----|-------|-----|
| Questions not showing | JS built DOM at runtime, failed silently | Bake all HTML statically at build time |
| No buttons working (all) | Fixed overlay with display:none intercepting all touches | Use translateX to park overlay off-screen |
| Buttons not responding | Unescaped apostrophes in JS strings crashing entire script | Escape all apostrophes as `\'` in JS single-quoted strings |
| className manipulation failing on Safari | String regex replace unreliable cross-browser | Use classList exclusively |
| Fonts not loading | Google Fonts CDN blocked on school networks | Use system font stack, no external CDN |

### JS validation — always do this before shipping
```bash
# Extract JS and validate with Node before copying to outputs
python3 -c "
content = open('file.html').read()
start = content.find('<script>') + len('<script>')
end = content.rfind('</script>')
js = content[start:end]
with open('/tmp/test.js', 'w') as f:
    f.write(js)
"
node --check /tmp/test.js
```

### Python build pattern
- The HTML is built using Python string formatting
- Use a plain string concatenation approach (not f-strings with JS content) to avoid `{{` / `}}` escaping issues
- Or use f-strings but double-brace all JS curly braces: `{{` and `}}`
- Always check for apostrophes in JS strings after build

### File sizes
- Test file: ~2kb
- Full app v4: ~82kb
- PDFs: ~8–10kb each

---

## The 50 domains (8 categories)

### Mathematics & computing (7)
Mathematics & logic, Coding & programming, Artificial intelligence, Robotics & electronics, Data & statistics, Cybersecurity & cryptography, Game design & simulation

### Natural sciences (9)
Physics & forces, Chemistry & materials, Biology & living things, Genetics & evolution, Neuroscience & the brain, Astronomy & space, Ecology & environment, Geology & earth science, Climate & weather systems

### Engineering & making (6)
Mechanical engineering, Electrical engineering, Architecture & design, 3D printing & fabrication, Aviation & transport, Sustainable technology

### Humanities & society (9)
History & civilisations, Archaeology, Philosophy & ethics, Psychology & behaviour, Sociology & culture, Politics & governance, Law & justice, Economics & finance, Geography & places

### Language & communication (6)
Creative writing & fiction, Poetry & spoken word, Journalism & non-fiction, Linguistics & language, Debate & public speaking, Translation & interpretation

### Arts & creative practice (8)
Visual art & illustration, Photography & film, Animation & motion design, Graphic design & typography, Music composition, Music performance, Theatre & performance, Dance & movement

### Health & human development (6)
Medicine & human health, Mental health & therapy, Nutrition & dietetics, Sport science & exercise, Education & learning, Child & human development

### Business & enterprise (4)
Entrepreneurship & startups, Marketing & brand strategy, Social enterprise & impact, Supply chains & logistics

---

## Colour system

| Category | Primary | Light bg | Border |
|----------|---------|----------|--------|
| Maths & computing | #534AB7 | #EEEDFE | #AFA9EC |
| Natural sciences | #0F6E56 | #E1F5EE | #5DCAA5 |
| Engineering | #BA7517 | #FAEEDA | #EF9F27 |
| Humanities | #993C1D | #FAECE7 | #F0997B |
| Language | #72243E | #FBEAF0 | #ED93B1 |
| Arts | #993C1D | #FAECE7 | #F0997B |
| Health | #185FA5 | #E6F1FB | #85B7EB |
| Business | #3B6D11 | #EAF3DE | #97C459 |
| Gold (★★) | #9A6F00 | #FFF8DC | #FFD700 |

---

## Phase 4 — exact wording (do not change)

### Why this phase exists
The lesson is worthless if it ends with a feeling that doesn't become an action. The research on implementation intentions (if-then planning) shows that small, specific, low-cost commitments made in public are far more likely to be followed through than general intentions made privately.

### Step 1 — Name the spark (1 min)
**Teacher script:** "At the top of your paper, write: 'I think I'm curious about ________ because ________.' Fill in both blanks. Not 'science' — be more specific. Not 'I like it' — tell us why. You've got sixty seconds."

**Strong example:** "I think I'm curious about how the brain stores memories because I forget things and want to understand why."
**Too vague:** "I think I'm curious about maths."

### Step 2 — Choose a micro-action (1 min)
**Teacher script:** "You're going to pick one thing to do in the next 48 hours — not a project, not a goal, just a tiny first step."

Options:
- **Watch** — Find one video, ten minutes or less, on your topic tonight
- **Ask** — Find one person who knows something about this and ask them one question
- **Make** — Spend fifteen minutes trying to create or do something related to your topic
- **Read** — Find one article, one page of a book, or one thread online about it

### Step 3 — Say it out loud (2 min)
**Teacher script:** "We're going to go around the room. Each person says three things: your name, your spark, and your action. Ten seconds each. I'll start: I'm curious about how gifted students lose interest in learning, and I'm going to read one paper about it tonight."

**Warning:** Do not ask students to justify their choice. Justification pressure causes gifted students to second-guess and edit down to something more socially acceptable.

### Step 4 — Close the loop (1 min)
**Teacher script:** "Come back and tell me what happened — whether the feeling held up, got bigger, or completely changed. All three are useful information. Finding out you don't actually care about something after trying it? That's not failure, that's exactly what this lesson is for."

---

## Possible future improvements discussed but not built

- Student name field at top of survey (saves with their PDF)
- Teacher dashboard showing class-wide spark results
- A follow-up check-in survey (did you do your micro-action?)
- Adapting the lesson for specific age groups
- Shorter 20-domain version for younger students
- Class results aggregator so teacher can see most common sparks

---

## How to start a new session

Paste this into Claude at the start of a new chat:

> "I am continuing work on the Spark passion discovery lesson for gifted students. Here is my handoff document: [paste this file]. The latest working file is passion_survey_v4.html hosted on GitHub Pages. I need help with: [describe what you want to do next]."
