```yaml
number: 9557
title: "Add `--dry-run` to `uv pip uninstall`"
type: pull_request
state: merged
author: moreati
labels:
  - enhancement
assignees: []
merged: true
base: main
head: pip-uninstall--dry-run
created_at: 2024-12-01T22:41:10Z
updated_at: 2024-12-02T04:23:04Z
url: https://github.com/astral-sh/uv/pull/9557
synced_at: 2026-01-10T12:00:00Z
```

# Add `--dry-run` to `uv pip uninstall`

---

_Pull request opened by @moreati on 2024-12-01 22:41_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This proposes adding the command line option `uv pip uninstall --dry-run ...`, complementing the existing `uv pip install --dry-run ...` added for #1244 in #1436.

This option does not exist in PyPA's `pip uninstall`, if adopted it would be unique to `uv pip`. The code should be  considered PoC, it is baby's first Rust.

The initial motivation was while investigating https://github.com/moreati/ansible-uv/issues/2 - to allow Ansible module `moreati.uv.pip` to work with`state: absent` in "check_mode" (Ansible's equivalent of a dry run),  without requiring `packaging` or `setuptools`.

## Test Plan

One new unit test has been added. I pedge to add more if the feature is desired/accepted

Example usage

```console
➜  uv git:(pip-uninstall--dry-run) rm -rf .venv
➜  uv git:(pip-uninstall--dry-run) ./target/debug/uv venv                   
Using CPython 3.13.0
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
➜  uv git:(pip-uninstall--dry-run) ./target/debug/uv pip install httpx      
Resolved 7 packages in 178ms
Prepared 5 packages in 60ms
Installed 7 packages in 15ms
 + anyio==4.6.2.post1
 + certifi==2024.8.30
 + h11==0.14.0
 + httpcore==1.0.7
 + httpx==0.28.0
 + idna==3.10
 + sniffio==1.3.1
➜  uv git:(pip-uninstall--dry-run) ./target/debug/uv pip uninstall --dry-run httpx
Would uninstall 1 package
 - httpx==0.28.0
➜  uv git:(pip-uninstall--dry-run) ./target/debug/uv pip list                     
Package  Version
-------- -----------
anyio    4.6.2.post1
certifi  2024.8.30
h11      0.14.0
httpcore 1.0.7
httpx    0.28.0
idna     3.10
sniffio  1.3.1
```


---

_Review comment by @moreati on `crates/uv/src/commands/pip/uninstall.rs`:213 on 2024-12-01 22:42_

This `.dimmed()` might be redundant, given the one 2 lines down.

---

_@moreati reviewed on 2024-12-01 22:42_

---

_@moreati reviewed on 2024-12-01 22:42_

---

_Review comment by @moreati on `crates/uv/src/commands/pip/uninstall.rs`:233 on 2024-12-01 22:42_

This `.dimmed()` might be redundant, given the one 2 lines down.

---

_Comment by @charliermarsh on 2024-12-02 01:41_

I think this is a good idea! Happy to review it.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-12-02 02:17_

---

_Review requested from @charliermarsh by @charliermarsh on 2024-12-02 02:17_

---

_Label `enhancement` added by @charliermarsh on 2024-12-02 02:17_

---

_@charliermarsh approved on 2024-12-02 02:47_

---

_Merged by @charliermarsh on 2024-12-02 02:57_

---

_Closed by @charliermarsh on 2024-12-02 02:57_

---

_Branch deleted on 2024-12-02 04:16_

---

_Comment by @moreati on 2024-12-02 04:22_

Thanks, that was *way* faster and more decisive than I dreamed.

---

_Renamed from "RFC: Add `--dry-run` to `uv pip uninstall`" to "Add `--dry-run` to `uv pip uninstall`" by @moreati on 2024-12-02 04:23_

---
