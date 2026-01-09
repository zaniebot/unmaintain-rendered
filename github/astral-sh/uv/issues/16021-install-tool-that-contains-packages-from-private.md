---
number: 16021
title: Install tool that contains packages from private index
type: issue
state: closed
author: MauroPfister
labels:
  - question
assignees: []
created_at: 2025-09-25T07:41:27Z
updated_at: 2025-09-26T09:03:04Z
url: https://github.com/astral-sh/uv/issues/16021
synced_at: 2026-01-07T13:12:19-06:00
---

# Install tool that contains packages from private index

---

_Issue opened by @MauroPfister on 2025-09-25 07:41_

### Question

Hello

I have created a package `example-app` that contains a package `foo` from a private Google Artifact Registry (see `pyproject.toml` below). 

```toml
[project]
name = "example-app"
dynamic = ["version"]
description = "Example application"
readme = "README.md"
requires-python = ">=3.10"
dependencies = [
    "foo >= 3.0.0",
]

[project.scripts]
example-app = "example_app:main"

[tool.uv]
keyring-provider = "subprocess"

[tool.uv.sources]
# Fetch foo from our private google cloud platform index
foo = { index = "gcp-index" }

[[tool.uv.index]]
name = "gcp-index"
url = "https://oauth2accesstoken@###-python.pkg.dev/###/###/simple/" 
explicit = true

[build-system]
requires = ["hatchling", "uv-dynamic-versioning"]
build-backend = "hatchling.build"

[tool.hatch.version]
source = "uv-dynamic-versioning"

[tool.hatch.metadata]
allow-direct-references = true

[tool.uv-dynamic-versioning]
pattern-prefix = "example-app-"
```

Running the entry points works fine using `uv run example-app`. I can see that `uv` gets the `foo` dependency from our private index.

However, when I try to install the package `example-app` as a tool with `uv tool install -e .` I get the following error:
```
  × No solution found when resolving dependencies:
  ╰─▶ Because foo was not found in the package registry and example-app==1.0.1 depends on foo>=3.0.0, we can conclude that example-app==1.0.1 cannot be used.
      And because only example-app==1.0.1 is available and you require example-app, we can conclude that your requirements are unsatisfiable.
```

Do I need to configure something differently in `pyproject.toml`? I think the secondary error (only example-app=1.0.1 is available) has nothing to do with the first one but please correct me if I'm wrong.

