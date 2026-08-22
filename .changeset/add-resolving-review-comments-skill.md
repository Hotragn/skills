---
"mattpocock-skills": patch
---

Add the `resolving-review-comments` skill (in-progress bucket, model-invoked). It works the comments on an open PR of your own: a comment is treated as a claim rather than an instruction, and each one gets one of four verdicts (fix, reject, defer, ask) settled against the code, the spec, or the repo's documented standards. Verifying a claim is the agent's job and overruling a reviewer is the user's, so the verdict table is presented before any code moves. It is the inbound twin of `code-review`, and the author's side of the review that `triage` covers from the maintainer's side.
