---
name: meeting-todos
description: Extract action items from a meeting note and add them to the to-do list. Separates your tasks from others' tasks. Trigger /meeting-todos. Do NOT use for general task management or pulling full meeting transcripts.
argument-hint: "[date like 2026-04-09, or meeting title keyword — omit for most recent]"
---

# Meeting → To-Do Extractor

After a meeting, pull action items from the transcript or notes and update your to-do file.

## Trigger

`/meeting-todos` — optionally followed by a meeting title or date, e.g.:
- `/meeting-todos` → uses the most recent meeting note
- `/meeting-todos 2026-04-09` → finds meeting from that date
- `/meeting-todos Team Sync` → finds meeting matching that title

## First-run config

On first invocation, look for `[VAULT]/.meeting-todos-prefs.md`. If it doesn't exist, ask the user:

1. **Meeting notes folder:** Where do meeting notes live? (default: `Meeting Notes/`)
2. **To-do file path:** Where should action items go? (e.g. `TODO.md`, `tasks.md`, `Home/To-do.md`)
3. **Meeting source integration (optional):** Do you have a meeting notes integration like Granola, Otter, or Gemini Notes saving to a Google Doc? If yes, what's the source folder or doc path? If no, skip — just read meeting notes from the local folder.

Save preferences to `[VAULT]/.meeting-todos-prefs.md` as a single markdown file with frontmatter. Don't ask again on future runs.

## Step 1 — Find the meeting note

Check the configured `meeting_folder` (or any team meeting folder if set) for the most recent (or matching) meeting note.

If the user specified a date or title, find the matching file. If no argument, use the most recently modified file.

Tell the user which file you found before proceeding.

## Step 2 — Read the meeting

Read the full meeting note. It may contain:
- A **Notes** or **Summary** section (structured — prioritize this)
- A **Transcript** section (raw — extract from this if no summary)

## Step 3 — Extract action items

From the content, identify:

**Your tasks** — anything where:
- You were directly assigned ("You will...", "I'll...", your name + action)
- You volunteered ("I'm thinking...", "let me...")
- It requires your decision or follow-up ("we need to define X" where you're the one with context)
- You said you'd research or provide something

**Others' tasks** — anything assigned to someone else — list these separately for visibility but DO NOT add to your to-do.

**Decisions pending** — anything that was left unresolved and needs a meeting or decision soon.

## Step 4 — Confirm before writing

Show the user a preview:

```
## From: [Meeting Name] — [Date]

### Your action items (going to to-do):
- [ ] [task 1]
- [ ] [task 2]

### Others' tasks (not going to to-do — just FYI):
- [Name]: [task]

### Open questions / decisions pending:
- [item]

Shall I add these to your to-do? (yes/no, or edit first)
```

Wait for confirmation before writing.

## Step 5 — Update the to-do file

Use the `todo_path` from prefs.

Rules:
- Read the file first to understand current structure
- Add a new section or append to the most relevant existing section
- Use this format for the new items:

```markdown
## From [Meeting Name] — [Date]

- [ ] [task 1]
- [ ] [task 2]
```

Place it after any "Urgent / Active" section if the tasks are urgent, or at the bottom if they're not.

- Do NOT duplicate tasks already in the file
- Do NOT delete or reorder existing tasks
- Update the `updated:` field in frontmatter to today's date if it exists

## Step 6 — Save meeting summary (optional)

If the meeting note did NOT already have a structured summary, offer to add one at the top of the meeting file (above the Transcript section):

```markdown
## Summary

**Date:** [date]
**Attendees:** [names]
**Key decisions:** [bullet points]
**Your action items:** [bullet points]
**Others' action items:** [bullet points]
**Next steps:** [bullet points]
```

Ask the user before writing.

## Rules

- Always confirm before writing to to-do
- Transcripts in any language are fine — extract in the language of the meeting, or match the user's preference
- Be specific: write "Research response time benchmarks for X market" not "Do research"
- Include context in the task where helpful: "Discuss referral API payload with [Name] (what data to send when partner can't take event)"
- If a meeting had no clear action items, say so — don't invent tasks
- **NEVER fail silently.** If the meeting file doesn't exist or can't be read, tell the user immediately

## Optional integrations

- **Google Workspace MCP:** if installed, you can pull from Google Docs (e.g., Gemini Notes auto-saved meeting transcripts). The skill will detect the MCP and offer the option.
- **Granola:** if Granola is installed and saves to a local folder, set that as your `meeting_folder` in prefs.
- **Otter, Fireflies, Read.ai, etc.:** any meeting-transcription tool that exports markdown to a folder works the same way.
