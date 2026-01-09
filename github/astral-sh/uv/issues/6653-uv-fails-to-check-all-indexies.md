---
number: 6653
title: uv fails to check all indexies
type: issue
state: closed
author: theunkn0wn1
labels:
  - duplicate
  - question
assignees: []
created_at: 2024-08-26T20:24:37Z
updated_at: 2024-08-26T23:31:46Z
url: https://github.com/astral-sh/uv/issues/6653
synced_at: 2026-01-07T13:12:17-06:00
---

# uv fails to check all indexies

---

_Issue opened by @theunkn0wn1 on 2024-08-26 20:24_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->


two issues compound here:
1. Uv checks the extra index url before the primary index url
2. Uv gives up if it finds *any* version in the extra index url, even if that version doesn't satisfy the constraints.

I have a situation where:
-  _really old_ versions of package `the-downstream` exists in the extra index url
- modern versions of the package exist in the primary index url
- When I try to add the `name-redacted` package, pinning it to a modern version, uv fails to resolve with an incorrect resolution.



### Global configuration
contents of `~/.config/uv/uv.conf`, partially redacted to protect the innocent.
```toml
index-url = "https://redacted_user:redacted@private-index/simple"
extra-index-url = [ "https://some-other-private-index/simple"]
```

### Executed command  (verbose)
package names, urls redacted to protect the innocent.

```
> $ uv add "the-downstream>=3.1.0"   -v       
DEBUG uv 0.3.3
DEBUG Found project root: `/tmp/something/some-package`
DEBUG No workspace root found, using project root
DEBUG The virtual environment's Python version satisfies `Python >=3.12`
DEBUG Using request timeout of 30s
DEBUG Using request timeout of 30s
DEBUG Starting clean resolution
DEBUG Acquired lock for `/home/name-redacted/.cache/uv/built-wheels-v3/editable/eae9d6d218105b40`
DEBUG No static `PKG-INFO` available for: some-package @ file:///tmp/something/some-package (MissingPkgInfo)
DEBUG Found static `pyproject.toml` for: some-package @ file:///tmp/something/some-package
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.12.5
DEBUG Solving with target Python version: >=3.12
DEBUG Adding direct dependency: some-package*
DEBUG Searching for a compatible version of some-package @ file:///tmp/something/some-package (*)
DEBUG Adding transitive dependency for some-package==0.1.0: the-downstream>=3.1.0
DEBUG Found stale response for: https://some-other-private-index/simple/the-downstream/
DEBUG Sending revalidation request for: https://some-other-private-index/simple/the-downstream/
DEBUG Found modified response for: https://some-other-private-index/simple/the-downstream/
DEBUG Searching for a compatible version of the-downstream (>=3.1.0)
DEBUG No compatible version found for: the-downstream
DEBUG Searching for a compatible version of some-package @ file:///tmp/something/some-package (<0.1.0 | >0.1.0)
DEBUG No compatible version found for: some-package
  × No solution found when resolving dependencies:
  ╰─▶ Because only the-downstream<=1.0.3 is available and your project depends on the-downstream>=3.1.0, we can conclude that your project's requirements are unsatisfiable.
  help: If this is intentional, run `uv add --frozen` to skip the lock and sync steps
```

### Additional info
`the-downstream>=3.1.0` is available in the primary index, and the index is accessible.
If i reconfigure the global configuration to swap `index-url` and `extra-index-url` with each other, the install succeeds.
```toml
extra-index-url = ["https://redacted_user:redacted@private-index.privatedomain/simple"]
index-url = "https://some-other-private-index.privatedomain/simple"
```

---

_Comment by @charliermarsh on 2024-08-26 20:28_

Take a look at this section in the docs: https://docs.astral.sh/uv/pip/compatibility/#packages-that-exist-on-multiple-indexes. You can set `--index-strategy unsafe-best-match` or `--index-strategy = unsafe-first-match`.

---

_Label `question` added by @charliermarsh on 2024-08-26 20:28_

---

_Label `duplicate` added by @zanieb on 2024-08-26 20:45_

---

_Comment by @theunkn0wn1 on 2024-08-26 23:26_

reading that documentation, UV behaves the exact opposite of how i expected.

with `poetry`, we set our internal private index as the `primary` index so it always checked it first.
we set our pypi mirror as a `secondary` so it had a fallback for nonlocal packages
we set public pypi as `explicit` so it would never reach out to public pypi.

But with UV the logic is reversed,
we set our internal private index as the secondary/extra, and our pypi mirror as primary.


---

_Comment by @charliermarsh on 2024-08-26 23:30_

Yes, that sounds right to me. We don't support "named" index priorities like that yet -- they will come in the future. For now, we're just leveraging the existing `--index-url` and `--extra-index-url` scheme that came from `uv pip`. With `--index-url` and `--extra-index-url`, it is far more common for users to prefer that packages are selected from the _extra_ index if possible. (E.g., it's very common to set the PyTorch index to `--extra-index-url`; if you did that, you would never want `torch` from PyPI to be preferred.)

---

_Closed by @charliermarsh on 2024-08-26 23:31_

---
