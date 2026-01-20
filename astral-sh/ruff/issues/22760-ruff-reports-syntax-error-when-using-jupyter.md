```yaml
number: 22760
title: Ruff reports syntax error when using Jupyter Notebook Magic Commands
type: issue
state: open
author: Sinthoras7
labels: []
assignees: []
created_at: 2026-01-20T11:18:12Z
updated_at: 2026-01-20T11:18:12Z
url: https://github.com/astral-sh/ruff/issues/22760
synced_at: 2026-01-20T11:45:51Z
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
