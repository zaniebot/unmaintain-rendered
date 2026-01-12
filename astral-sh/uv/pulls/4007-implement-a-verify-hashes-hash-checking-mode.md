```yaml
number: 4007
title: "Implement a `--verify-hashes` hash-checking mode"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/ver
created_at: 2024-06-04T01:51:13Z
updated_at: 2024-07-17T21:25:32Z
url: https://github.com/astral-sh/uv/pull/4007
synced_at: 2026-01-12T16:05:58Z
```

# Implement a `--verify-hashes` hash-checking mode

---

_@charliermarsh_

## Summary

This is an alternative to `--require-hashes` which will validate a hash if it's present, but ignore requirements that omit hashes or are absent from the lockfile entirely.

So, e.g., transitive dependencies that are missing will _not_ error; nor will dependencies that are included but lack a hash.

Closes https://github.com/astral-sh/uv/issues/3305.


---

_Label `enhancement` added by @charliermarsh on 2024-06-04 01:51_

---

_Comment by @charliermarsh on 2024-06-04 01:52_

@helderco -- what do you think of this behavior? I'm wondering if we should instead implement it such that if _any_ hash is present, we enforce hashes (i.e., a setting that enables pip's default "implied hashes" behavior).


---

_Marked ready for review by @charliermarsh on 2024-06-04 01:52_

---

_Comment by @helderco on 2024-06-05 06:37_

Yeah, requiring all hashes if any are present sgtm!

---

_Comment by @charliermarsh on 2024-07-17 21:16_

Moving forward with this since we need it as the default for the lockfile.

---

_Merged by @charliermarsh on 2024-07-17 21:25_

---

_Closed by @charliermarsh on 2024-07-17 21:25_

---

_Branch deleted on 2024-07-17 21:25_

---
