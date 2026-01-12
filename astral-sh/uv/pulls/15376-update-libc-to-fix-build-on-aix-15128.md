```yaml
number: 15376
title: "Update libc to fix build on AIX #15128"
type: pull_request
state: merged
author: ponchofiesta
labels:
  - internal
assignees: []
merged: true
base: main
head: 15128-aix-libc
created_at: 2025-08-19T10:15:08Z
updated_at: 2025-09-10T08:00:55Z
url: https://github.com/astral-sh/uv/pull/15376
synced_at: 2026-01-12T16:11:43Z
```

# Update libc to fix build on AIX #15128

---

_@ponchofiesta_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Updates libc dependencies to fix #15128

Note: The whole build on AIX still doesn't work but libc works now.

## Test Plan

<!-- How was it tested? -->
Build on AIX 7300-03-00-2446:

```
bash-5.2$ cargo build --offline
   Compiling proc-macro2 v1.0.95
   Compiling unicode-ident v1.0.18
   Compiling libc v0.2.175       # worked
   Compiling cfg-if v1.0.1
   Compiling memchr v2.7.5
   Compiling serde v1.0.219
   Compiling pin-project-lite v0.2.16
   Compiling shlex v1.3.0
   Compiling itoa v1.0.15
   Compiling once_cell v1.21.3
   Compiling smallvec v1.15.1
   Compiling bytes v1.10.1
   Compiling stable_deref_trait v1.2.0
   Compiling ryu v1.0.20
   Compiling tracing-core v0.1.34
   Compiling bitflags v2.9.1
   Compiling foldhash v0.1.5
   Compiling allocator-api2 v0.2.21
   Compiling equivalent v1.0.2
   Compiling litemap v0.8.0
   Compiling quote v1.0.40
   Compiling jobserver v0.1.33     # worked too (depends on libc)
   Compiling hashbrown v0.15.5
   Compiling syn v2.0.104
   Compiling serde_json v1.0.142
   Compiling cc v1.2.30
   Compiling autocfg v1.5.0
   Compiling writeable v0.6.1
   Compiling signal-hook-registry v1.4.5
   Compiling mio v1.0.4
   Compiling socket2 v0.6.0
   Compiling icu_normalizer_data v2.0.0
   Compiling icu_properties_data v2.0.1
   Compiling percent-encoding v2.3.1
   Compiling form_urlencoded v1.2.1
[...]
```

---

_@charliermarsh approved on 2025-08-19 10:16_

---

_Label `internal` added by @konstin on 2025-08-19 10:17_

---

_Merged by @konstin on 2025-08-19 10:27_

---

_Closed by @konstin on 2025-08-19 10:27_

---

_Branch deleted on 2025-09-10 08:00_

---
