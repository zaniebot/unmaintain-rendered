```yaml
number: 5386
title: False positive PLC0208
type: issue
state: closed
author: frenck
labels:
  - bug
  - good first issue
assignees: []
created_at: 2023-06-27T10:51:35Z
updated_at: 2023-06-27T16:31:47Z
url: https://github.com/astral-sh/ruff/issues/5386
synced_at: 2026-01-12T15:54:45Z
```

# False positive PLC0208

---

_@frenck_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

There is a bug / false positive in rule PLC0208: `iteration-over-set`.

According to the description: "Checks for iterations over set literals".

However, it does trigger when not literals are not used.

Examples code from our codebase:

```python
for entity_id in {
    *self.hass.states.async_entity_ids(),
    *self._prefs.google_entity_configs,
}:
   ...
```

Results in: ```Use a sequence type instead of a `set` when iterating over values```

Current ruff version 0.0.272

Came up during the review of extending Ruff rulesets, the follow PR might provide more context to this issue: <https://github.com/home-assistant/core/pull/94359#pullrequestreview-1473654844>

Verified that pylint doesn't trigger on these cases.

../Frenck

---

_Label `bug` added by @charliermarsh on 2023-06-27 13:17_

---

_Comment by @charliermarsh on 2023-06-27 13:17_

Thanks!

---

_Label `good first issue` added by @charliermarsh on 2023-06-27 13:17_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-06-27 15:21_

---

_Closed by @charliermarsh on 2023-06-27 15:33_

---

_Comment by @frenck on 2023-06-27 16:31_

Awesome! Thanks for the quick turn-around ❤️ 

---
