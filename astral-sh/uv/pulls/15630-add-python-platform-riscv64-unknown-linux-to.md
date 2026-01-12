```yaml
number: 15630
title: "Add `--python-platform riscv64-unknown-linux` to various commands"
type: pull_request
state: merged
author: directhex
labels:
  - enhancement
  - cli
assignees: []
merged: true
base: main
head: dev/dhx/riscv64-target
created_at: 2025-09-02T15:04:59Z
updated_at: 2025-09-02T17:17:30Z
url: https://github.com/astral-sh/uv/pull/15630
synced_at: 2026-01-12T16:11:51Z
```

# Add `--python-platform riscv64-unknown-linux` to various commands

---

_@directhex_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

We (and I'm sure many others) are currently doing a lot of RISC-V work in QEMU.  It is possible to significantly improve the speed of Python-related builds by taking care of the environment setup using an AMD64 `uv` binary (bypassing binfmt/qemu-system emulation).

Some approx numbers from local testing in riscv64 Ubuntu in QEMU:

| Resolver arch | Command | Time |
| --- | --- | --- |
| riscv64 | `pip install --upgrade --break-system-packages --index-url=https://gitlab.com/api/v4/projects/riseproject%2Fpython%2Fwheel_builder/packages/pypi/simple openai-harmony` | 15s |
| riscv64 | `uv pip install --upgrade --system --break-system-packages --index-url=https://gitlab.com/api/v4/projects/riseproject%2Fpython%2Fwheel_builder/packages/pypi/simple openai-harmony` | 5s |
| amd64 | `uv pip install --python-platform=riscv64-unknown-linux --upgrade --system --break-system-packages --index-url=https://gitlab.com/api/v4/projects/riseproject%2Fpython%2Fwheel_builder/packages/pypi/simple openai-harmony` | 4s |

The numbers from some larger internal packages with deeper dependency trees are much more pronounced - 3m6 vs 43s vs 8s, in one example.

Manylinux 2.39 is specified, as it's the first (only?) RISC-V manylinux

## Test Plan

Locally, in QEMU.

`$ docker run --platform linux/riscv64 -it ubuntu:latest`, get amd64 libc into LD_LIBRARY_PATH, tests as above

---

_@Gankra approved on 2025-09-02 17:09_

seems wholy uncontroversial, thanks!

---

_Label `enhancement` added by @Gankra on 2025-09-02 17:09_

---

_Label `cli` added by @Gankra on 2025-09-02 17:09_

---

_Renamed from "Add a RISCV64 Linux --python-platform target triple for `uv pip install`" to "Add `--python-platform riscv64-unknown-linux` to various commands" by @Gankra on 2025-09-02 17:10_

---

_Merged by @Gankra on 2025-09-02 17:17_

---

_Closed by @Gankra on 2025-09-02 17:17_

---
