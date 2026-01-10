---
number: 8601
title: "`uv sync` reports no solution found when `python_version>=` is used"
type: issue
state: closed
author: njzjz
labels:
  - bug
  - duplicate
  - resolver
assignees: []
created_at: 2024-10-27T01:41:35Z
updated_at: 2024-10-28T13:48:28Z
url: https://github.com/astral-sh/uv/issues/8601
synced_at: 2026-01-10T01:24:30Z
---

# `uv sync` reports no solution found when `python_version>=` is used

---

_Issue opened by @njzjz on 2024-10-27 01:41_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Minimal reproduced `pyproject.toml`:

```toml
[project]
name = "test-uv"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.9"
dependencies = [
    'jax>=0.4.33;python_version>="3.10"',
]
```

`uv sync`:

```
  × No solution found when resolving dependencies:
  ╰─▶ Because only the following versions of jax{python_full_version >= '3.10'} are available:
          jax{python_full_version >= '3.10'}<=0.4.33
          jax{python_full_version >= '3.10'}==0.4.34
          jax{python_full_version >= '3.10'}==0.4.35
      and the requested Python version (>=3.9) does not satisfy Python>=3.10, we can conclude that
      jax{python_full_version >= '3.10'}>=0.4.33 is incompatible.
      And because your project depends on jax{python_full_version >= '3.10'}>=0.4.33, we can conclude that your
      project's requirements are unsatisfiable.
```

Here, I expect Python 3.9 to install nothing and Python 3.10 and above to install `jax>=0.4.33`. I don't understand uv's logic.

(In the actual project, jax is an optional dependency for extra features that are only available in Python 3.10)

---

_Comment by @zanieb on 2024-10-27 01:54_

This is... a confusing error. Thanks for the report.

I don't even understand the part that says:

> only the following versions of jax{python_full_version >= '3.10'} are available:
>          jax{python_full_version >= '3.10'}<=0.4.33
 >         jax{python_full_version >= '3.10'}==0.4.34
>          jax{python_full_version >= '3.10'}==0.4.35

Like, why did we choose to enumerate a couple of those versions explicitly?

For debugging purposes... the derivation tree looks like

```
Resolver derivation tree before reduction
  root==0a0.dev0 depends on test-uv*
      test-uv==0.1.0 depends on jax{python_full_version >= '3.10'}>=0.4.33
        no versions of Python>=3.10
        no versions of jax{python_full_version >= '3.10'}>0.4.33, <0.4.34 | >0.4.34, <0.4.35 | >0.4.35
    no versions of test-uv<0.1.0 | >0.1.0
Resolver derivation tree after reduction
  test-uv==0.1.0 depends on jax{python_full_version >= '3.10'}>=0.4.33
    no versions of Python>=3.10
    no versions of jax{python_full_version >= '3.10'}>0.4.33, <0.4.34 | >0.4.34, <0.4.35 | >0.4.35
```

