```yaml
number: 5593
title: "Don't install dev dependencies from deps"
type: issue
state: closed
author: konstin
labels:
  - bug
  - preview
assignees: []
created_at: 2024-07-30T10:06:36Z
updated_at: 2024-07-31T02:32:34Z
url: https://github.com/astral-sh/uv/issues/5593
synced_at: 2026-01-12T15:58:57Z
```

# Don't install dev dependencies from deps

---

_@konstin_

Summary: uv install dev deps from path dependencies. We should only install dev deps that belong to the current project, and we must ignore them while locking.

Reproducer: The following should not install mypy.
```
uv init foo
cd foo
uv add tqdm
uv add --dev mypy
cd ..
uv init bar
cd bar
uv add ../foo
```

Full logs:
```console
$ uv init foo
warning: `uv init` is experimental and may change without warning
Initialized project `foo` at `/home/konsti/projects/uv/x/foo`
$ cd foo
$ uv add tqdm
warning: `uv add` is experimental and may change without warning
Using Python 3.12.1
Creating virtualenv at: .venv
Resolved 3 packages in 74ms
   Built foo @ file:///home/konsti/projects/uv/x/foo
Prepared 2 packages in 158ms
Installed 2 packages in 0.42ms
 + foo==0.1.0 (from file:///home/konsti/projects/uv/x/foo)
 + tqdm==4.66.4
$ uv add --dev mypy
warning: `uv add` is experimental and may change without warning
Resolved 6 packages in 2ms
   Built foo @ file:///home/konsti/projects/uv/x/foo
Prepared 4 packages in 124ms
Uninstalled 1 package in 0.24ms
Installed 4 packages in 7ms
 - foo==0.1.0 (from file:///home/konsti/projects/uv/x/foo)
 + foo==0.1.0 (from file:///home/konsti/projects/uv/x/foo)
 + mypy==1.11.0
 + mypy-extensions==1.0.0
 + typing-extensions==4.12.2
$ cd ..
$ uv init bar
warning: `uv init` is experimental and may change without warning
Initialized project `bar` at `/home/konsti/projects/uv/x/bar`
$ cd bar
$ uv add ../foo
warning: `uv add` is experimental and may change without warning
Using Python 3.12.1
Creating virtualenv at: .venv
warning: `uv.sources` is experimental and may change without warning
Resolved 7 packages in 10ms
   Built bar @ file:///home/konsti/projects/uv/x/bar
   Built foo @ file:///home/konsti/projects/uv/x/foo
Prepared 6 packages in 152ms
Installed 6 packages in 7ms
 + bar==0.1.0 (from file:///home/konsti/projects/uv/x/bar)
 + foo==0.1.0 (from file:///home/konsti/projects/uv/x/foo)
 + mypy==1.11.0
 + mypy-extensions==1.0.0
 + tqdm==4.66.4
 + typing-extensions==4.12.2
```

---

_Label `bug` added by @konstin on 2024-07-30 10:06_

---

_Label `preview` added by @konstin on 2024-07-30 10:06_

---

_Comment by @charliermarsh on 2024-07-30 12:07_

Dependency groups are enabled globally right now.

---

_Comment by @eth3lbert on 2024-07-30 12:43_

I think the culprit is here https://github.com/astral-sh/uv/blob/750b3a7c8c2ecf9315df01d609e8bcddc11fd09a/crates/uv/src/commands/project/add.rs#L243, and changing it to respect the `dependency_type` should work! (though I haven't written a test yet).
But from the following log it seems correct to me!

``` zsh
+ : mre
+ rm -rf src/ uv.lock pyproject.toml README.md foo/ bar/
+ :
+ uv-dev init foo
warning: `uv init` is experimental and may change without warning
Initialized project `foo` at `/private/tmp/i5593/foo`
+ cd foo
+ uv-dev add --no-cache tqdm
warning: `uv add` is experimental and may change without warning
Using Python 3.12.4 interpreter at: /etc/profiles/per-user/eth/bin/python3
Creating virtualenv at: .venv
Resolved 3 packages in 1.49s
   Built foo @ file:///private/tmp/i5593/foo
Prepared 2 packages in 1.65s
Installed 2 packages in 2ms
 + foo==0.1.0 (from file:///private/tmp/i5593/foo)
 + tqdm==4.66.4
+ uv-dev add --no-cache --dev mypy
warning: `uv add` is experimental and may change without warning
Resolved 6 packages in 1.47s
   Built foo @ file:///private/tmp/i5593/foo
Prepared 4 packages in 5.26s
Uninstalled 1 package in 0.75ms
Installed 4 packages in 23ms
 - foo==0.1.0 (from file:///private/tmp/i5593/foo)
 + foo==0.1.0 (from file:///private/tmp/i5593/foo)
 + mypy==1.11.0
 + mypy-extensions==1.0.0
 + typing-extensions==4.12.2
+ cd ..
+ uv-dev init bar
warning: `uv init` is experimental and may change without warning
Initialized project `bar` at `/private/tmp/i5593/bar`
+ cd bar
+ uv-dev add --no-cache ../foo
warning: `uv add` is experimental and may change without warning
Using Python 3.12.4 interpreter at: /etc/profiles/per-user/eth/bin/python3
Creating virtualenv at: .venv
⠙ Resolving dependencies...                                                                         
warning: `uv.sources` is experimental and may change without warning
Resolved 7 packages in 941ms
   Built bar @ file:///private/tmp/i5593/bar
   Built foo @ file:///private/tmp/i5593/foo
Prepared 3 packages in 1.77s
Installed 3 packages in 3ms
 + bar==0.1.0 (from file:///private/tmp/i5593/bar)
 + foo==0.1.0 (from file:///private/tmp/i5593/foo)
 + tqdm==4.66.4
+ :
+ :
+ :
+ cat pyproject.toml
[project]
name = "bar"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "foo",
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.uv.sources]
foo = { path = "../foo" }
+ :
```

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-30 15:53_

---

_Closed by @charliermarsh on 2024-07-31 02:32_

---

_Closed by @charliermarsh on 2024-07-31 02:32_

---
