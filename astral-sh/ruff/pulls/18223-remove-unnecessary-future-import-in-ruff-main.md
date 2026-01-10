```yaml
number: 18223
title: "remove unnecessary `__future__` import in `ruff.__main__`"
type: pull_request
state: closed
author: jorenham
labels: []
assignees: []
base: main
head: remove-the-future
created_at: 2025-05-20T15:34:34Z
updated_at: 2025-05-20T15:54:16Z
url: https://github.com/astral-sh/ruff/pull/18223
synced_at: 2026-01-10T18:51:02Z
```

# remove unnecessary `__future__` import in `ruff.__main__`

---

_Pull request opened by @jorenham on 2025-05-20 15:34_

## Summary

The `__future__.annotations` import in `ruff.__main__` was not needed, as there are no forward references or type-check-only imports.

Instead, I manually stringified the `list[str]` annotation, for compatibility with older Python versions.

Since the `pyproject.toml` ruff config for `isort` requires `__future__.annotations` to be present everywhere, I assumed that there must be some reason for that, and did not change it. I instead used a local `.ruff.toml` to override it for the only the `ruff` python package.

## Test Plan

Check that `python -m ruff` does not raise.


---

_Comment by @MichaReiser on 2025-05-20 15:37_

Thank you.

The future annotation was introduced in https://github.com/astral-sh/ruff/pull/15090 to fix a runtime error. Nothing has changed to this file since then, so I think it's still needed

---

_Comment by @jorenham on 2025-05-20 15:42_

> Thank you.
> 
> The future annotation was introduced in #15090 to fix a runtime error. Nothing has changed to this file since then, so I think it's still needed

I can't see how a `from __future__ import annotations` would fix the `FileNotFoundError` reported in #15090 🤔 

---

_Comment by @jorenham on 2025-05-20 15:43_

But either way, this indeed won't work now on older Python versions. Let me fix that real quick.

**edit**: I believe it should also work with older Python versions now :)

---

_Comment by @MichaReiser on 2025-05-20 15:51_

I prefer keeping the future annotations because it protects against accidentially introducing backwards incompatible types and I don't see any harm to it.

---

_Closed by @MichaReiser on 2025-05-20 15:51_

---

_Comment by @jorenham on 2025-05-20 15:54_

> I don't see any harm to it.

Usually it's no problem. But it can make the life of runtime type-checkers more difficult, because it requires them to `eval` all annotations because of that. But in this case I doubt that it matters, so I can see how the additional complexity might not be worth it here. 

---

_Branch deleted on 2025-05-20 15:54_

---
