```yaml
number: 6605
title: "Respect `--no-build-isolation-package` in `uv sync`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/pkg
created_at: 2024-08-25T13:59:29Z
updated_at: 2024-08-26T17:48:42Z
url: https://github.com/astral-sh/uv/pull/6605
synced_at: 2026-01-10T13:09:51Z
```

# Respect `--no-build-isolation-package` in `uv sync`

---

_Pull request opened by @charliermarsh on 2024-08-25 13:59_

## Summary

This was an oversight. The existing test was (correctly) failing, but for the wrong reason (failing to build the package during _resolution_).

---

_Label `bug` added by @charliermarsh on 2024-08-25 13:59_

---

_Merged by @charliermarsh on 2024-08-25 14:15_

---

_Closed by @charliermarsh on 2024-08-25 14:15_

---

_Branch deleted on 2024-08-25 14:15_

---

_Comment by @kabouzeid on 2024-08-25 18:12_

Still doesn't work for me. `--no-build-isolation` works, `--no-build-isolation-package` doesn't. Not only with `sync`, but also `add`, `pip`, and config files.

```console
➜ uv --version
uv 0.3.3 (1c580723c 2024-08-25)

➜ cat pyproject.toml
[project]
name = "6605"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "source-distribution @ https://files.pythonhosted.org/packages/10/1f/57aa4cce1b1abf6b433106676e15f9fa2c92ed2bd4cf77c3b50a9e9ac773/source_distribution-0.0.1.tar.gz",
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

➜ uv sync
Resolved 2 packages in 2ms
error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: 6605 @ file:///tmp/6605
  Caused by: Build backend failed to build wheel through `build_editable()` with exit status: 1
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 11, in <module>
  File "/home/abou_zeid/.cache/uv/builds-v0/.tmp10Vbaz/lib/python3.12/site-packages/hatchling/build.py", line 83, in build_editable
    return os.path.basename(next(builder.build(directory=wheel_directory, versions=['editable'])))
                            ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/abou_zeid/.cache/uv/builds-v0/.tmp10Vbaz/lib/python3.12/site-packages/hatchling/builders/plugin/interface.py", line 90, in build
    self.metadata.validate_fields()
  File "/home/abou_zeid/.cache/uv/builds-v0/.tmp10Vbaz/lib/python3.12/site-packages/hatchling/metadata/core.py", line 266, in validate_fields
    self.core.validate_fields()
  File "/home/abou_zeid/.cache/uv/builds-v0/.tmp10Vbaz/lib/python3.12/site-packages/hatchling/metadata/core.py", line 1376, in validate_fields
    getattr(self, attribute)
  File "/home/abou_zeid/.cache/uv/builds-v0/.tmp10Vbaz/lib/python3.12/site-packages/hatchling/metadata/core.py", line 1230, in dependencies
    self._dependencies = list(self.dependencies_complex)
                              ^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/abou_zeid/.cache/uv/builds-v0/.tmp10Vbaz/lib/python3.12/site-packages/hatchling/metadata/core.py", line 1215, in dependencies_complex
    raise ValueError(message)
ValueError: Dependency #1 of field `project.dependencies` cannot be a direct reference unless field `tool.hatch.metadata.allow-direct-references` is set to `true`
---

➜ uv sync --no-build-isolation-package source-distribution
Resolved 2 packages in 2ms
error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: 6605 @ file:///tmp/6605
  Caused by: Build backend failed to build wheel through `build_editable()` with exit status: 1
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 11, in <module>
  File "/home/abou_zeid/.cache/uv/builds-v0/.tmpUBzU1v/lib/python3.12/site-packages/hatchling/build.py", line 83, in build_editable
    return os.path.basename(next(builder.build(directory=wheel_directory, versions=['editable'])))
                            ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/abou_zeid/.cache/uv/builds-v0/.tmpUBzU1v/lib/python3.12/site-packages/hatchling/builders/plugin/interface.py", line 90, in build
    self.metadata.validate_fields()
  File "/home/abou_zeid/.cache/uv/builds-v0/.tmpUBzU1v/lib/python3.12/site-packages/hatchling/metadata/core.py", line 266, in validate_fields
    self.core.validate_fields()
  File "/home/abou_zeid/.cache/uv/builds-v0/.tmpUBzU1v/lib/python3.12/site-packages/hatchling/metadata/core.py", line 1376, in validate_fields
    getattr(self, attribute)
  File "/home/abou_zeid/.cache/uv/builds-v0/.tmpUBzU1v/lib/python3.12/site-packages/hatchling/metadata/core.py", line 1230, in dependencies
    self._dependencies = list(self.dependencies_complex)
                              ^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/abou_zeid/.cache/uv/builds-v0/.tmpUBzU1v/lib/python3.12/site-packages/hatchling/metadata/core.py", line 1215, in dependencies_complex
    raise ValueError(message)
ValueError: Dependency #1 of field `project.dependencies` cannot be a direct reference unless field `tool.hatch.metadata.allow-direct-references` is set to `true`
---

➜ uv sync --no-build-isolation
Resolved 2 packages in 3ms
error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: 6605 @ file:///tmp/6605
  Caused by: Build backend failed to build wheel through `build_editable()` with exit status: 1
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 8, in <module>
ModuleNotFoundError: No module named 'hatchling'
---
```


---

_Comment by @charliermarsh on 2024-08-25 20:04_

This is not necessarily incorrect... If you set `--no-build-isolation`, then the failure is probably a failure to build `6605` itself due to missing `hatchling`. But if you set `--no-build-isolation-package source-distribution`, then the failure is in building `6605` but due to an error in _how you've packaged it_, so it never even gets to the part where it needs to build `source-distribution`.

---

_Comment by @charliermarsh on 2024-08-25 20:04_

