```yaml
number: 13882
title: "Change `GlobDirFilter` fallback to `true`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - enhancement
  - preview
assignees: []
merged: true
base: main
head: micha/fix-no-dfa-fallback
created_at: 2025-06-06T11:51:15Z
updated_at: 2025-06-06T12:07:36Z
url: https://github.com/astral-sh/uv/pull/13882
synced_at: 2026-01-10T11:10:42Z
```

# Change `GlobDirFilter` fallback to `true`

---

_Pull request opened by @MichaReiser on 2025-06-06 11:51_

## Summary

I think `GlobDirFilter::match_directory` should return `true` if it failed to construct a DFA
to force a full directory traversal. Returning `false` means that the build backend excludes all files which
is unlikely what we want in that situation.



---

_Review requested from @konstin by @MichaReiser on 2025-06-06 11:51_

---

_Marked ready for review by @MichaReiser on 2025-06-06 12:06_

---

_Renamed from "Change fallback to `true`" to "Change `GlobDirFilter` fallback to `true`" by @MichaReiser on 2025-06-06 12:06_

---

_Label `enhancement` added by @konstin on 2025-06-06 12:07_

---

_Label `preview` added by @konstin on 2025-06-06 12:07_

---

_@konstin approved on 2025-06-06 12:07_

Thanks!

---

_Merged by @konstin on 2025-06-06 12:07_

---

_Closed by @konstin on 2025-06-06 12:07_

---

_Branch deleted on 2025-06-06 12:07_

---
