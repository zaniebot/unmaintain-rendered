```yaml
number: 14291
title: "UV Index authenticate = \"always\" not respected"
type: issue
state: closed
author: asnoeyink
labels:
  - bug
assignees: []
created_at: 2025-06-26T18:16:59Z
updated_at: 2025-06-30T10:49:51Z
url: https://github.com/astral-sh/uv/issues/14291
synced_at: 2026-01-12T16:01:46Z
```

# UV Index authenticate = "always" not respected

---

_@asnoeyink_

### Summary

Hello, 
I have a project where I'm defining an explicit index for a particular package. The package repo is hosted on GitLab, and I'm using the keyring to store credentials.

uv appears to ignore `authenticate = "always"` when doing a `uv sync` and skips the keyring lookup, even though `authenticate = "always"` is supposed to add `--mode creds`. 

![Image](https://github.com/user-attachments/assets/6b739aad-8ed7-4d7c-9d00-38b54e01c858)

The kicker is that this appears to be somewhat non-deterministic. Sometimes uv will authenticate right after adding or removing another index from `pyproject.toml`, but this is not reliable. 

I am able to reliably authenticate if I provide the `UV_INDEX_<index>_USERNAME` environment variable, but looking through past issues it seems that `authenticate = "always"` is supposed to bypass the username lookup.

Relevant `pyproject.toml`:
```
[tool.uv]
keyring-provider = "subprocess"

[tool.uv.sources]
my_package = { index = "my_index" }

[[tool.uv.index]]
name = "my_index"
url = "https://gitlab.com/api/v4/groups/my_index/-/packages/pypi/simple/"
authenticate = "always"
explicit = true

# Sometimes adding or removing this index and then doing a uv sync makes it authenticate:
#[[tool.uv.index]]
#name = "pypi-public"
#url = "https://pypi.org/simple/"
#default = true
```

### Platform

Fedora 42

### Version

0.7.15

### Python version

Python 3.11

---

_Label `bug` added by @asnoeyink on 2025-06-26 18:17_

---

_Comment by @zanieb on 2025-06-26 18:33_

Can you share the full logs please? 

---

_Assigned to @jtfmumm by @zanieb on 2025-06-26 18:33_

---

_Comment by @asnoeyink on 2025-06-26 18:44_

> Can you share the full logs please?

Here you go (with some redactions 😄 )

[logs.txt](https://github.com/user-attachments/files/20931609/logs.txt)

---

_Comment by @zanieb on 2025-06-26 18:50_

Thanks!

For others, the full log for the skip is

> DEBUG Skipping keyring fetch for https://gitlab.com/api/v4/groups/1234567/-/packages/pypi/files/98974a83e3ba76abb2a2c4dd905343c55b97e1a6613bc0a0212c6ccbcb95ceae/my_project-0.3.22-py3-none-any.whl without username; use `authenticate = always` to force

Can you share trace-level logs? `-vv`


---

_Comment by @asnoeyink on 2025-06-26 19:00_

[logs_with_trace.txt](https://github.com/user-attachments/files/20931767/logs.txt)

I think this is the relevant part:

```
TRACE Request for https://gitlab.com/api/v4/groups/1234567/-/packages/pypi/files/98974a83e3ba76abb2a2c4dd905343c55b97e1a6613bc0a0212c6ccbcb95ceae/my_package-0.3.22-py3-none-any.whl failed with 401 Unauthorized, checking for credentials
TRACE No credentials in cache for realm https://gitlab.com
DEBUG No netrc file found
DEBUG Skipping keyring fetch for https://gitlab.com/api/v4/groups/1234567/-/packages/pypi/files/98974a83e3ba76abb2a2c4dd905343c55b97e1a6613bc0a0212c6ccbcb95ceae/my_package-0.3.22-py3-none-any.whl without username; use `authenticate = always` to force
TRACE Considering retry of response HTTP 401 Unauthorized for https://gitlab.com/api/v4/groups/1234567/-/packages/pypi/files/98974a83e3ba76abb2a2c4dd905343c55b97e1a6613bc0a0212c6ccbcb95ceae/my_package-0.3.22-py3-none-any.whl
TRACE Cannot retry error: not an IO error
```

---

_Comment by @zanieb on 2025-06-26 19:43_

I wonder if this is because the index URL is `https://gitlab.com/api/v4/groups/my_index/-/packages/pypi/simple/` and `https://gitlab.com/api/v4/groups/1234567/-/packages/pypi/files/98974a8...` is not a child of that URL. Is the `my_index`/ `1234567` value the same?

---

_Comment by @zanieb on 2025-06-26 19:46_

(We should be normalizing the `/simple/` away, so they'd only fail to match if we're loading the URL in a different way or a segment differs)

---

_Comment by @zanieb on 2025-06-26 19:52_

Does it work if you delete the `uv.lock` and do `uv lock`?

---

_Comment by @asnoeyink on 2025-06-26 20:00_

`my_index` is the GitLab group name. It was in the URL when we were using Poetry, and so I assumed it'd be the same for `uv`. However, after going back to the [GitLab docs](https://docs.gitlab.com/user/packages/pypi_repository/#install-from-a-group) it looks like they use the group id number instead (1234567). I swapped out the name for the number and it started working consistently.

> Does it work if you delete the uv.lock and do uv lock?

It worked once, then I deleted the `.venv` directory and tried a `uv sync --no-cache` and it failed to authenticate again (using the group name).

Using the group ID appears to solve the problem entirely. I'm not sure if there's still a uv issue or if this is just a GitLab quirk? At any rate, thank you for pointing me in the right direction!

---

_Comment by @zanieb on 2025-06-26 20:13_

> However, after going back to the [GitLab docs](https://docs.gitlab.com/user/packages/pypi_repository/#install-from-a-group) it looks like they use the group id number instead (1234567). I swapped out the name for the number and it started working consistently.

Nice! Yeah the problem is that we can't recognize the `files/...` URL as a child of the parent index URL and therefore don't propagate settings (such as the authentication policy) to it. I'm... not sure what we can do here. I think the long-term fix is to just replace use of keyring so we don't have limitations around the `--creds-mode`. We could consider being more lenient on index URL prefix matching, but that can cause other problems.

---

_Comment by @asnoeyink on 2025-06-26 20:22_

Yeah, that makes sense. I think using the group ID is more correct anyway, so I'm satisfied with that as the solution until `keyring` gets replaced.

Thanks for your help!

---

_Closed by @asnoeyink on 2025-06-26 20:22_

---

_Comment by @RazerM on 2025-06-30 10:49_

@asnoeyink FWIW I made https://pypi.org/project/keyring-gitlab-pypi/ to make uv, keyring, and GitLab work better together

---
