---
number: 15344
title: Using keyring with package index
type: issue
state: open
author: DWhettam
labels:
  - question
assignees: []
created_at: 2025-08-18T08:47:22Z
updated_at: 2025-08-18T13:54:56Z
url: https://github.com/astral-sh/uv/issues/15344
synced_at: 2026-01-07T13:12:19-06:00
---

# Using keyring with package index

---

_Issue opened by @DWhettam on 2025-08-18 08:47_

### Question

Hi,

I asked this question in a related issue (https://github.com/astral-sh/uv/issues/15311), but have created a separate issue here.

I am trying to use gitlab package repository as an index, and authenticate it with keyring. I am able to get this working using environment variables to specify username and password, but I'd like to avoid storing these in plain text if possible. 

My pyproject.toml has the index specified as follows:

```
[[tool.uv.index]]
name = "gitlab"
url = "https://gitlab.com/api/v4/groups/111005682/-/packages/pypi/simple/"
explicit = false
authenticate = "always"
username = "oauth2"
```

and I have set:

```
export UV_KEYRING_PROVIDER=subprocess
```


How can I then link this to keyring so uv knows to pull my credentials from there? I've tried setting the password in keyring with: `uv run keyring set gitlab oauth2`, and a few other naming combinations but it doesn't seem to work. Do I need to explicitly tell uv to look at keyring for authentication? Currently I am getting the following error when trying to resolve dependencies:

```
uv add matplotlib
⠴ pyyaml==6.0.2                                                                                                                                                                                                                                                                                                                                                                               error: Failed to fetch: `https://gitlab.com/api/v4/groups/<group_id>/-/packages/pypi/simple/my_package/`         
Caused by: Missing credentials for https://gitlab.com/api/v4/groups/<group_id>/-/packages/pypi/simple/my_package/
```

but if I set `export UV_INDEX_GITLAB_USERNAME=oauth2` and `export UV_INDEX_GITLAB_PASSWORD=<PAT>` it works without issue
Thanks!

### Platform

Linux 6.14.0-27-generic x86_64 GNU/Linux

### Version

uv 0.7.19

---

_Label `question` added by @DWhettam on 2025-08-18 08:47_

---
