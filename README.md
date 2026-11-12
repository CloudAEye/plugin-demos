# CloudAEye plugin demos

Real open-source projects for trying the [CloudAEye plugin for Claude Code](https://github.com/CloudAEye/cloudaeye-plugin)
on your own machine. Each demo is a folder holding one project, plus one change a coding agent made
from a vague ticket, the kind of change that reaches review every day. The change sits on a branch of
its own, so you can review it before it is committed and again as a pull request.

| Demo | Project | Stack | The change | Branch |
|---|---|---|---|---|
| 1 | [full-stack-fastapi-template](full-stack-fastapi-template/) | FastAPI, SQLModel, PostgreSQL, React | Rate-limit password reset | `demo-1/reset-rate-limit` |

You set things up once, then you can run any demo.

- [Set up](#set-up)
- [Demo 1: full-stack-fastapi-template](#demo-1-full-stack-fastapi-template)
- [Troubleshooting](#troubleshooting)

## Set up

When you finish this section, you have your own fork of this repository and it is connected to your
own CloudAEye account.

### What you need

- A GitHub account.
- Claude Code, installed and signed in.
- Git, Python 3 and curl on your `PATH`. The plugin's commands run through them. On Windows, Git for
  Windows includes Git and curl, and Python comes from [python.org](https://www.python.org/downloads/).
- Optional: the [GitHub CLI](https://cli.github.com) (`gh`), which makes forking and opening a pull
  request one command each.

To check, run these in a terminal. Each one should print a version:

```bash
git --version
python3 --version
curl --version
claude --version
```

On Windows, if `python3` prints nothing or opens the Microsoft Store, run `python --version`
instead. The plugin tries both.

### 1. Create a CloudAEye account

Sign up at [console.cloudaeye.com/auth/sign-up](https://console.cloudaeye.com/auth/sign-up). The Free
plan needs no card.

The Free plan runs `/cloudaeye:review`, `/cloudaeye:inspect`, `/cloudaeye:describe` and
`/cloudaeye:ask`. The other commands in these demos need the Pro plan. Each run of a command uses
credits; [Pricing](https://docs.cloudaeye.com/user-guide/code-review/billing/pricing.html) lists how
many.

### 2. Fork this repository

CloudAEye only reviews repositories you connect to your own account, and you can only connect a
repository you control. So the demos run on your own fork.

**With the GitHub CLI.** This forks the repository with all its branches and clones your fork:

```bash
gh repo fork CloudAEye/plugin-demos --clone
```

```bash
cd plugin-demos
```

**On github.com.** Open [github.com/CloudAEye/plugin-demos/fork](https://github.com/CloudAEye/plugin-demos/fork)
and **clear "Copy the main branch only"**, because each demo's change is on its own branch. Create the
fork, then clone it. Replace `<your-username>` with your GitHub username:

```bash
git clone https://github.com/<your-username>/plugin-demos.git
```

```bash
cd plugin-demos
```

If you forked the main branch only, that is fine: each demo shows how to fetch its branch.

### 3. Install the plugin

Start Claude Code in the repository's root folder:

```bash
claude
```

Then type these in Claude Code, one at a time:

```text
/plugin marketplace add CloudAEye/cloudaeye-plugin
```

```text
/plugin install cloudaeye@cloudaeye
```

Claude Code may offer some settings during the install. They are all optional, so you can skip them.

**Restart Claude Code now.** Type `/exit`, then run `claude` again. The commands appear right after the
install, but the connection to CloudAEye's server only starts when Claude Code starts.

If you prefer a terminal to Claude Code, these two commands do the same install:

```bash
claude plugin marketplace add CloudAEye/cloudaeye-plugin
```

```bash
claude plugin install cloudaeye@cloudaeye
```

### 4. Sign in

In Claude Code:

```text
/mcp
```

1. Pick the CloudAEye server, listed as `plugin:cloudaeye:cloudaeye`.
2. Choose **Authenticate**. A browser window opens.
3. Sign in with your CloudAEye account and allow access.
4. When the browser says you are connected, go back to Claude Code.

Claude Code keeps you signed in after this, and refreshes the sign-in on its own.

### 5. Connect your fork

In Claude Code, from the repository's root folder:

```text
/cloudaeye:init
```

1. It reads your fork's address from the git remote named `origin`.
2. It asks which branch to compare changes against, showing `main`. Press Enter to keep `main`.
3. The first time, it opens CloudAEye's GitHub page in your browser. Install the CloudAEye GitHub App
   there, choose **Only select repositories**, and pick your `plugin-demos` fork.
4. Go back to Claude Code. `/cloudaeye:init` checks every ten seconds for up to five minutes, and
   reports the repository as initialized once the app can see it.

CloudAEye then indexes the repository. If a command reports that the repository has no code graph
yet, wait a few minutes and run the command again.

### 6. Check the install

```text
/cloudaeye:help
```

It lists every CloudAEye command. You're ready to run a demo.

**Tip:** you can run a shell command without leaving Claude Code by starting the line with `!`, for
example `!git status`.

## Demo 1: full-stack-fastapi-template

Copied from [fastapi/full-stack-fastapi-template](https://github.com/fastapi/full-stack-fastapi-template)
(MIT License, © 2019 Sebastián Ramírez), with its git history. Its own README and LICENSE are in the
folder.

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

### Get the change

```bash
git checkout demo-1/reset-rate-limit
```

If git says there is no such branch, your fork has the main branch only. Fetch the branch from this
repository, push it to your fork, then run the checkout again:

```bash
git fetch https://github.com/CloudAEye/plugin-demos.git demo-1/reset-rate-limit:demo-1/reset-rate-limit
```

```bash
git push origin demo-1/reset-rate-limit
```

The change is committed on this branch, and that's fine. CloudAEye's commands review everything
since your branch left `main`, committed or not, so they see the change exactly as the coding agent
left it.

### Review it before it is committed

Run these in Claude Code, from the repository's root folder, in any order. Each one prints a short
card. The longer ones (`review-arch`, `control-flow`, `risk-score`, `slop-score`, `ultrareview`) also
save the whole report, diagrams included, to `.cloudaeye/reports/<command>.md`. If you run a command
again on the same change, it answers from the earlier run and costs nothing more.

Find bugs and security issues, each with a number, file, line and severity:

```text
/cloudaeye:review
```

Check whether the change does what the ticket asked. You get DONE or NOT DONE, with what is missing:

```text
/cloudaeye:check-task Password reset is getting abused and sometimes the link doesn't work. Make reset more robust and rate limit it.
```

See how much careful reading the change needs, as a score out of 100 with the findings behind it:

```text
/cloudaeye:slop-score
```

See what the change did to the architecture: the system, the repository's layers and the change itself:

```text
/cloudaeye:review-arch
```

See the order of calls before and after the change, as sequence diagrams:

```text
/cloudaeye:control-flow
```

Find out whether the change needs a human reviewer, and why:

```text
/cloudaeye:risk-score
```

Run every check at once, opening with a merge verdict and a table of results:

```text
/cloudaeye:ultrareview
```

### Fix a finding

Pick a finding's number from the `/cloudaeye:review` output and ask for a fix plan. CloudAEye plans the
fix and Claude Code edits the files:

```text
/cloudaeye:implement [1]
```

Replace `1` with the number you picked, or list several, like `[1,3]`. Run `/cloudaeye:review` again
afterwards to see whether the fix landed.

To put the demo back the way it was:

```bash
git restore .
```

### Review it as a pull request

Open a pull request in **your fork**, from `demo-1/reset-rate-limit` into `main`.

On github.com, GitHub suggests `CloudAEye/plugin-demos` as the base repository. Change it to your
fork. CloudAEye can only read a pull request whose two branches are both in the repository you
connected.

With the GitHub CLI, replacing `<your-username>`:

```bash
gh pr create --repo <your-username>/plugin-demos --base main --head demo-1/reset-rate-limit --title "Rate-limit password reset" --body "Password reset is getting abused and sometimes the link doesn't work. Make reset more robust and rate limit it."
```

Note the pull request's number. The commands below use `#1`, so put your own number in its place.

Run every check on the pull request and post the whole report as one comment:

```text
/cloudaeye:ultrareview #1
```

Score the pull request's risk and post the risk card on it:

```text
/cloudaeye:risk-score #1
```

Find out who should review it and what they should check:

```text
/cloudaeye:assign-reviewer #1
```

Nobody reviews their own pull request. In a personal fork, you are often the only person with
access, so this command may find nobody to suggest. Add collaborators to your fork to see it choose
between people.

Find out which open pull request to review next, across every repository you connected:

```text
/cloudaeye:triage-prs
```

**What gets written to GitHub:** on a pull request, `ultrareview`, `risk-score`, `review-arch` and
`control-flow` post a comment. They post once per commit, and some edit their comment when you run
them again. `assign-reviewer` requests a review from the recommended reviewer when that person can
review in the repository. Add `just suggest` after the number to get the suggestion without the
request. Nothing else in this demo writes to GitHub.

## Troubleshooting

| What you see | What to do |
|---|---|
| `/plugin marketplace add` fails | That shorthand clones over SSH. Quit Claude Code, set `CLAUDE_CODE_PLUGIN_PREFER_HTTPS=1` in your shell, start `claude` again and retry. |
| `/plugin install` says the plugin is not found | Run `/plugin marketplace update cloudaeye`, then the install again. |
| The `/cloudaeye:*` commands exist but nothing runs | The server did not connect. Restart Claude Code. |
| `/mcp` shows CloudAEye as **Needs authentication** | Choose it in `/mcp`, then **Authenticate**. |
| `cloudaeye_error=python_not_found` | Install Python 3 and make sure it is on your `PATH`. |
| `ambiguous_remote` from `/cloudaeye:init` | Your clone has several git remotes and none is named `origin`. `git remote -v` lists them. Name your fork's remote `origin`. |
| `base_source=head` in a command's output | The repository is not connected to your account. Run `/cloudaeye:init`. |
| A command says it needs the Pro plan | The Free plan runs `review`, `inspect`, `describe` and `ask`. See [Pricing](https://docs.cloudaeye.com/user-guide/code-review/billing/pricing.html). |
| A pull-request command says the pull request "comes from" another repository | The pull request was opened against `CloudAEye/plugin-demos`. Open it in your fork instead. |

The plugin's full guide is at [docs.cloudaeye.com](https://docs.cloudaeye.com/user-guide/mcp/setup-claude-code.html).
For anything else, write to [support@cloudaeye.com](mailto:support@cloudaeye.com).
