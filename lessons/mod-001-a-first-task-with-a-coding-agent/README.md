# Module 001: A First Task with a Coding Agent

> **Role:** AI-Assisted Software Engineer · **Level:** L20 · **Time:** ~2 hours (reading + exercises)

Before you can delegate well, you need to delegate once. This module walks a single, well-scoped task from brief to merged change and names the moves that make the cycle work, so the next task goes smoother and the one after that is routine.

The orientation module ([Module 00](../00-your-relationship-to-the-model/README.md)) placed you alongside the model. This module is the first time you actually hand work to it.

## What you'll be able to do by the end

- Name what a coding agent is (and isn't) in enough detail that a non-engineer colleague gets it.
- Write a four-part brief — Goal, Context, Constraints, Done — for a small, real task.
- Run one task end to end with any mainstream coding agent and bring back a change you'd be willing to commit.
- Spot three common first-task failure modes in your own output.

## What a coding agent is

A **coding agent** is a tool that reads an instruction in natural language, executes a chain of actions in a code environment (reading files, running commands, editing files, running tests), and returns a change set — usually a diff, sometimes a draft commit or pull request. It is operated in turns: you brief, it acts, you review, you correct.

It is not the same as:

| Tool shape | What it does | Who drives |
|---|---|---|
| **Autocomplete** (inline suggestions) | Predicts the next few tokens as you type. | You, continuously. |
| **Chat completion** (answer in a sidebar) | Returns a text answer, snippets you paste in by hand. | You, with the model as reference. |
| **Coding agent** (this module) | Takes multi-step actions in your repo on its own. | You, in turns — the model drives between your turns. |
| **Autonomous engineer** (aspirational) | Takes a goal, works to completion, reports back. | The model, with you reviewing at the end. |

Mainstream products that implement the coding-agent shape today include Anthropic's **Claude Code**, GitHub's **Copilot coding agent**, **Cursor**'s agent mode, **Aider**, and OpenAI's **Codex** CLI, among others. The product names change; the loop does not.

<!-- needs-research: confirm current product-page framing for Copilot coding agent, Cursor agent, Codex CLI; cite the exact docs sections where each defines its agent/task loop -->

The practical consequence: skills in this module are tool-agnostic. The brief you write for one of these tools will work, with small adjustments, in any of the others. Switching tools is cheap; switching briefing habits is expensive. Invest in the habits.

## The task cycle

Every delegated task runs through the same four steps. When a task goes wrong, it almost always went wrong at one of them.

```
┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐
│  Brief   │ -> │   Run    │ -> │  Review  │ -> │ Correct  │
└──────────┘    └──────────┘    └──────────┘    └──────────┘
      ^                                                │
      └────────────────────────────────────────────────┘
```

1. **Brief.** You write what you want, with enough context that the agent can act. This is the work with the highest leverage; a strong brief is worth an hour of reviewing weak output.
2. **Run.** The agent reads, edits, runs commands, and returns a change. You are mostly watching here — not helping. If you find yourself "helping" every few seconds, your brief was wrong, not the agent.
3. **Review.** You read the diff (not just the summary), run the tests, and sanity-check the behavior. This is non-optional. The model doesn't know what it doesn't know.
4. **Correct.** Either (a) issue a follow-up brief that names the specific thing to change, (b) take the keyboard back and finish by hand, or (c) revert and re-brief from scratch. There is no "ask it to try again and hope".

The first task in a new repo often goes around this loop two or three times. By the tenth task in the same repo, one pass is normal.

## Which tasks belong in the loop

Not every task should be delegated, and the quickest way to sour on coding agents is to send them work they can't do well. A good first-pass filter:

**Favor tasks where:**

- **The right answer is clear to you.** You're using the agent for speed, not for ideas. If you don't know what "done" looks like, the agent will pick a direction, and you'll spend the review re-deciding whether it was the right one.
- **The scope is contained.** One file, one function, one endpoint. Multi-file refactors and cross-cutting concerns are later-module territory; starting there teaches you the wrong lessons about the tool.
- **There's prior art in the repo.** If a similar change exists, the agent can usually mirror it. "Do what `billing/emails.py` does, but for signup" is a workable context line. Greenfield features without anchors are harder.
- **Verification is cheap.** Tests exist, the behavior is observable, or the diff is small enough to read line by line. If you can't check the output, the agent isn't helping — it's just moving the uncertainty into code.

**Hold back, for now, when:**

- The task requires taste calls you haven't made yet (naming, API shape, architectural direction). Decide first, delegate second.
- The task depends on non-obvious domain facts the agent has no way to know. You can brief one or two; a whole document's worth means you're writing the code either way.
- The failure mode is silent or expensive (migrations, destructive data operations, anything that writes to a shared system). First tasks belong in reversible corners.

As your judgment of the tool sharpens, these edges move. Day-one candidates are small and legible; by month six, "small and legible" will mean something larger than it does today.

## How the agent reads the repo

A quick mental model, because it changes what you put in Context.

A coding agent usually has a short list of tools it can invoke each turn: read a file, list a directory, search by pattern (grep), run a shell command, and edit a file. Different products wrap these differently, but the shape is consistent — the agent proposes a tool call, the harness runs it, the result comes back, and the agent decides what to do next. It doesn't see your whole repo at once; it pulls context in piece by piece.

Two consequences for how you brief:

- **It already knows what it can grep.** You don't need to paste the contents of a file it can trivially read. Point at the file, not the lines; the agent will fetch what it needs.
- **It doesn't know what isn't in the code.** Decisions that live in Slack threads, PR comments, or someone's head are invisible to it. If the reason "we always do it this way" isn't written down anywhere, write it in the Context section — or don't be surprised when the agent chooses otherwise.

The practical rule: brief for what the repo *can't tell the agent on its own*. Everything else is noise that pushes the useful parts of the brief further from the model's attention.

## What makes a brief work: Goal, Context, Constraints, Done

A workable brief has four parts. The order matters less than the presence of all four.

### Goal

One sentence. What should be different about the code after the task is finished? Written as an outcome, not an instruction.

- Weak: "Edit `auth.py` and add a function."
- Workable: "Add password reset so a user can request a reset link by email and use it to set a new password."

Why outcome, not instruction: an outcome gives the agent room to pick the right implementation. An instruction locks it into yours, which is only better if you were already right.

### Context

The things the agent can't see on its own that it needs to see:

- Which files are in scope (and, just as usefully, which are out of scope).
- Conventions that aren't obvious from the code: naming rules, lint quirks, the test runner, the preferred libraries.
- Prior art: "we solved a similar thing in `billing/emails.py`, mirror that pattern".
- What the system actually does, if a reader couldn't infer it from the directory names.

A coding agent working in a repo for the first time knows what it can grep for. It doesn't know your team's taste. Context is where you write your taste down.

### Constraints

The rules the output must obey, written as hard limits rather than hopes:

- "Don't add a new dependency."
- "Keep this under 100 lines in the touched file."
- "The public API of `UserService` must not change."
- "All new code must have a test; no test-free edits."

Constraints are what you'd notice were wrong in review if they were violated. Writing them up front turns a two-pass review into a one-pass review.

### Done

How the agent (and you) will know the task is finished:

- Which tests should pass that don't pass today.
- The exact command the agent should run to verify (and show you the output of).
- What the diff should and shouldn't touch (e.g., "no changes under `migrations/`").

The best Done section includes a test command the agent runs itself. The second-best describes the behavior in enough detail that you could write one.

## A worked example

Here is a task someone in this role could reasonably hand off on day one. The task: add input validation to a small Flask endpoint that currently crashes on malformed JSON.

**Weak brief (what first-timers usually write):**

> Fix the signup endpoint to handle bad input better.

This loses time in every direction. What counts as bad input? What does "better" mean? Which file? Should it log, 400, raise? The agent will guess, and it will guess something — but you'll spend the review figuring out whether its guess matches yours.

**Workable brief (the four parts):**

> **Goal:** The `POST /signup` endpoint should return a 400 with a structured error when the request body is missing a required field or has the wrong type, instead of crashing with a 500.
>
> **Context:** The endpoint is in `app/routes/auth.py`. Required fields are `email` (string), `password` (string, min length 8), and `display_name` (string). Validation elsewhere in the repo uses the `pydantic` models in `app/schemas/`; follow that pattern — don't hand-roll a validator.
>
> **Constraints:**
> - No new dependencies.
> - Don't change the response shape for successful requests.
> - Error responses should be `{"error": "<human message>", "field": "<field_name>"}`, matching `app/routes/login.py`.
>
> **Done:**
> - The existing test file `tests/test_signup.py` still passes.
> - Add at least three new test cases there: missing field, wrong type, short password.
> - Run `pytest tests/test_signup.py -v` and include the output in your summary.

Read both briefs side by side. The weak one is shorter. The workable one gets you a reviewable diff in one pass. The time you spent writing the extra lines is time you didn't spend arguing with the output.

## Common first-task failure modes

Three mistakes explain most first-task disappointments. Learn to spot them in your own work.

### 1. Under-briefing

Symptoms: the agent asks clarifying questions you could have answered in the brief; or (worse) it doesn't ask, and its guesses don't match your taste. You end up reviewing a diff that looks reasonable in isolation but isn't what you'd have written.

Fix: before each run, read your own brief and ask, "If a new hire read only this, would they produce roughly what I want?" If no, add the missing part — usually Context, sometimes Done.

### 2. Over-briefing (instruction-mode)

Symptoms: you've written pseudocode in the Goal section. The agent dutifully transcribes it. Any value you were hoping to get from a second mind is gone.

Fix: strip the Goal down to an outcome. If you had to tell the agent exactly how to do the thing, you already did the task; the agent is just typing it.

### 3. Not reading the diff

Symptoms: you glance at the summary, see "added validation and three tests", approve, move on. Two weeks later a bug report shows the validation accepts `password=""` because the agent wrote `if password is None` where you'd have written `if not password`.

Fix: no exceptions on the first ten tasks in a new repo — read every line of every diff. Run the tests yourself, not just read the model's claim that it ran them. The reviewing muscle is the main thing this role compounds; don't skip the reps.

## Taking the keyboard back

The loop above has a hidden fourth option in **Correct**: finish by hand. Knowing when to take the keyboard back is a skill, not a failure. Signs it's time:

- You've issued three corrections on the same point and it's still wrong. The brief is probably not fixable by one more sentence; the task may need to be decomposed, or it may be a task you should write yourself.
- The remaining work is 20 lines and you already know exactly what they are. Briefing takes longer than typing.
- The agent is confidently wrong about a domain fact (e.g., the semantics of your billing system), and correcting the fact in the brief would take a whole document. Write the code; leave the agent out of this one.

Taking the keyboard back is not giving up. It's choosing the cheaper path. The AI-assisted software engineer is defined by making that call well, not by delegating everything regardless.

## Before you move on

- Read your team's (or your tool's) docs on how to invoke its coding agent. For Claude Code, see the [Claude Code overview](https://docs.claude.com/en/docs/claude-code/overview) and its [best-practices guidance](https://www.anthropic.com/engineering/claude-code-best-practices). For GitHub Copilot's agent mode, see the [Copilot coding agent docs](https://docs.github.com/en/copilot/concepts/agents/about-coding-agent). <!-- needs-research: verify Cursor agent and Aider doc URLs before citing them here; both exist but their canonical doc paths change often -->
- Do [Exercise 01: Run a first task](exercises/exercise-01-run-a-first-task.md). You will not get full value from Exercise 02 without having done this one first.
- Do [Exercise 02: Rewrite a weak brief](exercises/exercise-02-rewrite-a-weak-brief.md).
- Once both are done, return to [CURRICULUM.md](../../CURRICULUM.md) for the next module.
