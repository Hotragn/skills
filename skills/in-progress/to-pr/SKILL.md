---
name: to-pr
description: "Turn the work on this branch into a pull request, written from the spec rather than from the diff."
disable-model-invocation: true
---

Open a pull request for the finished work on the current branch.

The body is written from the **primary sources**: the ticket, the spec, the ADRs, your own commit messages. Not from reading the diff back. A body derived from the diff tells the reviewer what changed, which the diff already told them. The question they open a PR with is *why*, and that answer only exists upstream.

The issue tracker should have been provided to you. If `docs/agents/issue-tracker.md` is missing, tell the user to run `/setup-matt-pocock-skills`. The tracker holds the ticket; the **code host** holds the PR (`gh pr` on GitHub, `glab mr` on GitLab). Often the same tool, sometimes not: a repo tracking work in Jira or in local markdown still opens its PR on the code host, so take the ticket from the tracker config and the PR commands from the git remote.

## Process

### 1. Pin the branch

The **fixed point** is the merge-base with the branch this will merge into, defaulting to the repo's default branch. Capture the diff once (`git diff <fixed-point>...HEAD`, three-dot) and the commit list (`git log <fixed-point>..HEAD --oneline`).

Confirm three things before going further: you are not on the default branch, the tree is clean, and the diff is non-empty. An uncommitted file or an empty diff should stop you here, not after a PR exists with your name on it.

### 2. Find the primary sources

- **The ticket.** Issue references in the commit messages (`#123`, `Closes #45`), a number or path the user passed, or the ticket matching the branch name. Fetch it through the workflow in `docs/agents/issue-tracker.md`. Where there is genuinely no ticket, say so in the body rather than inventing a rationale for the change.
- **The spec**, where this ticket is one of several under it.
- **The ADRs** covering the area you touched. A hard-to-reverse decision this branch took that no ADR records belongs in an ADR, not in a PR paragraph that dies with the merge: call the Skill tool with "domain-modeling" and record it before opening.
- **Your own commit messages.** Where they already explain the change, link them. Where they don't, that gap is what the body is for.

### 3. One PR, one intent

State the intent in a sentence, then check every commit on the branch against it. Where the branch carries a second one, typically a refactor you did on the way, that is a second PR: offer to split, smallest first. Two intents in one PR make the reviewer hold both at once, and their attention is the resource this whole skill spends.

### 4. Earn the green

Run the project's own checks: typecheck, then tests, then format. Fix what is broken before opening, since a red PR spends a reviewer on something CI already told them.

Then name in the body only the checks you actually ran, with the command. "All green" is a claim the reviewer will trust and never re-run, so it has to be one you earned.

### 5. Review it before they do

Where the branch has not been through a review yet, call the Skill tool with "code-review" against the fixed point from step 1, and answer its findings first. The cheapest finding is the one a human never had to write.

### 6. Write the body

<pr-body-template>

## Why

The problem this solves, in the reviewer's terms, with a link to the ticket. Two or three sentences. Where the ticket already says it well, link it and stop.

## What changed

The shape of the change, by module and seam, in a handful of lines. Not a file list: the diff is a better file list than you can write.

## Decisions

Anything chosen consciously that could defensibly have gone the other way, with the reason. Where an ADR covers it, link the ADR and add nothing.

## How to verify

The commands you ran, verbatim, and what a reviewer runs to see the behaviour for themselves.

## Where to look first

The one or two hunks carrying the risk. This is the most valuable line in the body: it spends what you know about the change to aim the reviewer's attention.

## Out of scope

What you deliberately left, with tracker links where a follow-up ticket exists, so the reviewer does not spend a comment filing it.

</pr-body-template>

Three rules bind the body:

- **Reference the ticket the way the code host expects**, so merging closes it (`Closes #45`). Where the tracker is not the code host, link the ticket by URL or path and close it by hand on merge.
- **Every sentence is backed by the diff.** A reviewer reads the body as a contract and reviews the code against it, so a claim the diff does not support becomes a finding, or worse, an approval.
- **Redact.** On most repos this is public: no keys, tokens, internal URLs, or customer data, including inside pasted output.

### 7. Open it, then close the loop

Push the branch, then open the PR against the target from step 1. Open it as a **draft** while anything is outstanding: checks red, review findings unanswered, a question waiting on the reviewer. Mark it ready when nothing is.

Then link the PR from the ticket, so someone reading the ticket finds the code. Leave the triage roles alone: those describe how ready an issue is to be worked, and this work is done. Report the PR URL to the user.

## Why the body comes from the sources

Reading the diff back yields *what* changed, and the reviewer already has the diff. What they cannot reconstruct from it is intent: the problem behind the change, the option you rejected, the constraint that made an ugly line the right one. That lives in the ticket, the spec, and your head, and the PR is the last moment it is still cheap to write down.

Write it and the review argues about the decision. Leave it out and the review argues about the code, one line at a time.
