```yaml
number: 8934
title: Allow installation of manylinux wheels on riscv64
type: pull_request
state: merged
author: markdryan
labels: []
assignees: []
merged: true
base: main
head: markdryan/riscv64_manylinux
created_at: 2024-11-08T12:12:10Z
updated_at: 2024-11-08T15:57:52Z
url: https://github.com/astral-sh/uv/pull/8934
synced_at: 2026-01-10T12:00:00Z
```

# Allow installation of manylinux wheels on riscv64

---

_Pull request opened by @markdryan on 2024-11-08 12:12_


<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

auditwheel is capable of generating riscv64 wheels for manylinux_2_31 and above.  Here we modify uv-platform-tags so that those wheels can be installed using uv.

Fixes: #8889

## Test Plan

- ran `cargo nextest run` locally on an x86 machine
- also ran `cargo nextest run` locally on a riscv64 VM but there were a fair few failures (with and without this patch)
- built a riscv64 uv wheel, installed it on a riscv64 VM and checked that I could use the newly built version of uv to install manylinux_2_35_riscv64 wheels.

---

_Comment by @charliermarsh on 2024-11-08 14:11_

Are you able to link to any references around the riscv64 / manylinux relationship, for posterity?

---

_Comment by @konstin on 2024-11-08 14:13_

It's documented in https://github.com/pypa/auditwheel/blob/dd3df250063f520950f7f7c3e30544c701b5ec9c/src/auditwheel/policy/manylinux-policy.json#L329, i wanted to add this for the review

---

_Comment by @markdryan on 2024-11-08 14:40_

> Are you able to link to any references around the riscv64 / manylinux relationship, for posterity?

There's a [thread](https://discuss.python.org/t/packaging-support-for-riscv64/58475) discussing the status of packaging support for riscv64 in the Python packaging discussion forums.  As far as I'm aware, the situation as of today is that, auditwheel>=6.10 and maturin>=v1.7.1 can create manylinux riscv64 wheels and pip>=24.1 can install them.  Neither the manylinux container images, pypi/wharehouse nor cibuildwheel support riscv64 yet, but this doesn't in itself prevent riscv64 manylinux wheels from being built, installed or uploaded to other registries, such as those provided by gitlab.


---

_@konstin approved on 2024-11-08 15:57_

Thanks!

I've confirmed with `docker run --rm -it --platform linux/riscv64 ubuntu` that `uv pip install cmake --only-binary :all: -v --index-url https://gitlab.com/api/v4/projects/riseproject%2Fpython%2Fwheel_builder/packages/pypi/simple` picks the manylinux over the linux package after this change

---

_Merged by @konstin on 2024-11-08 15:57_

---

_Closed by @konstin on 2024-11-08 15:57_

---
