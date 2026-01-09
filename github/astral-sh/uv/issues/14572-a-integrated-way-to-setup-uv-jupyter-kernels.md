---
number: 14572
title: A integrated way to setup uv jupyter kernels
type: issue
state: open
author: lucasew
labels:
  - enhancement
assignees: []
created_at: 2025-07-11T23:37:45Z
updated_at: 2025-07-11T23:37:45Z
url: https://github.com/astral-sh/uv/issues/14572
synced_at: 2026-01-07T13:12:18-06:00
---

# A integrated way to setup uv jupyter kernels

---

_Issue opened by @lucasew on 2025-07-11 23:37_

### Summary

Here is a silly experiment: https://github.com/lucasew/playground/blob/master/projetos/20250709uvx-jupyter/jupyter-kernel/setup.py

Basically a script that creates entries for jupyter kernels so when you launch either the normal jupyter or use something like the jupyter vscode extension you can set the kernel used to be uv and then it sets up a brand new empty venv with a specific python version

From that you can `uv pip install` anything you need inline using the notebook "!" and when you restart the kernel it does like how colab does: it starts another brand new environment.

Because uv uses hardlinks to deduplicate files it doesn't clutter the system in a way that could cause issues, unless the system doesn't support hardlinks.

New runs installing the same packages would cache hit and now each jupyter can have it's own independent runtime. No need to have concurrent jupyter instances running to work in different projects!

I was thinking on making a pypi package for this but maybe you guys may be interested in integrating the logic directly into uv. Maybe with a logic to delete the ephemeral venv when the "kernel" stops.

The ephemeral venvs created by the kernel thing only have ipython, so not even jupyter is required inside them, less dependencies to conflict with when dealing with jurassic libs!

### Example

`uv install-jupyter-kernels 3.13 3.12 pypy`

---

_Label `enhancement` added by @lucasew on 2025-07-11 23:37_

---
