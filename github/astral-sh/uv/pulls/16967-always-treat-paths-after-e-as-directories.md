---
number: 16967
title: "Always treat paths after `-e` as directories"
type: pull_request
state: open
author: charliermarsh
labels:
  - error messages
assignees: []
base: main
head: charlie/err
created_at: 2025-12-03T14:48:29Z
updated_at: 2025-12-06T13:50:58Z
url: https://github.com/astral-sh/uv/pull/16967
synced_at: 2026-01-07T13:12:19-06:00
---

# Always treat paths after `-e` as directories

---

_Pull request opened by @charliermarsh on 2025-12-03 14:48_

## Summary

Closes https://github.com/astral-sh/uv/issues/16582.


---

_Marked ready for review by @charliermarsh on 2025-12-03 14:50_

---

_Label `error messages` added by @charliermarsh on 2025-12-03 14:50_

---

_Review requested from @konstin by @charliermarsh on 2025-12-03 14:50_

---

_Comment by @charliermarsh on 2025-12-03 15:02_

It looks like `missing_editable_file` regressed a bit.

---

_Converted to draft by @charliermarsh on 2025-12-03 15:02_

---

_Comment by @codspeed-hq[bot] on 2025-12-03 15:06_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/charlie%2Ferr?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #16967 will **not alter performance**

<sub>Comparing <code>charlie/err</code> (2688a12) with <code>main</code> (2abe56a)</sub>



### Summary

`✅ 5` untouched  





---

_Marked ready for review by @charliermarsh on 2025-12-04 01:15_

---

_Review comment by @konstin on `crates/uv-pypi-types/src/parsed_url.rs`:95 on 2025-12-04 11:20_

This creates an unintuitive error message downstream: If I have a directory `my.project.whl` and
```
-e my.project.whl
```
I get:
```
$ uv  pip install -r requirements.txt
error: The wheel filename "my.project.whl" is invalid: Must have a version
```

If I use a valid wheel filename as the directory name, it accepts it, build the directory, but expects the output to conform to the wheel filename:

```
-e my_project-0.1-py3-none-any.whl
```

`my_project-0.1-py3-none-any.whl/pyproject.toml`:

```toml
[project]
name = "foo"
version = "0.1.0"
```

```
$ uv pip install -r requirements.txt 
  × Failed to build `my-project @ file:///home/konsti/projects/uv/debug/my_project-0.1-py3-none-any.whl`
  ╰─▶ Package metadata name `foo` does not match given name `my-project`
```

I could see that kind of directory name be created by using some unzipping tool on a wheel.

On a sidenote, it only checks the package name, while ignoring version mismatches, so the following installs successfully:

```toml
[project]
name = "my_project"
version = "1.2.3"
```

---

_@konstin reviewed on 2025-12-04 11:27_

---
