---
number: 7229
title: Error out when using the same normalised source twice
type: issue
state: closed
author: mkniewallner
labels:
  - bug
assignees: []
created_at: 2024-09-09T19:33:05Z
updated_at: 2024-09-14T03:37:25Z
url: https://github.com/astral-sh/uv/issues/7229
synced_at: 2026-01-07T13:12:17-06:00
---

# Error out when using the same normalised source twice

---

_Issue opened by @mkniewallner on 2024-09-09 19:33_

When mapping dependencies to define sources, because of the TOML specification, uv will raise an error if defining the same source twice. For instance with this `pyproject.toml`:
```toml
[project]
name = "foo"
version = "0.1.0"
dependencies = ["python-multipart"]

[tool.uv.sources]
python-multipart = { url = "https://files.pythonhosted.org/packages/3d/47/444768600d9e0ebc82f8e347775d24aef8f6348cf00e9fa0e81910814e6d/python_multipart-0.0.9-py3-none-any.whl" }
python-multipart = { url = "https://files.pythonhosted.org/packages/c0/3e/9fbfd74e7f5b54f653f7ca99d44ceb56e718846920162165061c4c22b71a/python_multipart-0.0.8-py3-none-any.whl" }
```

the following error will be raised:
```console
$ uv lock
warning: Failed to parse `pyproject.toml` during settings discovery:
  TOML parse error at line 8, column 1
    |
  8 | python-multipart = { url = "https://files.pythonhosted.org/packages/c0/3e/9fbfd74e7f5b54f653f7ca99d44ceb56e718846920162165061c4c22b71a/python_multipart-0.0.8-py3-none-any.whl" }
    | ^
  duplicate key `python-multipart` in table `tool.uv.sources`

error: Failed to parse: `pyproject.toml`
  Caused by: TOML parse error at line 8, column 1
  |
8 | python-multipart = { url = "https://files.pythonhosted.org/packages/c0/3e/9fbfd74e7f5b54f653f7ca99d44ceb56e718846920162165061c4c22b71a/python_multipart-0.0.8-py3-none-any.whl" }
  | ^
duplicate key `python-multipart` in table `tool.uv.sources`
```
(btw not sure we should have both a warning and error here)

But while uv applies normalisation for sources (similarly to dependencies), it will not error out if defining the same source twice with different casing that would result in the same name after normalisation:
```toml
[project]
name = "foo"
version = "0.1.0"
dependencies = ["python-multipart"]

[tool.uv.sources]
python-multipart = { url = "https://files.pythonhosted.org/packages/3d/47/444768600d9e0ebc82f8e347775d24aef8f6348cf00e9fa0e81910814e6d/python_multipart-0.0.9-py3-none-any.whl" }
python_multipart = { url = "https://files.pythonhosted.org/packages/c0/3e/9fbfd74e7f5b54f653f7ca99d44ceb56e718846920162165061c4c22b71a/python_multipart-0.0.8-py3-none-any.whl" }
```

Here, locking will work, and will end up choosing the latest source defined in the list (here in `python_multipart`, and if reversing the list, `python-multipart` is chosen instead).

Should uv also error out in this case, like it does when using the exact same casing?

---

_Comment by @charliermarsh on 2024-09-09 19:39_

Yeah, I think we should probably error in both cases. Good find.

---

_Comment by @charliermarsh on 2024-09-09 19:40_

> (btw not sure we should have both a warning and error here)

Agree though it's somewhat annoying to fix because these happen very far apart from one another.

---

_Label `bug` added by @charliermarsh on 2024-09-09 19:40_

---

_Comment by @mkniewallner on 2024-09-09 19:45_

> > (btw not sure we should have both a warning and error here)
> 
> Agree though it's somewhat annoying to fix because these happen very far apart from one another.

Yeah to be fair this is really just a tiny annoyance, mentioned it just because I stumbled across it 😄 

---

_Comment by @charliermarsh on 2024-09-09 19:47_

It bothers me lol.

---

_Referenced in [renovatebot/renovate#31297](../../renovatebot/renovate/pulls/31297.md) on 2024-09-09 22:14_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-14 03:15_

---

_Referenced in [astral-sh/uv#7383](../../astral-sh/uv/pulls/7383.md) on 2024-09-14 03:25_

---

_Closed by @charliermarsh on 2024-09-14 03:37_

---

_Closed by @charliermarsh on 2024-09-14 03:37_

---
