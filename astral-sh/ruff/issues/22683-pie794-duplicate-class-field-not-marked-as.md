```yaml
number: 22683
title: PIE794 Duplicate class field not marked as duplicate
type: issue
state: closed
author: bhirsz
labels:
  - bug
  - rule
assignees: []
created_at: 2026-01-18T16:16:26Z
updated_at: 2026-01-19T14:35:59Z
url: https://github.com/astral-sh/ruff/issues/22683
synced_at: 2026-01-19T15:24:54Z
```

# PIE794 Duplicate class field not marked as duplicate

---

_@bhirsz_

### Summary

Playground link: https://play.ruff.rs/e8e979ae-a4d2-49a1-b883-7242fc0a2a6c

```
from dataclasses import dataclass


@dataclass
class TextEdit:
    start_line: int
    start_line: int
```

I have Python dataclass with duplicated class field. It didn't trigger any ruff rule. The closes rule I have found is PIE794 which reports duplicate class fields (with values assigned).

### Version

ruff v0.14.13

---

_Comment by @MichaReiser on 2026-01-19 10:28_

I took a look at PIE794 and I'm getting the impression that skipping annotated assignments without a value is unintentional. The rule tries to filter out self-referential assignments but that logic also filters out any annotated assignments without a value.

https://github.com/astral-sh/ruff/blob/c50863a0ee1180ceb609621ba2f07a3b977848ee/crates/ruff_linter/src/rules/flake8_pie/rules/duplicate_class_field_definition.rs#L79-L99



---

_Comment by @bhirsz on 2026-01-19 11:06_

Yes - and like I mentioned, the rule description would suggest that it shouldn't be skipped in this case.
Potentially we could catch it before _ => continue:

```
    Stmt::AnnAssign(ast::StmtAnnAssign { value: None, .. }) => {
    }
```

But I don't know Rust.

---

_Label `bug` added by @ntBre on 2026-01-19 14:26_

---

_Label `rule` added by @ntBre on 2026-01-19 14:26_

---

_Closed by @MichaReiser on 2026-01-19 14:36_

---
