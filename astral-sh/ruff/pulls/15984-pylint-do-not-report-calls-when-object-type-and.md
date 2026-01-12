```yaml
number: 15984
title: "[`pylint`] Do not report calls when object type and argument type mismatch, remove custom escape handling logic (`PLE1310`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: PLE1310
created_at: 2025-02-06T01:38:21Z
updated_at: 2025-02-07T20:35:02Z
url: https://github.com/astral-sh/ruff/pull/15984
synced_at: 2026-01-12T15:55:53Z
```

# [`pylint`] Do not report calls when object type and argument type mismatch, remove custom escape handling logic (`PLE1310`)

---

_@InSyncWithFoo_

## Summary

Resolves #15968.

Previously, these would be considered violations:

```python
b''.strip('//')
''.lstrip('//', foo = "bar")
```

...while these are not:

```python
b''.strip(b'//')
''.strip('\\b\\x08')
```

Ruff will now not report when the types of the object and that of the argument mismatch, or when there are extra arguments.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2025-02-06 01:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pylint/rules/bad_str_strip_call.rs`:154 on 2025-02-06 04:07_

Looks like this doc-comment should be updated (since the return type is not a `bool`)

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pylint/rules/bad_str_strip_call.rs`:165 on 2025-02-06 04:23_

Why are we no longer linting for versions below Python 3.9? As far as I can tell the lint makes sense, it's just that the suggestion to use `removeprefix`/`removesuffix` does not.

---

_@dylwil3 requested changes on 2025-02-06 04:25_

Looks good! Function doc comment needs an update, and I'm confused about why we changed the behavior for Python versions below 3.9.

---

_Label `bug` added by @MichaReiser on 2025-02-06 07:52_

---

_Label `rule` added by @MichaReiser on 2025-02-06 07:52_

---

_@MichaReiser reviewed on 2025-02-06 08:00_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/bad_str_strip_call.rs`:140 on 2025-02-06 08:00_

Can you explain why it's no longer necessary to handle escapes?

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/pylint/bad_str_strip_call.py`:67 on 2025-02-06 08:02_

Let's add a test that uses escaped backslashes

```suggestion
"\\test".strip("\\")
```

---

_@MichaReiser reviewed on 2025-02-06 08:02_

---

_@InSyncWithFoo reviewed on 2025-02-06 18:40_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/pylint/rules/bad_str_strip_call.rs`:140 on 2025-02-06 18:40_

Because there are no escapes to be handled. `.chars()` already converted such escapes to actual `char`s to begin with. Maybe back in the days this was necessary, but it isn't now.

---

_Review requested from @dylwil3 by @MichaReiser on 2025-02-07 17:22_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pylint/rules/bad_str_strip_call.rs`:66 on 2025-02-07 18:59_

My one remaining hesitation is that in this message we only report the first duplicated character. So it might be strange if the user writes

```python
"".strip("aabb")
```

but the diagnostic only complains about `"a"`. The alternative of listing _all_ the duplicated characters also seems unwieldy... 

I think we should just revert to the previous message - sorry for not thinking of this earlier! 

In that case `find_duplicate_in_string` can also go back to being `has_duplicates`, etc.

---

_@dylwil3 requested changes on 2025-02-07 18:59_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/pylint/rules/bad_str_strip_call.rs`:66 on 2025-02-07 19:18_

I was thinking of cases like `.strip('\\b\\x09')` where it might be hard to see at first sight where the duplicated character is. I guess that can be its own PR.

---

_@InSyncWithFoo reviewed on 2025-02-07 19:18_

---

_@dylwil3 approved on 2025-02-07 20:30_

Thank you!

---

_Merged by @dylwil3 on 2025-02-07 20:31_

---

_Closed by @dylwil3 on 2025-02-07 20:31_

---

_Branch deleted on 2025-02-07 20:34_

---
