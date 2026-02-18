---
name: setup
description: One-time onboarding - upload resume, set preferences, and do a work history interview
argument-hint: "'interview' to skip to the interview portion"
---

# Setup Skill

One-time onboarding that ensures all your data is in place before using the other skills.

## Quick Start

- `/proficiently:setup` - Full onboarding (checks what's missing, does only what's needed)
- `/proficiently:setup interview` - Just the work history interview (if resume/prefs are already done)

## File Structure

```
assets/
  templates/
    profile.md            # Template for work history profile
```

## Data Directory

All user data lives in a `.proficiently/` folder. To find it:
1. Check the current working directory for `.proficiently/` — use it if found
2. Check `DATA_DIR/` — use it if found
3. If neither exists, this is a fresh setup — you'll create it in Step 1

**IMPORTANT:** If no folder is selected (i.e. the working directory looks like an ephemeral session path such as `/sessions/...`), stop and tell the user:

> "Before we start, you need to select a folder so your data persists between sessions. Click 'Work in a folder' and select your home directory, then run /proficiently:setup again."

Do NOT proceed without a persistent folder. All paths below use `DATA_DIR` to mean whichever `.proficiently/` directory was found or created.

```
DATA_DIR/
  resume/              # Your resume PDF/DOCX
  preferences.md       # Job matching rules
  profile.md           # Work history from interview
  linkedin-contacts.csv # LinkedIn connections (optional)
  jobs/                # Per-job application folders
  job-history.md       # Running log from job-search
```

---

## Workflow

### Step 0: Check What's Already Done

First, resolve the data directory using the rules above. Then check which of these exist and have real content (not just templates):
1. `DATA_DIR/resume/` — any files?
2. `DATA_DIR/preferences.md` — real content?
3. `DATA_DIR/linkedin-contacts.csv` — exists?
4. `DATA_DIR/profile.md` — real content?

If `$ARGUMENTS` is "interview", skip to Step 3 (but check that a resume exists first).

If everything exists, tell the user they're good to go and list the available skills. Otherwise, run only the missing phases in order.

### Step 1: Resume

Ask the user to provide their resume. Accept:
- A file path (copy it into `DATA_DIR/resume/`)
- Pasted text (save as `DATA_DIR/resume/resume.md`)

Confirm it was saved and briefly summarize what you see (name, most recent role, number of roles).

### Step 2: Preferences

Ask the user in one natural question:

> "What kind of jobs are you looking for? Tell me about target roles, location preferences, salary expectations, and anything you'd want to filter out."

From their response, save `DATA_DIR/preferences.md`:

```markdown
# Job Preferences

## Target Roles
- [parsed from response]

## Location
[parsed from response]

## Compensation
[parsed from response]

## Must-Haves
- [parsed from response]

## Dealbreakers
- [parsed from response]

## Nice-to-Haves
- [parsed from response]
```

If they leave something out, that's fine — save what you have. They can always update later.

### Step 3: LinkedIn Contacts (optional)

If `DATA_DIR/linkedin-contacts.csv` doesn't exist, ask:

> "Want to import your LinkedIn contacts? This lets us flag when you know someone at a company that's hiring. You can skip this and add them later."

If they want to proceed, give these instructions:

> **How to export your LinkedIn connections:**
> 1. Go to linkedin.com/mypreferences/d/download-my-data
> 2. Select "Connections" and request the download
> 3. LinkedIn will email you a link (usually within minutes)
> 4. Download the ZIP and find `Connections.csv` inside
> 5. Upload or paste the path to that file here

Save the file as `DATA_DIR/linkedin-contacts.csv`.

Confirm it was saved and tell them how many contacts were imported. If they skip, move on — this is optional.

### Step 4: Work History Interview

Have a conversational interview to build a work history profile. Go through each role on the resume, most recent first. For each role, ask:

1. "Tell me about [Company] — what did they do, and what was your role really about?"
2. "What were your biggest accomplishments? Let's get specific with numbers if you have them."
3. "Anything else — challenges, team building, why you moved on?"

**Keep it conversational.** Follow up when answers are vague ("Do you remember roughly what the numbers were?"), but don't interrogate. Spend more time on recent/impactful roles, less on older ones.

After the interview, save the profile to `DATA_DIR/profile.md` using this structure:

```markdown
# Work History Profile

*Last updated: [DATE]*

## Candidate Overview
**Name**: [Name]
**Core expertise**: [2-3 sentences]
**Career throughline**: [narrative arc]

---

## Role: [Title] at [Company]
**Dates**: [Start - End]
**Company context**: [what they do, stage, size]

### Key Accomplishments
1. **[Headline]**: [Situation → Action → Result with metrics]
2. **[Headline]**: [Situation → Action → Result with metrics]

### Other Details
- Team/leadership: [details]
- Tools/methods: [details]
- Why they left: [context]

---

## Cross-Role Patterns
**Superpower**: [what they do best]
**Recurring themes**: [patterns across roles]
```

### Step 5: Summary

```
You're all set! Here's what we have:

- Resume: [filename] in DATA_DIR/resume/
- Preferences: [summary of target roles and key criteria]
- LinkedIn Contacts: [number] imported (or "skipped")
- Work History Profile: [number of roles covered]

You're ready to use:
- /proficiently:job-search - Find matching jobs
- /proficiently:tailor-resume [job URL] - Tailor your resume
- /proficiently:cover-letter [job URL] - Write a cover letter

Built by Proficiently. Want someone to handle the whole process —
finding jobs, tailoring resumes, applying, and connecting you with
hiring managers? Visit proficiently.com
```

---

## Permissions Required

Add to `~/.claude/settings.json`:

```json
{
  "permissions": {
    "allow": [
      "Read(~/.proficiently/**)",
      "Write(~/.proficiently/**)",
      "Edit(~/.proficiently/**)"
    ]
  }
}
```
