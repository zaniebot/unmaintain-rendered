---
number: 13467
title: "Allow glob patterns in `[tool.uv.sources]` to simplify use of dynamic local wheels"
type: issue
state: closed
author: flitzpiepe93
labels:
  - enhancement
assignees: []
created_at: 2025-05-15T13:48:01Z
updated_at: 2025-05-15T16:15:46Z
url: https://github.com/astral-sh/uv/issues/13467
synced_at: 2026-01-10T01:25:34Z
---

# Allow glob patterns in `[tool.uv.sources]` to simplify use of dynamic local wheels

---

_Issue opened by @flitzpiepe93 on 2025-05-15 13:48_

## Summary

I'd like to request support for glob patterns (e.g. `*.whl`) in the `[tool.uv.sources]` section of `pyproject.toml`, to simplify development workflows involving dynamically versioned local wheels.

## Background

The [uv documentation](https://github.com/astral-sh/uv/blob/main/docs/docs/spec.md#uvsources) describes how local wheel files can be specified like this:

```toml
[project]
dependencies = ["my-package"]

[tool.uv.sources]
my-package = { path = "../dist/my_package-0.1.0-py3-none-any.whl" }
```

This works well when the filename is known and static. However, in some workflows (e.g. using [jsii](https://github.com/aws/jsii)), wheel filenames are generated dynamically and include a version or build hash – for example:

```
../dist/my_package-1.2.345-py3-none-any.whl
../dist/my_package-1.2.346-py3-none-any.whl
```

Keeping the filename in sync with each new build makes testing tedious.

## Proposal

It would be very helpful if `uv` could support simple glob patterns in the `path` of `[tool.uv.sources]`, e.g.:

```toml
[project]
dependencies = ["my-package"]

[tool.uv.sources]
my-package = { path = "../dist/my_package-*.whl" }
```

Expected behavior:

- If the pattern matches **exactly one** file: use it.
- If it matches **multiple files**: optionally pick the latest (e.g., by modification time), or fail with a clear error.
- If it matches **none**: fail with a clear error.

## Motivation

This would simplify local development workflows significantly, especially when wheel versions are managed by external tools or build systems and not easily predictable.

## Current Workaround

A shell glob works with `uv pip install`:

```bash
uv pip install ../dist/*.whl
```

But this cannot be expressed in `pyproject.toml` today, which limits support for fully reproducible local development environments via `uv`.

Thanks for your great work on uv!


### Example

_No response_

---

_Label `enhancement` added by @flitzpiepe93 on 2025-05-15 13:48_

---

_Comment by @konstin on 2025-05-15 14:06_

This is the use case for flat indexes (https://docs.astral.sh/uv/configuration/indexes/#flat-indexes): You have a directory of wheels, and uv picks the right one.

---

_Comment by @flitzpiepe93 on 2025-05-15 16:15_

Thanks for the help!

---

_Closed by @flitzpiepe93 on 2025-05-15 16:15_

---
