---
"mattpocock-skills": patch
---

Add the `to-pr` skill (in-progress bucket, user-invoked). It takes the finished work on a branch and opens the pull request for it: pin the fixed point, gather the primary sources (ticket, spec, ADRs, commit messages), check the branch carries one intent, run the repo's own checks, then write a body from those sources rather than from the diff. It closes the loop by linking the PR from the ticket. This fills the gap at the end of the main flow, where `implement` commits to a branch and nothing carries the work to a reviewer.
