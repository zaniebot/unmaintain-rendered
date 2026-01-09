---
number: 13126
title: "Better error message for incorrect format of (requires-python = \"version\") in *.toml"
type: issue
state: closed
author: Aweryc
labels:
  - help wanted
  - error messages
assignees: []
created_at: 2025-04-27T07:48:49Z
updated_at: 2025-06-03T19:23:26Z
url: https://github.com/astral-sh/uv/issues/13126
synced_at: 2026-01-07T13:12:18-06:00
---

# Better error message for incorrect format of (requires-python = "version") in *.toml

---

_Issue opened by @Aweryc on 2025-04-27 07:48_

### Summary

Hello! Have the same issue like in [issue ](https://github.com/astral-sh/uv/issues/13117).
Have in pyproject.toml

> [project]
> name = "any-keeper"
> version = "0.1.0"
> description = "Add your description here"
> readme = "README.md"
> requires-python = "3.12"
> dependencies = [
>     "gspread>=6.2.0",
>     "mypy>=1.11",
>     "peewee>=3.17.9",
> ]


Run a 

`uv add pandas`

Got error message 

> `error: Failed to parse: `pyproject.toml`
>
>   Caused by: TOML parse error at line 6, column 19
>   |
> 6 | requires-python = "3.12"
>   |                   ^^^^^^
> Failed to parse version: Unexpected end of version specifier, expected operator:
> 3.12
> ^^^^
> `


I think this error message not very helpful. I first time using *.tolm and spend a lot time to [figure out](https://packaging.python.org/en/latest/specifications/version-specifiers/#id5) whats the problem and fix it for

>requires-python = ">=3.12"

### Example

_No response_

---

_Label `enhancement` added by @Aweryc on 2025-04-27 07:48_

---

_Label `enhancement` removed by @charliermarsh on 2025-04-27 15:09_

---

_Label `error messages` added by @charliermarsh on 2025-04-27 15:09_

---

_Comment by @charliermarsh on 2025-04-27 15:09_

I think this error message is reasonably good, though would be open to PRs to improve it if they're not too invasive.

---

_Label `help wanted` added by @charliermarsh on 2025-05-11 03:22_

---

_Comment by @charliermarsh on 2025-05-11 03:23_

Detecting that the specifier was _just_ a version and adding some sort of ("Did you mean `==3.12`?") could be useful.

---

_Comment by @jmwoliver on 2025-06-03 12:20_

I'm looking at pushing a PR for this one today if you could assign it to me.

---

_Referenced in [astral-sh/uv#13803](../../astral-sh/uv/pulls/13803.md) on 2025-06-03 12:36_

---

_Closed by @konstin on 2025-06-03 19:18_

---

_Comment by @Aweryc on 2025-06-03 19:23_

Thank you!

---
