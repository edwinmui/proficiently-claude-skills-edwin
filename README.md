# Proficiently

A Claude Code plugin for AI-powered job searching, resume tailoring, and cover letter writing.

## Skills

| Skill | Command | Description |
|-------|---------|-------------|
| [Job Search](./skills/job-search/) | `/proficiently:job-search` | Automated job search on hiring.cafe with smart filtering |
| [Tailor Resume](./skills/tailor-resume/) | `/proficiently:tailor-resume` | Create tailored resumes for specific job postings |
| [Cover Letter](./skills/cover-letter/) | `/proficiently:cover-letter` | Write natural, persuasive cover letters |

## How They Work Together

1. **`/proficiently:job-search`** finds jobs that match your preferences and resume
2. **`/proficiently:tailor-resume`** rewrites your resume for a specific job posting, saves the job posting and tailored resume together
3. **`/proficiently:cover-letter last`** writes a cover letter using the most recent job's posting and tailored resume

All skills share a `data/` directory for personal files. Each job application gets its own folder containing the posting, tailored resume, and cover letter.

## Installation

### 1. Install the plugin

```bash
claude plugin add https://github.com/proficientlyjobs/proficiently-claude-skills.git
```

### 2. Add your resume

Copy your resume into the plugin's data directory:

```bash
cp /path/to/your/resume.pdf "$(claude plugin path proficiently)/data/resume/"
```

### 3. Run setup

```
/proficiently:job-search setup
```

This will verify your resume and configure your job preferences.

## Prerequisites

- [Claude Code CLI](https://claude.ai/code)
- [Claude in Chrome](https://chromewebstore.google.com/detail/claude-in-chrome) extension (for browser automation)
- Chrome browser running with the extension active

## File Structure

```
proficiently-claude-skills/
├── .claude-plugin/
│   └── plugin.json                     # Plugin manifest
├── skills/
│   ├── job-search/
│   │   ├── SKILL.md
│   │   ├── assets/templates/
│   │   └── scripts/
│   ├── tailor-resume/
│   │   ├── SKILL.md
│   │   ├── assets/templates/
│   │   └── scripts/
│   └── cover-letter/
│       ├── SKILL.md
│       └── scripts/
├── data/                               # All personal data (gitignored)
│   ├── resume/                         # Your resume PDF/DOCX
│   ├── profile.md                      # Work history from interview
│   ├── preferences.md                  # Job matching rules
│   ├── job-history.md                  # Running log from job-search
│   └── jobs/                           # One folder per application
│       ├── google-lead-gpm-2026-02-11/
│       │   ├── posting.md              # Saved job description
│       │   ├── resume.md               # Tailored resume
│       │   └── cover-letter.md         # Cover letter
│       └── ...
└── README.md
```

All personal data (resume, preferences, generated documents) is gitignored and stays local.

## License

MIT
