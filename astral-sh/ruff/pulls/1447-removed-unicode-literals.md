```yaml
number: 1447
title: Removed unicode literals
type: pull_request
state: closed
author: colin99d
labels: []
assignees: []
base: main
head: main
created_at: 2022-12-29T20:21:15Z
updated_at: 2022-12-29T20:30:54Z
url: https://github.com/astral-sh/ruff/pull/1447
synced_at: 2026-01-12T15:55:06Z
```

# Removed unicode literals

---

_@colin99d_

A part of #827 

---

_@charliermarsh reviewed on 2022-12-29 20:24_

---

_Review comment by @charliermarsh on `resources/test/fixtures/pyupgrade/UP025.py`:12 on 2022-12-29 20:24_

Can you also add a test for `U"Hello"`? Kinds can be capitalized IIRC.

---

_@charliermarsh reviewed on 2022-12-29 20:24_

---

_Review comment by @charliermarsh on `resources/test/fixtures/pyupgrade/UP025.py`:1 on 2022-12-29 20:24_

(Also just checking: is this the pyupgrade test suite?)

---

_@charliermarsh reviewed on 2022-12-29 20:26_

---

_Review comment by @charliermarsh on `src/pyupgrade/plugins/rewrite_unicode_literal.rs`:20 on 2022-12-29 20:26_

Can probably be written a bit more cleanly as:

```rust
let mut contents = String::new(value.len() + 2);
contents.push('"');
contents.push_str(value);
contents.push('"');
```

Actually, though, I think this may need to be adjusted to respect the quotation style.

For example:
```
u"""Abc"""
```

Here, we need to respect the triple quotes.

There's a function to help with this: `src/pydocstyle/helpers.rs#leading_quote`. You'll need to use `checker.locator` to grab the entire expression, not just the `value` contents though.


---

_Closed by @colin99d on 2022-12-29 20:30_

---
