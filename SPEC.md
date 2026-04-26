# SPEC.md — Git Workflow Automator

## 1. Concept & Vision

A smart git assistant that takes the intimidation out of complex version control workflows. Paste any diff, branch name, or git log — get back a complete, sequenced git workflow with AI-generated commit messages, PR descriptions, and copy-paste commands. Feels like having a senior developer looking over your shoulder saying "here's exactly what to do next."

## 2. Design Language

- **Aesthetic:** Developer-tool dark — VS Code-inspired with terminal warmth
- **Colors:**
  - Background: `#0d1117`
  - Surface: `#161b22`
  - Border: `#30363d`
  - Primary: `#58a6ff` (link blue)
  - Accent: `#3fb950` (success green)
  - Text: `#c9d1d9`
  - Muted: `#8b949e`
- **Typography:** `'JetBrains Mono', 'Fira Code', monospace` for all code/command output; `system-ui, -apple-system, sans-serif` for UI text
- **Motion:** Minimal — results slide in, subtle glow on command blocks, 200ms ease-out transitions

## 3. Layout & Structure

```
┌─────────────────────────────────────────────────────┐
│ Header: Logo + Tagline                               │
├─────────────────────────────────────────────────────┤
│ Input Section:                                       │
│ ┌─────────────────────────────────────────────────┐ │
│ │ Paste git diff / branch name / log here...     │ │
│ │                                                 │ │
│ │                                                 │ │
│ └─────────────────────────────────────────────────┘ │
│ [Analyze Git Workflow]                               │
├─────────────────────────────────────────────────────┤
│ Output Section (appears after analysis):             │
│ ┌─ Branch Strategy ────────────────────────────────┐ │
│ │ main → feature/xyz → PR merge                   │ │
│ └─────────────────────────────────────────────────┘ │
│ ┌─ Commit Sequence ──────────────────────────────┐ │
│ │ 1. fix: resolve off-by-one in loop             │ │
│ │ 2. feat: add user authentication flow          │ │
│ │ 3. docs: update README for new env vars         │ │
│ └─────────────────────────────────────────────────┘ │
│ ┌─ PR Description ───────────────────────────────┐ │
│ │ ## Summary                                      │ │
│ │ ## Changes                                       │ │
│ │ ## Testing                                       │ │
│ └─────────────────────────────────────────────────┘ │
│ ┌─ One-Click Commands ────────────────────────────┐ │
│ │ git checkout -b feature/xyz                    │ │
│ │ git add . && git commit -m "fix: ..."          │ │
│ │ git push -u origin feature/xyz                 │ │
│ └─────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────┘
```

- Single column, max-width 720px, centered
- Responsive: full-width on mobile

## 4. Features & Interactions

### Input Modes
- **Diff paste:** Detects `diff --git`, parses added/removed lines, generates commit strategy
- **Branch name:** Parses branch name for type/ticket, suggests workflow
- **Git log:** Parses commit history, suggests squashing/branching/cleanup

### AI Workflow Generation
1. Parse input to detect type (diff/log/branch)
2. Analyze changes for commit grouping
3. Generate commit messages (Conventional Commits format)
4. Suggest branch strategy
5. Write PR description with summary/changes/testing sections
6. Output runnable git commands

### Interactions
- Paste into textarea → click button → instant analysis
- Copy individual command blocks with one click (shows "Copied!" feedback)
- Copy full workflow as markdown

### States
- **Empty:** Placeholder text with example
- **Loading:** Spinner + "Analyzing your git workflow..."
- **Results:** Full workflow display
- **Error:** Red border on textarea + inline error message

## 5. Component Inventory

### Header
- Git icon (SVG) + "Git Workflow Automator" title
- Tagline in muted text below

### Input Card
- Dark surface background with subtle border
- Textarea: 200px min-height, resizable, monospace font
- Placeholder with example diff snippet

### Analyze Button
- Full-width below textarea
- Primary blue, hover brightens, disabled state while loading

### Output Blocks (Branch Strategy / Commit Sequence / PR Description / Commands)
- Each in a bordered card with label header
- Command blocks: copy button top-right, monospace text, dark bg
- "Copied!" toast on copy (fades 1.5s)

### Copy All Button
- Appears below all output blocks
- Secondary style, copies markdown-formatted workflow

## 6. Technical Approach

- **Single HTML file** — all CSS and JS inline
- **Vanilla JS** — no frameworks, no build step
- **Client-side AI simulation:** Rule-based parsing of git diff/log/branch formats to generate plausible workflow output. No API calls.
- **LocalStorage:** Save last input for convenience (optional)
- **Clipboard API:** For copy functionality