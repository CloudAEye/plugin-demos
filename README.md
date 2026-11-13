# CloudAEye plugin demos

Real open-source projects for trying the [CloudAEye plugin for Claude Code](https://github.com/CloudAEye/cloudaeye-plugin)
on your own machine. Each demo is a folder holding one project, plus one change a coding agent made
from a vague ticket, the kind of change that reaches review every day. The change sits on a branch of
its own.

All you need to try it is the plugin and a free CloudAEye account: no fork, no GitHub App, nothing to
connect. This repository is already connected to CloudAEye for everyone, so the demo commands show the
answers CloudAEye stored for each demo's change, at no cost on any plan. To edit the code and review
your own changes, fork it and run `/cloudaeye:init` on your fork; [Make your own changes](#make-your-own-changes)
shows how.

| Demo | Project | Stack | The change | Branch |
|---|---|---|---|---|
| 1 | [full-stack-fastapi-template](full-stack-fastapi-template/) | FastAPI, SQLModel, PostgreSQL, React | Rate-limit password reset | `demo-1/reset-rate-limit` |

- [Set up](#set-up)
- [Demo 1: full-stack-fastapi-template](#demo-1-full-stack-fastapi-template)
- [Make your own changes](#make-your-own-changes)
- [Troubleshooting](#troubleshooting)

## Set up

You do this once.

### What you need

- Claude Code, installed and signed in.
- Git, Python 3 and curl on your `PATH`. The plugin's commands run through them. On Windows, Git for
  Windows includes Git and curl, and Python comes from [python.org](https://www.python.org/downloads/).
- A CloudAEye account to sign in with. Every command needs one, the free ones included, because that
  is how CloudAEye knows who is asking. If you have none, sign up at
  [console.cloudaeye.com/auth/sign-up](https://console.cloudaeye.com/auth/sign-up): the Free plan needs
  no card.

You do not need a GitHub account to try the demos. You need one only to [fork](#make-your-own-changes).

To check, run these in a terminal. Each one should print a version:

```bash
git --version
python3 --version
curl --version
claude --version
```

On Windows, if `python3` prints nothing or opens the Microsoft Store, run `python --version`
instead. The plugin tries both.

### 1. Clone this repository

```bash
git clone https://github.com/CloudAEye/plugin-demos.git
```

```bash
cd plugin-demos
```

### 2. Install the plugin

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

### 3. Sign in

The plugin cannot reach CloudAEye until you sign in once. In Claude Code:

```text
/mcp
```

1. Pick the CloudAEye server, listed as `plugin:cloudaeye:cloudaeye`.
2. Choose **Authenticate**. A browser window opens.
3. Sign in with your CloudAEye account and allow access.
4. When the browser says you are connected, go back to Claude Code.

Claude Code keeps you signed in after this, and refreshes the sign-in on its own.

That is all the setup. The commands recognise this repository on their own, so you can go straight to
[Demo 1](#demo-1-full-stack-fastapi-template).

If you want the list in Claude Code, `/cloudaeye:init` prints the branch to check out for each demo and
the commands that have stored answers. Here it connects nothing: that is only for
[your own fork](#make-your-own-changes).

```text
/cloudaeye:init
```

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

Don't edit anything yet. The stored answers are for this exact change; once you edit a file, the
commands tell you how to continue on your own fork.

### What you can try here

This demo shows CloudAEye at the two points where a change gets reviewed:

- **Before the commit**, on the demo's change in your working tree: the seven commands in
  [Run the commands](#run-the-commands) below. Each one answers from what CloudAEye stored for this
  change, at no cost.
- **On a pull request**: `/cloudaeye:ultrareview #1` posted its report on
  [pull request #1](https://github.com/CloudAEye/plugin-demos/pull/1) and asked a reviewer to look.
  Open it to see what your team would see on GitHub.

Through the plugin, CloudAEye's commands work both ways: before a commit and on a pull request. See the
[command guide](https://docs.cloudaeye.com/user-guide/mcp/skills.html) for details, and
[fork this repository](#make-your-own-changes) to try them all on your own changes.

### Run the commands

Run these in Claude Code from the repository's root folder, in any order. Each one prints a short
card. The longer ones (`review-arch`, `control-flow`, `risk-score`, `slop-score`, `ultrareview`) also
save the whole report, diagrams included, to `.cloudaeye/reports/<command>.md`. `/cloudaeye:init`
prints the same list, so if the two ever differ, go by what init printed.

Find bugs and security issues, each with a number, file, line and severity:

```text
/cloudaeye:review
```

Check whether the change does what the ticket asked. You get DONE or NOT DONE, with what is missing.
Type the ticket exactly as written here, because the stored answer is for this text:

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

**The history behind the risk card is staged.** Its trend sections read a repository's past: the
author's earlier pull requests and their scores, reverts, the bugs reported in the changed files, and
how much new code was rewritten. A copy of a project carries its git history and none of that, and
GitHub cannot backdate it, so for this demo it is made up: the pull requests, the revert and the bug
reports the card names are not on GitHub. Everything else on the card is computed from the
repository: the change, who wrote the lines it touches, and its AI slop score.

Run every check at once, opening with a merge verdict and a table of results:

```text
/cloudaeye:ultrareview
```

Any other command, such as `/cloudaeye:implement` to plan a fix, runs only on your own fork. The
command says so and shows how.

## Make your own changes

To edit the code, plan fixes with `/cloudaeye:implement`, or open pull requests, fork this repository
and connect the fork to your own CloudAEye account. From then on every command runs live on your
fork, on your own account and credits.

The Free plan runs `/cloudaeye:review`, `/cloudaeye:inspect`, `/cloudaeye:describe` and
`/cloudaeye:ask`. The other commands need the Pro plan;
[Pricing](https://docs.cloudaeye.com/user-guide/code-review/billing/pricing.html) lists what each
one costs.

### 1. Fork and connect

With the [GitHub CLI](https://cli.github.com), from the repository's root folder:

```bash
gh repo fork CloudAEye/plugin-demos --remote
```

This creates your fork on GitHub with all its branches, and makes it the `origin` remote of your
clone. Then, in Claude Code:

```text
/cloudaeye:init
```

1. It asks which branch to compare changes against, showing `main`. Press Enter to keep `main`.
2. The first time, it opens CloudAEye's GitHub page in your browser. Install the CloudAEye GitHub App
   there, choose **Only select repositories**, and pick your `plugin-demos` fork.
3. Go back to Claude Code. `/cloudaeye:init` checks every ten seconds for up to five minutes, and
   reports the repository as initialized once the app can see it.

CloudAEye then indexes your fork. If a command reports that the repository has no code graph yet,
wait a few minutes and run the command again.

Without the GitHub CLI: fork on github.com, clear **"Copy the main branch only"**, then point your
clone at the fork, replacing `<your-username>`:

```bash
git remote set-url origin https://github.com/<your-username>/plugin-demos.git
```

### 2. Fix a finding

Run `/cloudaeye:review`, pick a finding's number, and ask for a fix plan. CloudAEye plans the fix and
Claude Code edits the files:

```text
/cloudaeye:implement [1]
```

Replace `1` with the number you picked, or list several, like `[1,3]`. Run `/cloudaeye:review` again
afterwards to see whether the fix landed. To put the demo back the way it was:

```bash
git restore .
```

### 3. Review it as a pull request

Open a pull request in **your fork**, from `demo-1/reset-rate-limit` into `main`. On github.com,
GitHub suggests `CloudAEye/plugin-demos` as the base repository; change it to your fork. With the
GitHub CLI, replacing `<your-username>`:

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

**What gets written to GitHub:** on a pull request in your fork, `ultrareview`, `risk-score`,
`review-arch` and `control-flow` post a comment. They post once per commit, and some edit their
comment when you run them again. `assign-reviewer` requests a review from the recommended reviewer
when that person can review in the repository. Add `just suggest` after the number to get the
suggestion without the request. Nothing is ever written to `CloudAEye/plugin-demos` itself.

## Troubleshooting

| What you see | What to do |
|---|---|
| `/plugin marketplace add` fails | That shorthand clones over SSH. Quit Claude Code, set `CLAUDE_CODE_PLUGIN_PREFER_HTTPS=1` in your shell, start `claude` again and retry. |
| `/plugin install` says the plugin is not found | Run `/plugin marketplace update cloudaeye`, then the install again. |
| The `/cloudaeye:*` commands exist but nothing runs | The server did not connect. Restart Claude Code. |
| `/mcp` shows CloudAEye as **Needs authentication** | Choose it in `/mcp`, then **Authenticate**. |
| `cloudaeye_error=python_not_found` | Install Python 3 and make sure it is on your `PATH`. |
| A command says your working tree differs from the demo's change | You edited a file, or you are not on the demo's branch. Run `git status`, then `git restore .` or check out the branch again. To keep your edits, [fork](#make-your-own-changes). |
| A command says it has no stored answer | Only the commands init lists have one. Run anything else on [your own fork](#make-your-own-changes). |
| `ambiguous_remote` from `/cloudaeye:init` | Your clone has several git remotes and none is named `origin`. `git remote -v` lists them. Name your fork's remote `origin`. |
| A command says it needs the Pro plan | On your fork, the Free plan runs `review`, `inspect`, `describe` and `ask`. See [Pricing](https://docs.cloudaeye.com/user-guide/code-review/billing/pricing.html). |
| A pull-request command says the pull request "comes from" another repository | The pull request was opened against `CloudAEye/plugin-demos`. Open it in your fork instead. |

The plugin's full guide is at [docs.cloudaeye.com](https://docs.cloudaeye.com/user-guide/mcp/setup-claude-code.html).
For anything else, write to [support@cloudaeye.com](mailto:support@cloudaeye.com).
