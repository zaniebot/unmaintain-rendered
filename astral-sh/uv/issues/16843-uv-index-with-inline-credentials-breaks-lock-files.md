```yaml
number: 16843
title: UV Index with inline credentials breaks lock files
type: issue
state: closed
author: g0t00
labels:
  - bug
assignees: []
created_at: 2025-11-25T12:54:44Z
updated_at: 2025-12-06T14:06:47Z
url: https://github.com/astral-sh/uv/issues/16843
synced_at: 2026-01-12T16:02:39Z
```

# UV Index with inline credentials breaks lock files

---

_@g0t00_

### Summary

I tried to embedded authentification credentials directly into the index definition as shown here: https://docs.astral.sh/uv/concepts/indexes/#providing-credentials-directly
This breaks behaviour in combination with lock files `uv run --locked` does not work anymore. Also `uv lock --check` failes, even when I just run `uv lock` (And another uv lock, does result in identical file)

```bash
❯ uv lock --check
Resolved 97 packages in 1.22s
The lockfile at `uv.lock` needs to be updated, but `--locked` was provided. To update the lockfile, run `uv lock`.
```

The documentation provides info on how the credentials are removed for the lock file. I would assume this somehow breaks checking for the lockfile. But the current behaviour makes lockfiles completly unusable. I would assume this is unintended behaviour.
At least there should be a clear warning in the documentation

### Platform

Many different linux distributions (Ubuntu 22, 24, current Manjaro Releases...)

### Version

uv 0.9.11 (8d8aabb88 2025-11-20)

### Python version

_No response_

---

_Label `bug` added by @g0t00 on 2025-11-25 12:54_

---

_Comment by @charliermarsh on 2025-11-28 14:32_

I'm unable to reproduce this. Can you include a minimal reproducible example please? Here's the `pyproject.toml` I used, which hits our "authenticated but actually public" proxy that only exists for testing:

```toml
[project]
name = "foo"
version = "0.1.0"
requires-python = ">=3.14.0"
dependencies = ["anyio"]

[[tool.uv.index]]
name = "internal"
url = "https://public:heron@pypi-proxy.fly.dev/basic-auth/simple/"
```

Running `uv lock --check` from that `pyproject.toml` is clean.

---

_Label `bug` removed by @charliermarsh on 2025-11-28 14:33_

---

_Label `needs-mre` added by @charliermarsh on 2025-11-28 14:33_

---

_Comment by @charliermarsh on 2025-12-03 03:43_

Closing but can always reopen with a reproduction.

---

_Closed by @charliermarsh on 2025-12-03 03:43_

---

_Comment by @g0t00 on 2025-12-05 15:44_

After some research I managed to reproduce the issue with a MWE. It seems to be a lot more specific than we initally thought:
So we require a private repository that actually enforces the auth, and and both indices need to be marked as default and the dependency need to be marked as dev. 


```toml
[project]
name = "uv-example"
version = "0.1.0"
requires-python = ">=3.13.0"
dependencies = []

[dependency-groups]
dev = ["uv-dummy==0.1.0"]
[[tool.uv.index]]
name = "PyPI"
url = "https://pypi.org/simple/"
default = true
[[tool.uv.index]]
name = "internal"
url = "http://public:heron@localhost:8080/simple"
default = true

[tool.uv.sources]
uv-dummy = { index= "internal" }
```
With the above `pyproject.toml` I get the wrong behaviour:
```bash
$ uv lock
Resolved 2 packages in 6ms
$ uv lock --check
Resolved 2 packages in 7ms
The lockfile at `uv.lock` needs to be updated, but `--check` was provided. To update the lockfile, run `uv lock`.
```
I used the `pypiserver` to get the required pypi repository. The following snippet can be used to spin up a docker container with a dummy package and the required authentification

```bash
#Generate uv dummy
mkdir uv-dummy
cd uv-dummy
uv init
uv build
cd ..
echo "public:$apr1$puwk0l7n$JMspOWN9hjyNLcnJAsy7I/" > htpasswd.txt
docker run -p 8080:8080 -v ./uv-dummy/dist:/data/packages -v ./htpasswd.txt:/data/.htpasswd pypiserver/pypiserver:latest run -a update,download,list -P /data/.htpasswd
```

This seems to be a bit of an edge case which we randomly hit in our internal setup, but I still think this should work.


---

_Comment by @charliermarsh on 2025-12-05 19:22_

It's because you have the second `default = true`. Do you know why that's present? Removing either of the two `default = true` resolves the issue.

---

_Reopened by @charliermarsh on 2025-12-05 19:30_

---

_Label `needs-mre` removed by @charliermarsh on 2025-12-05 19:31_

---

_Label `bug` added by @charliermarsh on 2025-12-05 19:31_

---

_Comment by @charliermarsh on 2025-12-05 19:31_

We should either handle this correctly or make repeated `default = true` in the same file an error.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-12-06 03:05_

---

_Closed by @charliermarsh on 2025-12-06 14:06_

---
