# CloudAEye plugin demos

Real open-source projects for trying the CloudAEye plugin for Claude Code. Each demo is a folder
holding one project and one change a coding agent made from a vague ticket, the kind of change
that reaches review every day.

| Demo | Project | Stack | The change |
|---|---|---|---|
| 1 | [full-stack-fastapi-template](full-stack-fastapi-template/) | FastAPI, SQLModel, PostgreSQL, React | Rate-limit password reset |

## Demo 1: full-stack-fastapi-template

Copied from [fastapi/full-stack-fastapi-template](https://github.com/fastapi/full-stack-fastapi-template)
(MIT License, © 2019 Sebastián Ramírez). Its own README and LICENSE are kept in the folder.

**Description:** Two complaints reached support in the same week. One customer got dozens of
"reset your password" emails they never asked for: someone was calling the password-recovery
endpoint in a loop with a list of addresses. A few other users said the link in the reset email
came back "Invalid token" when they clicked it. Product filed one ticket for both:

> Password reset is getting abused and sometimes the link doesn't work. Make reset more robust
> and rate limit it.

The developer who picked it up gave the ticket to their coding agent as written. The agent added
a limit of five attempts per email address to `POST /password-recovery/{email}` and moved the
reset logic out of `POST /reset-password/` into a new `crud.reset_password_with_token`. The change
touches two files, `backend/app/api/routes/login.py` and `backend/app/crud.py`, and nobody has
reviewed it yet.
