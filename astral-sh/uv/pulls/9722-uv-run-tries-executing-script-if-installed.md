```yaml
number: 9722
title: "`uv run` tries executing script if installed"
type: pull_request
state: closed
author: 2022tgoel
labels: []
assignees: []
base: main
head: tarushii/run_script
created_at: 2024-12-08T15:58:20Z
updated_at: 2025-02-12T18:10:26Z
url: https://github.com/astral-sh/uv/pull/9722
synced_at: 2026-01-12T16:08:57Z
```

# `uv run` tries executing script if installed

---

_@2022tgoel_

## Summary

This PR implements a solution to https://github.com/astral-sh/uv/issues/9167. It prioritizes executing the `uv run` command as a python script, if the script is installed, over executing the command as a python package or external command. 

## Test Plan

I tested my solution against the repoduction of the issue created here: https://github.com/raqbit/uv-scripts-repro/, and verified that `uv run foo` now executes the python script.


---

_Closed by @zanieb on 2025-02-12 18:08_

---

_Closed by @zanieb on 2025-02-12 18:08_

---

_Comment by @zanieb on 2025-02-12 18:09_

Sorry I didn't get to reviewing this — I looked at it and wasn't sure what to think of the approach and figured I'd need to play with it myself to make a suggestion. I just found the time to try implementing the feature and landed on https://github.com/astral-sh/uv/pull/11431

I think this approach is theoretically better, but I decided to go with the simpler practical approach for now.

---

_Comment by @zanieb on 2025-02-12 18:10_

Thank you for taking the time to contribute though, it saved me time to not need to implement this to see how it looked.

---
