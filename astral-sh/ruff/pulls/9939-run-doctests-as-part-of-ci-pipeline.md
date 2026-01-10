```yaml
number: 9939
title: Run doctests as part of CI pipeline
type: pull_request
state: merged
author: MichaReiser
labels:
  - ci
assignees: []
merged: true
base: main
head: doctests-
created_at: 2024-02-12T08:41:43Z
updated_at: 2024-02-12T15:53:40Z
url: https://github.com/astral-sh/ruff/pull/9939
synced_at: 2026-01-10T22:57:09Z
```

# Run doctests as part of CI pipeline

---

_Pull request opened by @MichaReiser on 2024-02-12 08:41_

## Summary

Add explicit command to run doctest. Run tests for all features. 

Fixes https://github.com/astral-sh/ruff/issues/9938

## Test Plan

See CI pipeline


---

_@MichaReiser reviewed on 2024-02-12 08:43_

---

_Review comment by @MichaReiser on `.github/workflows/ci.yaml`:123 on 2024-02-12 08:43_

I removed the `-j 12`. It gives us minimal speedup but has the downside that it requires manual updating (that likely doesn't happen) when any properties about the runners change.

---

_Label `ci` added by @MichaReiser on 2024-02-12 08:53_

---

_Review comment by @MichaReiser on `.config/nextest.toml`:1 on 2024-02-12 09:08_

`cargo insta` doesn't support passing custom test runner flags. Luckily, nextest supports setting the profile via environment flag..

---

_@MichaReiser reviewed on 2024-02-12 09:08_

---

_Merged by @MichaReiser on 2024-02-12 09:18_

---

_Closed by @MichaReiser on 2024-02-12 09:18_

---

_Branch deleted on 2024-02-12 09:18_

---

_Comment by @github-actions[bot] on 2024-02-12 09:27_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@charliermarsh reviewed on 2024-02-12 14:40_

---

_Review comment by @charliermarsh on `.github/workflows/ci.yaml`:123 on 2024-02-12 14:40_

Did you benchmark it? It seemed like it was about 30s faster for WIndows (https://github.com/astral-sh/ruff/pull/9921#issuecomment-1937094551).

---

_@charliermarsh reviewed on 2024-02-12 14:41_

---

_Review comment by @charliermarsh on `.github/workflows/ci.yaml`:129 on 2024-02-12 14:41_

Oh I didn't know about this, huh. Did you know about this @zanieb?

---

_Comment by @charliermarsh on 2024-02-12 14:43_

It looks like this PR also re-adds `--unreferenced reject`.

---

_@charliermarsh reviewed on 2024-02-12 14:44_

---

_Review comment by @charliermarsh on `.github/workflows/ci.yaml`:160 on 2024-02-12 14:44_

Why is this on the Windows tests? If anything, shouldn't it only be on the Linux tests, since doctests add so much overhead, and Windows is the bottleneck?

---

_@MichaReiser reviewed on 2024-02-12 14:54_

---

_Review comment by @MichaReiser on `.github/workflows/ci.yaml`:160 on 2024-02-12 14:54_

`cargo insta` uses `nextest` to run the "normal" test but calls `cargo test --doc` for doctests. 

So the doctests run on both Windows and Linux. 

However, we never used the unreferenced-snapshots feature on Windows, so it felt useless to use `cargo insta` on Windows for a feature that we don't need. That's why it makes the call directly.



---

_Review comment by @MichaReiser on `.github/workflows/ci.yaml`:123 on 2024-02-12 14:55_

I did not but you mentioned on your PR that it is minimal :laughing: 

> Basically no difference:



---

_@MichaReiser reviewed on 2024-02-12 14:55_

---

_@charliermarsh reviewed on 2024-02-12 14:56_

---

_Review comment by @charliermarsh on `.github/workflows/ci.yaml`:160 on 2024-02-12 14:56_

Ah that makes sense, thanks! Doctests are so slow, honestly I'm tempted not to run them on Windows.

---

_@charliermarsh reviewed on 2024-02-12 14:59_

---

_Review comment by @charliermarsh on `.github/workflows/ci.yaml`:123 on 2024-02-12 14:59_

Lol you got me

---

_@charliermarsh reviewed on 2024-02-12 15:00_

---

_Review comment by @charliermarsh on `.github/workflows/ci.yaml`:123 on 2024-02-12 15:00_

I ended up keeping it because the Windows difference was kinda big though...

---

_@zanieb reviewed on 2024-02-12 15:53_

---

_Review comment by @zanieb on `.github/workflows/ci.yaml`:129 on 2024-02-12 15:53_

Yeah I talked about this in my earlier pull request I linked :p I think the key here is setting the profile via environment variable? Nice work @MichaReiser 

---
