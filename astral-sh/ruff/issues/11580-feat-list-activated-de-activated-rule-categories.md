```yaml
number: 11580
title: "feat: List activated / de-activated rule categories"
type: issue
state: open
author: kkirsche
labels: []
assignees: []
created_at: 2024-05-28T12:49:47Z
updated_at: 2024-09-09T12:07:03Z
url: https://github.com/astral-sh/ruff/issues/11580
synced_at: 2026-01-10T11:09:53Z
```

# feat: List activated / de-activated rule categories

---

_Issue opened by @kkirsche on 2024-05-28 12:49_

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

Good morning Astral team,

Thanks for the wonderful work you all do. With the various rule groups listed in [the Ruff documentation](https://docs.astral.sh/ruff/rules/), it can sometimes be hard to understand how many categories have or have not been enabled and which ones are inactive. With the tool gaining new capabilities often, this can provide greater discoverability and can assist teams with gradual transitions over to ruff.

I was wondering if it would be possible for `ruff` to have a feature that prints out the various rule category names and which are enabled and which are disabled. For example, maybe something like:

```
$ poetry run ruff config --rule-status
Pyflakes (F) (active)
pycodestyle (E, W) (partial)
    Error (E)  (active)
    Warning (W) (inactive)
mccabe (C90) (inactive)
isort (I)  (active)
...
```

Your consideration to provide a way to understand what is or is not active is greatly appreciated. I hope you all have a wonderful week, thanks again.

---

_Comment by @kkirsche on 2024-05-28 12:52_

I'm pretty new to Rust, but if there is any guidance about how / where to implement this, I'd be open to attempting a pull request

---

_Comment by @MichaReiser on 2024-05-28 13:29_

Thank you for the kind words. 

I don't think there's a way to show the categories today. The only thing that Ruff offers is to show the enabled rules by running `ruff check --show-settings `

---

_Label `question` added by @charliermarsh on 2024-05-28 13:58_

---

_Label `question` removed by @MichaReiser on 2024-09-09 12:07_

---
