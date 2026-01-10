```yaml
number: 12141
title: "Can't resolve dependencies with git dependency"
type: issue
state: open
author: adriangb
labels:
  - needs-decision
assignees: []
created_at: 2025-03-12T18:48:39Z
updated_at: 2025-03-12T22:46:08Z
url: https://github.com/astral-sh/uv/issues/12141
synced_at: 2026-01-10T03:50:31Z
```

# Can't resolve dependencies with git dependency

---

_Issue opened by @adriangb on 2025-03-12 18:48_

### Summary

I am getting at least a confusing error message if not a bug with the following dependencies:

```toml
[project]
name = "test"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.11"
dependencies = [
  "opentelemetry-api==1.31.0",
  "opentelemetry-instrumentation-asgi@git+https://github.com/open-telemetry/opentelemetry-python-contrib.git@v0.52b0#subdirectory=instrumentation/opentelemetry-instrumentation-asgi",
]
```

```
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of opentelemetry-api==1.31.0 and your project depends on opentelemetry-api==1.31.0, we can conclude that your project's requirements are unsatisfiable.
```

I find this error message rather confusing: clearly `opentelemetry-api==1.31.0` _does_ exist, but it must be getting filtered out. Saying it doesn't exist is confusing.

Note that I pointed it specifically to the rev `v0.52b0` which is the rev corresponding to the version released on PyPi: https://github.com/open-telemetry/opentelemetry-python-contrib/releases/tag/v0.52b0

Without the git dependency resolution works:

```toml
[project]
name = "test"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.11"
dependencies = [
  "opentelemetry-api==1.31.0",
  "opentelemetry-instrumentation-asgi==0.52b0",  # should be the same as the version in git
]
```

### Platform

macOS Darwin 24.3.0 arm64

### Version

uv 0.6.6 (Homebrew 2025-03-12)

### Python version

3.12

---

_Label `bug` added by @adriangb on 2025-03-12 18:48_

---

_Comment by @charliermarsh on 2025-03-12 21:11_

I think this is because `https://github.com/open-telemetry/opentelemetry-python-contrib` defines its own uv sources, and we respect sources on Git dependencies:

```
[tool.uv.sources]
opentelemetry-api = { git = "https://github.com/open-telemetry/opentelemetry-python", branch = "main", subdirectory = "opentelemetry-api" }
```

So now you have a dependency on two conflicting versions of `opentelemetry-api`. You could run with `--no-sources` to ignore those sources?

(I'm not sure why the error message is so lacking, though.)

---

_Comment by @adriangb on 2025-03-12 22:37_

Ahh makes sense thank you for explaining. Indeed with `--no-sources` resolution works 😄. Is opentelemetry doing something weird here? Or Am I doing something unreasonable? To me it would seem reasonable to want to test one off these instrumentation packages off of git. I don't care where the `opentelemetry-api` and `opentelemetry-sdk` come from but I obviously need them installed for things to work. I came here from https://github.com/open-telemetry/opentelemetry-python-contrib/pull/3012#issuecomment-2718182084 btw, I find the inline deps make for an amazing MRE tool 🚀 

---

_Comment by @charliermarsh on 2025-03-12 22:46_

I need to give it a little thought... I think opentelemetry and you are both doing something reasonable, though the combination of behaviors here is pretty confusing in the end.

---

_Label `bug` removed by @charliermarsh on 2025-03-12 22:46_

---

_Label `needs-decision` added by @charliermarsh on 2025-03-12 22:46_

---
