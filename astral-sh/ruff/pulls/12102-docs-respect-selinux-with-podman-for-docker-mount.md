```yaml
number: 12102
title: "Docs: Respect SELinux with podman for docker mount"
type: pull_request
state: merged
author: maximiliankolb
labels:
  - documentation
assignees: []
merged: true
base: main
head: podman_mount
created_at: 2024-06-29T12:52:12Z
updated_at: 2024-07-06T09:57:40Z
url: https://github.com/astral-sh/ruff/pull/12102
synced_at: 2026-01-10T21:47:02Z
```

# Docs: Respect SELinux with podman for docker mount

---

_Pull request opened by @maximiliankolb on 2024-06-29 12:52_

Tested on Fedora 40 with Podman 5.1.1 and ruff "0.5.0" and "latest". source: https://unix.stackexchange.com/q/651198


## Error without fix

````
$ podman run --rm -it -v .:/io ghcr.io/astral-sh/ruff:latest check
error: Failed to initialize cache at /io/.ruff_cache: Permission denied (os error 13)
warning: Encountered error: Permission denied (os error 13)
All checks passed!

$ podman run --rm -it -v .:/io ghcr.io/astral-sh/ruff:latest format
error: Failed to initialize cache at /io/.ruff_cache: Permission denied (os error 13)
error: Encountered error: Permission denied (os error 13)
````

## Summary

Running ruff by using a docker container requires `:Z` when mounting the current directory on Fedora with SELinux and Podman.

## Test Plan

````
$ podman run --rm -it -v .:/io:Z ghcr.io/astral-sh/ruff:latest check
$ podman run --rm -it -v .:/io:Z ghcr.io/astral-sh/ruff:0.5.0 check
````

---

_Label `documentation` added by @charliermarsh on 2024-06-29 17:53_

---

_Merged by @charliermarsh on 2024-07-05 20:39_

---

_Closed by @charliermarsh on 2024-07-05 20:39_

---

_Comment by @charliermarsh on 2024-07-05 20:40_

Thanks!

---

_Branch deleted on 2024-07-06 09:57_

---
