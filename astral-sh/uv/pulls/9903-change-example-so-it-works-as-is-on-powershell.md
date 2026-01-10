```yaml
number: 9903
title: change example so it works as is on powershell and cmd
type: pull_request
state: merged
author: jimkutter
labels:
  - documentation
assignees: []
merged: true
base: main
head: docs-windows-example-update
created_at: 2024-12-15T02:58:23Z
updated_at: 2024-12-15T17:19:53Z
url: https://github.com/astral-sh/uv/pull/9903
synced_at: 2026-01-10T12:00:01Z
```

# change example so it works as is on powershell and cmd

---

_Pull request opened by @jimkutter on 2024-12-15 02:58_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

When going through the docs at https://docs.astral.sh/uv/guides/install-python/#automatic-python-downloads, when you try to copy and paste the example on Windows, in either PowerShell or cmd.exe the example won't work.

Inverting the quotes fixes it, and still works on other shells (I only tried bash under wsl)

## Test Plan

On any windows system, from powershell, running the example yields the following error:

```
> uv run --python 3.12 python -c 'print("hello world")'
  File "<string>", line 1
    print(hello world)
          ^^^^^^^^^^^
SyntaxError: invalid syntax. Perhaps you forgot a comma?
```

on cmd.exe

```
> uv run --python 3.12 python -c 'print("hello world")'

``` 
(there is no output)

Inverting the quotes on powershell
```
> uv run --python 3.12 python -c "print('hello world')"
hello world
```

on cmd.exe
```
> uv run --python 3.12 python -c "print('hello world')"
hello world


```

<!-- How was it tested? -->


---

_@zanieb approved on 2024-12-15 17:19_

Thanks!

---

_Label `documentation` added by @zanieb on 2024-12-15 17:19_

---

_Merged by @zanieb on 2024-12-15 17:19_

---

_Closed by @zanieb on 2024-12-15 17:19_

---

_Comment by @zanieb on 2024-12-15 17:19_

Welcome to the project :)

---
