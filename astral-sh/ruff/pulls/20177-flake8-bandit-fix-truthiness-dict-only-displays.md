```yaml
number: 20177
title: "[`flake8-bandit`] Fix truthiness: dict-only `**` displays not truthy for `shell` (`S602`, `S604`, `S609`)"
type: pull_request
state: merged
author: danparizher
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: fix-19927
created_at: 2025-08-31T14:40:23Z
updated_at: 2025-09-10T21:45:07Z
url: https://github.com/astral-sh/ruff/pull/20177
synced_at: 2026-01-12T15:56:56Z
```

# [`flake8-bandit`] Fix truthiness: dict-only `**` displays not truthy for `shell` (`S602`, `S604`, `S609`)

---

_@danparizher_

## Summary
Fixes #19927


---

_Comment by @github-actions[bot] on 2025-08-31 14:53_

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

_Comment by @dscorbett on 2025-08-31 18:20_

With this change, `{**{}}` is considered falsey; what about `{**{**{}}}`?

---

_Comment by @danparizher on 2025-08-31 19:08_

> With this change, `{**{}}` is considered falsey; what about `{**{**{}}}`?

Good call, I had nested dict displays as truthy. I just pushed a change to have them checked recursively.

---

_Label `bug` added by @ntBre on 2025-09-09 18:19_

---

_Label `rule` added by @ntBre on 2025-09-09 18:19_

---

_Review comment by @ntBre on `crates/ruff_python_ast/src/helpers.rs`:1297 on 2025-09-09 18:37_

Thanks for diving deep here, but I think I'd kind of lean towards just considering any starred mapping to be of unknown truthiness (so just removing the `Expr::Name` requirement from `matches!`). I don't think nested, unpacked, empty dict literals are very common, so it seems like a nicer trade-off between implementation complexity and accuracy. That's also what we do for lists, sets, and tuples just above this. `Expr::is_starred_expr` for those elements is analogous to `matches!(item, DictItem { key: None, ... })` for dicts.

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/flake8_bandit/S602.py`:24 on 2025-09-09 18:38_

It might also be nice to add a test case for a more common version of this problem like `{**self.shell_defaults, **self.fetch_shell_config(self.username)}` mentioned [here](https://github.com/astral-sh/ruff/issues/19927#issuecomment-3194809639). But the code fix should be the same anyway.

---

_@ntBre reviewed on 2025-09-09 18:39_

---

_Review requested from @ntBre by @danparizher on 2025-09-09 20:48_

---

_Comment by @danparizher on 2025-09-09 20:49_

The CI failures seem to be unrelated to my changes, wondering if the one for `cargo fuzz build` was introduced elsewhere

---

_Comment by @ntBre on 2025-09-09 20:59_

Yeah no worries about the fuzz build. There was a breaking libcst release that we need to handle.

---

_@ntBre approved on 2025-09-10 20:50_

Thank you! I'll probably close and reopen to re-trigger CI, but this looks ready to go once it passes.

---

_Closed by @ntBre on 2025-09-10 20:50_

---

_Reopened by @ntBre on 2025-09-10 20:50_

---

_Merged by @ntBre on 2025-09-10 21:06_

---

_Closed by @ntBre on 2025-09-10 21:06_

---

_Branch deleted on 2025-09-10 21:45_

---
