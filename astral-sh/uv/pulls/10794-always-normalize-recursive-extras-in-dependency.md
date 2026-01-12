```yaml
number: 10794
title: Always normalize recursive extras in dependency metadata
type: pull_request
state: closed
author: charliermarsh
labels:
  - bug
  - no-build
assignees: []
draft: true
base: main
head: charlie/flatten-extras
created_at: 2025-01-20T22:57:21Z
updated_at: 2025-01-21T21:08:24Z
url: https://github.com/astral-sh/uv/pull/10794
synced_at: 2026-01-12T16:09:29Z
```

# Always normalize recursive extras in dependency metadata

---

_@charliermarsh_

## Summary

Some build backends flattten recursive extras, while others do not (see: https://discuss.python.org/t/core-metadata-for-self-referential-extras/77793/6). If a build backend _does_ flatten its recursive extras, then if we read requirements from the `pyproject.toml` directly, and compare those requirements to the build backend-returned metadata (as is the case for, e.g., dynamic versions), the comparison will fail.

Instead, the `DistributionDatabase` now always returns metadata with these recursive extras normalized out.

Closes https://github.com/astral-sh/uv/issues/10776.


---

_Label `bug` added by @charliermarsh on 2025-01-20 22:59_

---

_Label `no-build` added by @charliermarsh on 2025-01-20 23:12_

---

_@charliermarsh reviewed on 2025-01-20 23:31_

---

_Review comment by @charliermarsh on `crates/uv-pypi-types/src/metadata/requires_dist.rs`:224 on 2025-01-20 23:31_

This is pretty weird, but we have a lot of lockfile tests (`lock_self_*`) that test cases in which a package depends on itself, and puts a version specifier on that dependency. I don't think we can drop those.

For example:

```toml
[project]
name = "project"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = ["typing-extensions", "project==0.1.0"]
```


---

_Review requested from @zanieb by @charliermarsh on 2025-01-20 23:35_

---

_Review requested from @konstin by @charliermarsh on 2025-01-20 23:35_

---

_@charliermarsh reviewed on 2025-01-20 23:38_

---

_Review comment by @charliermarsh on `crates/uv-pypi-types/src/metadata/requires_dist.rs`:224 on 2025-01-20 23:38_

I'm not fully comfortable with this whole situation, self-dependencies seem to be poorly specified right now...

For example, Hatchling drops the `==0.2.0` in the `foo` extra here:

```toml
[project]
name = "project"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = []

[project.optional-dependencies]
foo = ["project[bar]==0.2.0"]
bar = ["iniconfig"]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

---

_Added to milestone `v0.6.0` by @charliermarsh on 2025-01-21 00:06_

---

_Comment by @charliermarsh on 2025-01-21 00:06_

I might make this part of v0.6, and do a smaller fix for the linked issue.

---

_@konstin reviewed on 2025-01-21 07:50_

---

_Review comment by @konstin on `crates/uv-pypi-types/src/metadata/requires_dist.rs`:224 on 2025-01-21 07:50_

I tried but couldn't reproduce this:

```toml
[project]
name = "hatch-drops-self-specifier"
version = "0.1.0"

[project.optional-dependencies]
foo = ["project[bar]==0.2.0"]
bar = ["iniconfig"]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

```shell
rm -rf venv
uv venv
uv build
uv pip install --find-links dist hatch-drops-self-specifier --no-cache
cat .venv/lib/python3.*/site-packages/hatch_drops_self_specifier-0.1.0.dist-info/METADATA
```

```
Metadata-Version: 2.4
Name: hatch-drops-self-specifier
Version: 0.1.0
Provides-Extra: bar
Requires-Dist: iniconfig; extra == 'bar'
Provides-Extra: foo
Requires-Dist: project[bar]==0.2.0; extra == 'foo'
```


---

_Comment by @konstin on 2025-01-21 08:26_

(I'll wait with reviewing on how https://discuss.python.org/t/core-metadata-for-self-referential-extras/77793 goes, maybe we don't need to do this because we agree on not flattening)

---

_Comment by @charliermarsh on 2025-01-21 13:17_

That conversation is sort of irrelevant for solving https://github.com/astral-sh/uv/issues/10776 though. The behavior already exists.

---

_Converted to draft by @charliermarsh on 2025-01-21 18:06_

---

_Comment by @charliermarsh on 2025-01-21 21:08_

Let's just skip this for now.

---

_Closed by @charliermarsh on 2025-01-21 21:08_

---
