```yaml
number: 3881
title: "pick some real Python projects and attempt to port them to `uv` with universal locking"
type: issue
state: closed
author: BurntSushi
labels:
  - internal
assignees: []
created_at: 2024-05-28T13:40:57Z
updated_at: 2024-07-30T15:35:16Z
url: https://github.com/astral-sh/uv/issues/3881
synced_at: 2026-01-10T04:53:49Z
```

# pick some real Python projects and attempt to port them to `uv` with universal locking

---

_Issue opened by @BurntSushi on 2024-05-28 13:40_

I'd like to start small (as in, a small dependency tree) but with a project that benefits from having a platform independent lock file. So probably a project that utilizes the universal locking of Poetry or PDM.

I'd also like to try locking packse's `pyproject.toml` and see what happens.

The idea is to see what issues arise with our universal locking implementation in real projects.

Ref #3347 

---

_Label `internal` added by @BurntSushi on 2024-05-28 13:40_

---

_Comment by @mjclarke94 on 2024-05-29 16:57_

Pretty much anything which leverages `torch` would be a good stress test given you need to use different index urls on a per platform basis. 

---

_Comment by @adeebshihadeh on 2024-06-09 18:24_

We're excited to switch for [openpilot](https://github.com/commaai/).

Previous attempt here: https://github.com/commaai/openpilot/pull/32530. 

---

_Assigned to @BurntSushi by @BurntSushi on 2024-06-17 13:35_

---

_Comment by @helderco on 2024-06-19 16:20_

Curious to try it in [dagger](https://github.com/dagger/dagger/tree/main/sdk/python) modules, but I'll need to catch up on these changes.

---

_Comment by @jab on 2024-06-19 16:55_

[bidict](https://github.com/jab/bidict) has a pretty small dependency tree (no runtime deps, and just a [handful of test deps](https://github.com/jab/bidict/blob/main/dev-deps/test.in)), in case it's a good candidate for testing this with -- it would exercise resolving different dependencies for different versions of python as well as for different dependency groups all into the same lockfile. It currently uses `uv pip compile`-[generated](https://github.com/jab/bidict/blob/bf916bd/dev-deps/update_dev_dependencies#L48) requirements files (rather than Poetry or PDM). Looking forward to simplifying all those test.txt requirements files (under the [dev-deps/*](https://github.com/jab/bidict/tree/main/dev-deps) subtree) that have been required without this feature.

---

_Comment by @zanieb on 2024-06-19 16:58_

I ported packse in https://github.com/astral-sh/packse/pull/183 if anyone is interested in what that looked like.

---

_Closed by @charliermarsh on 2024-07-30 15:35_

---
