```yaml
number: 15985
title: "[`pylint`] Also report when the object isn't a literal (`PLE1310`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: PLE1310-2
created_at: 2025-02-06T01:43:40Z
updated_at: 2025-02-10T16:12:57Z
url: https://github.com/astral-sh/ruff/pull/15985
synced_at: 2026-01-10T19:57:22Z
```

# [`pylint`] Also report when the object isn't a literal (`PLE1310`)

---

_Pull request opened by @InSyncWithFoo on 2025-02-06 01:43_

## Summary

Follow-up to #15984.

Previously, `PLE1310` would only report when the object is a literal:

```python
'a'.strip('//')  # error

foo = ''
foo.strip('//')  # no error
```

After this change, objects whose type can be inferred to be either `str` or `bytes` will also be reported in preview.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2025-02-06 01:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2025-02-06 08:03_

Thank you. I'll convert this to draft until https://github.com/astral-sh/ruff/pull/15984 is merged. It's otherwise fairly hard to review the changes and I can't change the base because you're an external contributor :(

---

_Converted to draft by @MichaReiser on 2025-02-06 08:03_

---

_Label `rule` added by @MichaReiser on 2025-02-06 08:03_

---

_Marked ready for review by @InSyncWithFoo on 2025-02-07 21:37_

---

_@MichaReiser reviewed on 2025-02-09 18:44_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/bad_str_strip_call.rs`:91 on 2025-02-09 18:44_

Isn't that the same as a `if typign::is_string() {...} else if typing::is_bytes {}`?

---

_@MichaReiser reviewed on 2025-02-09 18:44_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/bad_str_strip_call.rs`:186 on 2025-02-09 18:44_

```suggestion
    let value = &*value;
```

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/analyze/typing.rs`:757 on 2025-02-09 18:45_

Shouldn't this be `Bytes`? If yes, it'd be great to add a test that catches this

---

_@MichaReiser reviewed on 2025-02-09 18:45_

---

_@InSyncWithFoo reviewed on 2025-02-09 20:24_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/pylint/rules/bad_str_strip_call.rs`:91 on 2025-02-09 20:24_

I prefer `match`'s parallelism. No strong feelings though.

---

_Label `preview` added by @MichaReiser on 2025-02-10 08:30_

---

_@MichaReiser approved on 2025-02-10 08:31_

Thanks

---

_Merged by @MichaReiser on 2025-02-10 08:31_

---

_Closed by @MichaReiser on 2025-02-10 08:31_

---

_Branch deleted on 2025-02-10 16:12_

---
