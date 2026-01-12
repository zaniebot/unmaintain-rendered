```yaml
number: 11394
title: Warn for builds in non-build and workspace root pyproject.toml
type: pull_request
state: merged
author: konstin
labels:
  - error messages
assignees: []
merged: true
base: main
head: konsti/error-invalid-setuptools-build
created_at: 2025-02-10T18:55:03Z
updated_at: 2025-02-17T15:57:19Z
url: https://github.com/astral-sh/uv/pull/11394
synced_at: 2026-01-12T16:09:49Z
```

# Warn for builds in non-build and workspace root pyproject.toml

---

_@konstin_

When running `uv pip install .` in a directory with a pyproject.toml that does not configure a build, we will invoke setuptools and get a wheel we can't parse (https://github.com/astral-sh/uv/issues/11344). This PR adds warnings around these setups.

---

_Label `error messages` added by @konstin on 2025-02-10 18:55_

---

_Review comment by @charliermarsh on `crates/uv-build-frontend/src/lib.rs`:530 on 2025-02-10 21:32_

This looks like a repeated line.

---

_@charliermarsh reviewed on 2025-02-10 21:32_

---

_Comment by @charliermarsh on 2025-02-10 21:33_

What happens if a future version of `setuptools` respects a new dependency source, other than those enumerated here?

---

_Comment by @charliermarsh on 2025-02-10 21:35_

It seems a little sketchy to show these preemptively. Could we show them as a hint after the operation fails?

---

_Comment by @konstin on 2025-02-10 22:00_

> What happens if a future version of setuptools respects a new dependency source, other than those enumerated here?

If a user was to not declare `[build-system]` but add that new hypothetical non-PEP 621 metadata source, we would show a false positive error. I've opted for an error here matching the existing `Error::InvalidSourceDist` that errors if there are neither pyproject.toml nor setup.py, given that i consider it unlikely there will be a pre-PEP 517, post PEP 621 combination in the future, and given that a missing `[build-system]` is legacy behavior.

---

_Comment by @zanieb on 2025-02-10 22:12_

Just checking my understanding here... why does this work without error?

```
❯ mkdir example
❯ cd example
❯ touch pyproject.toml
❯ uvx pip install -e .
Installed 1 package in 6ms
Obtaining file:///Users/zb/workspace/uv/example
  Installing build dependencies ... done
  Checking if build backend supports build_editable ... done
  Getting requirements to build editable ... done
  Preparing editable metadata (pyproject.toml) ... done
Building wheels for collected packages: UNKNOWN
  Building editable for UNKNOWN (pyproject.toml) ... done
  Created wheel for UNKNOWN: filename=UNKNOWN-0.0.0-0.editable-py3-none-any.whl size=2564 sha256=9a02c4c29e39a2d66dd0ba0746adcdc16cfd2cf3c72cba21ae045d1b9d19d565
  Stored in directory: /private/var/folders/6p/k5sd5z7j31b31pq4lhn0l8d80000gn/T/pip-ephem-wheel-cache-q0_zs90o/wheels/53/b1/66/9fb19b1939bbe46f93d4fe49b431cb32e3425de7bda20e8c88
Successfully built UNKNOWN
Installing collected packages: UNKNOWN
Successfully installed UNKNOWN-0.0.0
```

Similarly, `uvx --from build pyproject-build` succeeds (as well as a subsequent `pip install` from the wheel).

Does that mean we'd be breaking compatibility with `pip` here? I can't tell from the summary if this changes `uv pip install` behavior or just `uv build`.

---

_Comment by @konstin on 2025-02-11 10:13_

Our current released behavior is that we don't support these cases, because we consider `UNKNOWN` to be unset, which results in the `Metadata field Name not found`. Apparently we do this already since f51432382ae3fce110afd8d7c2f88684078cbeb6, which we copied from python-pkginfo-rs, which doesn't document why it has this behavior.

```
$ mkdir a
$ cd a
$ touch pyproject.toml
$ uv pip install .
Using Python 3.13.0 environment at: /home/konsti/projects/uv/debug/foo/.venv
error: Failed to parse metadata from built wheel
  Caused by: Metadata field Name not found
$ uv pip install -e .
Using Python 3.13.0 environment at: /home/konsti/projects/uv/debug/foo/.venv
error: Failed to parse metadata from built wheel
  Caused by: Metadata field Name not found
```

I've tried to only touch the paths that were already failing and give them more actionable error message and catch the "`uv pip install .` in a workspace root case".

I know example of how to configure setuptools using setup.py, setup.cfg and pyproject.toml. It is not clear to me what uses cases setuptools intends to support outside of that, or what the correct way of handling `UNKNOWN` is.



---

_@charliermarsh reviewed on 2025-02-13 17:41_

---

_Review comment by @charliermarsh on `crates/uv-build-frontend/src/lib.rs`:542 on 2025-02-13 17:41_

So my concern here is... what if, in the future, setuptools supports something like `setup.toml`? Then existing versions of uv will fail here, overly-aggressively. That's partly why I was advocating that we do this "after-the-fact" (i.e., hint when we create an `UNKNOWN` wheel). How hard is that?


---

_Renamed from "Errors for builds in non-build and workspace root pyproject.toml" to "Warn for builds in non-build and workspace root pyproject.toml" by @konstin on 2025-02-17 14:14_

---

_Comment by @konstin on 2025-02-17 14:27_

Changed it to only warnings except for the no-pyproject.toml case that `pip` and `build` also reject.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-02-17 15:27_

---

_@charliermarsh approved on 2025-02-17 15:48_

---

_Merged by @charliermarsh on 2025-02-17 15:57_

---

_Closed by @charliermarsh on 2025-02-17 15:57_

---

_Branch deleted on 2025-02-17 15:57_

---
