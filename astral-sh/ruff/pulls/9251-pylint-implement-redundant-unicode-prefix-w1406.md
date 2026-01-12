```yaml
number: 9251
title: "[`pylint`] Implement `redundant-unicode-prefix` (`W1406`)"
type: pull_request
state: closed
author: diceroll123
labels:
  - rule
  - preview
assignees: []
base: main
head: add-W1406
created_at: 2023-12-23T05:41:09Z
updated_at: 2023-12-23T17:06:04Z
url: https://github.com/astral-sh/ruff/pull/9251
synced_at: 2026-01-12T15:55:28Z
```

# [`pylint`] Implement `redundant-unicode-prefix` (`W1406`)

---

_@diceroll123_

## Summary

Implements [`W1406`/`redundant-u-string-prefix`](https://pylint.readthedocs.io/en/stable/user_guide/messages/warning/redundant-u-string-prefix.html)

See: #970 

## Test Plan

`cargo test`

---

_Comment by @github-actions[bot] on 2023-12-23 06:06_

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

_Comment by @bluetech on 2023-12-23 08:37_

Ruff already implements [UP025](https://docs.astral.sh/ruff/rules/unicode-kind-prefix/), is there a difference or can they be aliases?

---

_@charliermarsh approved on 2023-12-23 12:45_

Thanks!

---

_Label `rule` added by @charliermarsh on 2023-12-23 12:46_

---

_Label `preview` added by @charliermarsh on 2023-12-23 12:46_

---

_Renamed from "[pylint] - implement `W1406`/`redundant-u-string-prefix`" to "[`pylint`] Implement `redundant-unicode-prefix` (`W1406`)" by @charliermarsh on 2023-12-23 12:46_

---

_Comment by @charliermarsh on 2023-12-23 12:52_

Ah sorry, I agree -- this should be identical to `UP025`, though I'm not seeing it in the playground (which is how I typically check for redundant rules: https://play.ruff.rs/3009f32d-71b3-4f1f-a320-2f6364c0051a). Need to investigate.

---

_Comment by @charliermarsh on 2023-12-23 13:17_

Ah yeah, it is a duplicate -- sorry @diceroll123.

---

_Closed by @charliermarsh on 2023-12-23 13:17_

---

_Comment by @diceroll123 on 2023-12-23 17:06_

No worries!

---
