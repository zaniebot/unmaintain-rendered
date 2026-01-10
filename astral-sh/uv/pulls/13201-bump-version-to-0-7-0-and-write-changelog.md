```yaml
number: 13201
title: Bump version to 0.7.0 and write changelog
type: pull_request
state: merged
author: zanieb
labels:
  - no-build
assignees: []
merged: true
base: release/070
head: zb/changelog-07
created_at: 2025-04-29T19:17:28Z
updated_at: 2025-04-29T20:31:48Z
url: https://github.com/astral-sh/uv/pull/13201
synced_at: 2026-01-10T11:10:41Z
```

# Bump version to 0.7.0 and write changelog

---

_Pull request opened by @zanieb on 2025-04-29 19:17_

The changelog diff is deranged. Rendered at https://github.com/astral-sh/uv/blob/zb/changelog-07/CHANGELOG.md#070

---

_Label `no-build` added by @zanieb on 2025-04-29 19:37_

---

_@ntBre reviewed on 2025-04-29 19:44_

---

_Review comment by @ntBre on `CHANGELOG.md`:63 on 2025-04-29 19:44_

```suggestion
  Previously, `uvx` would attempt to execute a command even if it was not provided by a Python package. For example, if we presume `foo` is an empty Python package which provides no command, `uvx foo` would invoke the `foo` command on the `PATH` (if present). Now, uv will error early if the `foo` executable is not provided by the requested Python package. This check is not enforced when `--from` is used, so patterns like `uvx --from foo bash -c "..."` are still valid. uv also still allows `uvx foo` where the `foo` executable is provided by a dependency of `foo` instead of `foo` itself, as this is fairly common for packages which depend on a dedicated package for their command-line interface.
```

---

_Review comment by @konstin on `CHANGELOG.md`:10 on 2025-04-29 19:47_

The sentence is a bit hard to parse

```suggestion
- **Update `uv version` to show or change the project version instead of showing the uv version ([#12349](https://github.com/astral-sh/uv/pull/12349))**
```

---

_Review comment by @konstin on `crates/uv/tests/it/build_backend.rs`:304 on 2025-04-29 19:52_

```suggestion
        requires = ["uv_build>=0.7,<0.8"]
```

---

_Review comment by @konstin on `crates/uv/tests/it/build_backend.rs`:390 on 2025-04-29 19:52_

```suggestion
        requires = ["uv_build>=0.7,<0.8"]
```

---

_Review comment by @konstin on `crates/uv/tests/it/build_backend.rs`:452 on 2025-04-29 19:52_

```suggestion
        requires = ["uv_build>=0.7,<0.8"]
```

---

_@konstin reviewed on 2025-04-29 19:53_

left some drive-by comments

---

_@konstin reviewed on 2025-04-29 19:54_

---

_Review comment by @konstin on `crates/uv/tests/it/build_backend.rs`:304 on 2025-04-29 19:54_

We could also do `<10000` here, the no-upper-bound warning is silenced as long as there is any upper bound at all-

---

_@charliermarsh reviewed on 2025-04-29 19:55_

---

_Review comment by @charliermarsh on `CHANGELOG.md`:12 on 2025-04-29 19:55_

```suggestion
  Previously, `uv version` displayed uv's version. Now, `uv version` will display or update the project's version. This interface was [heavily requested](https://github.com/astral-sh/uv/issues/6298) and, after much consideration, we decided that transitioning the top-level command was the best option.
```

---

_@charliermarsh reviewed on 2025-04-29 19:55_

---

_Review comment by @charliermarsh on `CHANGELOG.md`:46 on 2025-04-29 19:55_

Include code example?

---

_@charliermarsh reviewed on 2025-04-29 19:56_

---

_Review comment by @charliermarsh on `CHANGELOG.md`:59 on 2025-04-29 19:56_

```suggestion
  Since PyTorch's indexes always return a HTTP 403 for missing packages, uv special-cases indexes on the `pytorch.org` domain to ignore that error code by default.
```

---

_@zanieb reviewed on 2025-04-29 19:56_

---

_Review comment by @zanieb on `crates/uv/tests/it/build_backend.rs`:304 on 2025-04-29 19:56_

Does this matter?

As an aside, updating these manually is really a bummer. It'd be great to template these in the test suite based on the current version.

---

_@zanieb reviewed on 2025-04-29 19:57_

---

_Review comment by @zanieb on `CHANGELOG.md`:46 on 2025-04-29 19:57_

It seems redundant / not worth the space — I removed it after having it initially.

---

_@charliermarsh reviewed on 2025-04-29 19:57_

---

_Review comment by @charliermarsh on `CHANGELOG.md`:67 on 2025-04-29 19:57_

```suggestion
  When determining credentials for querying a package URL, uv previously sent the full URL to the `keyring` command. However, some keyring plugins expect to receive the _index URL_ (which is usually a parent of the package URL). Now, uv requests credentials for the index URL instead. This behavior matches `pip`.
```

---

_@charliermarsh reviewed on 2025-04-29 19:57_

---

_Review comment by @charliermarsh on `CHANGELOG.md`:75 on 2025-04-29 19:57_

```suggestion
  Python 3.7 is EOL and not formally supported by uv; however, Python 3.7 was previously available for download on a subset of platforms.
```

---

_Review comment by @zanieb on `CHANGELOG.md`:10 on 2025-04-29 19:57_

```suggestion
- **Update `uv version` to display and update project versions ([#12349](https://github.com/astral-sh/uv/pull/12349))**
```

---

_@zanieb reviewed on 2025-04-29 19:57_

---

_@charliermarsh approved on 2025-04-29 19:58_

---

_@zanieb reviewed on 2025-04-29 19:58_

---

_Review comment by @zanieb on `crates/uv/tests/it/build_backend.rs`:304 on 2025-04-29 19:58_

Ah that's why there's a bound? I see

---

_Merged by @zanieb on 2025-04-29 20:31_

---

_Closed by @zanieb on 2025-04-29 20:31_

---

_Branch deleted on 2025-04-29 20:31_

---
