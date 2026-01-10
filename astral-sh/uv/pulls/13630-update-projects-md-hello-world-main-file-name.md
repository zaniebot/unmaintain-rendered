```yaml
number: 13630
title: Update projects.md hello-world main file name
type: pull_request
state: closed
author: jmiddleton
labels: []
assignees: []
base: main
head: patch-1
created_at: 2025-05-23T23:04:00Z
updated_at: 2025-05-24T00:20:04Z
url: https://github.com/astral-sh/uv/pull/13630
synced_at: 2026-01-10T11:10:42Z
```

# Update projects.md hello-world main file name

---

_Pull request opened by @jmiddleton on 2025-05-23 23:04_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

with the latest version of uv, I run the hello-world example but the main file name is now hello.py, not main.py as per the doc.

## Test Plan

Below logs of the steps to create the project:

```bash
me@macos git % uv init uv-hello-world
Initialized project `uv-hello-world` at `/Users/me/Documents/git/uv-hello-world`
me@macos git % cd uv-hello-world
me@macos uv-hello-world % ls
hello.py	pyproject.toml	README.md

me@macos uv-hello-world % uv init
error: Project is already initialized in `/Users/me/Documents/git/uv-hello-world` (`pyproject.toml` file exists)

me@macos uv-hello-world % uv run main.py
Using CPython 3.12.6 interpreter at: /Library/Frameworks/Python.framework/Versions/3.12/bin/python3.12
Creating virtual environment at: .venv
error: Failed to spawn: `main.py`
  Caused by: No such file or directory (os error 2)

me@macos uv-hello-world % uv run hello.py
Hello from uv-hello-world!
```

---

_Comment by @charliermarsh on 2025-05-23 23:35_

You might be on an old version? We changed it to `main.py` a while back.

```
❯ uv init foo
Initialized project `foo` at `/Users/crmarsh/workspace/foo`

❯ ls foo
README.md      main.py        pyproject.toml
```

---

_Comment by @jmiddleton on 2025-05-24 00:18_

You are correct, thanks for letting me know.

---

_Closed by @jmiddleton on 2025-05-24 00:20_

---
