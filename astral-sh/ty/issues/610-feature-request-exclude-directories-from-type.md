```yaml
number: 610
title: "Feature Request: Exclude Directories from Type Checking"
type: issue
state: closed
author: sigridjineth
labels:
  - question
assignees: []
created_at: 2025-06-08T02:16:39Z
updated_at: 2025-06-08T02:34:02Z
url: https://github.com/astral-sh/ty/issues/610
synced_at: 2026-01-12T15:54:23Z
```

# Feature Request: Exclude Directories from Type Checking

---

_@sigridjineth_

### Question

Currently, Ty recursively checks all Python files in the provided directory. However, there is no built-in option to exclude certain directories (e.g., tests/, examples/).

This feature would significantly enhance usability by allowing selective type-checking, similar to how other tools (pyright, mypy, etc.) handle exclusions via configuration files or command-line flags.

How about Introducing a CLI option (e.g., --exclude or --ignore) to skip directories or paths, like:

```
ty check . --exclude example/
```

or allow configuration through pyproject.toml:

```
[tool.ty]
exclude = ["example/**", "tests/**"]
```

This addition would align Ty's capabilities with common practices and expectations from Python tooling ecosystems.

Thanks!

### Version

_No response_

---

_Label `question` added by @sigridjineth on 2025-06-08 02:16_

---

_Comment by @charliermarsh on 2025-06-08 02:18_

Thanks! This looks like a duplicate of https://github.com/astral-sh/ty/issues/176.

---

_Comment by @sigridjineth on 2025-06-08 02:19_

@charliermarsh nice! will check the issue then.

---

_Closed by @charliermarsh on 2025-06-08 02:34_

---
