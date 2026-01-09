---
number: 10811
title: SSH git url for package registry as index
type: issue
state: closed
author: ricardogaspar2
labels:
  - question
  - wontfix
assignees: []
created_at: 2025-01-21T12:40:38Z
updated_at: 2025-01-21T14:23:08Z
url: https://github.com/astral-sh/uv/issues/10811
synced_at: 2026-01-07T13:12:18-06:00
---

# SSH git url for package registry as index

---

_Issue opened by @ricardogaspar2 on 2025-01-21 12:40_

hey there 👋  @charliermarsh @gcamargo1 
Based on the feature https://github.com/astral-sh/uv/issues/1378
And the docs: https://docs.astral.sh/uv/configuration/files/

Related issues: https://github.com/astral-sh/uv/issues/3783

I'm trying to use this feature. Is it possible for it to work by setting some config in the `toml` files (`pyproject` or `uv`)? I've seen the `index` setting? 


I've tried but I am getting parsing error:
```
$ uv pip install -r requirements.txt
error: Failed to parse: `uv.toml`
  Caused by: TOML parse error at line 2, column 7
  |
2 | url = "git+ssh://git@gitlab.com:foo/bar/package-registry"
  |       ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
invalid port number
```

My `uv.toml` file:

```
[[index]]
url = "git+ssh://git@gitlab.com:foo/bar/package-registry"
default = true
```

```
$ uv --version
uv 0.5.1
```

---

_Comment by @charliermarsh on 2025-01-21 14:02_

You cannot use a Git URL as a registry. Git is only supported for individual package dependencies.

---

_Label `question` added by @charliermarsh on 2025-01-21 14:02_

---

_Label `wontfix` added by @charliermarsh on 2025-01-21 14:02_

---

_Comment by @ricardogaspar2 on 2025-01-21 14:23_

Thank you @charliermarsh .
I had to resort to the git-helper + https approach.

---

_Closed by @ricardogaspar2 on 2025-01-21 14:23_

---
