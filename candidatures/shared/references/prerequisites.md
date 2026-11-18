# Prerequisites by Skill

Check that required data files exist before proceeding. If any required file is missing, show the failure message and stop.

## Required Files

| File | setup | job-search | tailor-resume | cover-letter | apply |
|------|:-----:|:----------:|:------------:|:------------:|:-----:|
| `DATA_DIR/resume/*` | — | Required | Required | Required | Required |
| `DATA_DIR/preferences.md` | — | Required | — | — | — |
| `DATA_DIR/profile.md` | — | — | Recommended | Recommended | Recommended |
| `DATA_DIR/linkedin-contacts.csv` | — | — | — | — | — |
| `DATA_DIR/reponses-types.md` | — | — | — | — | Required |

## Failure Messages

- **Resume missing**: "Run `/candidatures:setup` first to upload your resume."
- **Preferences missing**: "Run `/candidatures:setup` first to configure your resume and preferences."
- **Profile missing (tailor-resume)**: Warn that the resume will be based only on resume text and may require more corrections. Recommend running `/candidatures:setup interview` first. Allow the user to proceed if they choose.
- **Profile missing (cover-letter)**: Warn that the cover letter will be based only on the resume. Recommend running `/candidatures:setup interview` first. Proceed anyway.
