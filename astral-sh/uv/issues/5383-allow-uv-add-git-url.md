---
number: 5383
title: "Allow `uv add --git <url>`"
type: issue
state: open
author: zanieb
labels:
  - cli
assignees: []
created_at: 2024-07-23T22:19:49Z
updated_at: 2024-08-20T18:22:21Z
url: https://github.com/astral-sh/uv/issues/5383
synced_at: 2026-01-10T01:23:48Z
---

# Allow `uv add --git <url>`

---

_Issue opened by @zanieb on 2024-07-23 22:19_

e.g. `uv add [<package>] --git <url>`

Unlike `uv add <git-url>` the user should not have to include a `git+` prefix.

Part of https://github.com/astral-sh/uv/issues/5381

---

_Label `cli` added by @zanieb on 2024-07-23 22:24_

---

_Label `preview` added by @zanieb on 2024-07-23 22:24_

---

_Referenced in [astral-sh/uv#5381](../../astral-sh/uv/issues/5381.md) on 2024-07-23 22:27_

---

_Comment by @eth3lbert on 2024-07-24 08:55_

I'd like to request some clarification on the following:

1. What is the expected outcome of this operation?

2. Would the result of `uv add "httpx>0.2.0" --git https://github.com/encode/httpx` differ from `uv add "httpx>0.2.0" git+https://github.com/encode/httpx`?

3. Should we consider issues #4604  or #5341  during implementation?


---

_Comment by @zanieb on 2024-07-24 14:38_

1. The same as `uv add git+https://github.com/encode/httpx`

2. This drops the version specifiers from the `project.dependencies` entry for `httpx`, they need to be respected.

```
❯ uv init
warning: `uv init` is experimental and may change without warning
Initialized project `example`
❯ uv add "httpx>0.2.0" git+https://github.com/encode/httpx
warning: `uv add` is experimental and may change without warning
 Updated https://github.com/encode/httpx (beb501f)
⠙ Resolving dependencies...                                                                                                                                                                                                                                                    warning: `uv.sources` is experimental and may change without warning
 Updated https://github.com/encode/httpx (beb501f)
Resolved 8 packages in 379ms
   Built example @ file:///Users/zb/workspace/example
Prepared 1 package in 388ms
Uninstalled 1 package in 0.36ms
Installed 1 package in 0.85ms
 - example==0.1.0 (from file:///Users/zb/workspace/example)
 + example==0.1.0 (from file:///Users/zb/workspace/example)
❯ cat pyproject.toml
[project]
name = "example"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "httpx",
]

[tool.uv]
dev-dependencies = []

[tool.uv.sources]
httpx = { git = "https://github.com/encode/httpx" }
```

3. I think we can keep this separate from:
 - #4604, 
 - #4603
 - #5341

---

_Comment by @eth3lbert on 2024-07-26 00:50_

> 2\. This drops the version specifiers from the `project.dependencies` entry for `httpx`, they need to be respected.

Since issue #5339 has been resolved, is this statement still valid?

---

Let me summarize a bit. Please correct me if I'm wrong.

- This command adds a git source from a given URL (`git+` prefix is optional).
- This should be considered equivalent to `uv add git+URL`.
- This should preserve the version specifier if provided.
  - Since issue #5339 has been resolved, I think the following should be considered equivalent:
    - `uv add "httpx > 0.2.0" "git+https://github.com/encode/httpx"`
    - `uv add "httpx > 0.2.0" --git "https://github.com/encode/httpx"`

---
Unclear Part

- If the provided package name differs from the actual one, e.g. `uv add "asdf" --git "httpx://github.com/encode/httpx"`
    this should be considered as `uv add "asdf @ git+https://github.com/encode/httpx"`
    which results in:

  ```toml
    dependency = [ "asdf" ]
    [tool.uv.sources]
    asdf = { git = "https://github.com/encode/httpx" }
  ```

- Furthermore, if the provided package name differs from the actual one and includes a `version specifier`, e.g. `uv add "asdf > 0.2.0" --git "https://github.com/encode/httpx"`
    this should be considered as `uv add "asdf @ git+https://github.com/encode/httpx" "asdf > 0.2.0"`
    which results in:

  ```toml
    dependency = ["asdf > 0.2.0"]
    [tool.uv.source]
    asdf = {git = "https://github.com/encode/httpx"}
  ```
- Additional complexity arises when the git reference is involved.

    If the package name differs from the actual one and includes a version specifier, e.g. `uv add "asdf > 0.2.0" --git "https://github.com/encode/httpx" --tag 0.24.1`

    This cannot be run with a single `uv add` command. Currently, you need to execute two separate commands:
    `uv add "asdf @ git+https://github.com/encode/httpx" --tag 0.24.1`
    `uv add 'asdf > 0.2.0'`

  Note:
  - `uv add` does not currently support `uv add 'asdf @ git+https://github.com/encode/httpx@0.24.1'`, you must either use the `--tag` flag or the `--raw-sources` flag.
  - however, `uv pip` install allows, `uv pip install 'asdf @ git+https://github.com/encode/httpx@0.24.1'` this is valid.
  - but, `pip install 'asdf @ git+https://github.com/encode/httpx@0.24.1'` will error with: `has inconsistent name: expected 'asdf', but metadata has 'httpx'`

---

Based on my understanding, if the `--git` flag is used, we simply rewrite the `requirements` from `--git` and the `[package]` as needed (similar to how `cargo add` works, although `cargo add` doesn't currently allow specifying a git URL with a version).

---

_Label `preview` removed by @zanieb on 2024-08-20 18:22_

---
