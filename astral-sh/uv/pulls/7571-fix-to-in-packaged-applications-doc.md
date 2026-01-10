```yaml
number: 7571
title: "Fix `-` to `_` in Packaged applications doc"
type: pull_request
state: merged
author: JacobCoffee
labels: []
assignees: []
merged: true
base: main
head: stale-doc
created_at: 2024-09-20T06:41:29Z
updated_at: 2024-09-20T06:47:53Z
url: https://github.com/astral-sh/uv/pull/7571
synced_at: 2026-01-10T12:53:50Z
```

# Fix `-` to `_` in Packaged applications doc

---

_Pull request opened by @JacobCoffee on 2024-09-20 06:41_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Small stale/typo char in docs when generating a project

```
➜ ntp -v uv-test && uv venv --python 3.12 --seed && uv init --app --package example-packaged-app
Directory /tmp/testing/uv-test created and switched to.
Using Python 3.12.4 interpreter at: /Users/coffee/.local/share/mise/installs/python/3.12/bin/python3.12
Creating virtual environment with seed packages at: .venv
 + pip==24.2
Activate with: source .venv/bin/activate.fish
Virtual environment created with Python 3.12 and activated.
Using Python 3.12.4 interpreter at: /Users/coffee/.local/share/mise/installs/python/3.12/bin/python3.12
Creating virtual environment with seed packages at: .venv
 + pip==24.2
Activate with: source .venv/bin/activate.fish
Initialized project `example-packaged-app` at `/private/tmp/testing/uv-test/example-packaged-app`

/tmp/testing/uv-test via  pyenv (uv-test) on ☁  (us-east-2)
➜ tree
   0 B    ┌─ README.md
4096 B    ├─ pyproject.toml
4096 B    │     ┌─ __init__.py
4096 B    │  ┌─ example_packaged_app
4096 B    ├─ src
8192 B ┌─ example-packaged-app
8192 B uv-test
```

## Test Plan

<!-- How was it tested? -->

Eyeballs

---

_@lucab approved on 2024-09-20 06:46_

Thanks for the docs fix!

---

_Renamed from "doc: fix `-` to `_` in Packaged applications section" to "Fix `-` to `_` in Packaged applications doc" by @lucab on 2024-09-20 06:47_

---

_Merged by @lucab on 2024-09-20 06:47_

---

_Closed by @lucab on 2024-09-20 06:47_

---
