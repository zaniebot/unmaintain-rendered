---
number: 17044
title: Option to disable UV_FROZEN
type: issue
state: open
author: AndreuCodina
labels:
  - enhancement
assignees: []
created_at: 2025-12-09T09:10:42Z
updated_at: 2025-12-09T11:49:07Z
url: https://github.com/astral-sh/uv/issues/17044
synced_at: 2026-01-10T01:26:13Z
---

# Option to disable UV_FROZEN

---

_Issue opened by @AndreuCodina on 2025-12-09 09:10_

### Summary

When using uv in a CI, we always want to use `--no-frozen`. For example:

```bash
uv run --frozen -- ruff check
uv run --frozen -- ruff format --diff
uv run --frozen -- ty check
uv run --frozen -- pytest tests/unit
uv run --frozen -- pytest tests/integration
uv run --frozen -- pytest tests/ai
```

So I could add `UV_FROZEN=1` and remove all `--frozen`, but there's a case when I need `UV_FROZEN` to be unset because `--locked` and `--frozen` aren't compatible:

```bash
uv sync --locked
```

So, what do you think about adding `--no-frozen`? Doing this, we could have:

```bash
export UV_FROZEN=1

uv sync --locked --no-frozen

uv run -- ruff check
uv run -- ruff format --diff
uv run -- ty check
uv run -- pytest tests/unit
uv run -- pytest tests/integration
uv run -- pytest tests/ai
```

Another option would be to have the option to disable UV_FROZEN. For example:

```bash
export UV_FROZEN=1

UV_FROZEN=0 uv sync --locked

uv run -- ruff check
...
```

### Example

_No response_

---

_Label `enhancement` added by @AndreuCodina on 2025-12-09 09:10_

---

_Renamed from "Add --no-frozen" to "Option to disable UV_FROZEN" by @AndreuCodina on 2025-12-09 09:10_

---

_Comment by @VictorHuu on 2025-12-09 11:49_

If the maintainers consider the feature request in this issue necessary to address, I will proceed to submit a PR to implement the changes.

---
