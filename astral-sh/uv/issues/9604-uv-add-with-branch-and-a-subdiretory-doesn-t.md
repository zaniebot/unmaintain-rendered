```yaml
number: 9604
title: "`uv add` with `--branch` and a subdiretory doesn't check out the branch before checking the subdirectory"
type: issue
state: closed
author: m-o-leary
labels:
  - bug
  - help wanted
assignees: []
created_at: 2024-12-03T11:11:46Z
updated_at: 2024-12-04T13:42:08Z
url: https://github.com/astral-sh/uv/issues/9604
synced_at: 2026-01-10T04:36:21Z
```

# `uv add` with `--branch` and a subdiretory doesn't check out the branch before checking the subdirectory

---

_Issue opened by @m-o-leary on 2024-12-03 11:11_


I run the following command:
```bash
uv add git+https://gitlab.com/my_internal_repo#subdirectory=path/to/package --branch=my_feature_branch -v
```

Ouput:
```bash
DEBUG uv 0.4.7
DEBUG Found project root: `/Users/martin/dev/python_playground/my_test_project`
DEBUG No workspace root found, using project root
DEBUG Reading requests from `/Users/martin/dev/python_playground/my_test_project/.python-version`
DEBUG The virtual environment's Python version satisfies `Python 3.11`
DEBUG Using request timeout of 30s
DEBUG Fetching source distribution from Git: https://gitlab.com/my_internal_repo
DEBUG Acquired lock for `https://gitlab.com/my_internal_repo`
DEBUG Updating Git source `https://gitlab.com/my_internal_repo`
DEBUG Performing a Git fetch for: https://gitlab.com/my_internal_repo
DEBUG Released lock at `/Users/martin/Library/Caches/uv/git-v0/locks/c39373a30072b8c1`
DEBUG Acquired lock for `/Users/martin/Library/Caches/uv/built-wheels-v3/git/eca3f73aa66e62d1/4956502d077fb38a`
DEBUG No static `pyproject.toml` available for: git+https://gitlab.com/my_internal_repo#subdirectory=path/to/package (MissingPyprojectToml)
DEBUG No static `PKG-INFO` available for: git+https://gitlab.com/my_internal_repo#subdirectory=path/to/package (MissingPkgInfo)
DEBUG Released lock at `/Users/martin/Library/Caches/uv/built-wheels-v3/git/eca3f73aa66e62d1/4956502d077fb38a/.lock`
error: Failed to read from the distribution cache
  Caused by: failed to read directory `/Users/martin/Library/Caches/uv/git-v0/checkouts/c39373a30072b8c1/4956502d/path/to/package`
  Caused by: No such file or directory (os error 2)
```
I would expect that the branch would be checked out first before checking for the pyproject.toml which would avoid the `MissingPyprojectToml` error. 

If I change the url to include the branch:
```bash
uv add git+https://gitlab.com/my_internal_repo@my_feature_branch#subdirectory=path/to/package
``` 
The install works as expected

Relevant Docs: https://docs.astral.sh/uv/concepts/projects/dependencies/#git

---

_Label `bug` added by @zanieb on 2024-12-03 14:27_

---

_Label `help wanted` added by @zanieb on 2024-12-03 14:27_

---

_Comment by @zanieb on 2024-12-03 14:27_

Thanks for the report!

---

_Comment by @blueraft on 2024-12-03 17:13_

This works for me though.

```sh
❯ cat pyproject.toml
[project]
name = "uv-test"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = []

❯ uv add git+https://github.com/blueraft/branch-subdir-repo#subdirectory=a --branch=feat
Using CPython 3.12.1
Creating virtual environment at: .venv
 Updated https://github.com/blueraft/branch-subdir-repo (b87fd2f)
Resolved 2 packages in 3ms
Installed 1 package in 3ms
 + a==0.1.0 (from git+https://github.com/blueraft/branch-subdir-repo@b87fd2fc75ca79f416542d9050c6df7714f8e03a#subdirectory=a)

❯ cat pyproject.toml
[project]
name = "uv-test"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "a",
]

[tool.uv.sources]
a = { git = "https://github.com/blueraft/branch-subdir-repo", subdirectory = "a", branch = "feat" }
```

The `pyproject.toml` file only exists in the `feat` [branch](https://github.com/blueraft/branch-subdir-repo/tree/feat/a).





---

_Comment by @charliermarsh on 2024-12-03 22:36_

Interesting. @m-o-leary, are you able to provide a reproduction?

---

_Comment by @m-o-leary on 2024-12-04 08:17_

I'll work on it today 

---

_Comment by @m-o-leary on 2024-12-04 13:42_

Ah my bad. 

I was on `0.4.7` -> running `uv self update` and retrying solves it!

---

_Closed by @m-o-leary on 2024-12-04 13:42_

---
