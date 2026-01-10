---
number: 11998
title: "Won't resolve source distribution locally if dynamic version is set in Cargo.toml with maturin"
type: issue
state: closed
author: idealseal
labels:
  - question
  - needs-mre
assignees: []
created_at: 2025-03-06T10:13:12Z
updated_at: 2025-03-06T18:43:35Z
url: https://github.com/astral-sh/uv/issues/11998
synced_at: 2026-01-10T01:25:14Z
---

# Won't resolve source distribution locally if dynamic version is set in Cargo.toml with maturin

---

_Issue opened by @idealseal on 2025-03-06 10:13_

### Summary

I'm trying to vendor `pydantic-core` in my local source tree. I have a dependency on `pydantic` and put

```toml 
[tool.uv.sources]
pydantic = { path = "src/foo/vendor/pydantic" } 
pydantic-core = { path = "src/foo/vendor/pydantic-core" } 
```

in my `pyproject.toml`.

Running `uv sync -v` shows that `pydantic` is used from the local source distribution, `pydantic-core` is downloaded.

I guess this is because it can't resolve the version of `pydantic-core` and ignores the local installation.

Is this out-of-scope for uv? Is there a way to communicate this from the build system to uv? Is this a `maturin` issue?

### Platform

Linux

### Version

uv 0.6.4

### Python version

Irrelevant

---

_Label `bug` added by @idealseal on 2025-03-06 10:13_

---

_Comment by @charliermarsh on 2025-03-06 14:00_

We only respect sources for direct dependencies. I'm guessing `pydantic-core` is a transitive dependency? If you declare a dependency on `pydantic-core`, it should do what you want.

---

_Label `bug` removed by @charliermarsh on 2025-03-06 14:00_

---

_Label `question` added by @charliermarsh on 2025-03-06 14:00_

---

_Comment by @idealseal on 2025-03-06 14:23_

No, I tried this, it doesn't work either. With and without a version specifier.

---

_Comment by @charliermarsh on 2025-03-06 14:27_

If you can create a minimal reproducible example that we can run, I'm happy to help. Ideally a GitHub repo that I can clone and run to reproduce.

---

_Label `needs-mre` added by @charliermarsh on 2025-03-06 14:27_

---

_Comment by @idealseal on 2025-03-06 14:42_

https://github.com/idealseal/source-dist-resolve

---

_Assigned to @charliermarsh by @charliermarsh on 2025-03-06 16:00_

---

_Comment by @charliermarsh on 2025-03-06 17:05_

Ah, the problem is that they have `package = false` set in the `pydantic-core/pyproject.toml`, so we don't install it as a package in your environment. If you change this, it works as expected:

```diff
--- a/src/foo/pydantic-core/pyproject.toml
+++ b/src/foo/pydantic-core/pyproject.toml
@@ -161,4 +161,4 @@ fix = ["create", "fix"]
 [tool.uv]
 # this ensures that `uv run` doesn't actually build the package; a `make`
 # command is needed to build
-package = false
+package = true
```

I think we need to give you a way to override that in the `tool.uv.sources`.

---

_Referenced in [astral-sh/uv#12014](../../astral-sh/uv/pulls/12014.md) on 2025-03-06 18:11_

---

_Referenced in [astral-sh/uv#12015](../../astral-sh/uv/issues/12015.md) on 2025-03-06 18:21_

---

_Closed by @charliermarsh on 2025-03-06 18:28_

---

_Closed by @charliermarsh on 2025-03-06 18:28_

---

_Comment by @idealseal on 2025-03-06 18:43_

Thanks ❤ 

---
