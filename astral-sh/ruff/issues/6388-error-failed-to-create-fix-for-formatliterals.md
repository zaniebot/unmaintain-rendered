```yaml
number: 6388
title: "error: Failed to create fix for FormatLiterals: Failed to extract argument at: 1"
type: issue
state: closed
author: domsj
labels:
  - bug
assignees: []
created_at: 2023-08-07T13:12:12Z
updated_at: 2023-08-07T19:13:23Z
url: https://github.com/astral-sh/ruff/issues/6388
synced_at: 2026-01-10T11:09:48Z
```

# error: Failed to create fix for FormatLiterals: Failed to extract argument at: 1

---

_Issue opened by @domsj on 2023-08-07 13:12_

```
$ ruff check .
error: Failed to create fix for FormatLiterals: Failed to extract argument at: 1
$ echo $?
0
```

I'm sometimes getting this "error" since upgrading to ruff 0.0.282. Error between quotes, because it does seem to not error out due to this.
Clearing the cache (`rm -rf .ruff_cache`) allows to always reproduce it.

Unfortunately I don't know which code in my project is causing the issue (and I can't share the project).

Tried running with `-v`, but it's not giving much insight either.

```
$ rm -rf .ruff_cache/ && ruff check . -v
...
[2023-08-07][15:08:00][ruff::rules::isort::categorize][DEBUG] Categorized 'pandas' as Known(ThirdParty) (NoMatch)
[2023-08-07][15:08:00][ruff::rules::isort::categorize][DEBUG] Categorized 'google.cloud' as Known(ThirdParty) (NoMatch)
error: Failed to create fix for FormatLiterals: Failed to extract argument at: 1
[2023-08-07][15:08:00][ruff::rules::isort::categorize][DEBUG] Categorized 'logging' as Known(StandardLibrary) (KnownStandardLibrary)
...
[2023-08-07][15:08:00][ruff_cli::commands::run][DEBUG] Checked 51 files in: 429.3884ms
```


---

_Comment by @charliermarsh on 2023-08-07 13:25_

I was able to reproduce by working backwards from the code -- it triggers on, e.g.:

```python
"{1} {0}".format(...)
```

Cases in which the positional indices are non-sequential, but the number of arguments doesn't match the number of indices (which is valid, but we try to reorder them). 

---

_Label `bug` added by @charliermarsh on 2023-08-07 13:25_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-07 13:25_

---

_Closed by @charliermarsh on 2023-08-07 19:13_

---
