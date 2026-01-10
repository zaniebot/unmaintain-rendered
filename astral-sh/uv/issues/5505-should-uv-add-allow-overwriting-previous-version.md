---
number: 5505
title: "Should `uv add` allow overwriting previous version specifiers?"
type: issue
state: open
author: eth3lbert
labels:
  - cli
assignees: []
created_at: 2024-07-27T03:00:32Z
updated_at: 2024-08-20T18:21:32Z
url: https://github.com/astral-sh/uv/issues/5505
synced_at: 2026-01-10T01:23:49Z
---

# Should `uv add` allow overwriting previous version specifiers?

---

_Issue opened by @eth3lbert on 2024-07-27 03:00_

Let consider the following process:

After running `uv init`, perform the following actions:

- Add a package with a `git URL` and `tag`.
- Add the same package but with a `version specifier`.
- Add the same package again using only the `git URL` (the `version specifier` is retained, and the `tag` is removed from the source).

In the end, the previously added `version specifier` is _retained_, and there's currently no way to overwrite it without manually editing the file or removing and re-adding the package.

``` shell
+ : mre
+ uv add 'httpx @ git+https://github.com/encode/httpx' --tag 0.24.1
warning: `uv add` is experimental and may change without warning
⠙ Resolving dependencies...                                                                         warning: `uv.sources` is experimental and may change without warning
 Updated https://github.com/encode/httpx (fcf1bc7)
Resolved 8 packages in 2.53s
   Built mre @ file:///private/tmp/mre
Prepared 2 packages in 846ms
Uninstalled 3 packages in 5ms
Installed 3 packages in 14ms
 - httpcore==1.0.5
 + httpcore==0.17.3
 - httpx==0.27.0 (from git+https://github.com/encode/httpx@7c0cda153d301bde9a011e1dd7157d7e2b20889d)
 + httpx==0.24.1 (from git+https://github.com/encode/httpx@fcf1bc73dbe13bc61d18a6e998237a5021d3341c?tag=0.24.1#fcf1bc73dbe13bc61d18a6e998237a5021d3341c)
 - mre==0.1.0 (from file:///private/tmp/mre)
 + mre==0.1.0 (from file:///private/tmp/mre)
+ :
+ :
+ :
+ cat pyproject.toml
[project]
name = "mre"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "httpx",
]

[tool.uv.sources]
httpx = { git = "https://github.com/encode/httpx", tag = "0.24.1" }
+ :
+ :
+ :
+ uv add 'httpx < 0.24.1'
warning: `uv add` is experimental and may change without warning
⠙ Resolving dependencies...                                                                         warning: `uv.sources` is experimental and may change without warning
Resolved 8 packages in 38ms
   Built mre @ file:///private/tmp/mre
Prepared 1 package in 770ms
Uninstalled 1 package in 0.68ms
Installed 1 package in 1ms
 - mre==0.1.0 (from file:///private/tmp/mre)
 + mre==0.1.0 (from file:///private/tmp/mre)
+ :
+ :
+ :
+ cat pyproject.toml
[project]
name = "mre"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "httpx<0.24.1",
]

[tool.uv.sources]
httpx = { git = "https://github.com/encode/httpx", tag = "0.24.1" }
+ :
+ :
+ :
+ uv add 'httpx @ git+https://github.com/encode/httpx'
warning: `uv add` is experimental and may change without warning
⠙ Resolving dependencies...                                                                         warning: `uv.sources` is experimental and may change without warning
 Updated https://github.com/encode/httpx (7c0cda1)
Resolved 8 packages in 3.78s
   Built mre @ file:///private/tmp/mre
Prepared 2 packages in 770ms
Uninstalled 3 packages in 4ms
Installed 3 packages in 15ms
 - httpcore==0.17.3
 + httpcore==1.0.5
 - httpx==0.24.1 (from git+https://github.com/encode/httpx@fcf1bc73dbe13bc61d18a6e998237a5021d3341c)
 + httpx==0.27.0 (from git+https://github.com/encode/httpx@7c0cda153d301bde9a011e1dd7157d7e2b20889d#7c0cda153d301bde9a011e1dd7157d7e2b20889d)
 - mre==0.1.0 (from file:///private/tmp/mre)
 + mre==0.1.0 (from file:///private/tmp/mre)
+ :
+ :
+ :
+ cat pyproject.toml
[project]
name = "mre"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "httpx<0.24.1",
]

[tool.uv.sources]
httpx = { git = "https://github.com/encode/httpx" }
+ :
```


I'm not sure if this behavior is intentional or a bug.

Related #5339 

---

_Comment by @charliermarsh on 2024-07-27 12:26_

I believe this was added intentionally (per https://github.com/astral-sh/uv/issues/5339). Maybe what we need is a way to provide "no version specifier" on the command-line?

---

_Label `cli` added by @charliermarsh on 2024-07-27 12:26_

---

_Label `preview` added by @charliermarsh on 2024-07-27 12:26_

---

_Comment by @eth3lbert on 2024-07-27 14:15_

> I believe this was added intentionally (per #5339). Maybe what we need is a way to provide "no version specifier" on the command-line?

Yes, that should work as well. I don't have a preference.

---

Here are some of my findings and thoughts that make sense to me:

1. Git source might better not specify a version and the actual version should be determined after resolving it?
    - Since `uv pip install "httpx < 0.24.1 @ git+https://github.com/encode/httpx"` are already disallowed, it makes more sense to simply use the version resolved from the provided git source. Additionally, other package managers seem to work similarly, using the resolved version from the provided git source.
    - https://github.com/astral-sh/uv/issues/4604#issuecomment-2195751972

2. In the second step, `uv add "httpx < 0.24.1"` adds a version specifier onto an existing git source package. Perhaps we should consider converting it to a normal package with a version specifier, no longer treating it as a git source?
    - This behavior aligns with `cargo`, `poetry`, and `pdm`.
 
3. If we already added a package with a version specifier, for example, `httpx < 0.24.1`, and then run `uv add "httpx"`, what should happen? This behavior differs between package managers and requires further discussion. (This also applies to git source.)

    - `poetry` would inform you that you already have it installed.
    - `pdm` would update it to the latest version using `>=`.
    - `cargo` would keep the existing version, although it might display `cargo add` but its the the same version constraint.

What are your thoughts?

---

_Label `preview` removed by @zanieb on 2024-08-20 18:21_

---