And the [`Requires-Python` of `jax==0.4.33`](https://inspector.pypi.io/project/jax/0.4.33/packages/49/48/0e32458ab7e02d75f423fe8c2ab10d7fa1aba9b314391d2659e68891912b/jax-0.4.33-py3-none-any.whl/jax-0.4.33.dist-info/METADATA#line.12) looks fine.

And the verbose logs are

```
❯ RUST_LOG=uv=trace uv lock -v
DEBUG uv 0.4.25 (69650e16c 2024-10-21)
TRACE Found `pyproject.toml` at: `/Users/zb/workspace/uv/pyproject.toml`
DEBUG Project is contained in non-workspace project: `/Users/zb/workspace/uv`
DEBUG Found workspace root: `/Users/zb/workspace/uv/example`
DEBUG Adding current workspace member: `/Users/zb/workspace/uv/example`
TRACE Cached interpreter info for Python 3.11.10, skipping probing: .venv/bin/python3
DEBUG The virtual environment's Python version satisfies `Python >=3.9`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: test-uv @ file:///Users/zb/workspace/uv/example
TRACE Found `pyproject.toml` at: `/Users/zb/workspace/uv/pyproject.toml`
DEBUG Project is contained in non-workspace project: `/Users/zb/workspace/uv`
DEBUG No workspace root found, using project root
TRACE Performing lookahead for test-uv @ file:///Users/zb/workspace/uv/example
DEBUG Solving with installed Python version: 3.11.10
DEBUG Solving with target Python version: >=3.9
DEBUG Adding direct dependency: test-uv*
DEBUG Searching for a compatible version of test-uv @ file:///Users/zb/workspace/uv/example (*)
DEBUG Adding transitive dependency for test-uv==0.1.0: jax{python_full_version >= '3.10'}>=0.4.33
TRACE Fetching metadata for jax from https://pypi.org/simple/jax/
TRACE cached request https://pypi.org/simple/jax/ is storable because its response has a 'public' cache-control directive
TRACE freshness lifetime found via cache-control max age setting: 600s
DEBUG Found fresh response for: https://pypi.org/simple/jax/
TRACE Received package metadata for: jax
DEBUG Searching for a compatible version of jax{python_full_version >= '3.10'} (>=0.4.33)
TRACE Selecting candidate for jax with range >=0.4.33 with 169 remote versions
TRACE found candidate for package PackageName("jax") with range Range { segments: [(Included("0.4.33"), Unbounded)] } after 1 steps: "0.4.35" version
DEBUG No compatible version found for: Python
DEBUG Searching for a compatible version of jax{python_full_version >= '3.10'} (>=0.4.33, <0.4.35 | >0.4.35)
TRACE Selecting candidate for jax with range >=0.4.33, <0.4.35 | >0.4.35 with 169 remote versions
TRACE found candidate for package PackageName("jax") with range Range { segments: [(Included("0.4.33"), Excluded("0.4.35")), (Excluded("0.4.35"), Unbounded)] } after 2 steps: "0.4.34" version
DEBUG Searching for a compatible version of jax{python_full_version >= '3.10'} (>=0.4.33, <0.4.34 | >0.4.34, <0.4.35 | >0.4.35)
TRACE Selecting candidate for jax with range >=0.4.33, <0.4.34 | >0.4.34, <0.4.35 | >0.4.35 with 169 remote versions
TRACE found candidate for package PackageName("jax") with range Range { segments: [(Included("0.4.33"), Excluded("0.4.34")), (Excluded("0.4.34"), Excluded("0.4.35")), (Excluded("0.4.35"), Unbounded)] } after 3 steps: "0.4.33" version
DEBUG Searching for a compatible version of jax{python_full_version >= '3.10'} (>0.4.33, <0.4.34 | >0.4.34, <0.4.35 | >0.4.35)
TRACE Selecting candidate for jax with range >0.4.33, <0.4.34 | >0.4.34, <0.4.35 | >0.4.35 with 169 remote versions
TRACE Exhausted all candidates for package jax with range >0.4.33, <0.4.34 | >0.4.34, <0.4.35 | >0.4.35 after 169 steps
DEBUG No compatible version found for: jax{python_full_version >= '3.10'}
DEBUG Searching for a compatible version of test-uv @ file:///Users/zb/workspace/uv/example (<0.1.0 | >0.1.0)
DEBUG No compatible version found for: test-uv
```

I think the resolver error is bogus because the resolution itself is wrong. This looks like a case where we do not fork correctly.

If you do:

```
dependencies = [
    'jax>=0.4.33;python_version>="3.10"',
    'jax>=0.4.33;python_version>="3.10"',
]
```

it'll force the resolver to fork here and resolution succeeds.

I presume this would be solved by https://github.com/astral-sh/uv/pull/6143

---

_Label `bug` added by @zanieb on 2024-10-27 01:55_

---

_Label `resolver` added by @zanieb on 2024-10-27 01:55_

---

_Referenced in [deepmodeling/deepmd-kit#4263](../../deepmodeling/deepmd-kit/pulls/4263.md) on 2024-10-27 05:41_

---

_Comment by @charliermarsh on 2024-10-27 18:53_

It's the same as https://github.com/astral-sh/uv/issues/4668.

---

_Closed by @charliermarsh on 2024-10-27 18:53_

---

_Label `duplicate` added by @zanieb on 2024-10-27 19:00_

---

_Comment by @charliermarsh on 2024-10-28 13:48_

This should "just work" without any workarounds in the next version (see: https://github.com/astral-sh/uv/pull/8628).

---
