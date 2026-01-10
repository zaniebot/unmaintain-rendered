---
number: 1554
title: pip sync with multiple files should only fail if there are incompatible requirements found
type: issue
state: closed
author: ericbn
labels: []
assignees: []
created_at: 2024-02-16T23:53:33Z
updated_at: 2024-02-17T00:03:38Z
url: https://github.com/astral-sh/uv/issues/1554
synced_at: 2026-01-10T01:23:07Z
---

# pip sync with multiple files should only fail if there are incompatible requirements found

---

_Issue opened by @ericbn on 2024-02-16 23:53_

### Bug description

I have a repository containing different projects with varying requirements.txt because each project is build and deployed separately with their respective dependencies. I use pip-sync to update the virtual environment with all the requirements.txt at once, and pip-sync will only fail if the same package is declared more than once with different pinned versions. This is a feature as it helps me make sure I don't have conflicting versions, which would cause my virtual environment to be inconsistent with at least one project.

uv fails when there are duplicate package declarations, even if they all have the same pinned version.

### Steps to reproduce

```
$ cd $(mktemp -d)
$ mkdir project{1,2}
$ echo 'attrs==23.2.0' > project1/requirements.txt
$ echo 'attrs==23.2.0' > project2/requirements.txt
$ uv venv
$ . .venv/bin/activate
$ find . -name 'requirements.txt' | xargs uv pip sync
```

### Current behavior

```
error: Failed to determine installation plan
  Caused by: Detected duplicate package in requirements: attrs
```

### Expected behavior

Should install the duplicate package if the version is the same in all files, as pip-tools:
```
$ uv pip install pip-tools
$ find . -name 'requirements.txt' | xargs pip-sync
...
Installing collected packages: attrs
Successfully installed attrs-23.2.0
```

Should fail only if there are different versions for the same package, as pip-tools:

```
$ mkdir project3
$ echo 'attrs==23.1.0' > project3/requirements.txt
$ find . -name 'requirements.txt' | xargs pip-sync
Incompatible requirements found: attrs==23.1.0 (from -r ./project3/requirements.txt (line 1)) and attrs==23.2.0 (from -r ./project1/requirements.txt (line 1))
```

### Additional context

Using uv 0.1.2 and python 3.11.7


---

_Comment by @ericbn on 2024-02-16 23:58_

This was also reported in #1552.

---

_Closed by @ericbn on 2024-02-17 00:03_

---
