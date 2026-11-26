---
name: sourstack
description: >-
  Orchestrator mode for a ticket. The parent does not implement. It delegates
  design, code, proof, a cross-provider review, and a naive UI pass. Use only
  when the user says sourstack, /sourstack, or attaches this skill.
disable-model-invocation: true
---

# Sourstack

## Trigger

This skill is off until the user says `sourstack` or attaches it. There is no second skill for this.

That word is orchestrator mode. You coordinate. You do not implement. You do not edit product code, commit, or push. Design, implementation, proof, review, and the UI pass are delegates. You write the briefs, read their reports, route findings, ask on the stuck list, and open the pull request from the branch the coder pushed.

A message that does not say `sourstack` is not this mode. Do the work in that chat yourself.

Branch from the base the user names. With no base, the coder branches from the latest default branch.

## Match the work

Pick one and say which.

- A read-only question. Hand it to a research delegate. No edits.
- A defect. Reproduce it, name the root cause, then fix that cause.
- New or changed behavior. Name the data shape before anyone writes logic.
- Structure only. Preserve behavior. Say the check that proves nothing user-visible changed.
- A past decision caused the ticket. Run the design pass even when the diff looks small.

Skip the design pass only for a local fix whose shape is already right. Say why in one line.

## Design pass

Delegate this to a high-reasoning model. It traces the subsystem the ticket touches.

If an older decision causes the ticket, deleting that code is a candidate. Design the smaller system that remains, then build that.

Name the data shape, who owns each piece of state, and the boundary callers use. When two shapes are plausible, settle it by running something: a short prototype, or two design attempts. Pick from what you observed.

A one-way door is a step that is expensive to undo: dropped data, a public API removal, an auth or permission model, a production change. When that door has an obvious answer, take it and log why. When it does not, stop and ask. Put the options in the question, your recommendation first, and what each option costs to undo.

A reversible choice is yours. Log it and continue.

## Implement

Delegate the code to a high-reasoning coder. The brief carries the goal, the named data shape, the files in scope, the success check, and what is out of scope. A fix round is a fresh delegate with the original brief, the ledger, and the current diff.

## Prove it before review

The coder delegate brings the proof back: the old failure then the fix, the repo's own checks, a focused test that failed before the fix when the repo has a test setup. For a screen, that delegate drives the running app. A green unit test does not prove the screen.

You read that evidence. You do not redo the work to produce it. When the proof is missing, send the coder back. When there is no way to prove the change, ask the user and skip the review loop.

## Review, in parallel

Start these together after proof exists. Leave the checkout alone until they finish.

**Code review.** One delegate on a different provider than the coder, high reasoning, read-only. It does not edit, commit, push, or start its own agents. It reads the diff and the surrounding code.

Brief:

```
Review only. Intent: <one or two sentences>. Out of scope: <scope>. Checks already run: <results>.
Base: <sha>. Diff: git diff <base>.
blocker = the intended case is wrong. should = a real bug callers will hit, inside the intent. nit = everything else.
Reply VERDICT: APPROVE or CHANGES, then lines of:
S1 [blocker|should|nit] <file>:<line> <claim>. Fix: <suggestion>
```

**Naive UI pass.** Two delegates on a cheaper model. Each brief contains the ticket and the pull request description, or the goal when the PR does not exist yet. It does not contain the diff. Tell them not to open the changed files. They use the product as a person who was handed the description and does not know the code. Each finding says what they did, what they expected, what happened, and a screenshot when they can take one, labeled blocker, should, or nit.

## Route what comes back

Check every finding yourself.

- A blocker or should that is a local bug goes to the coder. The fix is the smallest change that answers that finding. Prefer deleting or narrowing over a new helper, flag, or layer.
- A blocker or should that means the shape is still wrong goes back through the design pass.
- A nit is declined in one line, or listed as a follow-up. It does not grow the diff.
- A one-way door with no obvious answer waits on the user.

Disagree in one or two lines with a file, a test, or output a delegate already ran. Agreement is not the way to end the loop.

## Budget

Three rounds. Round 1 is the full diff. Later rounds look at changed code and open findings.

Extend to five only while a blocker or should is still open and the change touches security, permissions, auth, data loss, or concurrency. Stop early when a round returns only nits.

Each round is a new delegate. Send the ledger: fixed, declined, and still open, with evidence. Budget end is not approval. When a finding stays disputed after two replies, show both sides and ask the user.

## Ship

The coder rebases onto the latest default branch when the repo expects that, then pushes. You open the pull request. The description leads with the problem, then how the change was proved, then the ledger of declined findings.

Leave the pull request open. Merging, auto-merge, and a merge queue are outside this skill.

## Ask only when you are stuck

Ask with the harness question tool, with the options and your recommendation in the question.

- You cannot prove the change.
- The goal is ambiguous in a way that changes the result.
- The next step deletes data, touches production, leaves the repo, or force-pushes a shared branch.
- A one-way door has no obvious answer.
- The budget ended with a blocker still disputed.

Decide the rest. List those decisions in the report.

## Models

In T3, call `orchestrator_capabilities` once before the first delegate. Design and code use a high-reasoning model. Review uses a different provider, read-only. The UI pass uses a cheaper model, two delegates, in parallel with review.

Models named in the user message, `AGENTS.md`, or `CLAUDE.md` win. When a provider is unavailable, say so. A same-provider reviewer does not count as the cross-review.

## Report

Pull request link, head SHA, what you proved and how, the decisions you made, the review ledger, and what you did with each UI finding.
