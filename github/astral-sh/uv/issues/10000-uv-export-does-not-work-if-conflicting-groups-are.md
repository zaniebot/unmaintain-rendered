---
number: 10000
title: "`uv export` does not work if conflicting groups are defined"
type: issue
state: closed
author: ghislainbourgeois
labels:
  - bug
  - duplicate
assignees: []
created_at: 2024-12-18T14:28:41Z
updated_at: 2024-12-18T15:04:25Z
url: https://github.com/astral-sh/uv/issues/10000
synced_at: 2026-01-07T13:12:18-06:00
---

# `uv export` does not work if conflicting groups are defined

---

_Issue opened by @ghislainbourgeois on 2024-12-18 14:28_

I have a project that requires a different version of a library based on the infrastructure we are testing against. I defined those dependencies in 2 different dependency groups and told `uv` that they conflict:

```
[project]
name = "tls-certificates-interface"
version = "0.1.0"
requires-python = ">=3.10"

dependencies = [
    "cryptography",
    "jsonschema",
    "ops",
    "pydantic",
    "rpds-py==0.18.1"
]

[dependency-groups]
test = [
    "coverage[toml]",
    "pytest",
    "pytest-operator",
    "pytest-asyncio==0.21.2",
    "ops[testing]",
]
dev = [
    "codespell",
    "pyright",
    "ruff",
]
test-juju-2 = [
    "juju==2.9.49.0"
]
test-juju-3 = [
    "juju>3"
]

[tool.uv]
conflicts = [
    [
        { group = "test-juju-2" },
        { group = "test-juju-3" },
    ],
]
```

It works well for most of our workflows, but it fails when trying to run `uv export` to generate a `requirements.txt` file for another tool, with the error:

```
$ uv export --no-group test-juju-2 --no-group test-juju-3
Resolved 83 packages in 1ms
error: Cannot represent `uv.lock` with conflicting extras or groups as `requirements.txt`
```

I have tried many different switches to get uv to ignore those groups, as I do not need them in the requirements file, but found no way of making `uv export` work with those conflicts.

A workaround I found was to use this instead:

```
uv pip compile pyproject.toml -o requirements.txt
```

I manually validated the output against the `uv.lock` file and everything seemed to match, but I am not certain this is guaranteed when using the pip interface.

---

_Comment by @charliermarsh on 2024-12-18 14:29_

I believe this was already fixed in more recent versions.

---

_Comment by @ghislainbourgeois on 2024-12-18 14:49_

You are right, I just upgraded to the latest version and it works. Thank you, sorry for the noise!

---

_Closed by @ghislainbourgeois on 2024-12-18 14:49_

---

_Comment by @charliermarsh on 2024-12-18 15:04_

No problem, glad it was easy to resolve.

---

_Label `bug` added by @charliermarsh on 2024-12-18 15:04_

---

_Label `duplicate` added by @charliermarsh on 2024-12-18 15:04_

---
