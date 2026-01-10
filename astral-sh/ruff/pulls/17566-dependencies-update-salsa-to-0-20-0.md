```yaml
number: 17566
title: "dependencies: Update salsa to 0.20.0"
type: pull_request
state: closed
author: CreatedBySeb
labels:
  - dependencies
assignees: []
base: main
head: update-salsa
created_at: 2025-04-22T20:51:02Z
updated_at: 2025-07-15T19:53:57Z
url: https://github.com/astral-sh/ruff/pull/17566
synced_at: 2026-01-10T17:58:13Z
```

# dependencies: Update salsa to 0.20.0

---

_Pull request opened by @CreatedBySeb on 2025-04-22 20:51_

## Summary

Updates `salsa` to `0.20.0` as a crate dependency, rather than a git dependency. The previous version was https://github.com/salsa-rs/salsa/commit/87bf6b6c2d5f6479741271da73bd9d30c2580c26, which is included in the `0.20.0` release (https://github.com/salsa-rs/salsa/pull/753).

Removing one of the two git dependencies in Ruff removes a blocker to #43, and given the crates release for salsa is ahead of the depended upon git commit, this seems like a good time to make the swap.

## Test Plan

Since this is a dependency change rather than a functional change, I followed the testing from the [Contributing docs](https://docs.astral.sh/ruff/contributing/#development) on both an M1 MacBook Air and an AMD Linux machine.

```
cargo clippy --workspace --all-targets --all-features -- -D warnings
RUFF_UPDATE_SCHEMA=1 cargo nextest run
uvx pre-commit run --all-files --show-diff-on-failure
```

I used nextest rather than regular test per advice earlier in the guide, but did also try `RUFF_UPDATE_SCHEMA=1 cargo test` on the Linux machine in case there was any difference.

I was not sure what the correct way to use the fuzzers to test this change might be, since the information on how to use them wasn't very clear to me.

---

_Label `dependencies` added by @ntBre on 2025-04-22 22:22_

---

_Review requested from @MichaReiser by @ntBre on 2025-04-22 22:22_

---

_Comment by @github-actions[bot] on 2025-04-22 22:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @MichaReiser on 2025-04-23 05:57_

Thanks for the PR.

We're getting close to switching to the crates.io Salsa dependency but there have still been multiple recent occasions where we had to fix a bug in Salsa and the git dependency allowed us to move faster because of it. That's why I want to keep the git dependency for a little longer until things on the salsa side settle down. 

---

_Closed by @MichaReiser on 2025-04-23 05:57_

---

_Comment by @manmartgarc on 2025-07-15 19:53_

have things settled down yet?

---

_Comment by @MichaReiser on 2025-07-15 19:53_

No

---
