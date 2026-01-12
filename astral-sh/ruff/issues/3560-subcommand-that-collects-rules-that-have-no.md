```yaml
number: 3560
title: Subcommand that collects rules that have no violations
type: issue
state: open
author: konstin
labels:
  - cli
  - wish
assignees: []
created_at: 2023-03-16T15:41:22Z
updated_at: 2024-01-20T09:58:42Z
url: https://github.com/astral-sh/ruff/issues/3560
synced_at: 2026-01-12T15:54:43Z
```

# Subcommand that collects rules that have no violations

---

_@konstin_

Currently, you need to manually search for non-default rules that you already fulfill that could be activated. it would be great if there was a subcommand that checks all rules on your codebase and then tells you about which additional rules you could activate because they are already fulfilled anyway

---

_Comment by @MichaReiser on 2023-03-16 16:49_

That's a great idea! The challenge I see today is that some rules are very opinionated or not even relevant for some project. I don't think we should enable these, even if there are no violations in the code base. That brings us to recommended rules...

---

_Label `cli` added by @charliermarsh on 2023-03-17 03:37_

---

_Comment by @evanrittenhouse on 2023-03-17 13:50_

I wonder if we could parse some project info file (like `pyproject.toml`, for example), and check which linters/packaegs are both installed and supported by Ruff. Then we could check all rules under those linters and ensure that there aren't any violations. Not sure if that makes sense as an approach. 

As an example, assuming all linters and `pandas` were present in Project A's `pyproject.toml` (or some other project file, though that could probably come in future versions), the CLI command would output all rules without violations. If all linters were present in Project B's `pyrpoject.toml`, but `pandas` wasn't, we'd output all rules except for the pandas ones. Same with extensions (`flake8` vs `flake8-debugger`, for example)

---

_Label `wish` added by @charliermarsh on 2023-09-21 01:03_

---

_Comment by @ScDor on 2024-01-20 09:58_

Until it's part of ruff, I created [adopt-ruff](https://github.com/ScDor/adopt-ruff), an external tool that does exactly that 🙂 Available as CLI tool and a GitHub Action. 

(Thanks Charlie for the reference here) 

---
