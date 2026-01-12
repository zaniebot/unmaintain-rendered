```yaml
number: 12880
title: "Is there a way to enforce that `uv sync` and `uv run` only use `uv`-installed Python?"
type: issue
state: closed
author: johnthagen
labels:
  - question
assignees: []
created_at: 2025-04-14T14:16:38Z
updated_at: 2025-04-14T14:20:40Z
url: https://github.com/astral-sh/uv/issues/12880
synced_at: 2026-01-12T16:01:14Z
```

# Is there a way to enforce that `uv sync` and `uv run` only use `uv`-installed Python?

---

_@johnthagen_

### Question

Sometimes locally or in CI we want to enforce that `uv sync` or `uv run` _only_ use a `uv`-installed Python interpreter and not accidentally pick up a system interpretter or something else that might otherwise be in the `PATH`. This could be within CI or a container where Python is present by default, but we do not want to use it accidentally.

For example, in one instance we want to ensure that we use a `python-build-standalone` interpretter with a much lower minimum glibc requirement than most default Python versions provided in containers.

One concern could be having some steps in our CI like:

```yaml
    - uv python install 3.12
    - uv sync --locked
    - uv run ...
```

But if the underlying application changes its `requires-python` to 3.13, `uv sync` could fallback silently to 3.13 interpretter that exists that we didn't intend for it to use.

### Platform

macOS 14 arm64

### Version

uv 0.6.14 (a4cec56dc 2025-04-09)

---

_Label `question` added by @johnthagen on 2025-04-14 14:16_

---

_Comment by @konstin on 2025-04-14 14:19_

You can use the `--managed-python` option to enforce uv managed python installations.

---

_Comment by @johnthagen on 2025-04-14 14:20_

Perfect, thank you! Closing this question.

---

_Closed by @johnthagen on 2025-04-14 14:20_

---
