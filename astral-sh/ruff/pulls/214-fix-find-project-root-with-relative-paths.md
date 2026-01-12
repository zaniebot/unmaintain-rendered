```yaml
number: 214
title: Fix find_project_root with relative paths
type: pull_request
state: merged
author: andersk
labels: []
assignees: []
merged: true
base: main
head: find_project_root
created_at: 2022-09-16T22:59:38Z
updated_at: 2022-09-17T00:04:46Z
url: https://github.com/astral-sh/ruff/pull/214
synced_at: 2026-01-12T15:55:04Z
```

# Fix find_project_root with relative paths

---

_@andersk_

The `common_path` crate doesn’t work correctly with relative paths: `common_path("a/b", "c/d") = None`. (I would have reported this upstream to `common_path`, but its [repository](https://gitlab.com/pwoolcoc/common-path) doesn’t exist anymore.)

Additionally, we want to be able to find a parent of the current directory, but `.ancestors()` will not ascend past `"."`.

Work around both problems by converting paths to absolute.

---

_Review comment by @Stranger6667 on `src/pyproject.rs`:75 on 2022-09-16 23:11_

Nit: `Vec` allocation is not necessary, as `common_path_all` accepts an iterator. I suggest merging `map` calls:

```rust
sources.iter().map(|source| cwd.join(source).as_path())
```

---

_@Stranger6667 reviewed on 2022-09-16 23:12_

---

_@andersk reviewed on 2022-09-16 23:13_

---

_Review comment by @andersk on `src/pyproject.rs`:75 on 2022-09-16 23:13_

`common_path_all` accepts an iterator of `Path` references. Your suggestion doesn’t compile as the temporary `PathBuf` isn’t in scope long enough.

---

_@Stranger6667 reviewed on 2022-09-16 23:18_

---

_Review comment by @Stranger6667 on `src/pyproject.rs`:75 on 2022-09-16 23:18_

Oh indeed, I missed that. Sorry!

---

_Comment by @charliermarsh on 2022-09-16 23:39_

Nice, this looks reasonable. Do you know what problems this was causing in practice?

(Also, in the given example of `common_path("a/b", "c/d") = None` -- what's the "correct" return value there? "."?)


---

_Comment by @andersk on 2022-09-17 00:02_

If you have a project with `pyproject.toml`, `a/b.py`, `c/d.py`, then previously

* `ruff a/b.py c/d.py` failed to respect any of the settings in `pyproject.toml`;
* `cd a; ruff b.py` failed to respect any of the settings in `pyproject.toml`.

`common_path("a/b", "c/d") = Some(".")` seems like it’d be more correct, and would have fixed the first of these points but not the second.

---

_Comment by @charliermarsh on 2022-09-17 00:04_

This makes sense, thank you!

---

_Merged by @charliermarsh on 2022-09-17 00:04_

---

_Closed by @charliermarsh on 2022-09-17 00:04_

---

_Branch deleted on 2022-09-17 00:04_

---
