```yaml
number: 596
title: Incorrect typing
type: issue
state: closed
author: Ryang20718
labels: []
assignees: []
created_at: 2025-06-06T22:01:45Z
updated_at: 2025-06-06T22:09:39Z
url: https://github.com/astral-sh/ty/issues/596
synced_at: 2026-01-12T15:54:23Z
```

# Incorrect typing

---

_@Ryang20718_

### Summary

It is incorrectly reporting an error on `branch_name`
```
error[invalid-argument-type]: Argument to function `_is_monitored_branch` is incorrect
  --> /tmp/a.py:13:33
   |
11 |     """Post a message into performance watcher channel in case the run failed"""
12 |     branch_name = run_metadata.github_ref_name if run_metadata.github_ref_name else ""
13 |     if not _is_monitored_branch(branch_name):
   |                                 ^^^^^^^^^^^ Expected `str`, found `str | None`
14 |         return
   |
info: Function defined here
  --> /tmp/a.py:16:5
   |
14 |         return
15 |
16 | def _is_monitored_branch(branch_name: str):
   |     ^^^^^^^^^^^^^^^^^^^^ ---------------- Parameter declared here
17 |     return branch_name
   |
info: `invalid-argument-type` is enabled by default
```

```
from dataclasses import dataclass
from typing import Optional
@dataclass
class RuntimePerfRunMetadata:
    onboard_config_modes: str

    github_ref_name: Optional[str]


def _post_slack_msg(run_metadata: RuntimePerfRunMetadata):
    """Post a message into performance watcher channel in case the run failed"""
    branch_name = run_metadata.github_ref_name if run_metadata.github_ref_name else ""
    if not _is_monitored_branch(branch_name):
        return

def _is_monitored_branch(branch_name: str):
    return branch_name

```

### Version

0.0.1a8

---

_Closed by @AlexWaygood on 2025-06-06 22:09_

---
