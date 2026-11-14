# CloudAEye plugin demos

Try the [CloudAEye plugin for Claude Code](https://github.com/CloudAEye/cloudaeye-plugin) on real
open-source projects, on your own machine.

Each demo is one project plus one change a coding agent made from a vague ticket: the kind of change
that reaches code review every day. The change sits on a branch of its own, and CloudAEye has already
reviewed it, so every command answers in seconds.

> [!NOTE]
> **Nothing to set up on GitHub.** This repository is already connected to CloudAEye for everyone:
> no fork, no GitHub App, nothing to connect. All you need is the plugin and a free CloudAEye account,
> and the demo commands cost nothing on any plan.

| Demo | Project | Stack | The change | Branch |
|---|---|---|---|---|
| 1 | [full-stack-fastapi-template](full-stack-fastapi-template/) | FastAPI, SQLModel, PostgreSQL, React | Rate-limit password reset | `demo-1/reset-rate-limit` |

**Contents**

- [Quick start](#quick-start)
- [Demo 1: full-stack-fastapi-template](#demo-1-full-stack-fastapi-template)
- [Make your own changes](#make-your-own-changes)
- [Troubleshooting](#troubleshooting)

## Quick start

Three steps, done once.

### Requirements

- **Claude Code**, installed and signed in.
- **Git** and **Python 3** on your `PATH`: the plugin's commands run through them.
  - On Windows, [Git for Windows](https://git-scm.com/download/win) provides Git, and Python comes
    from [python.org](https://www.python.org/downloads/).
- **A CloudAEye account** to sign in with. Every command needs one, the free ones included.
  - No account yet? [Sign up](https://console.cloudaeye.com/auth/sign-up): the Free plan needs no card.

You do **not** need a GitHub account to try the demos, only to [fork](#make-your-own-changes).

To check, run these in a terminal. Each one should print a version:

```bash
git --version
python3 --version
claude --version
```

On Windows, if `python3` prints nothing or opens the Microsoft Store, run `python --version` instead.
The plugin tries both.

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

- The install may offer some settings. They are all optional: skip them.
- **Restart Claude Code now:** type `/exit`, then run `claude` again. The commands appear straight
  away, but the connection to CloudAEye only starts when Claude Code starts.

<details>
<summary>Prefer a terminal? The same install in two commands</summary>

```bash
claude plugin marketplace add CloudAEye/cloudaeye-plugin
```

```bash
claude plugin install cloudaeye@cloudaeye
```

</details>

### 3. Sign in

The plugin cannot reach CloudAEye until you sign in once. In Claude Code:

```text
/mcp
```

1. Pick the CloudAEye server, listed as `plugin:cloudaeye:cloudaeye`.
2. Choose **Authenticate**. A browser window opens.
3. Sign in with your CloudAEye account and allow access.
4. When the browser says you are connected, go back to Claude Code.

Claude Code keeps you signed in from then on.

**That is all the setup.** The commands recognise this repository on their own, so go straight to
[Demo 1](#demo-1-full-stack-fastapi-template).

> [!TIP]
> - `/cloudaeye:init` prints the branch to check out for each demo and the commands you can try. Here
>   it connects nothing: connecting is only for [your own fork](#make-your-own-changes).
> - Run a shell command without leaving Claude Code by starting the line with `!`, for example
>   `!git status`.

## Demo 1: full-stack-fastapi-template

Copied from [fastapi/full-stack-fastapi-template](https://github.com/fastapi/full-stack-fastapi-template)
(MIT License, © 2019 Sebastián Ramírez), with its git history. Its own README and LICENSE are in the
folder.

### The scenario

Two complaints reached support in the same week:

- One customer got dozens of "reset your password" emails they never asked for: someone was calling
  the password-recovery endpoint in a loop with a list of addresses.
- A few other users said the link in the reset email came back "Invalid token" when they clicked it.

Product filed one ticket for both:

> Password reset is getting abused and sometimes the link doesn't work. Make reset more robust and
> rate limit it.

The developer who picked it up gave the ticket to their coding agent as written. The agent:

- added a limit of five attempts per email address to `POST /password-recovery/{email}`;
- moved the reset logic out of `POST /reset-password/` into a new `crud.reset_password_with_token`.

The change touches two files, `backend/app/api/routes/login.py` and `backend/app/crud.py`, and nobody
has reviewed it yet.

### Get the change

```bash
git checkout demo-1/reset-rate-limit
```

> [!IMPORTANT]
> Don't edit anything yet. The stored answers are for this exact change; once you edit a file, the
> commands tell you how to continue on your own fork.

### What you can try

The demo shows CloudAEye at the two points where a change gets reviewed: **before the commit**, on the
change in your working tree, and **on a pull request**, where the report is posted for the team.

| Command | What it shows | Runs on |
|---|---|---|
| `/cloudaeye:review` | Bugs and security issues, each with a number, file, line and severity | Before the commit |
| `/cloudaeye:check-task` | Whether the change does what the ticket asked: DONE or NOT DONE, with what is missing | Before the commit |
| `/cloudaeye:slop-score` | How much careful reading the change needs, as a score out of 100 with the findings behind it | Before the commit |
| `/cloudaeye:review-arch` | What the change did to the architecture: the system, the layers and the change itself | Before the commit |
| `/cloudaeye:control-flow` | The order of calls before and after the change, as sequence diagrams | Before the commit |
| `/cloudaeye:risk-score` | Whether the change needs a human reviewer, and why | Before the commit |
| `/cloudaeye:ultrareview #1` | Every check at once, with a merge verdict, posted on the pull request with who should review | On [pull request #1](https://github.com/CloudAEye/plugin-demos/pull/1) |

Through the plugin, CloudAEye's commands work both ways: before a commit and on a pull request. See the
[command guide](https://docs.cloudaeye.com/user-guide/mcp/skills.html) for details, and
[fork this repository](#make-your-own-changes) to try them all on your own changes.

### Run the commands

Run these in Claude Code from the repository's root folder, in any order.

- Each one prints a short card.
- The longer ones also save the whole report, diagrams included, to `.cloudaeye/reports/<command>.md`.

**Find bugs and security issues**

```text
/cloudaeye:review
```

**Check the change against the ticket.** Type the ticket exactly as written here, because the stored
answer is for this text:

```text
/cloudaeye:check-task Password reset is getting abused and sometimes the link doesn't work. Make reset more robust and rate limit it.
```

**Score the AI slop**

```text
/cloudaeye:slop-score
```

**Review the architecture**

```text
/cloudaeye:review-arch
```

**Draw the control flow**

```text
/cloudaeye:control-flow
```

**Score the change's risk**

```text
/cloudaeye:risk-score
```

> [!NOTE]
> **The history behind the risk card is staged.** Its trend sections read a repository's past: the
> author's earlier pull requests and their scores, reverts, the bugs reported in the changed files, and
> how much new code was rewritten. A copy of a project carries its git history and none of that, and
> GitHub cannot backdate it, so for this demo it is made up: the pull requests, the revert and the bug
> reports the card names are not on GitHub. Everything else on the card is computed from the
> repository: the change, who wrote the lines it touches, and its AI slop score.

**Run every check on the pull request.** The report opens with a merge verdict and a table of
results, and on a pull request it is posted as one comment, with who should review. See it on
[pull request #1](https://github.com/CloudAEye/plugin-demos/pull/1):

```text
/cloudaeye:ultrareview #1
```
See the `ultrareview` output  [here](https://github.com/CloudAEye/plugin-demos/pull/1#issuecomment-6843378130)

Any other command, such as `/cloudaeye:implement` to plan a fix, runs on your own fork only. The
command says so and shows how.

## Make your own changes

Fork this repository and connect the fork to your own CloudAEye account to:

- edit the code and review your own changes;
- plan fixes with `/cloudaeye:implement`;
- open pull requests and run the pull-request commands on them.

From then on every command runs live on your fork, on your own account and credits:

- **Free plan:** `/cloudaeye:review`, `/cloudaeye:inspect`, `/cloudaeye:describe` and `/cloudaeye:ask`.
- **Pro plan:** every other command. [Pricing](https://docs.cloudaeye.com/user-guide/code-review/billing/pricing.html)
  lists what each one costs.

### 1. Fork and connect

With the [GitHub CLI](https://cli.github.com), from the repository's root folder:

```bash
gh repo fork CloudAEye/plugin-demos --remote
```

This creates your fork on GitHub with all its branches, and makes it the `origin` remote of your clone.
Then, in Claude Code:

```text
/cloudaeye:init
```

1. It asks which branch to compare changes against, showing `main`. Press Enter to keep `main`.
2. The first time, it opens CloudAEye's GitHub page in your browser. Install the CloudAEye GitHub App
   there, choose **Only select repositories**, and pick your `plugin-demos` fork.
3. Go back to Claude Code. `/cloudaeye:init` checks every ten seconds for up to five minutes, and
   reports the repository as initialized once the app can see it.

CloudAEye then indexes your fork. If a command reports that the repository has no code graph yet, wait
a few minutes and run it again.

<details>
<summary>Without the GitHub CLI</summary>

Fork on github.com and clear **"Copy the main branch only"**, then point your clone at the fork,
replacing `<your-username>`:

```bash
git remote set-url origin https://github.com/<your-username>/plugin-demos.git
```

</details>

### 2. Fix a finding

Run `/cloudaeye:review`, pick a finding's number, and ask for a fix plan. CloudAEye plans the fix and
Claude Code edits the files:

```text
/cloudaeye:implement [1]
```

- Replace `1` with the number you picked, or list several, like `[1,3]`.
- Run `/cloudaeye:review` again afterwards to see whether the fix landed.
- To put the demo back the way it was:

```bash
git restore .
```

### 3. Review it as a pull request

Open a pull request in **your fork**, from `demo-1/reset-rate-limit` into `main`. On github.com, GitHub
suggests `CloudAEye/plugin-demos` as the base repository: change it to your fork. With the GitHub CLI,
replacing `<your-username>`:

```bash
gh pr create --repo <your-username>/plugin-demos --base main --head demo-1/reset-rate-limit --title "Rate-limit password reset" --body "Password reset is getting abused and sometimes the link doesn't work. Make reset more robust and rate limit it."
```

The commands below use `#1`: put your own pull request's number in its place.

**Run every check and post the report as one comment**

```text
/cloudaeye:ultrareview #1
```

**Score the risk and post the risk card**

```text
/cloudaeye:risk-score #1
```

**Find who should review it, and what they should check**

```text
/cloudaeye:assign-reviewer #1
```

Nobody reviews their own pull request, and in a personal fork you are often the only person with
access, so this command may find nobody to suggest. Add collaborators to your fork to see it choose
between people.

**Pick the next pull request to review, across every repository you connected**

```text
/cloudaeye:triage-prs
```

> [!NOTE]
> **What gets written to GitHub**
> - On a pull request in your fork, `ultrareview`, `risk-score`, `review-arch` and `control-flow` post
>   a comment: once per commit, and some edit their comment when you run them again.
> - `assign-reviewer` requests a review from the recommended reviewer when that person can review in
>   the repository. Add `just suggest` after the number to get the suggestion without the request.
> - Nothing is ever written to `CloudAEye/plugin-demos` itself.

## Troubleshooting

| What you see | What to do |
|---|---|
| `/plugin marketplace add` fails | That shorthand clones over SSH. Quit Claude Code, set `CLAUDE_CODE_PLUGIN_PREFER_HTTPS=1` in your shell, start `claude` again and retry. |
| `/plugin install` says the plugin is not found | Run `/plugin marketplace update cloudaeye`, then the install again. |
| The `/cloudaeye:*` commands exist but nothing runs | The server did not connect. Restart Claude Code. |
| `/mcp` shows CloudAEye as **Needs authentication** | Choose it in `/mcp`, then **Authenticate**. |
| `cloudaeye_error=python_not_found` | Install Python 3 and make sure it is on your `PATH`. |
| A command says your working tree differs from the demo's change | You edited a file, or you are not on the demo's branch. Run `git status`, then `git restore .` or check out the branch again. To keep your edits, [fork](#make-your-own-changes). |
| A command says it has no stored answer | Only the commands in [What you can try](#what-you-can-try) have one. Run anything else on [your own fork](#make-your-own-changes). |
| `ambiguous_remote` from `/cloudaeye:init` | Your clone has several git remotes and none is named `origin`. `git remote -v` lists them. Name your fork's remote `origin`. |
| A command says it needs the Pro plan | On your fork, the Free plan runs `review`, `inspect`, `describe` and `ask`. See [Pricing](https://docs.cloudaeye.com/user-guide/code-review/billing/pricing.html). |
| A pull-request command says the pull request "comes from" another repository | The pull request was opened against `CloudAEye/plugin-demos`. Open it in your fork instead. |

## Help

- The plugin's full guide: [docs.cloudaeye.com](https://docs.cloudaeye.com/user-guide/mcp/setup-claude-code.html)
- Every command and its options: [command guide](https://docs.cloudaeye.com/user-guide/mcp/skills.html)
- Anything else: [support@cloudaeye.com](mailto:support@cloudaeye.com)
