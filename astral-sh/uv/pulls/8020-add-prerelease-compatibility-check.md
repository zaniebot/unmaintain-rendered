```yaml
number: 8020
title: Add prerelease compatibility check
type: pull_request
state: merged
author: trag1c
labels:
  - bug
assignees: []
merged: true
base: main
head: discard-prereleases
created_at: 2024-10-08T20:47:33Z
updated_at: 2024-10-09T01:55:12Z
url: https://github.com/astral-sh/uv/pull/8020
synced_at: 2026-01-10T12:54:01Z
```

# Add prerelease compatibility check

---

_Pull request opened by @trag1c on 2024-10-08 20:47_

## Summary

Closes #7977. Makes `PythonDownloadRequest` account for the prerelease part if allowed. Also stores the prerelease in `PythonInstallationKey` directly as a `Prerelease` rather than a string.

## Test Plan

Correctly picks the relevant prerelease (rather than picking the most recent one):
```
λ cargo run python install 3.13.0rc2
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.17s
     Running `target/debug/uv python install 3.13.0rc2`
Searching for Python versions matching: Python 3.13rc2
cpython-3.13.0rc2-macos-aarch64-none ------------------------------ 457.81 KiB/14.73 MiB                                                                                                                    ^C

λ cargo run python install 3.13.0rc3                 
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.17s
     Running `target/debug/uv python install 3.13.0rc3`
Searching for Python versions matching: Python 3.13rc3
Found existing installation for Python 3.13rc3: cpython-3.13.0rc3-macos-aarch64-none
```


---

_Label `bug` added by @zanieb on 2024-10-08 20:50_

---

_@zanieb approved on 2024-10-08 20:50_

Well done!

---

_Comment by @zanieb on 2024-10-08 20:51_

Looks like it will need an update following #8010 

---

_Comment by @trag1c on 2024-10-08 20:54_

Looks like GitHub didn't agree with me on the conflict resolution 😄

---

_Merged by @zanieb on 2024-10-08 21:20_

---

_Closed by @zanieb on 2024-10-08 21:20_

---

_@gaborbernat reviewed on 2024-10-09 00:23_

No tests here? 🤔 

---

_Comment by @zanieb on 2024-10-09 01:55_

Feel free to add some!

---
