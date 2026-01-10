```yaml
number: 16009
title: Add --exit-non-zero-on-format
type: pull_request
state: merged
author: thejcannon
labels:
  - cli
assignees: []
merged: true
base: main
head: exit-non-zero-on-format
created_at: 2025-02-07T02:40:55Z
updated_at: 2025-03-19T17:01:44Z
url: https://github.com/astral-sh/ruff/pull/16009
synced_at: 2026-01-10T19:40:36Z
```

# Add --exit-non-zero-on-format

---

_Pull request opened by @thejcannon on 2025-02-07 02:40_

## Summary

Fixes #8191 by introducing `--exit-non-zero-on-format` to `ruff format` which pretty much does what it says on the tin.

## Test Plan

Added a new test!


---

_Comment by @github-actions[bot] on 2025-02-07 02:58_

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

_Review requested from @zanieb by @MichaReiser on 2025-02-07 06:44_

---

_Label `cli` added by @MichaReiser on 2025-02-07 09:00_

---

_@zanieb approved on 2025-02-08 15:49_

Thanks!

---

_Review comment by @zanieb on `crates/ruff/src/args.rs`:533 on 2025-02-08 15:54_

Do you find `ruff format --exit-non-zero-on-format` awkward? Should it be `ruff format --exit-non-zero-on-change` or `--exit-non-zero-on-write`? It might be nice to switch to an agnostic name.

Should we retain the `on_fix` language we use for `ruff check`? I see the motivation for not, but at the very least we probably want an alias for parity with `ruff check`?


---

_@zanieb reviewed on 2025-02-08 15:54_

---

_Comment by @zanieb on 2025-02-08 15:54_

(On second thought, I do want to talk briefly about the name before merging)

---

_@thejcannon reviewed on 2025-02-08 21:53_

---

_Review comment by @thejcannon on `crates/ruff/src/args.rs`:533 on 2025-02-08 21:53_

The current name was meant to "reflect" "exit non zero on (action)", to mirror "on fix".

As for the ergonomics? I really don't have an opinion. I can't imagine _anyone_ using this flag outside of some kind of config file or script file of some sort. In which case the name is really only as important as "can be discovered" and "name reflects what happens". So I think the bike shed on the name (past probably "exit non zero on ...") doesn't bother me. I'm happy to use whatever _yall_ like most!


---

_Comment by @thejcannon on 2025-03-17 19:12_

@zanieb bump?

---

_Comment by @zanieb on 2025-03-17 19:16_

Thanks for the bump, sorry about the delay. Let me prod some others on painting this bikeshed.

---

_Comment by @thejcannon on 2025-03-19 14:23_

No worries! My hobby project exploring `lefthook` just uses my fork.

But this week I decided to make another hobby project and remembered this issue 😁

---

_Comment by @ntBre on 2025-03-19 14:42_

Thanks again for your work on this! After discussing internally, we liked your `--exit-non-zero-on-format` flag name but wanted to add an alias for `--exit-non-zero-on-fix` to match the linter. I just made that change and duplicated your tests with the new alias.

---

_Merged by @ntBre on 2025-03-19 14:55_

---

_Closed by @ntBre on 2025-03-19 14:55_

---

_Branch deleted on 2025-03-19 17:01_

---
