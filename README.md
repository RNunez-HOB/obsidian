# Data Science & ML Obsidian Vault

A personal knowledge vault for tracking your growth across Data Science and Machine Learning. Built for Obsidian's graph view.

## Setup

1. Open Obsidian.
2. Choose **Open folder as vault** and pick this `Data-Science-Vault/` folder.
3. Open `00-Index.md` — that's your map of content.
4. Open the Graph View (Ctrl/Cmd+G) to see the subject network.

## Structure

```
Data-Science-Vault/
├── 00-Index.md             ← start here (Map of Content)
├── README.md               ← this file
├── Subjects/               ← one note per subject (30 total)
├── Projects/               ← your project notes live here
└── Templates/              ← copy these when adding a new project
    └── _Template-Project.md
```

## Daily workflow

**Learned a new skill?**
Open the subject note, add a bullet under Learned, and (optionally) link to a project note that demonstrates it.

**Starting a new project?**
1. Duplicate `Templates/_Template-Project.md` into `Projects/`.
2. Rename it, e.g. `Churn-Prediction-Retail.md`.
3. Inside, link every subject it touches: `[[Regression Models]]`, `[[Feature Engineering]]`, etc.
4. Those subject notes will now show your project under their backlinks.

**Graph view tips**
- Hover over a node to see its connections light up.
- Click a subject to jump to its note.
- Use the tag filter to isolate what you're learning right now (`tag:#learning`).
- In graph settings, color groups by tag — e.g. color all `#project` nodes green and `#subject` nodes blue.

## Recommended Obsidian plugins

- **Dataview** — query your skills and projects (e.g. list all projects tagged `#deployed`).
- **Templater** — smarter templates when creating new project notes.
- **Graph Analysis** — surface which subjects have the most project connections.
