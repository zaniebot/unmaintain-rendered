```yaml
number: 3416
title: "[pylint] unspecified-encoding (W1514)"
type: pull_request
state: closed
author: csko
labels:
  - rule
assignees: []
base: main
head: pylint-w1514
created_at: 2023-03-09T10:20:10Z
updated_at: 2023-05-12T01:33:20Z
url: https://github.com/astral-sh/ruff/pull/3416
synced_at: 2026-01-12T03:56:38Z
```

# [pylint] unspecified-encoding (W1514)

---

_Pull request opened by @csko on 2023-03-09 10:20_

Partially addresses https://github.com/charliermarsh/ruff/issues/3278

Implements the core features for w1514, including having `"b"` in the `mode` arg.

Not all fixtures work properly, in particular:
- this only covers the builtin `open()`
- variables are not inferred; `open(x, encoding=y)`
- this does not handle class type arguments

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pylint/rules/unspecified_encoding.rs`:62 on 2023-03-13 08:08_

Nit: I recommend switching the `is_builtin` check and testing the function name because testing `is_builtin` is more expensive than comparing two strings
```suggestion
    // Look for open().
    if !matches!(&func.node, ExprKind::Name {id, ..} if id == OPEN_FUNC_NAME) {
        return;
    }
    
    // If `open` has been rebound, skip this check entirely.
    if !checker.ctx.is_builtin(OPEN_FUNC_NAME) {
        return;
    }
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pylint/rules/unspecified_encoding.rs`:80 on 2023-03-13 08:10_

Nit: I recommend inlining the `ENCODING_KEYWORD_ARGUMENT` constant because it is only used once and it can break the reading flow if I have to jump to the top to understand what the value of the constant is.

---

_@MichaReiser approved on 2023-03-13 08:12_

---

_@charliermarsh reviewed on 2023-03-17 03:48_

---

_Review comment by @charliermarsh on `crates/ruff/resources/test/fixtures/pylint/unspecified_encoding.py`:12 on 2023-03-17 03:48_

One issue here is that this rule is in conflict with `UP015`, which removes redundant open modes (so, e.g., it changes `open("foo", "r")` to `open("foo")`). We'd have to mark these rules as incompatible in the `INCOMPATIBLE_CODES` slice in `crates/ruff/src/registry.rs`.

---

_Label `rule` added by @charliermarsh on 2023-03-17 03:51_

---

_Comment by @charliermarsh on 2023-05-12 01:33_

Closing to keep the pull request queue up-to-date -- we'd have to resolve some incompatibilities with existing rules here.

---

_Closed by @charliermarsh on 2023-05-12 01:33_

---
