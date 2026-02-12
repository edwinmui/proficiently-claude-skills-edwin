---
name: cover-letter
description: Write a tailored cover letter for a specific job posting
argument-hint: "job URL, or 'last' to use the most recent job"
---

# Cover Letter Skill

Write natural, persuasive cover letters that sound like a real professional wrote them.

## Quick Start

- `/proficiently:cover-letter` - Start the flow (will ask for a job URL)
- `/proficiently:cover-letter https://...` - Write a cover letter for a specific job posting
- `/proficiently:cover-letter last` - Write a cover letter for the most recent job

## File Structure

```
scripts/
  write-cover-letter.md       # Cover letter writing agent prompt
```

User data (stored at ~/.proficiently/):
```
~/.proficiently/
  resume/              # Your resume PDF/DOCX
  preferences.md       # Job matching rules
  profile.md           # Work history from interview
  jobs/                # Per-job application folders
  job-history.md       # Running log from job-search
```

---

## Workflow

### Step 0: Check Prerequisites

Check that the required data files exist:
- `~/.proficiently/resume/*` - at least one resume file (besides README.md)

If the resume is missing, tell the user: "Run `/proficiently:setup` first." Then stop.

Check `~/.proficiently/profile.md` - if missing or just a template, warn that the cover letter will be based only on the resume (recommend running `/proficiently:setup interview` first for better results), but proceed.

### Step 1: Get Job Details

**If `$ARGUMENTS` is "last" or empty:**
- Check `~/.proficiently/jobs/` for the most recently modified folder
- If found, read `posting.md` and `resume.md` from that folder
- Confirm with the user which job this is for
- If no job folders exist, ask the user for a job URL

**If `$ARGUMENTS` is a URL:**
- Check if a job folder already exists for this company in `~/.proficiently/jobs/`
- If yes, read the existing `posting.md` and `resume.md`
- If no, use Claude in Chrome MCP tools to fetch the job posting:
  ```
  1. tabs_context_mcp -> get browser state
  2. tabs_create_mcp -> new tab
  3. navigate -> job URL
  4. get_page_text -> extract full job posting
  ```
- Save the posting to `~/.proficiently/jobs/[company-slug]-[date]/posting.md` if not already saved

If the page can't be loaded, ask the user to paste the job description directly.

### Step 2: Gather Materials

For the target job folder, check what exists:
- `posting.md` - the job description (required)
- `resume.md` - a tailored resume (optional, improves quality significantly)

If no tailored resume exists, use the original resume and work history profile directly.

### Step 3: Write the Cover Letter

Follow the framework in `scripts/write-cover-letter.md`. Use:
- The work history profile (or original resume if no profile)
- The tailored resume for this role (if available)
- The job posting

The cover letter must:
- Be 250-350 words
- Start with "Dear Hiring Manager,"
- End with "Regards, [Name]"
- Use ONLY hyphens, never em dashes
- Sound like a real human wrote it
- Never fabricate or exaggerate any detail
- Connect 2-3 specific, measurable achievements to the employer's needs

### Step 4: Present and Save

Save to `~/.proficiently/jobs/[company-slug]-[date]/cover-letter.md`

Present the cover letter to the user with:
- The full text
- A brief note on which achievements were highlighted and why
- The file path where it's saved

### Step 5: Iterate

Ask if the user wants to adjust:
- Tone (more formal, more casual, more technical)
- Which achievements to highlight
- Specific phrasing
- Length

Apply changes and re-save.

---

## Permissions Required

Add to `~/.claude/settings.json`:

```json
{
  "permissions": {
    "allow": [
      "Read(~/.claude/skills/**)",
      "Read(~/.proficiently/**)",
      "Write(~/.proficiently/**)",
      "Edit(~/.proficiently/**)",
      "mcp__claude-in-chrome__*"
    ]
  }
}
```
