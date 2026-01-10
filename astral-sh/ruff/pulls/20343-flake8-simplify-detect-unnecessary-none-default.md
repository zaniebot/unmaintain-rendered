```yaml
number: 20343
title: "[`flake8-simplify`] Detect unnecessary `None` default for additional key expression types (`SIM910`)"
type: pull_request
state: merged
author: danparizher
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: fix-20341
created_at: 2025-09-11T05:11:08Z
updated_at: 2025-09-12T14:21:14Z
url: https://github.com/astral-sh/ruff/pull/20343
synced_at: 2026-01-10T17:40:28Z
```

# [`flake8-simplify`] Detect unnecessary `None` default for additional key expression types (`SIM910`)

---

_Pull request opened by @danparizher on 2025-09-11 05:11_

## Summary

Fixes #20341


---

_Comment by @github-actions[bot] on 2025-09-11 05:23_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+6 -0 violations, +0 -0 fixes in 4 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/95333e34b11b8d241d1680eb3ac222cb5d44280e/superset/commands/database/tables.py#L143'>superset/commands/database/tables.py:143:34:</a> SIM910 [*] Use `extra_dict_by_name.get(table.table)` instead of `extra_dict_by_name.get(table.table, None)`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/binary-husky/gpt_academic">binary-husky/gpt_academic</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/binary-husky/gpt_academic/blob/3546f8c1c44994bcc2ef3b2b187619821f2c33e9/request_llms/bridge_all.py#L1304'>request_llms/bridge_all.py:1304:31:</a> SIM910 [*] Use `dict.get()` without default value
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/c7810afb334556f4d54150aff26ec47444c96edf/zerver/models/users.py#L736'>zerver/models/users.py:736:28:</a> SIM910 [*] Use `user_data.get(field.id)` instead of `user_data.get(field.id, None)`
+ <a href='https://github.com/zulip/zulip/blob/c7810afb334556f4d54150aff26ec47444c96edf/zerver/tornado/event_queue.py#L1231'>zerver/tornado/event_queue.py:1231:49:</a> SIM910 [*] Use `extra_user_data.get(client.user_profile_id)` instead of `extra_user_data.get(client.user_profile_id, None)`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/653cb50936a089b6f948a9c4cb53dddfd50a2ba6/astropy/table/mixins/registry.py#L66'>astropy/table/mixins/registry.py:66:16:</a> SIM910 [*] Use `dict.get()` without default value
+ <a href='https://github.com/astropy/astropy/blob/653cb50936a089b6f948a9c4cb53dddfd50a2ba6/astropy/wcs/wcsapi/fitswcs.py#L287'>astropy/wcs/wcsapi/fitswcs.py:287:34:</a> SIM910 [*] Use `CTYPE_TO_UCD1.get(ctype_name.upper())` instead of `CTYPE_TO_UCD1.get(ctype_name.upper(), None)`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM910 | 6 | 6 | 0 | 0 | 0 |

</p>
</details>




---

_@ntBre reviewed on 2025-09-11 19:50_

Thank you! Sorry for the hassle, but could you preview gate this too? This is a pretty big expansion in scope since it was restricted to only two types of expressions before.

---

_Label `rule` added by @ntBre on 2025-09-11 19:51_

---

_Label `preview` added by @ntBre on 2025-09-11 19:51_

---

_Review requested from @ntBre by @danparizher on 2025-09-11 22:58_

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/flake8_simplify/SIM910_preview.py`:18 on 2025-09-12 13:31_

This is actually just another `Expr::Name` as key since it's assigned to a variable. Even the if expression itself is only a single `Expr::If`, but this is fine either way!

---

_@ntBre approved on 2025-09-12 14:06_

Thank you! I just pushed one quick commit recombining the two test files. It makes it a little easier to stabilize later if we're already running the tests on both stable and preview.

---

_@ntBre reviewed on 2025-09-12 14:08_

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/flake8_simplify/SIM910_preview.py`:18 on 2025-09-12 14:08_

(I inlined this too while I was at it)

---

_Renamed from "[`flake8-simplify`] Detect unnecessary `None` default when key is a method call (`SIM910`)" to "[`flake8-simplify`] Detect unnecessary `None` default for additional key expression types (`SIM910`)" by @ntBre on 2025-09-12 14:17_

---

_Merged by @ntBre on 2025-09-12 14:17_

---

_Closed by @ntBre on 2025-09-12 14:17_

---

_Branch deleted on 2025-09-12 14:21_

---
