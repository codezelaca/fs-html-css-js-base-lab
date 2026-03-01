# FS HTML CSS JS Base Lab

A complete teaching repository for fundamentals of **HTML, CSS, and JavaScript** using a single-page product-style landing page.

This project is intentionally simple in tooling and rich in learning value. It demonstrates how to build and reason about real UI behavior without frameworks, while introducing patterns that map directly to modern frontend development.

## Table of Contents

1. [Project Overview](#project-overview)
2. [What Students Build](#what-students-build)
3. [Learning Outcomes](#learning-outcomes)
4. [Tech Stack and Philosophy](#tech-stack-and-philosophy)
5. [Repository Structure](#repository-structure)
6. [How to Run the Project](#how-to-run-the-project)
7. [Detailed Code Walkthrough](#detailed-code-walkthrough)
8. [UI and Interaction Flows](#ui-and-interaction-flows)
9. [Accessibility and UX Notes](#accessibility-and-ux-notes)
10. [State, Data, and Rendering Model](#state-data-and-rendering-model)
11. [Validation Logic Explained](#validation-logic-explained)
12. [Responsive Design Strategy](#responsive-design-strategy)
13. [Persistence with localStorage](#persistence-with-localstorage)
14. [Teaching Plan and Classroom Flow](#teaching-plan-and-classroom-flow)
15. [Hands-On Exercises](#hands-on-exercises)
16. [Manual Test Checklist](#manual-test-checklist)
17. [Common Issues and Troubleshooting](#common-issues-and-troubleshooting)
18. [Extension Ideas](#extension-ideas)
19. [Known Limitations in Current Implementation](#known-limitations-in-current-implementation)
20. [Suggested Refactors for Advanced Learners](#suggested-refactors-for-advanced-learners)
21. [Glossary](#glossary)
22. [Contributing for Instructors](#contributing-for-instructors)
23. [License](#license)

## Project Overview

This repo contains a compact frontend lab that teaches core web development concepts by building an interactive single-page interface with:

- A responsive header with mobile navigation toggle
- A hero area with dynamic active-user ticker
- Tabbed feature content driven by JavaScript state
- Pricing cards rendered from data with monthly and annual billing switch
- A sign-up form with client-side validation and inline errors
- Saved UI preferences using `localStorage`

The lab is designed to help students move from static pages to **state-driven UI thinking**.

## What Students Build

Students build and understand a page that includes:

- Semantic layout sections: `header`, `main`, `section`, `footer`
- Reusable CSS component classes and design tokens
- Dynamic DOM updates using `innerHTML` and event listeners
- Event delegation for scalable interaction handling
- Form input validation and success/error messaging
- Simple persistent preferences across reloads

By the end, learners should be comfortable reading and writing code that connects data, state, rendering, and user interaction.

## Learning Outcomes

After completing this lab, students should be able to:

1. Explain semantic HTML structure and why it matters.
2. Build responsive layouts with CSS Grid, Flexbox, and media queries.
3. Use CSS custom properties (`:root` variables) for maintainable styling.
4. Model UI behavior with a central JavaScript `state` object.
5. Render UI based on current state rather than hardcoded markup.
6. Attach and reason about event listeners for click/change/submit flows.
7. Validate form inputs with practical rules and user feedback.
8. Persist selected preferences in `localStorage` and reload safely.
9. Understand how vanilla JS patterns transfer to React/Next.js concepts.

## Tech Stack and Philosophy

### Stack

- HTML5
- CSS3
- Vanilla JavaScript (ES6+)
- Browser APIs (`localStorage`, DOM, events)

### Philosophy

- No build tools required.
- No frameworks required.
- Simple enough for beginners.
- Structured enough to teach professional frontend habits.

This makes the repo ideal for early labs, bootcamp modules, and fundamentals review sessions.

## Repository Structure

```text
fs-html-css-js-base-lab/
├── index.html     # Page structure and semantic content
├── styles.css     # Design tokens, layout, components, responsiveness
└── app.js         # State, data, rendering, events, validation, persistence
```

### File Responsibilities

- `index.html`: Defines all major UI sections and accessibility attributes.
- `styles.css`: Defines visual language, spacing system, component styles, and breakpoints.
- `app.js`: Drives interactive behavior and keeps UI synchronized with app state.

## How to Run the Project

### Option 1: Open directly in the browser

1. Clone or download the repository.
2. Open `index.html` in a browser.
3. Interact with tabs, billing toggle, pricing buttons, and form.

### Option 2: Use a local static server (recommended for teaching)

If you already use a local server tool (for example, VS Code Live Server), serve the folder and open the served URL. This is useful for classroom demos and consistent reload behavior.

### Requirements

- Modern browser (Chrome, Edge, Firefox, Safari)
- No package installation required

## Detailed Code Walkthrough

## HTML (`index.html`)

The HTML file is organized into reusable conceptual components:

1. **Site Header**
- Logo link
- Mobile menu button with ARIA attributes
- Primary navigation links to page sections

2. **Hero Section**
- Main headline
- Introductory teaching copy
- Two call-to-action buttons
- Dynamic stat card (`#liveUsers`)

3. **Features Section**
- Tab list (`role="tablist"`)
- Tab buttons with `data-tab` keys
- Empty tab panel (`#tabPanel`) populated by JS

4. **Pricing Section**
- Billing toggle (`#billingToggle`)
- Empty card container (`#pricingCards`) rendered from pricing data

5. **Signup Section**
- Form (`#signupForm`) with `novalidate`
- Inputs for name, email, and track
- Inline error targets (`data-error-for`)
- Status message (`#formMsg`)

6. **Footer**
- Session context text

### Why this HTML structure is effective for teaching

- Shows semantic landmarks clearly.
- Keeps dynamic containers explicit.
- Demonstrates form labeling and error mapping.
- Provides section IDs for navigation and smooth scrolling.

## CSS (`styles.css`)

The stylesheet is structured around maintainability and reuse.

### 1. Design Tokens

Defined in `:root`:

- Color system (`--bg`, `--card`, `--text`, `--muted`, `--line`)
- Radius (`--r`)
- Spacing scale (`--s1` to `--s6`)
- Max content width (`--max`)
- Shared shadow (`--shadow`)

This is a practical intro to design systems.

### 2. Base Layer

- `box-sizing: border-box`
- Margin/padding reset on `html, body`
- Base typography and link behavior
- `.sr-only` utility for accessible hidden text

### 3. Layout and Components

- Sticky header with translucent background and backdrop blur
- Grid-based hero layout
- Reusable button system (`.Btn`, `.Btn--primary`, `.Btn--ghost`)
- Generic section card treatment
- Tabs styling with active state class
- Pricing card grid
- Form field and inline error styling
- Footer styling

### 4. Responsive Breakpoints

- `@media (max-width: 900px)`:
  - Hero collapses to one column
  - Pricing cards stack vertically
- `@media (max-width: 720px)`:
  - Hamburger button appears
  - Nav becomes hidden popover
  - `.Nav.is-open` controls visibility

This gives students a clear, realistic responsive strategy.

## JavaScript (`app.js`)

The JS file follows a highly teachable architecture.

### 1. State

```js
const state = {
  billing: "monthly",
  activeTab: "speed",
};
```

State is the single source of truth for key UI behavior.

### 2. DOM references

The script grabs essential elements once (`menuBtn`, `nav`, `tabs`, `tabPanel`, `billingToggle`, `pricingCards`) and reuses them.

### 3. Data models

- `featureCopy`: content for each tab
- `pricing`: plan list with monthly/annual prices and perks

This demonstrates separation of data from presentation.

### 4. Helper functions

- `savePref()` and `loadPref()` for persistence
- `scrollToId(id)` for smooth section navigation

### 5. Rendering functions

- `renderNavButton(open)`
- `renderTabs()`
- `renderBillingToggle()`
- `renderPricing()`
- `renderAll()`

Each function updates one part of the UI from state.

### 6. Event handlers

- Menu button toggles mobile nav
- Tab buttons update `state.activeTab` and rerender tabs
- Billing toggle updates `state.billing` and rerenders pricing
- Pricing card clicks use event delegation and update form message

### 7. Form validation

- `setError(fieldName, message)`
- `isValidEmail(email)`
- `validate(values)`

Checks include:

- Name length at least 2 characters
- Valid email pattern and no consecutive dots
- Required track selection

### 8. Live users ticker

`startLiveUsersTicker()` updates `#liveUsers` every 1.5 seconds with bounded random change.

### 9. Boot sequence

- `loadPref()`
- `renderAll()`
- `startLiveUsersTicker()`

This initializes persisted state and paints UI correctly on load.

## UI and Interaction Flows

### Mobile menu

1. User clicks menu button.
2. `is-open` class toggles on nav.
3. `aria-expanded` syncs with actual state.

### Feature tabs

1. User clicks a tab button.
2. `state.activeTab` updates.
3. Tab panel content rerenders from `featureCopy`.
4. Active button class/ARIA updates.

### Pricing toggle

1. User toggles checkbox.
2. `state.billing` switches between monthly/annual.
3. Pricing cards rerender with matching price and suffix.

### Plan selection to signup

1. User clicks `Choose <Plan>`.
2. Event delegation identifies selected plan.
3. Informational message shown near form.
4. Page scrolls to signup section.

### Form submission

1. User submits form.
2. Validation runs and writes inline errors.
3. If valid, success message displays and form resets.

## Accessibility and UX Notes

Positive patterns already present:

- Semantic sectioning and landmarks
- ARIA on menu button and tablist
- Screen-reader-only helper class
- Inline field error slots per input
- Status region for form feedback (`role="status"`)
- Keyboard-friendly native controls (buttons, input, select)

Teaching opportunity:

- Discuss why `aria-expanded` must always match visible nav state.
- Explain `novalidate` as a learning choice for custom validation practice.

## State, Data, and Rendering Model

This repo is an excellent "pre-framework" exercise.

The mental model is:

1. Keep canonical UI facts in state.
2. Store text/plan content as plain data.
3. Render UI from state and data.
4. Let events mutate state.
5. Re-render the affected part.

That is the same core architecture students later use in component frameworks.

### Simplified flow

```text
User action -> Event handler -> Update state -> Render function -> Updated DOM
```

### Why this matters

Students avoid scattered one-off DOM changes and learn predictable UI logic.

## Validation Logic Explained

The form validation combines user-friendly feedback and practical rules.

- Name rule: prevents empty or too-short names.
- Email rule: uses a baseline regex and rejects obvious bad formats like double dots.
- Track rule: enforces explicit selection.

Inline error rendering with `data-error-for` is a reusable pattern:

- Keep validation rules separate.
- Keep UI error display centralized.
- Avoid deeply coupling rule logic to specific DOM structures.

## Responsive Design Strategy

The CSS teaches responsive behavior through progressive adjustments:

- Desktop first for clarity.
- Collapse multi-column content at smaller widths.
- Introduce mobile-specific nav pattern only when needed.

Key ideas for learners:

- Breakpoints should respond to layout pressure, not random device names.
- Content hierarchy should remain intact across screen sizes.
- Interactive elements must remain reachable and usable on mobile.

## Persistence with localStorage

The repo persists user preference state for:

- Selected tab
- Billing mode

Pattern used:

1. Save only minimal necessary state.
2. Parse safely with `try/catch`.
3. Validate loaded values before trusting them.
4. Render after loading.

This is a strong beginner-safe persistence pattern.

## Teaching Plan and Classroom Flow

This structure works well for a 90 to 150 minute guided lab.

### Module 1: HTML skeleton and semantics (20-30 min)

- Walk through section landmarks.
- Identify IDs used for navigation and JS targeting.
- Explain why some containers are intentionally empty and filled by JS.

### Module 2: CSS system thinking (20-30 min)

- Show tokenized variables.
- Explain reusable component classes.
- Demonstrate responsive breakpoints and why they are chosen.

### Module 3: JavaScript architecture (30-45 min)

- Introduce state and data separation.
- Trace one render function end-to-end.
- Trace one event flow from click to rerender.

### Module 4: Validation and persistence (20-30 min)

- Validate and submit flow.
- localStorage save/load cycle.
- Discuss reliability and graceful failure.

### Module 5: Refactor challenge (optional 20-40 min)

- Have students add a new tab and plan.
- Add one additional form rule.
- Improve code style or robustness without changing UX.

## Hands-On Exercises

Use these exercises in order.

### Exercise A: Add a new feature tab

Goal:
- Add a fourth tab called `Security`.

Tasks:
1. Add a new tab button in HTML with `data-tab="security"`.
2. Add matching `featureCopy.security` data in JS.
3. Verify tab rendering and active class behavior.

Learning focus:
- Data-driven rendering and button-state sync.

### Exercise B: Add a new pricing plan

Goal:
- Add an `Enterprise` plan with custom perks.

Tasks:
1. Add a new object to `pricing` array.
2. Confirm automatic rendering with existing map logic.
3. Test click behavior and signup prompt.

Learning focus:
- Rendering lists from data and event delegation stability.

### Exercise C: Add stronger validation

Goal:
- Prevent disposable/test domains.

Tasks:
1. Extend `validate(values)` with a blocked-domain check.
2. Show clear inline error message.
3. Test accepted and rejected emails.

Learning focus:
- Extending business rules safely.

### Exercise D: Add keyboard enhancement

Goal:
- Allow left/right arrow keys to move between tabs.

Tasks:
1. Add keydown listener on tab list.
2. Cycle selected index.
3. Update state and rerender.

Learning focus:
- Accessible keyboard interactions.

### Exercise E: Improve nav behavior

Goal:
- Close mobile nav when a nav link is clicked.

Tasks:
1. Add click listener on nav container.
2. Detect anchor clicks.
3. Remove `is-open` and sync `aria-expanded`.

Learning focus:
- State synchronization and UX details.

## Manual Test Checklist

Use this checklist before marking lab completion.

### General

- [ ] Page loads with styles and JS behavior active.
- [ ] Header, hero, features, pricing, signup, footer all visible.

### Navigation

- [ ] Mobile menu button appears below 720px.
- [ ] Menu opens/closes correctly.
- [ ] `aria-expanded` updates as menu state changes.

### Tabs

- [ ] Default tab content appears on load.
- [ ] Clicking each tab updates panel content.
- [ ] Active tab visual state and `aria-selected` are correct.

### Pricing

- [ ] Monthly pricing is default.
- [ ] Annual toggle updates all plan prices.
- [ ] Toggle state persists after reload.

### Plan Selection

- [ ] Clicking plan button updates signup message.
- [ ] Page scrolls to signup section.

### Form

- [ ] Empty submit shows errors.
- [ ] Invalid email shows email error.
- [ ] Missing track shows track error.
- [ ] Valid submit shows success message and resets fields.

### Persistence

- [ ] Active tab persists after reload.
- [ ] Billing mode persists after reload.

## Common Issues and Troubleshooting

### "Nothing happens when I click tabs or toggle"

Possible cause:
- `app.js` failed to load.

Checks:
1. Ensure `<script src="./app.js" defer></script>` is present.
2. Open browser console for runtime errors.
3. Confirm file names match exact casing.

### "Styles are missing"

Possible cause:
- Stylesheet path issue.

Checks:
1. Ensure `<link rel="stylesheet" href="./styles.css" />` exists.
2. Confirm `styles.css` is in repository root.

### "localStorage values do not persist"

Possible cause:
- Browser settings/private mode restrictions.

Checks:
1. Test in normal browsing mode.
2. Inspect Application/Storage tab in DevTools.

### "Form submits but data is not sent anywhere"

This is expected.

The lab focuses on client-side validation and UI feedback only. There is no backend integration in the current version.

## Extension Ideas

These are high-value next steps for assignments or capstones.

1. Connect form submission to a real API endpoint.
2. Add loading, success, and error states for async requests.
3. Add dark/light theme toggle persisted to localStorage.
4. Replace `innerHTML` rendering with element creation for stronger safety.
5. Add unit tests for `isValidEmail` and `validate`.
6. Add E2E checks for tabs, pricing toggle, and form flows.
7. Add analytics hooks for plan and CTA clicks.
8. Convert to modules and split by concerns.
9. Rebuild this exact app in React while preserving behavior.

## Known Limitations in Current Implementation

These are useful teaching points, not failures.

1. `signupForm` and `formMsg` are referenced directly without explicit `getElementById` declarations in JS.
2. This often works in browsers that expose element IDs as global variables, but it is brittle and not ideal for production reliability.
3. `innerHTML` is used for convenience and readability in a lab context. This is acceptable here, but it requires care in real applications where content might come from untrusted input.
4. The tab UI uses ARIA roles but does not include full keyboard-arrow tab navigation logic yet.
5. No automated tests are included.

## Suggested Refactors for Advanced Learners

1. Explicitly bind form DOM elements in JS:

```js
const signupForm = document.getElementById("signupForm");
const formMsg = document.getElementById("formMsg");
```

2. Extract tab rendering into smaller pure functions.
3. Move repeated inline style attributes from HTML templates into CSS classes.
4. Add a simple `render(state)` orchestrator with section-level diffing strategy.
5. Introduce JS modules (`state.js`, `render.js`, `events.js`, `validation.js`).
6. Add lightweight test runner setup (Vitest/Jest) for validation logic.

## Glossary

- **State**: The current values that control what the UI shows.
- **Rendering**: Updating the DOM to reflect current state/data.
- **Event delegation**: Attaching one parent listener to handle child element clicks efficiently.
- **Design tokens**: Reusable visual constants like colors and spacing variables.
- **Semantic HTML**: Meaningful structure elements that improve readability and accessibility.
- **ARIA**: Attributes that improve accessibility for assistive technologies.
- **Progressive enhancement**: Building baseline functionality first, then adding richer behavior.

## Contributing for Instructors

If you use this repository for teaching, keep updates aligned to the learning goals.

Recommended contribution pattern:

1. Keep each commit focused on one concept.
2. Prefer small, teachable diffs with clear comments.
3. Update this README whenever behavior changes.
4. Add manual test steps for every new feature.
5. Preserve beginner readability before advanced abstractions.

## License

No license file is currently included in this repository.

If you plan to distribute or reuse this material publicly, add an explicit license (for example MIT) based on your institution or team policy.
