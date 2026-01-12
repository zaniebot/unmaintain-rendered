```yaml
number: 9577
title: Not possible to authenticate into private registries via UV_INDEX env variables
type: issue
state: closed
author: mkravchenko-nable
labels:
  - needs-mre
assignees: []
created_at: 2024-12-02T14:40:59Z
updated_at: 2025-07-29T16:06:57Z
url: https://github.com/astral-sh/uv/issues/9577
synced_at: 2026-01-12T15:59:53Z
```

# Not possible to authenticate into private registries via UV_INDEX env variables

---

_@mkravchenko-nable_

Hello, as it described here [authorize into private registry](https://docs.astral.sh/uv/configuration/indexes/#searching-across-multiple-indexes) it should be possible to authorize into private registry, but each time I have same error:

`An index URL (https://host.com/example/api/pypi/python-local/simple) could not be queried due to a lack of valid authentication credentials (401 Unauthorized).`

In pyproject.toml I have:

```
[tool.uv]
index-strategy = "unsafe-best-match"
index-url = "https://pypi.org/simple"


[[tool.uv.index]]
name = "private"
url = "https://host.com/example/api/pypi/python-local/simple"
```

First of all, I have set two env variables: `UV_INDEX_PRIVATE_USERNAME` and `UV_INDEX_PRIVATE_PASSWORD` 
Next I'm trying to do (that bring unauthorized error):

```uv pip install -r pyproject.toml```

> If I directly providing user/password into the URL inside pyproject.toml it works as expected.

---

_Comment by @charliermarsh on 2024-12-02 14:50_

I've confirmed that we're setting the credentials here are as expected. It's working on my side. I'd need a more complete reproduction to hep out.

---

_Label `needs-mre` added by @charliermarsh on 2024-12-02 14:50_

---

_Comment by @charliermarsh on 2024-12-02 14:50_

Specifically, I did:

```toml
[project]
name = "foo"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = ["iniconfig"]

[tool.uv]
index-strategy = "unsafe-best-match"

[[tool.uv.index]]
name = "private"
default = true
url = "https://pypi-proxy.fly.dev/basic-auth/simple"
```

Then this worked:

```
UV_INDEX_PRIVATE_USERNAME=public UV_INDEX_PRIVATE_PASSWORD=heron cargo run pip install -r pyproject.toml --verbose
```

---

_Closed by @mkravchenko-nable on 2024-12-02 15:00_

---

_Comment by @mkravchenko-nable on 2024-12-03 09:52_

@charliermarsh, thank you for response. I found that if I have same host for different indexies it possible to put only one UV_INDEX_{}_USERNAME and UV_INDEX_{}_PASSWORD and it work for each one, example:

```
[[tool.uv.index]]
name = "private"
url = "https://host.com/example/api/pypi/python-local/simple"

[[tool.uv.index]]
name = "local"
url = "https://host.com/example/api/pypi/python-local-1/simple"
```

so here I just can provide:
```
UV_INDEX_PRIVATE_USERNAME=user
UV_INDEX_PRIVATE_PASSWORD=key
```

and it will also authinificate to index `local`. That was the problem.

---

_Comment by @HarshGajraSUNY on 2025-07-29 06:11_

@charliermarsh @mkravchenko-nable What if the host is different as you mentioned in your 1st comment? I am currently facing the same issue with 401 unauthorized when I have a private index from GCP and a default index set to pypi.

```An index URL (https://host.com/example/api/pypi/python-local/simple) could not be queried due to a lack of valid authentication credentials (401 Unauthorized).```


[[tool.uv.index]]
name= "PYPI"
url = "https://pypi.org/simple"


[[tool.uv.index]]
name = "GCL"
url = "https://host.pkg.dev/something-project/something-repo/simple"


---
