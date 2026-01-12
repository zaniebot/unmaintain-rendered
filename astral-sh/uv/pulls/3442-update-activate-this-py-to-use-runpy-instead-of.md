```yaml
number: 3442
title: Update activate_this.py to use runpy instead of exec in the docstring
type: pull_request
state: merged
author: FredStober
labels: []
assignees: []
merged: true
base: main
head: use_runpy_instead_of_exec
created_at: 2024-05-07T23:02:54Z
updated_at: 2024-05-13T01:21:25Z
url: https://github.com/astral-sh/uv/pull/3442
synced_at: 2026-01-12T16:05:39Z
```

# Update activate_this.py to use runpy instead of exec in the docstring

---

_@FredStober_

## Summary

runpy.run_path was added in python 2.7 and 3.2 - and every python that is not EOL supports it.

It is arguably nicer to read and the path is only given once in the command.

At least right now, runpy - unlike exec with S102 - is not flagged by any bandit-derived ruff check.
(I guess because it loads from a file instead of a simple string...)

Because of the import, it is also not a one-liner anymore. (But that could be fixed with an __import__('runpy').run_path...)

## Test Plan

import runpy
runpy.run_path('/path/to/venv/bin/activate_this.py')

---

_Comment by @zanieb on 2024-05-08 00:47_

Welcome to the project and thanks for contributing! I'm not sure this merits diverging from virtualenv — we copy these implementations from there and the less divergence we have the easier it is for us to maintain. Maybe it'd be worth checking if they're interested in it upstream?

---

_Comment by @FredStober on 2024-05-13 01:13_

The updated documentation was merged into the upstream virtualenv library - and this PR brings the same change to uv

---

_@charliermarsh approved on 2024-05-13 01:14_

Thanks for taking this upstream!

---

_Merged by @charliermarsh on 2024-05-13 01:21_

---

_Closed by @charliermarsh on 2024-05-13 01:21_

---
