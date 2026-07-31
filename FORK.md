# Fork of mattpocock/skills

- **Upstream repository:** https://github.com/mattpocock/skills
- **Fork date:** 2026-07-31
- **Last-synced SHA:** `ed37663cc5fbef691ddfecd080dff42f7e7e350d` (1.2.0)
- **Why this fork exists:** `claude plugin marketplace add` accepts no ref,
  branch or tag, so a marketplace tracks its source's default-branch HEAD. This
  fork's `main` is therefore the version dial for `mattpocock-skills@mattpocock`: upstream
  commits accumulate on the `upstream` remote and reach no session until a sync
  evaluates them and the maintainer promotes. See ADR 0007 in
  wagnersza/orchestrator-skills.
- **Local changes:** one **invocation overlay**, and nothing else. Every skill
  upstream marks user-invoked-only is made model-invocable here, by deleting two
  keys and nothing more: `disable-model-invocation: true` from `SKILL.md`
  frontmatter, and the `policy.allow_implicit_invocation: false` block from
  `agents/openai.yaml`. No skill body, description, name or reference file is
  edited. The overlay exists because these skills are driven by the
  `orchestrator-skills` orchestrator with no human in the session, so a skill the
  model cannot reach autonomously is a skill that never runs. Upstream's own
  `.agents/invocation.md` keeps the split for interactive use; this fork is not
  interactive.

The pinned SHA that drives any decision is read live from git (`git rev-parse
main` in this clone), never from this file.

## Re-applying the overlay after a promote

The overlay is a deletion of two known keys, so an upstream commit that adds a
new user-invoked skill re-introduces them. After every promote, re-run:

```bash
python3 -m scripts.invocation_overlay --clone ~/.orchestrator/forks/mattpocock --apply
```

from the `orchestrator-skills` repo root. Without `--apply` it prints what it
would change and touches nothing.
