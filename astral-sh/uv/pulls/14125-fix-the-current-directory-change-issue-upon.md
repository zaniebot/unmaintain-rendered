```yaml
number: 14125
title: Fix the current directory change issue upon activation of a relocatable venv within fish
type: pull_request
state: open
author: ri-char
labels: []
assignees: []
base: main
head: main
created_at: 2025-06-18T12:05:59Z
updated_at: 2025-06-18T12:05:59Z
url: https://github.com/astral-sh/uv/pull/14125
synced_at: 2026-01-10T11:10:42Z
```

# Fix the current directory change issue upon activation of a relocatable venv within fish

---

_Pull request opened by @ri-char on 2025-06-18 12:05_

## Summary


Activating a relocatable venv within fish shell will change current directory unexpectedly.
An example:
```fish
~ > pwd
/home/user

~ > uv venv --relocatable
Using CPython 3.13.3 interpreter at: /usr/bin/python3.13
Creating virtual environment at: ./.venv
Activate with: source .venv/bin/activate.fish

~ > source .venv/bin/activate.fish

(project) ~/.venv/bin > pwd   # current directory is changed here!
/home/user/.venv/bin
```
This PR removes `cd` command in activate script. And the current directory will not be changed any more.

Versions:
```
fish: 4.0.1
uv: 0.7.12
```

## Test Plan
```fish
set old_cwd (pwd)
uv venv --relocatable
source .venv/bin/activate.fish
set cwd (pwd)
test "$old_cwd" = "$cwd"
```


---
