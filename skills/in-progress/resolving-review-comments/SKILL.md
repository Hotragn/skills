---
name: resolving-review-comments
description: "Work the review comments on an open PR of your own: verify each claim, then fix, reject, defer or ask. Use when a reviewer or a review bot (CodeRabbit, Copilot, Qodo) has left comments and the user wants them addressed."
---

Work the comments on an open PR of your own, one at a time.

A review comment is a **claim**, not an instruction. Every comment earns a **verdict**, and the verdict is settled against a source: the code, the spec, or the repo's documented standards. Working the list instead is how a review of a correct diff produces a diff that is wrong and twice the size.

This is the author's side of a review. An external PR arriving at your repo is a request surface, and belongs to the `triage` skill instead.

## Process

### 1. Gather the round

**Fetch both kinds of comment.** Inline comments anchored to a hunk live in a different place from comments on the PR itself, and the obvious command only returns one of them. On GitHub: `gh pr view <n> --comments` for the conversation, plus `gh api repos/{owner}/{repo}/pulls/<n>/comments` for the inline threads. On GitLab: `glab mr notes <n>`.

**Read each thread to its end.** A later reply often settles the comment already, and answering a settled thread again spends the reviewer twice.

**Collapse duplicates.** A bot that flags one pattern across twelve hunks has made one claim and gets one verdict. The same point raised by two reviewers is one claim too.

**Check what the review was written against.** A comment anchored to a hunk you have since rewritten is stale; read the current code before acting on it.

### 2. Give every comment a verdict

Four verdicts. Nothing is skipped, and nothing carries two:

| Verdict | When | What you do |
| --- | --- | --- |
| **Fix** | the claim holds and it is in this PR's scope | change the code |
| **Reject** | the claim does not hold | reply with the source that says otherwise |
| **Defer** | the claim holds but belongs to other work | file it on the tracker, link the ticket in the reply |
| **Ask** | it cannot be settled without the reviewer | reply with the specific question |

Settle each claim against a source, never against how careful it sounds:

- **The code.** Run it, read the path, write the failing case. "This can be null" is answered by the type and the call sites.
- **The spec or ticket.** It owns scope. Behaviour nobody asked for is scope creep whether an agent added it or a reviewer suggested it.
- **The repo's documented standards**: `CODING_STANDARDS.md`, `CONTRIBUTING.md`, the lint and formatter config. A documented standard overrides a reviewer's preference, and it overrides a bot's smell claim outright.

**Verifying a claim is your job; overruling a reviewer is the user's.** Present the verdicts as one table before touching any code, and wait on everything that is not a plain fix. A rejection is a sentence sent to a colleague under the user's name.

Where nobody is attached to the session, work the fixes and leave the table for the user. A reject, a defer and an ask each put words in the user's mouth to a colleague, so none of the three is yours to send.

Bots review by pattern with no access to the spec, so they arrive in volume and they are confidently wrong some of the time. They also find real bugs. Grade them against the same three sources: nothing about a comment's author changes what settles it.

### 3. Work the round

1. **Fix in themes, not in comments.** Group accepted claims by what they are actually about, one commit per theme, named for the concern rather than the comment number.
2. **A comment that found a real bug earns a failing test first.** Call the Skill tool with "tdd" and go red on it, so the reviewer's finding is locked down and cannot return.
3. **Add commits on top.** A rebase or force-push during an open review detaches every comment from the hunk it was written against, and the reviewer loses their place. Tidy the history afterwards, where the repo wants it tidy.
4. **Push once, at the end of the round.** A push per comment re-triggers CI and fills the reviewer's inbox with a change they cannot yet read as a whole.
5. **Keep the PR body true.** Where a fix changes what the PR does or how risky it is to merge, update the body before re-requesting review: call the Skill tool with "pr" for its format.

### 4. Reply to every comment

Including the ones you fixed. A silent fix makes the reviewer re-read the whole diff to find out what became of their comment.

- **Fixed**: name what changed, and the commit that changed it.
- **Rejected**: give the source in a sentence or two, and stop there. The reviewer needs the reason, not agreement.
- **Deferred**: link the ticket.
- **Asked**: one specific question.

Resolve the threads you fixed. Leave rejected and asked threads open, since closing a disagreement on the reviewer's behalf ends a conversation they are still in. Then re-request review once, for the round as a whole.

The round is done when every comment carries a verdict and a reply, and the only threads still open are the ones the reviewer has to close.

## When the review is aimed at something bigger

A comment can be right about the code and still be pointed at the wrong thing: the spec asked for the wrong behaviour, or every reviewer keeps landing on the same seam because the module is the wrong shape. That is a decision, not a fix.

Stop the round, say what you are seeing, and tell the user to take it back to `/grill-with-docs` before more code moves. A PR that quietly redesigns itself under review is a PR nobody reviewed.
