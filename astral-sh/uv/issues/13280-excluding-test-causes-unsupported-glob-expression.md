---
number: 13280
title: "excluding `**/@test` causes \"Unsupported glob expression in: `tool.uv.build-backend.*-exclude`\""
type: issue
state: closed
author: jorenham
labels:
  - bug
assignees: []
created_at: 2025-05-03T22:29:06Z
updated_at: 2025-05-07T16:31:42Z
url: https://github.com/astral-sh/uv/issues/13280
synced_at: 2026-01-10T01:25:31Z
---

# excluding `**/@test` causes "Unsupported glob expression in: `tool.uv.build-backend.*-exclude`"

---

_Issue opened by @jorenham on 2025-05-03 22:29_

### Summary

Using the `uv_build` backend, I was trying to exclude the in-package `@test` directories in numpy/numtype. The contents of the `pyproject.toml` looks like

```toml
[build-system]
requires = ["uv_build>=0.7.2,<0.8"]
build-backend = "uv_build"

# snip

[tool.uv.build-backend]
source-exclude = [
    "**/@test",
    "**/*.yml",
    "**/.*",
    "/docs",
    "/tool",
    "CONTRIBUTING.md",
    "uv.lock",
]
wheel-exclude = [
    "**/@test",
    "**/*.yml",
    "**/.*",
    "/docs",
    "/tool",
    "CONTRIBUTING.md",
    "uv.lock",
]

# snip
```

### Platform

Ubuntu 22.04

### Version

0.7.2

### Python version

3.13.3

---

_Label `bug` added by @jorenham on 2025-05-03 22:29_

---

_Referenced in [numpy/numtype#470](../../numpy/numtype/issues/470.md) on 2025-05-03 22:30_

---

_Comment by @jorenham on 2025-05-03 22:38_

The full `pyproject.toml` can be found here: https://github.com/numpy/numtype/blob/61d553fec07151c5b61c9b5d76654431bbd67130/pyproject.toml

---

_Comment by @konstin on 2025-05-03 22:50_

We took this behavior from https://peps.python.org/pep-0639/#add-license-files-key, though it clearly falls short here: There is no way to escape characters outside the `[a-zA-z0-9_.-]` to use them verbatim.

---

_Referenced in [numpy/numtype#540](../../numpy/numtype/pulls/540.md) on 2025-05-04 16:38_

---

_Comment by @joey-the-33rd on 2025-05-04 16:48_

## Fixing Unsupported Glob Expression in `pyproject.toml`

The `unsupported glob expression` error is caused by the `@` character in the pattern `**/@test` found in your `pyproject.toml` file under the `[tool.uv.build-backend]` section. The `@` character is not supported by the `uv_build` backend's glob syntax.

### Solution

Update your `pyproject.toml` to replace the problematic pattern `**/@test` with the directory pattern `/@test/`. This change helps match directories named `@test` more accurately while staying compatible with the `uv_build` backend's supported syntax.

### Before Changes:

`[tool.uv.build-backend]
exclude = ["**/@test"]`


### After changes:
`[tool.uv.build-backend]
exclude = ["/@test/"] or =[ "**/@test/**"]`


***Full clip changes should look like this:***
This change ensures that in-package `@test` directories such as those in `numpy/numtype` are excluded without triggering the unsupported glob expression error.

```toml
[tool.uv.build-backend]
source-exclude = [
    "**/@test/**",
    "**/*.yml",
    "**/.*",
    "/docs",
    "/tool",
    "CONTRIBUTING.md",
    "uv.lock",
]
wheel-exclude = [
    "**/@test/**",
    "**/*.yml",
    "**/.*",
    "/docs",
    "/tool",
    "CONTRIBUTING.md",
    "uv.lock",
]


---

_Comment by @konstin on 2025-05-04 16:59_

@joey-the-33rd Please do not post AI comment or pull requests.

---

_Comment by @jorenham on 2025-05-04 17:03_

> [@joey-the-33rd](https://github.com/joey-the-33rd) Please do not post AI comment or pull requests.

That account made exactly 1001 contributions in the last couple of days, so I'm guessing it's a fully AI-controlled account, set out to waste all of our time.

---

_Referenced in [astral-sh/uv#13311](../../astral-sh/uv/pulls/13311.md) on 2025-05-06 10:38_

---

_Referenced in [astral-sh/uv#13313](../../astral-sh/uv/pulls/13313.md) on 2025-05-06 11:04_

---

_Comment by @konstin on 2025-05-06 14:31_

We're adding support for escaping characters with backslashes. This means that our glob syntax isn't exactly PEP 639 globs anymore, but an extension over them. The backslashes allow escaping arbitrary characters except backwards and forward slashes, which are rejected they look like path separators depending on the platform (we always use forward slashes for consistency, even on windows, like PEP 639 and similar to `file:///` URLs). The restrictions on this are still tight currently, you need to escape all non-alphanumeric non-metacharacters, but it allows evolving the rules in the future, both for more metacharacters and for more non-escaped characters.

---

_Closed by @konstin on 2025-05-07 16:31_

---
