```yaml
number: 16331
title: "Ability to reduce duplicated build between \"sync\" and \"build\" commands"
type: issue
state: closed
author: f3flight
labels:
  - question
assignees: []
created_at: 2025-10-16T22:05:28Z
updated_at: 2025-11-08T03:10:12Z
url: https://github.com/astral-sh/uv/issues/16331
synced_at: 2026-01-12T16:02:29Z
```

# Ability to reduce duplicated build between "sync" and "build" commands

---

_@f3flight_

### Summary

Suppose there's a Python project which has platform-dependent extensions, like some Rust code.
Suppose we want to build and publish wheels for it for all supported platforms and python versions. But we also do tests so we want to have a venv with this package installed.

Currently this is achieved via 2 commands, for each python version and on each OS:
```shell
uv sync ...
# tests here
uv build --wheel
# auditwheel call to add manylinux tags
```
Unfortunately, `uv sync` will build the package, and then `uv build` will also build the same package again, so the build time is doubled. The build time of this Rust code is significant (many minutes), so reducing duplicated build is important. I was not able to find a command to fetch the wheel built by `uv sync` from uv cache to skip `uv build`.

We also cannot workaround by building the wheel first because we want an "editable" install for the tests to make coverage work correctly, as opposed to the "production" wheel for publication.

Is there anything which could be improved here to avoid double building Rust extensions from source (which is done by `maturin`) while preserving editable state of the package installed during uv sync and having a normal wheel available as a file in the end?

I would envision a "uv cache get package==version" or similar to extract artifacts from cache, and then some command like "uv wheel-convert --to-normal" to convert an editable wheel into a normal wheel.

### Example

_No response_

---

_Label `enhancement` added by @f3flight on 2025-10-16 22:05_

---

_Renamed from "Ability to reduce duplicated wheel build between "sync" and "build" commands" to "Ability to reduce duplicated build between "sync" and "build" commands" by @f3flight on 2025-10-16 22:08_

---

_Comment by @konstin on 2025-10-17 08:24_

You can use `uv sync --no-install-project` and `uv pip install` the built wheel. You can also checkout this example for a different workflow with `uv venv`, `uv build` and `uv pip install` for publishing with a smoke test: https://docs.astral.sh/uv/guides/integration/github/#publishing-to-pypi

---

_Label `enhancement` removed by @konstin on 2025-10-17 08:24_

---

_Label `question` added by @konstin on 2025-10-17 08:24_

---

_Comment by @f3flight on 2025-10-17 16:57_

Thanks @konstin ! I don't think this solves the "editable" part though, right? If I need the install to be editable, I cannot install from a built wheel, or am I missing something?

---

_Comment by @konstin on 2025-10-19 09:10_

No, editables only have a transient format (which is technically a wheel but that should not be exposed). Can you use a shared `CARGO_TARGET_DIR` or something similar to use caching across both builds?

---

_Closed by @charliermarsh on 2025-11-08 03:10_

---
