---
number: 11363
title: "Wondering if there's a way for ruff to exclude a rule on certain files"
type: issue
state: closed
author: Ryang20718
labels:
  - question
assignees: []
created_at: 2024-05-10T16:40:35Z
updated_at: 2024-05-10T18:40:47Z
url: https://github.com/astral-sh/ruff/issues/11363
synced_at: 2026-01-07T13:12:15-06:00
---

# Wondering if there's a way for ruff to exclude a rule on certain files

---

_Issue opened by @Ryang20718 on 2024-05-10 16:40_

First off, want to say <3 this linter.

I was wondering if there's a way to exclude running a rule on a certain file type in `ruff.toml`. If this feature isn't present, that's totally fine!

Main use case for us is we have a lot of __init__.py files with bazel that import libraries, but don't necessarily end up being used (from the linter's perspective due to rust python bindings). We'd like to lint these files, for all other rules, but not f401.

---

_Comment by @UnknownPlatypus on 2024-05-10 17:21_

You are probably looking for the [per-file-ignores](https://docs.astral.sh/ruff/settings/#lint_per-file-ignores) setting.
You can add something like that to your `pyproject.toml` file:

```toml
[tool.ruff.lint.per-file-ignores]
"__init__.py" = ["F401"]
```

---

_Comment by @charliermarsh on 2024-05-10 18:40_

👍 `per-file-ignores` is the recommended solution here!

---

_Closed by @charliermarsh on 2024-05-10 18:40_

---

_Label `question` added by @charliermarsh on 2024-05-10 18:40_

---
