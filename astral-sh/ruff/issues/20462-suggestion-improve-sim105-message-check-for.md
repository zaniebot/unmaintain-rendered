```yaml
number: 20462
title: "Suggestion: Improve SIM105 message, check for improper use of contextlib.suppress"
type: issue
state: closed
author: NMertsch
labels:
  - good first issue
  - diagnostics
assignees: []
created_at: 2025-09-18T07:30:46Z
updated_at: 2025-09-25T15:19:28Z
url: https://github.com/astral-sh/ruff/issues/20462
synced_at: 2026-01-12T15:54:57Z
```

# Suggestion: Improve SIM105 message, check for improper use of contextlib.suppress

---

_@NMertsch_

### Summary

I recently received a [PR](https://gitlab.com/SiLA2/sila_python/-/merge_requests/76) containing a subtle bug, which was kind-of caused by ruff.

### What happened
[First commit](https://gitlab.com/StefanMa/sila_python-deletion_scheduled-74452258/-/commit/a63e34e853b701b10bf7e64807d8597edbde1089#ede45c3ac59be78ed757ad91acbe7945e3b47abe_109_113):

```python
try:
    something()
except SomeException:
    pass
```

[Ruff in CI](https://gitlab.com/StefanMa/sila_python-deletion_scheduled-74452258/-/jobs/11359074513):

```text
113 | /             try:
114 | |                 something()
115 | |             except SomeException:
116 | |                 pass
    | |____________________^
    |
help: Replace with `contextlib.suppress(SomeException)`
```

The PR author literally followed the Ruff message in the [next commit](https://gitlab.com/SiLA2/sila_python/-/commit/df38c61a6f625dec0563e73d8ba846e5dc057dd5?merge_request_iid=76), which satisfied ruff:

```python
try:
    something()
except SomeException:
    contextlib.suppress(SomeException)  # instead of `pass`
```

The correct usage is:

```python
with contextlib.suppress(SomeException):
    something()
```

The [documentation of SIM105](https://docs.astral.sh/ruff/rules/suppressible-exception/) is fine, but the user didn't look there due to the seemingly clear help message.

### Suggestions
* Change the help message of SIM105 to "Replace try-except block with `with contextlib.suppress(SomeException):`"
* Add a rule for flagging `contextlib.suppress(...)` as the only statement in an `except` block (or merge with [B022: useless-contextlib-suppress](https://docs.astral.sh/ruff/rules/useless-contextlib-suppress/))

---

_Label `good first issue` added by @MichaReiser on 2025-09-18 07:42_

---

_Label `diagnostics` added by @MichaReiser on 2025-09-18 07:42_

---

_Comment by @MichaReiser on 2025-09-18 07:42_

Including `try-except` in the message seems a good improvement

---

_Comment by @Raj-G07 on 2025-09-18 21:10_

**Note**: `contextlib.suppress` is slower than using `try-except-pass` directly. For **performance-critical code**, consider retaining the try-except-pass pattern.

---

_Closed by @ntBre on 2025-09-25 15:19_

---
