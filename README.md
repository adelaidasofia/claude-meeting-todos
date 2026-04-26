# claude-meeting-todos

A Claude Code plugin that extracts action items from meeting notes or transcripts and adds them to your to-do file. Separates your tasks from others' tasks, flags pending decisions, and confirms before writing.

---

## What it does

After a meeting, you run `/meeting-todos`. The skill:

1. Finds the most recent meeting note (or one you specify by date/title)
2. Reads the notes or transcript
3. Extracts your action items, separates them from others' tasks, and flags decisions still pending
4. Shows you a preview and waits for confirmation
5. Appends the items to your to-do file in the format you already use

The point is to never lose an action item to "I'll write that down later" — and to never let the conversation die in a notebook nobody opens.

---

## Install

```bash
claude plugin add github.com/adelaidasofia/claude-meeting-todos
```

Then in any Claude Code session:

```
/meeting-todos
```

On first use, Claude asks where your meeting notes live and where your to-do file is. Takes 30 seconds, never asks again.

---

## Requirements

- Claude Code
- A folder where meeting notes are saved as markdown
- A to-do file (any format)

**Optional integrations:**
- Google Workspace MCP — pull from Google Docs (e.g. Gemini Notes meeting transcripts)
- Granola, Otter, Fireflies, Read.ai — any tool that exports markdown to a folder works the same way
- The daily-journal plugin — if installed, action items can also flow into your journal's accountability check

---

## Configuration

On first use, preferences are saved to `[VAULT]/.meeting-todos-prefs.md`:

```markdown
meeting_folder: Meeting Notes/
todo_path: TODO.md
meeting_source_integration: none
```

Edit this file anytime to change your setup.

---

## How action items are extracted

**Your tasks** — anything where you were directly assigned, volunteered, or own the follow-up.

**Others' tasks** — assigned to someone else. Listed for visibility, NOT added to your to-do.

**Decisions pending** — unresolved questions surfaced for follow-up.

The skill writes them in the format your to-do file already uses (sectioned by meeting), without duplicating items already present and without reordering anything.

---

## Why a separate skill (vs. a generic to-do tool)

Most to-do tools assume the input is already a clean list. Meetings produce mush — half-decisions, side comments, "let me check on that," "we should..." Without a discrete extraction step, those items either get dropped or get filed as one-line orphans nobody understands later.

This skill makes the extraction visible: you see what got pulled before it lands in your to-do, and the items are sectioned by meeting so context isn't lost.

---

## Submitting to the Cowork marketplace

Once the repo is on GitHub, submit at: **[clau.de/plugin-directory-submission](https://clau.de/plugin-directory-submission)**

For the community marketplace:

```bash
claude plugin marketplace add anthropics/claude-plugins-community
```

---

## License

MIT
