---
number: 12678
title: "Should `uv sync` support pep-0496 markers?"
type: issue
state: closed
author: zaphar
labels:
  - question
  - needs-mre
assignees: []
created_at: 2025-04-04T18:48:19Z
updated_at: 2025-04-08T01:51:50Z
url: https://github.com/astral-sh/uv/issues/12678
synced_at: 2026-01-10T01:25:23Z
---

# Should `uv sync` support pep-0496 markers?

---

_Issue opened by @zaphar on 2025-04-04 18:48_

### Summary

I looked and couldn't find an issue on this already. I'm trying to use uv on a project with some optional dependencies that aren't supported on my platform. `uv sync` still tries to install them and then fails. `uv pip install -r requirements.txt` works just fine so this isn't a blocker or anything.

### Example

Say I have a requirements.txt with lines like this in it:

```
numpy==1.26.4
nvidia-cublas-cu11==11.10.3.66; sys_platform != 'darwin' and platform_machine != 'arm64'
```

Ideally uv sync would only try to sync numpy and skip the nvidia dep but currently it tries and fails to sync the nvidia dep.

---

_Label `enhancement` added by @zaphar on 2025-04-04 18:48_

---

_Comment by @zanieb on 2025-04-04 23:00_

Can you share more details? We do respect markers.

---

_Label `enhancement` removed by @zanieb on 2025-04-04 23:01_

---

_Label `question` added by @zanieb on 2025-04-04 23:01_

---

_Comment by @zaphar on 2025-04-05 02:23_

I have an internal project with those lines in the requirements.txt file that is referenced by the pyproject.toml. when I run `uv sync` it still tries to install the nvidia dependency but when I run `uv pip install -r` it works. This is with version 0.6.9.

---

_Comment by @zanieb on 2025-04-05 02:29_

We need more details — please share a concrete minimal reproduction as described in #9452

---

_Label `needs-mre` added by @zanieb on 2025-04-05 02:29_

---

_Referenced in [astral-sh/uv#12714](../../astral-sh/uv/issues/12714.md) on 2025-04-07 13:28_

---

_Comment by @charliermarsh on 2025-04-07 19:09_

We resolve for all environments in `uv sync`, because we're creating a universal lockfile. At install time, we only install the relevant dependencies. You can narrow the set of solved environments with https://docs.astral.sh/uv/reference/settings/#environments.

---

_Closed by @charliermarsh on 2025-04-07 19:09_

---

_Comment by @zaphar on 2025-04-07 19:29_

That explains it thank you. I guess I'll have to avoid the `uv sync` and `uv lock` functionality for now. It's a no go on projects that have os/arch dependency differences for hardware reasons.

---

_Comment by @zanieb on 2025-04-07 19:39_

> It's a no go on projects that have os/arch dependency differences for hardware reasons.

This shouldn't be the case, if you provide a concrete example I'm happy to help explain what's going on.

---

_Comment by @zaphar on 2025-04-08 01:51_

I think if I use the optional groups functionality I could do what I need. But for the moment I can't convert everything over to that for ci/cid reasons on this project. `uv pip` will suffice for the moment until I get everyone standardized on uv.

---
