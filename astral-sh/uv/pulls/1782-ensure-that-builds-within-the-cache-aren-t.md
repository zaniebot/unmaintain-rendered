```yaml
number: 1782
title: "Ensure that builds within the cache aren't considered Git repositories"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/git
created_at: 2024-02-20T20:16:54Z
updated_at: 2024-02-20T20:55:25Z
url: https://github.com/astral-sh/uv/pull/1782
synced_at: 2026-01-10T15:33:24Z
```

# Ensure that builds within the cache aren't considered Git repositories

---

_Pull request opened by @charliermarsh on 2024-02-20 20:16_

## Summary

Some packages encode logic to embed the current commit SHA in the version tag, when built within a Git repo. This typically results in an invalid (non-compliant) version. Here's an example from `pylzma`: https://github.com/fancycode/pylzma/blob/ccb0e7cff3f6ecd5d38e73e9ca35502d7d670176/version.py#L45.

This PR adds a phony, empty `.git` to the cache root, to ensure that any `git` commands fail.

Closes https://github.com/astral-sh/uv/issues/1768.

## Test Plan

- Create a tag on the current commit, like `v0.5.0`.
- Build `pylzma`, using a cache _within_ the repo:

```
rm -rf foo
cargo run venv
cargo run pip install "pylzma @ https://files.pythonhosted.org/packages/03/d8/10ef072c3cd4301a65a1b762b09eefa02baf8da23b9ea77ebe9546499975/pylzma-0.5.0.tar.gz" --verbose  --cache-dir bar
```


---

_Review requested from @BurntSushi by @charliermarsh on 2024-02-20 20:16_

---

_Review requested from @zanieb by @charliermarsh on 2024-02-20 20:16_

---

_Marked ready for review by @charliermarsh on 2024-02-20 20:16_

---

_@zanieb approved on 2024-02-20 20:18_

Nice.

Relatedly, should we include an empty `.gitignore` in the checkouts bucket?

---

_Comment by @charliermarsh on 2024-02-20 20:22_

Yeah, that seems wise.

---

_Merged by @charliermarsh on 2024-02-20 20:37_

---

_Closed by @charliermarsh on 2024-02-20 20:37_

---

_Branch deleted on 2024-02-20 20:37_

---

_Comment by @charliermarsh on 2024-02-20 20:44_

Perhaps worth mentioning that this does mean people can’t version the cache and check it into Git (I don’t think?).

---

_Comment by @charliermarsh on 2024-02-20 20:55_

I guess the “best” version of this would be to put the .git in the temporary directory when building, but then omit it when we persist the build to the cache.

---
