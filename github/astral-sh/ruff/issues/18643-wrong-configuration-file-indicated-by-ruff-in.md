---
number: 18643
title: Wrong configuration file indicated by ruff in verbose mode
type: issue
state: open
author: pakal
labels:
  - configuration
assignees: []
created_at: 2025-06-12T11:36:56Z
updated_at: 2025-07-24T12:02:01Z
url: https://github.com/astral-sh/ruff/issues/18643
synced_at: 2026-01-07T13:12:16-06:00
---

# Wrong configuration file indicated by ruff in verbose mode

---

_Issue opened by @pakal on 2025-06-12 11:36_

### Summary

If for example I have a pyproject.toml at repository root, and a ruff.toml inside debug/ subdirectory, then the settings of ruff.toml are the ones properly used for the launch, however the output improperly reports pyproject.toml as the configuration file (which is false).

```
❯ ruff check debug/  -v
[2025-06-12][12:17:55][ruff::resolve][DEBUG] Using configuration file (via parent) at: /[redacted]/pyproject.toml
[2025-06-12][12:17:55][ruff_workspace::pyproject][DEBUG] No `target-version` found in `ruff.toml`
```

I haven't tested all the cases, but I guess each hierarchical configuration file activated during the processing, should be indicated as so, in the program output (with verbose mode on).


### Version

0.11.13

---

_Comment by @MichaReiser on 2025-06-12 14:33_

CC: @dylwil3 

---

_Label `question` added by @MichaReiser on 2025-06-12 14:33_

---

_Label `configuration` added by @MichaReiser on 2025-06-12 14:33_

---

_Label `question` removed by @MichaReiser on 2025-07-24 12:02_

---
