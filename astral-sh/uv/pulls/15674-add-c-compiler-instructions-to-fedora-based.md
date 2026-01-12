```yaml
number: 15674
title: Add C compiler instructions to Fedora-based distros
type: pull_request
state: merged
author: mdevino
labels:
  - internal
assignees: []
merged: true
base: main
head: add-fedora-build
created_at: 2025-09-04T10:40:49Z
updated_at: 2025-09-04T12:23:39Z
url: https://github.com/astral-sh/uv/pull/15674
synced_at: 2026-01-12T16:11:53Z
```

# Add C compiler instructions to Fedora-based distros

---

_@mdevino_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

This PR adds instructions to install a C compiler on Fedora-based Linux distributions.

## Test Plan

```
# Start Fedora container interactively (can probably be done on Docker as well)
podman run -it registry.fedoraproject.org/fedora

# From now on, run all commands inside the container.
# Install Rustup
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

# Add cargo bin folder to PATH
export PATH="${HOME}/.cargo/bin:${PATH}"

# Install git, clone uv project and get into its folder
dnf install git
git clone https://github.com/astral-sh/uv.git
cd uv

# Try to compile uv and fail (error: linker `cc` not found)
cargo build

# Install C compiler
dnf install gcc

# Try to compile uv again. This time, successfully.
cargo build
```


---

_@zanieb approved on 2025-09-04 12:23_

---

_Merged by @zanieb on 2025-09-04 12:23_

---

_Closed by @zanieb on 2025-09-04 12:23_

---

_Label `internal` added by @zanieb on 2025-09-04 12:23_

---
