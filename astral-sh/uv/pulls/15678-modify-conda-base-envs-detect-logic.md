```yaml
number: 15678
title: modify conda base envs detect logic
type: pull_request
state: closed
author: gigberg
labels: []
assignees: []
base: main
head: main
created_at: 2025-09-04T13:05:06Z
updated_at: 2025-09-07T07:58:20Z
url: https://github.com/astral-sh/uv/pull/15678
synced_at: 2026-01-12T16:11:53Z
```

# modify conda base envs detect logic

---

_@gigberg_

Related to https://github.com/astral-sh/uv/issues/15604#issuecomment-3253567864, modify conda base envs detect logic

---

_Renamed from "Main" to "modify conda base envs detect logic" by @gigberg on 2025-09-04 13:06_

---

_Comment by @zanieb on 2025-09-04 13:06_

I don't think we can do this. People can create environments in arbitrary directories, right?

---

_Comment by @zanieb on 2025-09-04 13:12_

e.g.,

```
❯ conda create -p my-envs/foo
❯ conda env list

# conda environments:
#
base                 * /Users/zb/workspace/miniconda
test                   /Users/zb/workspace/miniconda/envs/test
                       /Users/zb/workspace/miniconda/my-envs/foo
```

---

_Comment by @gigberg on 2025-09-05 03:50_

> I don't think we can do this. People can create environments in arbitrary directories, right?

I’m not sure about this yet. My PR does have some issues, but what I care about is: why not just use `CONDA_DEFAULT_ENV==base` to check? I think the name base/root is unique, so we don’t need to use `CONDA_PREFIX` or design this `CondaEnvironmentKind()` function to inspect `CONDA_PREFIX`.



---

_Closed by @gigberg on 2025-09-07 07:57_

---

_Comment by @gigberg on 2025-09-07 07:58_

closed for #15679

---
