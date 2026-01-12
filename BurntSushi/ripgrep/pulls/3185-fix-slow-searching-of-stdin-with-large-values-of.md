```yaml
number: 3185
title: "fix slow searching of `stdin` with large values of `-A/--after-context`"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: master
head: ag/fix-slow-stdin-context
created_at: 2025-10-14T17:58:07Z
updated_at: 2025-10-14T18:27:45Z
url: https://github.com/BurntSushi/ripgrep/pull/3185
synced_at: 2026-01-12T18:23:15Z
```

# fix slow searching of `stdin` with large values of `-A/--after-context`

---

_@BurntSushi_

The commit messages have the details, but:

* When searching `stdin`, at least on my Linux machine, calls to `read` never seem to return more than 64K. This caused a pathological problem with large values of `-A/--after-context` that prevented ripgrep from amortizing `read` calls as well as it should.
* When `-A/--after-context` is used, we don't need to find the last N lines before rolling our search buffer. We still do when `-B/--before-context` is set though. And indeed, this can cause ripgrep to be materially slower with large values of `-B` but not with `-A`. Indeed, GNU grep suffers from a similar problem (but is far far slower than ripgrep).

Fixes #3184

---

_Renamed from "ag/fix slow stdin context" to "fix slow searching of `stdin` with large values of `-A/--after-context`" by @BurntSushi on 2025-10-14 17:58_

---

_Merged by @BurntSushi on 2025-10-14 18:27_

---

_Closed by @BurntSushi on 2025-10-14 18:27_

---

_Branch deleted on 2025-10-14 18:27_

---
