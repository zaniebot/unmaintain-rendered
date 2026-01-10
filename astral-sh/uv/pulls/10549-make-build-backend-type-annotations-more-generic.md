```yaml
number: 10549
title: Make build backend type annotations more generic
type: pull_request
state: merged
author: cthoyt
labels: []
assignees: []
merged: true
base: main
head: more-typing
created_at: 2025-01-13T00:26:44Z
updated_at: 2025-01-13T18:39:05Z
url: https://github.com/astral-sh/uv/pull/10549
synced_at: 2026-01-10T11:44:57Z
```

# Make build backend type annotations more generic

---

_Pull request opened by @cthoyt on 2025-01-13 00:26_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

As a follow-up to https://github.com/astral-sh/uv/pull/10434, this PR makes the dictionary type annotations more generic with the `typing.Mapping` abstract class. 

In theory, this allows `typing.TypedDict` to be passed. However, note that in practice, the uv build backend is set up to reject if anything is actually passed to this optional parameter 😄 

Suggested in https://github.com/astral-sh/uv/pull/10434#issuecomment-2584357031


## Test Plan

- `uvx ruff check --select ANN python/` checks typing is complete
- `uvx mypy --strict python/` checks typing is consistent

---

_Renamed from "Update build backend type annotations" to "Make build backend type annotations more generic" by @cthoyt on 2025-01-13 00:28_

---

_Review comment by @jorenham on `python/uv/_build_backend.py`:31 on 2025-01-13 05:04_

`list` is invariant, so it `list[str]` won't accept `list[LiteralString]` or other lists whose value type is strict subtype of `str`.

Repro:
https://basedpyright.com/?typeCheckingMode=all&code=GYJw9gtgBALgngBwJYDsDmUkQWEMoAySMApiAIYA2AyjCKmgLABQLLAJicFMABQD6ALiiUkAZxgBtCSAC6AGigB6AJRQAtAD4oAOTAoSwgHQm2zSmOGiJkoqQo06DWVAC8USQCJPslnwsqZkA

So if this wasn't intended, then you could use `collections.abc.Sequence`, which has a covariant value type parameter.

---

_Review comment by @jorenham on `python/uv/_build_backend.py`:21 on 2025-01-13 05:11_

`typing.Mapping` is deprecated since python 3.9, so it's recommended to import it from `collections.abc`: https://docs.python.org/3/library/typing.html#typing.Mapping

And it apparently already works on Python 3.8:
https://basedpyright.com/?pythonVersion=3.8&typeCheckingMode=all&code=GYJw9gtgBAxmA28CmMAuBLMA7AzgOgEMAjGKdCABzBFSgFkCKL0sBzAWACguCAuexszYBtHKhAAaKGJABdKAF4oAbwC%2BXIA

---

_@jorenham reviewed on 2025-01-13 05:11_

---

_@cthoyt reviewed on 2025-01-13 12:27_

---

_Review comment by @cthoyt on `python/uv/_build_backend.py`:21 on 2025-01-13 12:27_

I also checked on PyPI, and the uv python package only supports py38+, so this is doable. Updated in 808eb2d8d.

---

_@cthoyt reviewed on 2025-01-13 12:32_

---

_Review comment by @cthoyt on `python/uv/_build_backend.py`:31 on 2025-01-13 12:32_

done in 7250bdbbd

---

_@jorenham approved on 2025-01-13 13:31_

---

_Comment by @cthoyt on 2025-01-13 13:45_

ping @konstin  :)

---

_@konstin approved on 2025-01-13 18:34_

thanks for following up!

---

_Merged by @konstin on 2025-01-13 18:34_

---

_Closed by @konstin on 2025-01-13 18:34_

---

_Branch deleted on 2025-01-13 18:39_

---
