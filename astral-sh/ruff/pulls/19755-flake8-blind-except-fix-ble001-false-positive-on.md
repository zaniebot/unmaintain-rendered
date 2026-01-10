```yaml
number: 19755
title: "[`flake8-blind-except`] Fix `BLE001` false-positive on `raise ... from None`"
type: pull_request
state: merged
author: deliro
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: ble001-enhance
created_at: 2025-08-05T08:00:28Z
updated_at: 2025-08-13T17:49:39Z
url: https://github.com/astral-sh/ruff/pull/19755
synced_at: 2026-01-10T17:52:17Z
```

# [`flake8-blind-except`] Fix `BLE001` false-positive on `raise ... from None`

---

_Pull request opened by @deliro on 2025-08-05 08:00_

## Summary

- Refactored `BLE001` logic for clarity and minor speed-up.
- Improved documentation and comments (previously, `BLE001` docs claimed it catches bare `except:`s, but it doesn't).
- Fixed a false-positive bug with `from None` cause:

```python
# somefile.py

try:
    pass
except BaseException as e:
    raise e from None
```

### main branch
```
somefile.py:3:8: BLE001 Do not catch blind exception: `BaseException`
  |
1 | try:
2 |     pass
3 | except BaseException as e:
  |        ^^^^^^^^^^^^^ BLE001
4 |     raise e from None
  |

Found 1 error.
```

### this change

```cargo run -p ruff -- check somefile.py --no-cache --select=BLE001```

```
All checks passed!
```

## Test Plan

- Added a test case to cover `raise X from Y` clause
- Added a test case to cover `raise X from None` clause

---

_Comment by @github-actions[bot] on 2025-08-05 08:11_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/1fcf9acd3340f4a34f76452fec764c56e849659b/tools/lib/provision.py#L386'>tools/lib/provision.py:386:20:</a> BLE001 Do not catch blind exception: `BaseException`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| BLE001 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/1fcf9acd3340f4a34f76452fec764c56e849659b/tools/lib/provision.py#L386'>tools/lib/provision.py:386:20:</a> BLE001 Do not catch blind exception: `BaseException`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| BLE001 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---

_Renamed from "[`flake8-blind-except`] Enhance `BLE001`: fix docs; light speed-up; improve readability" to "[`flake8-blind-except`] Fix `BLE001`: false-positive trigger with `raise ... from None`" by @deliro on 2025-08-06 15:01_

---

_Review requested from @ntBre by @ntBre on 2025-08-08 14:08_

---

_Label `bug` added by @ntBre on 2025-08-12 18:26_

---

_Label `rule` added by @ntBre on 2025-08-12 18:26_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_blind_except/rules/blind_except.rs`:14 on 2025-08-12 18:34_

Good catch, thanks!

---

_@ntBre approved on 2025-08-12 18:45_

Looks good to me, thank you! And the change in the ecosystem report is exactly like the issue.

---

_Comment by @ntBre on 2025-08-12 18:46_

I think the conflicts are just from recent changes to our diagnostic format. Could you just rerun the tests locally and accept the new snapshots? That's probably the easiest way to resolve them.

---

_Comment by @deliro on 2025-08-12 20:22_

> I think the conflicts are just from recent changes to our diagnostic format. Could you just rerun the tests locally and accept the new snapshots? That's probably the easiest way to resolve them.

Thank you! Done

---

_Review requested from @ntBre by @deliro on 2025-08-12 20:40_

---

_Renamed from "[`flake8-blind-except`] Fix `BLE001`: false-positive trigger with `raise ... from None`" to "[`flake8-blind-except`] Fix `BLE001` false-positive on `raise ... from None`" by @ntBre on 2025-08-13 17:01_

---

_Merged by @ntBre on 2025-08-13 17:01_

---

_Closed by @ntBre on 2025-08-13 17:01_

---

_Branch deleted on 2025-08-13 17:49_

---