EDIT: When I run `uv tool install -e . --verbose` I see that `uv` can't find the credentials for the private index (I obfuscated the index url):
```
DEBUG uv 0.8.16
DEBUG Searching for default Python interpreter in managed installations or search path
DEBUG Searching for managed installations at `/home/mauro/.local/share/uv/python`
DEBUG Found managed installation `cpython-3.13.0-linux-x86_64-gnu`
DEBUG Found `cpython-3.13.0-linux-x86_64-gnu` at `/home/mauro/.local/share/uv/python/cpython-3.13.0-linux-x86_64-gnu/bin/python3.13` (managed installations)
DEBUG Using request timeout of 30s
DEBUG Found PEP 621 metadata for /home/mauro/Develop/example-app in `pyproject.toml` (example-app)
DEBUG Acquired lock for `/home/mauro/.local/share/uv/tools`
DEBUG Checking for Python environment at: `/home/mauro/.local/share/uv/tools/example-app`
DEBUG Using request timeout of 30s
DEBUG No static `pyproject.toml` available for: example-app @ file:///home/mauro/Develop/example-app (DynamicField("version"))
DEBUG Acquired lock for `/home/mauro/.cache/uv/sdists-v9/editable/356415a6d63dfcbf`
DEBUG Computed cache info: Some(Timestamp(SystemTime { tv_sec: 1758731046, tv_nsec: 66537989 })), None, None, {}, {"src": Some(Timestamp(Timestamp(SystemTime { tv_sec: 1758703684, tv_nsec: 500251165 })))}
DEBUG Using cached metadata for: example-app @ file:///home/mauro/Develop/example-app
DEBUG No workspace root found, using project root
DEBUG Released lock at `/home/mauro/.cache/uv/sdists-v9/editable/356415a6d63dfcbf/.lock`
DEBUG Solving with installed Python version: 3.13
DEBUG Solving with target Python version: >=3.13
DEBUG Adding direct dependency: example-app*
DEBUG Searching for a compatible version of example-app @ file:///home/mauro/Develop/example-app (*)
DEBUG Adding transitive dependency for example-app==1.0.1: foo>=3.0.0
DEBUG Found stale response for: https://###-python.pkg.dev/###/###/simple/foo/
DEBUG Sending revalidation request for: https://###-python.pkg.dev/###/###/simple/foo/
DEBUG No netrc file found
DEBUG Acquired lock for `credentials store`
DEBUG Released lock at `/home/mauro/.local/share/uv/credentials/credentials.toml.lock`
WARN Failed to load credentials from /home/mauro/.local/share/uv/credentials/credentials.toml: Failed to parse TOML credential file: TOML parse error at line 1, column 1
  |
1 | [[credential]]
  | ^^^^^^^^^^^^^^
missing field `service`

DEBUG Indexes search failed because of status code failure: 401 Unauthorized
DEBUG Searching for a compatible version of foo (>=3.0.0)
DEBUG No compatible version found for: foo
DEBUG Recording unit propagation conflict of foo from incompatibility of (example-app)
DEBUG Searching for a compatible version of example-app @ file:///home/mauro/Develop/example-app (<1.0.1 | >1.0.1)
DEBUG No compatible version found for: example-app
  × No solution found when resolving dependencies:
  ╰─▶ Because foo was not found in the package registry and example-app==1.0.1 depends on foo>=3.0.0, we can conclude that example-app==1.0.1 cannot be used.
      And because only example-app==1.0.1 is available and you require example-app, we can conclude that your requirements are unsatisfiable.
DEBUG Released lock at `/home/mauro/.local/share/uv/tools/.lock`
```
However, when running `uv run --verbose example-app` I can see that after trying to load credentials from all the other places, `uv` gets them successfully from `keyring`.

Is this expected behavior?

### Platform

Ubuntu 24.04 x86_64

### Version

uv 0.8.16

---

_Label `question` added by @MauroPfister on 2025-09-25 07:41_

---

_Comment by @MauroPfister on 2025-09-25 09:41_

I just checked the [documentation](https://docs.astral.sh/uv/concepts/authentication/http/#keyring-providers) again and noticed that I can pass the keyring-provider via argument to `uv tool`. This works:

```
uv tool install -e . --verbose --keyring-provider subprocess
```

I don't understand why `uv tool` doesn't respect `tool.uv.keyring-provider = "subprocess"` .

---

_Comment by @zanieb on 2025-09-25 13:34_

`uv tool` doesn't read your `pyproject.toml`, it'll read user-level settings though.

---

_Comment by @MauroPfister on 2025-09-25 13:37_

Oh, then I probably misunderstood how `uv tool`  works. How does it determine what dependencies to install?

---

_Comment by @zanieb on 2025-09-25 13:39_

Sorry, if you do `uv tool install .` it will read the `pyproject.toml` but as-if it was a package external to you, e.g., it's similar to if you do `uv tool install foo` and `foo` needs to be built from source. In that context, we do not read `[tool.uv]` options from the project, since that's not a part of the standard Python package interface.

---

_Comment by @MauroPfister on 2025-09-26 09:03_

Thanks for the clarification, this makes sense now. The [docs](https://docs.astral.sh/uv/concepts/configuration-files/#configuration-files) clearly explain this behavior, I just didn't find it.

My solution is to configure this at user level:
```toml
# ~/.config/uv/uv.toml

keyring-provider = "subprocess"
```

Thanks again @zanieb for the quick support!


---

_Closed by @MauroPfister on 2025-09-26 09:03_

---
