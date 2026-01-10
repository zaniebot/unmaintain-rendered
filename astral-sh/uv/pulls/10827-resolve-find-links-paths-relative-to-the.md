```yaml
number: 10827
title: "Resolve `find-links` paths relative to the configuration file"
type: pull_request
state: merged
author: NightRa
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-relative-find-links
created_at: 2025-01-21T21:34:36Z
updated_at: 2025-01-22T21:16:44Z
url: https://github.com/astral-sh/uv/pull/10827
synced_at: 2026-01-10T11:45:13Z
```

# Resolve `find-links` paths relative to the configuration file

---

_Pull request opened by @NightRa on 2025-01-21 21:34_

## One-liner
Relative find-links configuration to local path from a pyproject.toml or uv.toml is now relative to the config file
## Summary
### Background
One can configure find-links in a `pyproject.toml` or `uv.toml` file, which are located from the cli arg, system directory, user directory, or by traversing parent directories until one is encountered.

This PR addresses the following scenario:
- A project directory which includes a `pyproject.toml` or `uv.toml` file
- The config file includes a `find-links` option. (eg under `[tool.uv]` for `pyproject.toml`)
- The `find-links` option is configured to point to a local subdirectory in the project: `packages/`
- There is a subdirectory called `subdir`, which is the current working directory
- I run `uv run my_script.py`. This will locate the `pyproject.toml` in the parent directory
### Current Behavior
- uv tries to use the path `subdir/packages/` to find packages, and fails.
### New Behavior
- uv tries to use the path `packages/` to find the packages, and succeeds
- Specifically, any relative local find-links path will resolve to be relative to the configuration file.

### Why is this behavior change OK?
- I believe no one depends on the behavior that a relative find-links when running in a subdir will refer to different directories each time
- Thus this change only allows a more common use case which didn't work previously.

## Test Plan
- I re-created the setup mentioned above:
```
UvTest/
├── packages/
│   ├── colorama-0.4.6-py2.py3-none-any.whl
│   └── tqdm-4.67.1-py3-none-any.whl
├── subdir/
│   └── my_script.py
└── pyproject.toml
```
```toml 
# pyproject.toml
[project]
name = "uvtest"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "tqdm>=4.67.1",
]

[tool.uv]
offline = true
no-index = true
find-links = ["packages/"]
```
- With working directory under `subdir`, previously, running `uv sync --offline` would fail resolving the tdqm package, and after the change it succeeds.
- Additionally, one can use `uv sync --show-settings` to show the actually-resolved settings - now having the desired path in `flat_index.url.path`

## Alternative designs considered
- I considered modifying the `impl Deserialize for IndexUrl` to parse ahead of time directly with a base directory by having a custom `Deserializer` with a base dir field, but it seems to contradict the design of the serde `Deserialize` trait - which should work with all `Deserializer`s

## Future work
- Support for adjusting all other local-relative paths in `Options` would be desired, but is out of scope for the current PR.

---

_Review requested from @charliermarsh by @zanieb on 2025-01-21 21:35_

---

_Assigned to @charliermarsh by @zanieb on 2025-01-21 21:35_

---

_Comment by @charliermarsh on 2025-01-21 21:53_

Thanks. It would be useful to include a test here (that fails on `main` but passes on this branch) to guard against regressions (and make sure we understand the problematic scenario).

---

_Comment by @NightRa on 2025-01-21 21:57_

@charliermarsh 
- A test corresponding to the scenario I described would need to have a whl file for some package.
- Which minimal test package would you consider appropriate for this?
- This would also imply it needs to be checked in the uv repository
- Where would be an appropriate location in the repository to hold this whl file for the test?

---

_Comment by @charliermarsh on 2025-01-21 21:59_

We already have a few checked-in that you can use in `./scripts/links`. Grep around for tests that use `scripts/links` for examples?

---

_Comment by @NightRa on 2025-01-21 22:32_

@charliermarsh 
Done. 
Also checked the test fails on master and succeeds here.
Previously it would fail with:
```
error: Failed to read `--find-links` directory: C:\Users\...\AppData\Roaming\uv\tests\.tmpS7JaZu\temp\subdir\packages
  Caused by: The system cannot find the path specified. (os error 3)
```

---

_Comment by @NightRa on 2025-01-22 08:07_

@charliermarsh Anything else required to merge here?

---

_Comment by @charliermarsh on 2025-01-22 18:06_

No, I just need time to review the changes.

---

_Comment by @NightRa on 2025-01-22 18:08_

Great, thanks for your time! 

---

_Label `bug` added by @charliermarsh on 2025-01-22 19:34_

---

_@charliermarsh approved on 2025-01-22 19:34_

---

_Merged by @charliermarsh on 2025-01-22 19:44_

---

_Closed by @charliermarsh on 2025-01-22 19:44_

---

_Renamed from "[uv-settings]: Correct behavior for relative find-links paths when run from a subdir " to "Resolve `find-links` paths relative to the configuration file" by @zanieb on 2025-01-22 19:55_

---

_Comment by @NightRa on 2025-01-22 21:09_

Indeed wasn't sure about the &mut. Thanks for the fixes
Love the quick support. Would recommend. 10/10

---

_Comment by @charliermarsh on 2025-01-22 21:16_

Yeah the `&mut` worked, I just generally prefer to use these kinds of immutability patterns. Thanks for the PR!

---
