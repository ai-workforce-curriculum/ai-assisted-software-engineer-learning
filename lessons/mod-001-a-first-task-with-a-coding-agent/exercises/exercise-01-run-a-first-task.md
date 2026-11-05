# Exercise 01: Run a First Task

**Role:** AI-Assisted Software Engineer · **Time:** 45–75 minutes · **Output:** one brief, one diff, one short write-up

## Goal

Run one real task, end to end, with a coding agent of your choice. The point isn't the diff — it's the write-up. You're building the self-review habit that this role compounds on.

## Pick a task

Choose a **small, real** task in a repository you can actually work in. Good candidates:

- Add a missing test case to an existing test file.
- Rename a function and update its callers.
- Add a `--dry-run` flag to a small CLI.
- Fix a lint warning you've been ignoring.
- Add input validation to an endpoint (see the worked example in the module README).

Avoid, this time:

- Tasks where the right answer isn't clear to you yet. You'll be learning two things at once.
- Tasks spanning many files. Decomposition is a later module.
- Tasks in a repo the agent can't see. If you're using a tool that reads the local filesystem, work locally. If you're using a tool that reads a hosted repo, pick a repo it has access to.

## Steps

1. **Write the brief.** Use the four parts from the module: Goal, Context, Constraints, Done. Write it in a plain text file (not in the agent's chat window) so you have a clean copy to review later.
2. **Run it once.** Hand the brief to your agent. Watch what it does. Do not interrupt unless it's about to do something destructive or clearly off-track — let the first pass complete.
3. **Review the diff.** Every line. Run the tests yourself. If the agent claims it ran them, re-run them; the claim is not the result.
4. **Correct, at most twice.** If something's off, issue one follow-up brief naming the specific thing to change. If after two follow-ups it's still off, take the keyboard back and finish by hand — and note that in your write-up.
5. **Write it up.** One page (300–500 words) with four sections:
    - **The brief** — the final version of what you sent.
    - **What the agent got right** — specifics, not "it worked".
    - **What you had to fix** — specifics, and whether you fixed it in the brief or by hand.
    - **What you'd change in the brief next time** — the one or two sentences you'd add or remove.

## What good looks like

- Your brief is readable by someone who doesn't know the task — they could roughly reproduce your result from it alone.
- Your write-up names a concrete thing you'd change next time, not "I'd give it more context."
- The diff you kept is one you'd be willing to put your name on. If it isn't, don't commit it — this exercise is better served by an honest "I threw it away and did it by hand" than by a kept-but-shaky diff.

## Keep it

Save the brief and the write-up. You'll reread them in Exercise 02 and again at the end of the module ladder. The first brief you write is almost always the one you learn the most from reviewing later.
