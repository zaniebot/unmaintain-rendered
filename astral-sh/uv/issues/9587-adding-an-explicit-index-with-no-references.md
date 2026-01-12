```yaml
number: 9587
title: Adding an explicit index with no references causes private packages to fail dependency resolution
type: issue
state: closed
author: JonZeolla
labels:
  - needs-mre
assignees: []
created_at: 2024-12-02T22:25:48Z
updated_at: 2024-12-03T13:14:23Z
url: https://github.com/astral-sh/uv/issues/9587
synced_at: 2026-01-12T15:59:53Z
```

# Adding an explicit index with no references causes private packages to fail dependency resolution

---

_@JonZeolla_

I was following the docs to [add pytorch](https://docs.astral.sh/uv/guides/integration/pytorch/) to a repo and found that when I add the following:

```toml
[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true
```

To my `pyproject.toml`, I start to get `uv run` failures about dependency resolving:

```
  × No solution found when resolving dependencies for split (python_full_version >= '3.12' and python_full_version < '3.12.4' and platform_system == 'Darwin'):
  ╰─▶ Because internal-package-2 was not found in the package registry and your project depends on internal-package-2>=1.2.3, we can conclude that your project's requirements are unsatisfiable.
```

This isn't intuitive to me, because I have `explicit = true` and I haven't referenced the new index in any of my dependencies. I was able to get my `pyproject.toml` down to something like the following and still experience the issue:

```toml
[project]
name = "example"
version = "0"
dependencies = [
    "internal-package-1",
    "internal-package-2>=1.2.3",
]
requires-python = ">= 3.12"

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true
```

---

_Comment by @zanieb on 2024-12-02 23:39_

How are you providing your internal packages?

---

_Comment by @JonZeolla on 2024-12-02 23:45_

When I install I set the `UV_EXTRA_INDEX_URL` env like [this](https://docs.astral.sh/uv/guides/integration/alternative-indexes/#aws-codeartifact)

---

_Comment by @JonZeolla on 2024-12-02 23:54_

Uv run is `uv run ${scripts}/example.py`

---

_Comment by @zanieb on 2024-12-02 23:55_

cc @charliermarsh — I'm not sure what the expected interaction between mixing the two index interfaces is here.

I agree the behavior seems surprising though.

---

_Comment by @charliermarsh on 2024-12-02 23:57_

I'll take a look.

---

_Comment by @charliermarsh on 2024-12-03 00:44_

I can't reproduce this... The best I could do with public indexes is:

```toml
[project]
name = "example"
version = "0"
dependencies = [
    "jinja2"
]
requires-python = ">= 3.12"

[[tool.uv.index]]
name = "test-pypi"
url = "https://test.pypi.org"
explicit = true
```

And then `UV_EXTRA_INDEX_URL=https://download.pytorch.org/whl/cpu uv lock`.

The lockfile correctly references the PyTorch index.

---

_Label `needs-mre` added by @zanieb on 2024-12-03 00:47_

---

_Comment by @JonZeolla on 2024-12-03 00:52_

My `UV_EXTRA_INDEX_URL` points to the private repo (and includes creds) and after locking, `uv.lock` looks like:

```
[[package]]
name = "internal-package-2"
version = "1.2.3"
source = { registry = "https://example.d.codeartifact.us-east-1.amazonaws.com/pypi/example/simple/" }
dependencies = [
    { name = "pydantic" },
]
sdist = { url = "https://example.d.codeartifact.us-east-1.amazonaws.com/pypi/example/simple/internal-package-2/1.2.3/internal-package-2-1.2.3.tar.gz", hash = "sha256:2eb5da1f9c0c9bacb008f153d6378d31eb1a702bcd3e919f249128e7373bb904" }
wheels = [
    { url = "https://example.d.codeartifact.us-east-1.amazonaws.com/pypi/example/simple/internal-package-2/1.2.3/internal-package-2-1.2.3-py3-none-any.whl", hash = "sha256:26049800ca6d846f50b8fca4f2ab275354736f5b4efbfb49e5fc8245e8bb8c20" },
]
```

Not sure if that helps? I will keep digging for more context to share

---

_Comment by @charliermarsh on 2024-12-03 00:54_

So the `uv.lock` looks correct, right? But then you're seeing an issue when you `uv run`? (Are you no longer passing `UV_EXTRA_INDEX_URL` when you `uv run`?)

---

_Comment by @charliermarsh on 2024-12-03 00:54_

My guess is that we're re-resolving when you `uv run`, but omitting your internal index.

---

_Comment by @charliermarsh on 2024-12-03 00:54_

I think you'd need to use `uv run --frozen` as-is to avoid re-resolving, since the configuration "changed" between `uv lock` and `uv run`.

---

_Comment by @JonZeolla on 2024-12-03 00:59_

Right; not passing `UV_EXTRA_INDEX_URL` when I `uv run`. I don't seem to need it when I don't have `[[tool.uv.index]]`

When I `uv run -v` I get a log like:

DEBUG Ignoring existing lockfile due to missing remote index: internal-package-2 1.2.3 from https://example.d.codeartifact.us-east-1.amazonaws.com/pypi/example/simple/


---

_Comment by @charliermarsh on 2024-12-03 01:01_

Yeah, if you don't provide _any_ index configuration, we assume the configuration from the lockfile is up-to-date. If you provide _some_ configuration, then we re-lock. Unfortunately, I don't know what a better heuristic would be here...

---

_Comment by @JonZeolla on 2024-12-03 01:12_

`uv run --frozen` fixes it without needing to re-set the `UV_EXTRA_INDEX_URL` at `uv run` time; thanks @charliermarsh !

---

_Closed by @zanieb on 2024-12-03 01:48_

---

_Comment by @JonZeolla on 2024-12-03 13:14_

Thinking about this more, if all indexes are explicit and there's no references to them, maybe skip the re locking? That would have been more intuitive to me at the start

---
