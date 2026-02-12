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

### Step 0: Check What's Already Done

Check which of these exist and have real content (not just templates):
1. `~/.proficiently/resume/` — any files?
2. `~/.proficiently/preferences.md` — real content?
3. `~/.proficiently/profile.md` — real content?

If `$ARGUMENTS` is "interview", skip to Step 3 (but check that a resume exists first).

If everything exists, tell the user they're good to go and list the available skills. Otherwise, run only the missing phases in order.

### Step 1: Resume

Ask the user to provide their resume. Accept:
- A file path (copy it into `~/.proficiently/resume/`)
- Pasted text (save as `~/.proficiently/resume/resume.md`)

Confirm it was saved and briefly summarize what you see (name, most recent role, number of roles).

### Step 2: Preferences

Ask the user in one natural question:

> "What kind of jobs are you looking for? Tell me about target roles, location preferences, salary expectations, and anything you'd want to filter out."

From their response, save `~/.proficiently/preferences.md`:

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

### Step 3: Work History Interview

Have a conversational interview to build a work history profile. Go through each role on the resume, most recent first. For each role, ask:

1. "Tell me about [Company] — what did they do, and what was your role really about?"
2. "What were your biggest accomplishments? Let's get specific with numbers if you have them."
3. "Anything else — challenges, team building, why you moved on?"

**Keep it conversational.** Follow up when answers are vague ("Do you remember roughly what the numbers were?"), but don't interrogate. Spend more time on recent/impactful roles, less on older ones.

After the interview, save the profile to `~/.proficiently/profile.md` using this structure:

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

### Step 4: Summary

```
You're all set! Here's what we have:

- Resume: [filename] in ~/.proficiently/resume/
- Preferences: [summary of target roles and key criteria]
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
