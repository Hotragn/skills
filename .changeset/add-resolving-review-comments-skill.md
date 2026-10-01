---
"mattpocock-skills": patch
---

Add the `resolving-review-comments` skill (in-progress bucket, model-invoked). It works the comments that come back on an open PR of your own: a comment is treated as a claim rather than an instruction, and each one gets one of four verdicts (fix, reject, defer, ask) settled against the code, the spec, or the repo's documented standards. Verifying a claim is the agent's job and overruling a reviewer is the user's, so the verdict table is presented before any code moves. Fixes land as themed commits on top of the reviewed history, every comment gets a reply, and where a fix changes what the PR does, the body is updated in the `pr` skill's format. It is the inbound side of `code-review`, and the author's side of the review that `triage` covers from the maintainer's side.
