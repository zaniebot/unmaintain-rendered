```yaml
number: 13636
title: Command to print project root directory
type: issue
state: open
author: Hawk777
labels:
  - enhancement
assignees: []
created_at: 2025-05-24T01:24:08Z
updated_at: 2025-05-24T15:48:50Z
url: https://github.com/astral-sh/uv/issues/13636
synced_at: 2026-01-12T16:01:33Z
```

# Command to print project root directory

---

_@Hawk777_

### Summary

From what I understand from reading somewhere (I can’t remember where), UV’s mechanism for discovering the project root directory is somewhat complicated, involving searching upwards from the CWD for a `.python-version` file, and also searching for a `pyproject.toml` file but only one that contains a `[project]` table. That’s not exactly something that’s easy to do in a small utility shell script, yet discovering the project root can be useful in shell scripts. It would be nice if UV had a command to just print the discovered project root directory. It already does so in debug output when `-v` is passed to certain other unrelated subcommands, but it would be nice to not have to do horrid things with `sed` to the debug stream.

### Example

```
~/myproject/some/subdir $ uv print project-dir
/home/me/myproject
```

---

_Label `enhancement` added by @Hawk777 on 2025-05-24 01:24_

---
