```yaml
number: 13828
title: "uv run uninstalls ssh:// packages that are in dependencie list"
type: issue
state: closed
author: gertcuykens
labels:
  - bug
assignees: []
created_at: 2025-06-04T00:55:45Z
updated_at: 2025-06-04T13:35:17Z
url: https://github.com/astral-sh/uv/issues/13828
synced_at: 2026-01-12T16:01:37Z
```

# uv run uninstalls ssh:// packages that are in dependencie list

---

_@gertcuykens_

### Summary

```
uv pip install ".[tests]"
    Updated ssh://****@github.com/gertcuykens/jwt.git (da2448149c05e08bdfbad6aeaddf1f0a
    Updated ssh://****@github.com/gertcuykens/cms.git (90f999b11aed01981399a0d67b76b4a4                                                                               
Resolved 78 packages in 1.74s
      Built as2 @ file:///Users/gertcuykens/as2
Prepared 1 package in 873ms
Uninstalled 3 packages in 3ms
Installed 3 packages in 3ms
 ~ as2==0.1.0 (from file:///Users/gertcuykens/as2)
 - cms==0.1.0 (from git+ssh://****@github.com/gertcuykens/cms.git@35f8c5d3fb36bf5a8cd84bd494391d9b33bd0fc4)
 + cms==0.1.0 (from git+ssh://****@github.com/gertcuykens/cms.git@90f999b11aed01981399a0d67b76b4a40c3698ea)
 - jwt==0.1.0 (from git+ssh://****@github.com/gertcuykens/jwt.git@dc78c0aef49f00731903bcc139d733e3707fa1f4)
 + jwt==0.1.0 (from git+ssh://****@github.com/gertcuykens/jwt.git@da2448149c05e08bdfbad6aeaddf1f0ad2466e45)
```
everything fine when I run `./tests/msg.py` it works in my loaded venv. But as soon I use uv run without --no-sync it removes the external libs in .venv leaving only the meta egg info.
```
uv run ./tests/msg.py
Built as2 @ file:///Users/gertcuykens/as2
Uninstalled 3 packages in 16ms
Installed 3 packages in 3ms
```
To fix this i have to reinstall using `uv pip install ".[tests]"`

My toml file looks like this
```
dependencies = [
    "fastapi[all]",
    "cryptography",
    "asn1crypto",
    "opentelemetry-api",
    "opentelemetry-sdk",
    "opentelemetry-exporter-otlp",
    "opentelemetry-instrumentation-httpx",
    "pydantic",
    "pydantic-settings",
    "cms @ git+ssh://git@github.com/gertcuykens/cms.git@main",
    "jwt @ git+ssh://git@github.com/gertcuykens/jwt.git@main"
]

[project.optional-dependencies]
tests = [
    "pytest-asyncio",
    "pytest-cov",
    "httpx",
    "argon2-cffi",
    "blake3",
    "typer"
]
```

### Platform

Darwin 24.5.0 arm64

### Version

uv 0.7.9 (13a86a23b 2025-05-30)

### Python version

Python 3.13.3

---

_Label `bug` added by @gertcuykens on 2025-06-04 00:55_

---

_Comment by @charliermarsh on 2025-06-04 00:59_

Is your lockfile locked to different commits? You might want to run `uv lock --upgrade-package cms`.

---

_Comment by @gertcuykens on 2025-06-04 01:12_

`uv lock --upgrade-package cms`
ok that worked like magic... thanks

The contents of the lock file regarding cms was
```
[[package]]
name = "cms"
version = "0.1.0"
source = { git = "ssh://git@github.com/gertcuykens/cms.git?rev=main#35f8c5d3fb36bf5a8cd84bd494391d9b33bd0fc4" }
dependencies = [
    { name = "argon2-cffi" },
    { name = "asn1crypto" },
    { name = "blake3" },
    { name = "cryptography" },
    { name = "pydantic" },
    { name = "pytest-asyncio" },
    { name = "pytest-cov" },
]
```

So the question then boils down to why does `uv pip install` doesn't care about the lock file and `uv run` does or why does `uv pip install` didn't update the lock file? Did i miss it somewhere in the uv documentation?


---

_Comment by @konstin on 2025-06-04 07:40_

The `uv pip` commands, like `pip`, don't use any lockfile, but what they get passed on the CLI and do a resolution from that. `uv run`/`uv sync`/etc. do use the lockfile on the other hand.

---

_Comment by @gertcuykens on 2025-06-04 12:58_

ok that makes sense, but I think a error or info message should have been shown here right? After `Installed 3 pack...` like pip install shows what version is in use

```
uv run ./tests/msg.py
Built as2 @ file:///Users/gertcuykens/as2
Uninstalled 3 packages in 16ms
Installed 3 packages in 3ms
```

PS when i run it after fixing the uv.lock it actually does and shows it every time
```
    Updated ssh://****@github.com/gertcuykens/cms.git (90f999b11aed01981399a0d67b76b4a4
    Updated ssh://****@github.com/gertcuykens/jwt.git (da2448149c05e08bdfbad6aeaddf1f0a                                                                               
Uninstalled 1 package in 10ms
Installed 1 package in 4ms
```

So i believe there is a state where uv run silently ignores a error when something goes wrong as in a commit that isn't working anymore (as in force pushed) ?

---

_Comment by @charliermarsh on 2025-06-04 13:23_

No, that's expected. We don't show the full list of changes in `uv run`, we only show you a summary. You can run `uv sync` if you want to see the complete changelog.

---

_Comment by @gertcuykens on 2025-06-04 13:35_

Ok closing the issue then as expected behaviour and not a bug. But still my gut feeling is telling me some error internally was ignored when the commit from the uv lock turned out to point to something non existing or to something broken that was force pushed.

---

_Closed by @gertcuykens on 2025-06-04 13:35_

---