Here's a working example:

```toml
[project]
name = "foo"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "source-distribution @ https://files.pythonhosted.org/packages/10/1f/57aa4cce1b1abf6b433106676e15f9fa2c92ed2bd4cf77c3b50a9e9ac773/source_distribution-0.0.1.tar.gz"
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.hatch.metadata]
allow-direct-references = true
```

```
❯ cargo run sync --no-build-isolation-package source-distribution
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.15s
     Running `/Users/crmarsh/workspace/uv/target/debug/uv sync --no-build-isolation-package source-distribution`
Resolved 2 packages in 8ms
error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: source-distribution @ https://files.pythonhosted.org/packages/10/1f/57aa4cce1b1abf6b433106676e15f9fa2c92ed2bd4cf77c3b50a9e9ac773/source_distribution-0.0.1.tar.gz
  Caused by: Build backend failed to build wheel through `build_wheel()` with exit status: 1
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 8, in <module>
ModuleNotFoundError: No module named 'hatchling'
---
```

Though I'd recommend using `tool.uv.sources` instead so you don't have to fight `hatchling`:

```toml
[project]
name = "foo"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = ["source-distribution"]

[tool.uv.sources]
source-distribution = { url = "https://files.pythonhosted.org/packages/10/1f/57aa4cce1b1abf6b433106676e15f9fa2c92ed2bd4cf77c3b50a9e9ac773/source_distribution-0.0.1.tar.gz" }

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```


---

_Comment by @kabouzeid on 2024-08-26 08:00_

When I add
```toml
[tool.hatch.metadata]
allow-direct-references = true
```
from your example, it always builds successfully no matter what, even with build isolation enabled.

I still could not find a single case where `--no-build-isolation-package` did have any effect at all.

---

_Comment by @tdejager on 2024-08-26 08:03_

@charliermarsh I made some similar changes in: https://github.com/astral-sh/uv/pull/6517 where I also found that the options were not passed around correctly. I think the changes are the same and I'll merge this PR in there. I think the other changes would still be relevant, I hope.

---

_Comment by @charliermarsh on 2024-08-26 12:23_

@kabouzeid -- Are you running from source? None of those changes are released yet. Given:

```toml
[project]
name = "foo"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "source-distribution @ https://files.pythonhosted.org/packages/10/1f/57aa4cce1b1abf6b433106676e15f9fa2c92ed2bd4cf77c3b50a9e9ac773/source_distribution-0.0.1.tar.gz"
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.hatch.metadata]
allow-direct-references = true
```

Here's my running the exact sequence of commands -- not sure how else to help!

```
❯ cargo run sync --no-build-isolation-package source-distribution
Using Python 3.12.1
Creating virtualenv at: .venv
Resolved 2 packages in 13ms
error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: source-distribution @ https://files.pythonhosted.org/packages/10/1f/57aa4cce1b1abf6b433106676e15f9fa2c92ed2bd4cf77c3b50a9e9ac773/source_distribution-0.0.1.tar.gz
  Caused by: Build backend failed to build wheel through `build_wheel()` with exit status: 1
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 8, in <module>
ModuleNotFoundError: No module named 'hatchling'
---

❯ cargo run sync
Resolved 2 packages in 7ms
   Built foo @ file:///Users/crmarsh/workspace/puffin/foo
   Built source-distribution @ https://files.pythonhosted.org/packages/10/1f/57aa4cce1b1abf6b433106676e15f9fa2c92ed2bd4cf77c3b50a9e9ac773/source_distribution-0.0.1.tar.gz
Prepared 2 packages in 400ms
Installed 2 packages in 1ms
 + foo==0.1.0 (from file:///Users/crmarsh/workspace/puffin/foo)
 + source-distribution==0.0.1 (from https://files.pythonhosted.org/packages/10/1f/57aa4cce1b1abf6b433106676e15f9fa2c92ed2bd4cf77c3b50a9e9ac773/source_distribution-0.0.1.tar.gz)
```



---

_Comment by @kabouzeid on 2024-08-26 13:36_

Yes from source. Commit is shown in uv --version of my console log above. I will check again later today.

---

_Comment by @charliermarsh on 2024-08-26 13:37_

Do you have the build cached?

---

_Comment by @kabouzeid on 2024-08-26 17:01_

No, I also tried with `export UV_NO_CACHE=1` during the whole time to be sure. I'm not sure we were talking about the same thing, because I don't understand the example.

However, I just tried, and it seems that https://github.com/astral-sh/uv/pull/6517 fixes my issue.

---

_Comment by @charliermarsh on 2024-08-26 17:18_

Ok. I'm very confused, because it's the exact same example that you provided in https://github.com/astral-sh/uv/pull/6605#issuecomment-2308946848. And that change should have absolutely no effect on the `source-distribution` package. Were you testing with a _different_ package?

---

_Comment by @kabouzeid on 2024-08-26 17:45_

I pulled that example out of the uv tests because I assumed something must be wrong with the tests as well, since they don't catch the bug. I tested with a few different packages, and also with `source-distribution`. I don't really know what source-distribution and allow-direct-references = true do. In the example I posted `--no-build-isolation-package` had no effect. Maybe you tried with a newer commit, because I got the same result as you with the newest commit `uv 0.3.3 (622053237 2024-08-26)`.

For the other packages (eg flash-attn and minkowskiengine), it works now with --no-build-isolation-package , while it had no effect before. This PR didn't fix the issue for me, but it did work with `uv 0.3.3 (622053237 2024-08-26)`.

---

_Comment by @charliermarsh on 2024-08-26 17:48_

Okay, thanks. It sounds like you _were_ testing with a different package then. (`source-distribution` is just a PyPI package that we publish to test source distributions, like `requests` or any other PyPI package.)

---
