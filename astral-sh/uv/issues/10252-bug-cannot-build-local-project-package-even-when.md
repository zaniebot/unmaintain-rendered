```yaml
number: 10252
title: "Bug: cannot build local project package even when listed in `no-binary-package`"
type: issue
state: closed
author: CarrotManMatt
labels:
  - needs-mre
assignees: []
created_at: 2024-12-31T14:18:15Z
updated_at: 2024-12-31T16:48:14Z
url: https://github.com/astral-sh/uv/issues/10252
synced_at: 2026-01-12T16:00:09Z
```

# Bug: cannot build local project package even when listed in `no-binary-package`

---

_@CarrotManMatt_

I have a bit of a peculiar workflow. I like to use wheels for all my dependencies and never let them be built from sdists unless explicitly stated in the `no-binary-package` setting. An example of my `[tool.uv]` settings could look like this:

```toml
[build-system]
build-backend = "hatchling.build"
requires = ["hatchling"]

[project]
name = "my-cool-project"
version = "0.1.0"
dependencies = ["dependency-a", "dependency-b"]

[tool.uv]
no-binary-package = ["my-cool-project", "dependency-a"]
no-build = true
package = true
```
Where `dependency-a` publishes only sdists, and `dependency-b` publishes sdists and wheels.

To build my project I would then do: `uv build --no-sources --build`, However this hangs during the build process and never completes. (This also makes other commands which rely on building the project like `uv lock` & `uv sync` to also hang.

I believe this is a regression as I recall this functionality working correctly with uv version 0.5.11.

Is there any way to achieve my desired functionality or am I abusing some bug by using the `--build` flag when running `uv build`? Should I just remove the `[tool.uv.no-binary-package]` & `[tool.uv.no-build]` keys and rely on uv's building of sdist dependencies when necessary? I much prefer being explicit over which dependencies I allow to be built from sdists.

---

_Renamed from "Bug: cannot build local package even when listed in `no-binary-package`" to "Bug: cannot build local project package even when listed in `no-binary-package`" by @CarrotManMatt on 2024-12-31 14:18_

---

_Comment by @charliermarsh on 2024-12-31 14:36_

Unfortunately I'm unable to reproduce this. `uv build --no-sources --build` works fine for me with this:

```toml
[build-system]
build-backend = "hatchling.build"
requires = ["hatchling"]

[project]
name = "foo"
version = "0.1.0"
dependencies = ["iniconfig", "typing-extensions"]

[tool.uv]
no-binary-package = ["foo", "iniconfig"]
no-build = true
package = true
```

I'll need a Git repository that I can clone and run to help.


---

_Label `needs-mre` added by @charliermarsh on 2024-12-31 14:36_

---

_Comment by @CarrotManMatt on 2024-12-31 16:22_

Ah I see. I think it may be some sort of circular dependencies issue. (Admittedly I didn't test the above minimal reproduction myself, so I apologise for that.) It is a completely different problem than the one described here so I will close this issue.

I'll spend more time investigating myself for now as I don't want to waste anyones time. But for those that are curious, I have project A which has a direct dependency on project B. But project B depends on project A as a `[build-system.requires]` dependency. I didn't think this would cause an issue, as the build dependencies are not locked anywhere. Perhaps I need to force refresh A when building B (building A works successfully)?

---

_Closed by @CarrotManMatt on 2024-12-31 16:22_

---

_Comment by @CarrotManMatt on 2024-12-31 16:24_

Or is there any way to use the PyPI wheel of A that B depends upon, when attempting to build A, rather than the local project itself? I thought build isolation was meant to solve this issue? Surely the local project is not installed into the build environment; an external older released version is used?

For context. The two dependent projects are A = https://github.com/CarrotManMatt/typed_classproperties & B = https://github.com/CarrotManMatt/Pydowndoc

---

_Comment by @CarrotManMatt on 2024-12-31 16:48_

I have now created #10255 to track the new issue

---
