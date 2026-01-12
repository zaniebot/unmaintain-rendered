```yaml
number: 20244
title: "Stabilize `long-sleep-not-forever` (`ASYNC116`)"
type: pull_request
state: merged
author: ntBre
labels:
  - rule
assignees: []
merged: true
base: brent/0.13.0
head: brent/async116
created_at: 2025-09-04T17:55:15Z
updated_at: 2025-09-04T20:18:41Z
url: https://github.com/astral-sh/ruff/pull/20244
synced_at: 2026-01-12T15:56:57Z
```

# Stabilize `long-sleep-not-forever` (`ASYNC116`)

---

_@ntBre_

Tests and docs look good


---

_Added to milestone `v0.13` by @ntBre on 2025-09-04 17:55_

---

_Label `rule` added by @ntBre on 2025-09-04 17:55_

---

_Comment by @github-actions[bot] on 2025-09-04 18:06_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -4 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/python-trio/trio">python-trio/trio</a> (+0 -4 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/python-trio/trio/blob/fc352b2b8ee84071273142375d8babb44333d115/src/trio/_core/_tests/test_mock_clock.py#L115'>src/trio/_core/_tests/test_mock_clock.py:115:26:</a> RUF100 [*] Unused `noqa` directive (non-enabled: `ASYNC116`)
- <a href='https://github.com/python-trio/trio/blob/fc352b2b8ee84071273142375d8babb44333d115/src/trio/_core/_tests/test_mock_clock.py#L97'>src/trio/_core/_tests/test_mock_clock.py:97:26:</a> RUF100 [*] Unused `noqa` directive (non-enabled: `ASYNC116`)
- <a href='https://github.com/python-trio/trio/blob/fc352b2b8ee84071273142375d8babb44333d115/src/trio/_tests/test_ssl.py#L717'>src/trio/_tests/test_ssl.py:717:39:</a> RUF100 [*] Unused `noqa` directive (non-enabled: `ASYNC116`)
- <a href='https://github.com/python-trio/trio/blob/fc352b2b8ee84071273142375d8babb44333d115/src/trio/_tests/test_ssl.py#L742'>src/trio/_tests/test_ssl.py:742:39:</a> RUF100 [*] Unused `noqa` directive (non-enabled: `ASYNC116`)
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF100 | 4 | 0 | 4 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @ntBre on 2025-09-04 18:38_

The ecosystem changes are previously-unused noqa codes for ASYNC116 that are now used on stable. I took a look at these to see where a real project needed to use a noqa for this rule, but it was just trio intentionally testing long sleeps, with comments to support that:

```py
    # ignore ASYNC116, not sleep_forever, trying to test a large but finite sleep
    await sleep(100000)  # noqa: ASYNC116
```


---

_Marked ready for review by @ntBre on 2025-09-04 18:38_

---

_Review requested from @dylwil3 by @ntBre on 2025-09-04 18:38_

---

_@dylwil3 approved on 2025-09-04 20:11_

---

_Merged by @ntBre on 2025-09-04 20:18_

---

_Closed by @ntBre on 2025-09-04 20:18_

---

_Branch deleted on 2025-09-04 20:18_

---
