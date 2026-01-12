```yaml
number: 14199
title: Ruff should have a (subjective) blurb in docs when reimplementation differs from upstream
type: issue
state: open
author: jakkdl
labels:
  - documentation
assignees: []
created_at: 2024-11-08T11:24:28Z
updated_at: 2024-11-08T23:30:03Z
url: https://github.com/astral-sh/ruff/issues/14199
synced_at: 2026-01-12T15:54:53Z
```

# Ruff should have a (subjective) blurb in docs when reimplementation differs from upstream

---

_@jakkdl_

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

There are some plugins which only have partial support in ruff, i.e. my own lovechild flake8-async (~12/43 rules implemented) but also e.g. https://github.com/astral-sh/ruff/issues/13208
While the rules list always has a `for more, see xxx on PyPI.` it does not give any guidance on whether ruff is a suitable drop-in replacement - or if a user is finding a set of ruff rules to be useful whether they should consider using the original plugin as well. It might also give users an incorrect impression about the usefulness of an upstream plugin.
The one way this can be sort of currently gleaned is by looking for gaps in the list of rules, but that might be because the rules are duplicates between plugins, or removed rules, misses out if ruff simple implements them sequentially, etc etc.

There might also be cases where the ruff implementation is (subjectively) better, either because upstream is not as actively maintained, or ruff has better tooling. This could likewise be indicated in some way.

It'd perhaps even be nice if individual rules pages specified how they differ from upstream, which could double as documentation for the developers(?).

As the author of flake8-async I often end up repeatedly saying "sure you could use flake8-async in ruff but remember they only implement a small subset of the rules" and have a section discussing it in our docs: https://flake8-async.readthedocs.io/en/latest/usage.html#run-through-ruff

---

_Label `documentation` added by @AlexWaygood on 2024-11-08 23:30_

---
