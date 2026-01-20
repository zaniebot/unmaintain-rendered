```yaml
number: 22760
title: Ruff reports syntax error when using Jupyter Notebook Magic Commands
type: issue
state: open
author: Sinthoras7
labels:
  - question
assignees: []
created_at: 2026-01-20T11:18:12Z
updated_at: 2026-01-20T12:08:00Z
url: https://github.com/astral-sh/ruff/issues/22760
synced_at: 2026-01-20T12:35:38Z
```

# Ruff reports syntax error when using Jupyter Notebook Magic Commands

---

_@Sinthoras7_

### Summary

I can not include a playground link, because I was unable to get a Jupyter Notebook in the playground.

A minimal working example would be 
```
{
 "cells": [
  {
   "cell_type": "code",
   "execution_count": null,
   "id": "de97f870",
   "metadata": {},
   "outputs": [],
   "source": [
    "%load_ext autoreload "
   ]
  }
 ],
 "metadata": {
  "language_info": {
   "name": "python"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 5
}
```
i.e., just a single cell containing `%load_ext autoreload`.

There ruff tells me `SyntaxError: invalid syntax`
<img width="452" height="46" alt="Image" src="https://github.com/user-attachments/assets/b9169c96-b94c-407f-8ce4-708e7479a9eb" />

I use version 2026.34.0 of the vscode extension. 

### Version

ruff v0.14.11

---

_Comment by @MichaReiser on 2026-01-20 12:07_

I'm unable to reproduce this. I'm also confused by the `compile` text after the diagnostic because Ruff diagnostics use `Ruff`:

<img width="1269" height="166" alt="Image" src="https://github.com/user-attachments/assets/da1b7d97-db6e-4160-b0e8-1b860d468fb8" />

Do you have any other extensions installed that are named compile?

---

_Label `question` added by @MichaReiser on 2026-01-20 12:08_

---
