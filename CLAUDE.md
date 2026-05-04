# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Running the App

No build system. Open `index.html` directly in a browser. The three HTML files are fully self-contained:

- `index.html` — landing/portal selection page
- `studentView.html` — student-facing interface
- `advisorView.html` — advisor dashboard

## Architecture

**Pure client-side prototype** — vanilla JS, no frameworks, no npm, no backend. All CSS and JS are inline in each HTML file. All data is hardcoded mock data; nothing persists across page refreshes.

### studentView.html

Three views toggled by sidebar nav (`showSection(id)`):

1. **Degree Plan Visualizer** — renders 32 courses (`const DP`) across 8 semesters as clickable tiles. SVG arrows (`dpArrows()`) draw prerequisite links between tiles. Drop Impact mode (`dpImpact()`) uses BFS to highlight all downstream courses blocked by dropping a selected course. State lives in `dpDone` (Set) and `dpProg` (Set).

2. **Alex Academic Coach** — a scripted chatbot. Pre-authored responses are keyed in the `replies` object. User selections (via "quick take" ✓/✗/? buttons stored in `takes`) branch into `replies.fromtakes` for context-aware replies. Typing animation via `setTimeout` chains.

3. **Advisor Notes Hub** — static notes data object displayed in a tabbed view.

### advisorView.html

Five views toggled by sidebar nav:

1. **Risk Dashboard** — filters the `students` array (8 entries) by status (Red/Amber/Green). Expanding a student card shows an intelligence brief. Section demand alerts are hardcoded metric cards.

2. **Section Demand Alerts** — three hardcoded demand cards (MATH 250 urgent, COMP 305, FIN 310). "Send to Dean" button toggles a success state.

3. **Cohort Patterns**, **Bulk Messaging**, **Dean's Report** — static display/placeholder views.

### Key data shapes

```js
// studentView.html
const DP = [{ id, code, name, credits, prereqs: [], sem, desc }]  // 32 courses
const replies = { queryKey: "response text", fromtakes: fn }       // chatbot scripts
const notes = { tabId: [{ author, time, text, tags }] }

// advisorView.html
const students = [{ id, name, gpa, credits, status, alerts, brief }]  // 8 students
```

## CSS Conventions

Color palette is defined as CSS custom properties on `:root` (`--navy`, `--purple`, `--mint`, etc.). All views share this palette. Layouts use CSS Grid for metric cards and Flexbox for content areas.

## Known Prototype Limitations

- No persistence (data resets on refresh)
- No real auth, messaging, or PDF export — those buttons show placeholder alerts
- "Export PDF" and "Send to Dean" are UI demos only
