```yaml
number: 4160
title: Allow for testing of packages in integration tests
type: issue
state: closed
author: chanman3388
labels:
  - internal
assignees: []
created_at: 2023-04-30T11:39:14Z
updated_at: 2023-11-03T03:07:14Z
url: https://github.com/astral-sh/ruff/issues/4160
synced_at: 2026-01-12T15:54:44Z
```

# Allow for testing of packages in integration tests

---

_@chanman3388_

It is my understanding that we cannot use folders/packages in integration tests.
For the moment I believe that we just use `ruff::test::test_path` to test a specific module, but I think it should be easy enough to make a similar function, or perhaps even a parent function that calls `test_path` not unlike how `ruff_cli::commands::run` lints all the files in parallel (I have not yet fully thought through the implementation).

I am happy to implement this.


---

_Comment by @MichaReiser on 2023-05-01 09:48_

Adding support for testing multiple file would be cool to have to test any multi-file analysis that we plan to build properly. 

Ideally, I would prefer a single testing infrastructure for single file and multi-file analysis (we can have helpers for single-file or multi-file test cases) because the testing infrastructure re-implements the linting orchestration layer of ruff and any divergence in it from ruff's actual behavior results in a false sense of safety. That's why I think we should strive to have an as thin-as-possible testing implementation and share it if possible. 

---

_Label `internal` added by @charliermarsh on 2023-05-02 01:00_

---

_Comment by @chanman3388 on 2023-05-05 01:49_

I've had a little look into this further and had a few more questions. I was hoping to mirror the orchestration layer but I think this approach may be too heavy-handed. We could maybe do something like, detect the path we want to test is a python module or package then simply call `test_path` on each? This feels like it should work as we use `packaging::detect_package_root` to detect the package? Would something like that be sufficient?


---

_Comment by @charliermarsh on 2023-11-03 03:07_

Going to close this for now to keep the issue tracker actionable.

---

_Closed by @charliermarsh on 2023-11-03 03:07_

---
