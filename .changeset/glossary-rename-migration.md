---
"mattpocock-skills": patch
---

`setup-matt-pocock-skills` now looks for the pre-v1.3.0 `CONTEXT.md` and `CONTEXT-MAP.md` while exploring, and offers to `git mv` them to their `GLOSSARY` names and rewrite the names in `docs/agents/domain.md`. The other skills look only for the new names, so an unmoved glossary was silently ignored. The `domain-modeling` docs page's naming question, which a find-and-replace had turned into "Why is it `GLOSSARY.md` and not `GLOSSARY.md`?", now explains the rename and the migration. Fixes #1176.
