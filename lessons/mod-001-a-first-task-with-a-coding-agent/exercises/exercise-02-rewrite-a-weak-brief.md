# Exercise 02: Rewrite a Weak Brief

**Role:** AI-Assisted Software Engineer · **Time:** 30–45 minutes · **Output:** two briefs and a short comparison

## Goal

Take a deliberately bad brief, diagnose what's missing, and rewrite it into one that would produce a reviewable diff in a single pass. Then run both and see the gap for yourself.

This is the cheapest practice available for the single highest-leverage skill in the role: writing briefs that are worth running.

## The weak brief

Use this one, exactly as written, as your starting point:

> Make the API return better errors.

That's the whole brief. It is representative of what a first-time user types into the chat window and hits enter on. It is also, measurably, a waste of the model's turn and your review time.

## Steps

1. **Diagnose.** Before rewriting anything, write down what's missing, using the four parts from the module (Goal, Context, Constraints, Done). Which of the four is weakest? Which is absent entirely? You should have four short bullet points.
2. **Rewrite.** Produce a workable version against **your own project** (or the same project you used in Exercise 01). The rewritten brief should be specific to a real API in a real repo. If you don't have one, use any public repo with an HTTP API you can run locally. Your rewrite should name specific files, specific error shapes, and specific tests.
3. **Run both, if you can.** Hand the weak brief to your agent in a fresh session. Then hand the rewritten brief to the same agent in a separate fresh session. Save both outputs.
4. **Compare.** Write a one-page comparison (300–500 words) with three sections:
    - **Weak-brief output** — what the agent produced, and what's wrong with it. Be specific; "it's vague" isn't an answer.
    - **Strong-brief output** — what the agent produced, and what you'd still change.
    - **Which part of the brief paid off most** — of the four parts you added (Goal, Context, Constraints, Done), which one made the biggest difference to the output? This is the part you'll under-invest in, so name it clearly.

## What good looks like

- Your diagnosis is written before the rewrite, not after. (It's easy to rationalize a rewrite backwards; hard to do the diagnosis honestly first.)
- The rewritten brief is specific enough that a colleague could run it on the same repo and get a comparable result.
- Your comparison names the one part that mattered most. If your answer is "all four equally", you probably wrote it too late — redo the comparison with one of the four parts stripped out of the strong brief.

## Keep it

Keep both briefs side by side with your Exercise 01 write-up. The pair is the artifact that shows whether this module stuck: if six months from now you write briefs that look closer to the weak one than the strong one, you've drifted. Reread these to recalibrate.
