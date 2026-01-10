```yaml
number: 9269
title: unsafe ANN autofix seems to ignore return in for loop
type: issue
state: closed
author: twoertwein
labels:
  - bug
assignees: []
created_at: 2023-12-25T03:13:35Z
updated_at: 2023-12-29T16:46:38Z
url: https://github.com/astral-sh/ruff/issues/9269
synced_at: 2026-01-10T11:09:51Z
```

# unsafe ANN autofix seems to ignore return in for loop

---

_Issue opened by @twoertwein on 2023-12-25 03:13_

`ruff --select "ANN001,ANN2" --fix-only` (version 0.1.9) on the following code snippet seems to ignore the return statement in the for loop

https://github.com/pandas-dev/pandas/blob/b0c2e45997e8f164a181ce8e896dfb414e3eb60c/pandas/_version.py#L120
```py
def versions_from_parentdir(parentdir_prefix, root, verbose): ###   ruff adds  -> NoReturn
    """Try to determine the version from the parent directory name.

    Source tarballs conventionally unpack into a directory that includes both
    the project name and a version string. We will also support searching up
    two directory levels for an appropriately named parent directory
    """
    rootdirs = []
    
    for _ in range(3):
        dirname = os.path.basename(root)
        if dirname.startswith(parentdir_prefix):
            return { ###   return statement!
                "version": dirname[len(parentdir_prefix) :],
                "full-revisionid": None,
                "dirty": False,
                "error": None,
                "date": None,
            }
        rootdirs.append(root)
        root = os.path.dirname(root)  # up a level

    if verbose:
        print(
            f"Tried directories {str(rootdirs)} \
            but none started with prefix {parentdir_prefix}"
        )
    raise NotThisMethod("rootdir doesn't start with parentdir_prefix")
```

---

_Label `bug` added by @charliermarsh on 2023-12-25 13:11_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-25 13:11_

---

_Comment by @charliermarsh on 2023-12-25 13:11_

Thanks! Will take a look.

---

_Comment by @charliermarsh on 2023-12-25 13:51_

Will be fixed in the next release.

---

_Comment by @twoertwein on 2023-12-25 14:45_

The same issue happens here with if/else: https://github.com/pandas-dev/pandas/blob/b0c2e45997e8f164a181ce8e896dfb414e3eb60c/pandas/util/__init__.py#L1 ruff thinks `__getattr__` should 'return' `NoReturn`

---

_Comment by @charliermarsh on 2023-12-25 15:20_

Yeah all the same bug. I overlooked an interaction when I implemented the NoReturn.

---

_Closed by @charliermarsh on 2023-12-29 16:46_

---
