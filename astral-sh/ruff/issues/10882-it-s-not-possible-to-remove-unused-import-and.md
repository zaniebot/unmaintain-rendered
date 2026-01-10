---
number: 10882
title: "It's not possible to remove unused import AND sort them"
type: issue
state: closed
author: rruiter87
labels:
  - question
assignees: []
created_at: 2024-04-11T15:19:51Z
updated_at: 2024-04-12T14:04:31Z
url: https://github.com/astral-sh/ruff/issues/10882
synced_at: 2026-01-10T01:22:50Z
---

# It's not possible to remove unused import AND sort them

---

_Issue opened by @rruiter87 on 2024-04-11 15:19_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

I'm using ruff with the following pre-commit. My goal is to have sorted imports (I001) and unused ones are removed (F401). 

```yaml
  - repo: https://github.com/astral-sh/ruff-pre-commit
    # Ruff version.
    rev: v0.3.5
    hooks:
      # Run the linter.
      - id: ruff
        # --select I sorts imports
        args: [ --fix, --select, I  ]
      # Run the formatter.
      - id: ruff-format
```

With `args: [ --fix, --select, I  ]` it sorts the imports, but it doesn't remove unused ones. With `args: [ --fix ]` it doesn't sort, but removed unused imports. 

Is this intended behavior? And is there a way to get both to work? 

---

_Comment by @charliermarsh on 2024-04-11 15:29_

You can change your `--select` to `--extend-select`. Otherwise, you're overwriting the default set of enabled rules (which includes `F401`).


---

_Label `question` added by @charliermarsh on 2024-04-11 15:29_

---

_Comment by @rruiter87 on 2024-04-12 08:56_

Thanks that worked!

Maybe an idea to put a note about this behavior in the documentation pages of I001 https://docs.astral.sh/ruff/rules/unsorted-imports/ and F401 https://docs.astral.sh/ruff/rules/unused-import/?

I can open a PR if you want.

---

_Comment by @charliermarsh on 2024-04-12 14:04_

This behavior actually isn't specific to those rules, it applies to enabling / selecting rules in general! There's some documentation on it here: https://docs.astral.sh/ruff/linter/#rule-selection.

---

_Closed by @charliermarsh on 2024-04-12 14:04_

---
