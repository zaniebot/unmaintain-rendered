```yaml
number: 5051
title: "fix(git): lock cache on resolve"
type: pull_request
state: merged
author: shcheklein
labels:
  - bug
  - cache
assignees: []
merged: true
base: main
head: fix/concurrent-git-source-install
created_at: 2024-07-14T23:58:54Z
updated_at: 2024-07-16T20:07:45Z
url: https://github.com/astral-sh/uv/pull/5051
synced_at: 2026-01-12T16:06:36Z
```

# fix(git): lock cache on resolve

---

_@shcheklein_

Fixes a concurrency issue when multiple processes are installing the same package in different virtual environments from Git ref (not a specific Git commit).

## Symptoms

That's how some of symptoms looked like in our case:

```
DEBUG uv 0.2.21
DEBUG Checking for Python interpreter at path `/tmp/pytest-of-runner/pytest-0/tmp_venv_dir/python3.12/37bf51bfba4699a940ce31349422b24a5bc55a2b179ed7aec74459a9ae8d57b7/bin/python`
DEBUG Using Python 3.12.4 environment at /tmp/pytest-of-runner/pytest-0/tmp_venv_dir/python3.12/37bf51bfba4699a940ce31349422b24a5bc55a2b179ed7aec74459a9ae8d57b7/bin/python
DEBUG Acquired lock for `/tmp/pytest-of-runner/pytest-0/tmp_venv_dir/python3.12/37bf51bfba4699a940ce31349422b24a5bc55a2b179ed7aec74459a9ae8d57b7`
DEBUG At least one requirement is not satisfied: torch
DEBUG Using request timeout of 300s
DEBUG Found 37 packages in `--find-links` entry: /tmp/pytest-of-runner/pytest-0/tmp_venv_dir/python3.12/.cache/pip/wheels
DEBUG Updating git source `Url { scheme: "https", cannot_be_a_base: false, username: "***", password: None, host: Some(Domain("github.com")), port: None, path: "/iterative/datachain", query: None, fragment: None }`
DEBUG Attempting GitHub fast path for: https://api.github.com/repos/iterative/datachain/commits/fix-distributed-test
DEBUG failed to check github HTTP status client error (404 Not Found) for url (https://api.github.com/repos/iterative/datachain/commits/fix-distributed-test)
DEBUG Performing a Git fetch for: https://***@github.com/iterative/datachain
error: Failed to download and build: `datachain @ git+https://***@github.com/iterative/datachain@fix-distributed-test`
Caused by: Git operation failed
Caused by: process didn't exit successfully: `git clone --local /tmp/pytest-of-runner/pytest-0/tmp_venv_dir/python3.12/.cache/uv/git-v0/db/9d45a3e6f56b0a69 /tmp/pytest-of-runner/pytest-0/tmp_venv_dir/python3.12/.cache/uv/git-v0/checkouts/9d45a3e6f56b0a69/56b15b8` (exit status: 128)
--- stderr
fatal: destination path '/tmp/pytest-of-runner/pytest-0/tmp_venv_dir/python3.12/.cache/uv/git-v0/checkouts/9d45a3e6f56b0a69/56b15b8' already exists and is not an empty directory.
```

## Cause of the issue

It is the same command that is failing - `git clone`, and I think it's happening because it was trying to first get the repo to dereference the `fix-distributed-test` branch:

`Given a remote source distribution, return a precise variant, if possible.`

And it's happening w/i acquiring a lock around cache.

## Fix

I thinks we can reuse the existing `fetch` method that has already lock around cache:

https://github.com/astral-sh/uv/pull/5051/files#diff-f58bb99dee2c4922d156ace3e7de651f0d9a81fc8e9447a2ad865de5c53543fcR61-R68

```python
        // Avoid races between different processes, too.
        let lock_dir = cache.join("locks");
        ....
```

## Questions

- Are there any tests that cover concurrency? I'm quite new to Rust and if someone can point me to some examples and I can create a similar test or a new one.
- Is error handling done correctly in this PR (again, I'm new to Rust - I'll review and read about it,  but it's better also for someone else to review this)




---

_Comment by @shcheklein on 2024-07-16 19:49_

CI failure seems to be urelated.

---

_Marked ready for review by @shcheklein on 2024-07-16 19:49_

---

_Comment by @shcheklein on 2024-07-16 19:53_

Should be ready for initial review.

---

_Review requested from @charliermarsh by @charliermarsh on 2024-07-16 19:54_

---

_@charliermarsh approved on 2024-07-16 19:59_

This looks right to me, thank you.

---

_Comment by @charliermarsh on 2024-07-16 20:00_

We do have a test for this (`compile_git_concurrent_access`). I suspect it's _not_ failing because it already uses precise commits (and so the resolve phase is skipped entirely).

---

_Label `bug` added by @zanieb on 2024-07-16 20:01_

---

_Label `cache` added by @zanieb on 2024-07-16 20:01_

---

_Comment by @charliermarsh on 2024-07-16 20:05_

Ok I was able to reproduce on main:

```shell
rm -rf cache_dir
echo "example-pkg-a @ git+https://github.com/pypa/sample-namespace-packages.git#subdirectory=pkg_resources/pkg_a
" | ./target/debug/uv pip compile - --cache-dir cache_dir &
echo "example-pkg-b @ git+https://github.com/pypa/sample-namespace-packages.git#subdirectory=pkg_resources/pkg_b
" | ./target/debug/uv pip compile - --cache-dir cache_dir &
```

I think it's hard to reproduce in a test because we likely need to be accessing across processes.

---

_Comment by @charliermarsh on 2024-07-16 20:06_

Works as expected on your branch!

---

_Merged by @charliermarsh on 2024-07-16 20:06_

---

_Closed by @charliermarsh on 2024-07-16 20:06_

---

_Comment by @shcheklein on 2024-07-16 20:07_

thanks @charliermarsh for a quick turnaround! 👍 

---
