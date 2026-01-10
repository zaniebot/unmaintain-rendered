```yaml
number: 2466
title: Add empty gitignore during source distribution builds
type: pull_request
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
base: main
head: charlie/gitingore
created_at: 2024-03-14T19:50:36Z
updated_at: 2024-03-14T20:19:42Z
url: https://github.com/astral-sh/uv/pull/2466
synced_at: 2026-01-10T14:49:08Z
```

# Add empty gitignore during source distribution builds

---

_Pull request opened by @charliermarsh on 2024-03-14 19:50_

## Summary

It's not clear _why_ it matters in this case, but Hatchling respecting the `.gitignore` in our cache is causing certain builds to fail. This PR adds an empty `.gitignore` to the `built-wheels-v0` bucket and ensures that all unzipped distributions are stored in temporary folders within that bucket.

## Test Plan

Run `cargo run pip install "local_not_used_with_sdist_a @ file:///Users/crmarsh/Downloads/local_not_used_with_sdist_a-1.2.3" --reinstall --verbose --verbose -n`.

Then:

```
❯ ls .venv/lib/python3.12/site-packages/
__pycache__                                 _virtualenv.pth                             _virtualenv.py                              local_not_used_with_sdist_a                 local_not_used_with_sdist_a-1.2.3.dist-info
```


---

_Label `bug` added by @charliermarsh on 2024-03-14 19:50_

---

_Comment by @zanieb on 2024-03-14 19:54_

See https://github.com/pypa/hatch/issues/1273 for details on the upstream issue — tldr if a source distribution does not have a `.gitignore` file hatchling will traverse upward until it finds one — which in our case is our cache's `.gitignore` file which has a `*` in it.

We previously added a `.git` directory to prevent upward traversal, but `hatch` does yet support stopping traversal at git project boundaries.

---

_@zanieb reviewed on 2024-03-14 19:55_

---

_Review comment by @zanieb on `crates/uv-cache/src/lib.rs`:262 on 2024-03-14 19:55_

I'd rephrase as "Build backends (like hatchling) may traverse upwards..." but no strong feelings.

---

_@zanieb approved on 2024-03-14 19:55_

---

_Comment by @charliermarsh on 2024-03-14 20:04_

Hah, that test now _correctly_ fails.

---

_Comment by @charliermarsh on 2024-03-14 20:04_

Need https://github.com/astral-sh/uv/pull/2462 to merge first.

---

_Comment by @charliermarsh on 2024-03-14 20:19_

Ah shoot, this was accidentally merged as part of https://github.com/astral-sh/uv/pull/2462.

---

_Closed by @charliermarsh on 2024-03-14 20:19_

---
