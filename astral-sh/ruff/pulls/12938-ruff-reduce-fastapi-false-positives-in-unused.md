```yaml
number: 12938
title: "[`ruff`] Reduce FastAPI false positives in `unused-async` (`RUF029`)"
type: pull_request
state: merged
author: TomerBin
labels: []
assignees: []
merged: true
base: main
head: tomer/fix-RUF029-for-fastapi
created_at: 2024-08-16T16:28:16Z
updated_at: 2024-08-17T14:34:50Z
url: https://github.com/astral-sh/ruff/pull/12938
synced_at: 2026-01-12T15:55:42Z
```

# [`ruff`] Reduce FastAPI false positives in `unused-async` (`RUF029`)

---

_@TomerBin_

## Summary

Fixes #12925.




---

_Comment by @codspeed-hq[bot] on 2024-08-16 16:33_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/TomerBin:tomer/fix-RUF029-for-fastapi)

### Merging #12938 will **not alter performance**

<sub>Comparing <code>TomerBin:tomer/fix-RUF029-for-fastapi</code> (058a968) with <code>main</code> (96802d6)</sub>



### Summary

`✅ 32` untouched benchmarks






---

_Comment by @AlexWaygood on 2024-08-16 16:36_

Thanks for the PR!

> I've added a config option that allows FastAPI routes to be declared as async functions even when they are not doing IO operations.

Sorry, I think I might have been confusing when I mentioned configuration options in https://github.com/astral-sh/ruff/issues/12925#issuecomment-2293437236. I don't think we need a configuration option here specifically regarding whether fastAPI routes should be ignored by this rule or not; that seems way too specific. I would just ignore fastAPI routes unconditionally in this rule, without adding a new configuration option.

---

_Comment by @github-actions[bot] on 2024-08-16 16:42_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -70 violations, +0 -0 fixes in 2 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/lnbits/lnbits">lnbits/lnbits</a> (+0 -13 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/lnbits/lnbits/blob/dbb689c5c56017aa2f777327101477864e90c3d5/lnbits/core/views/admin_api.py#L45'>lnbits/core/views/admin_api.py:45:11:</a> RUF029 Function `api_monitor` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/lnbits/lnbits/blob/dbb689c5c56017aa2f777327101477864e90c3d5/lnbits/core/views/admin_api.py#L88'>lnbits/core/views/admin_api.py:88:11:</a> RUF029 Function `api_restart_server` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/lnbits/lnbits/blob/dbb689c5c56017aa2f777327101477864e90c3d5/lnbits/core/views/admin_api.py#L99'>lnbits/core/views/admin_api.py:99:11:</a> RUF029 Function `api_download_backup` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/lnbits/lnbits/blob/dbb689c5c56017aa2f777327101477864e90c3d5/lnbits/core/views/api.py#L204'>lnbits/core/views/api.py:204:11:</a> RUF029 Function `api_list_currencies_available` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/lnbits/lnbits/blob/dbb689c5c56017aa2f777327101477864e90c3d5/lnbits/core/views/api.py#L227'>lnbits/core/views/api.py:227:11:</a> RUF029 Function `img` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/lnbits/lnbits/blob/dbb689c5c56017aa2f777327101477864e90c3d5/lnbits/core/views/api.py#L49'>lnbits/core/views/api.py:49:11:</a> RUF029 Function `health` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/lnbits/lnbits/blob/dbb689c5c56017aa2f777327101477864e90c3d5/lnbits/core/views/api.py#L58'>lnbits/core/views/api.py:58:11:</a> RUF029 Function `api_wallets` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/lnbits/lnbits/blob/dbb689c5c56017aa2f777327101477864e90c3d5/lnbits/core/views/auth_api.py#L139'>lnbits/core/views/auth_api.py:139:11:</a> RUF029 Function `logout` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/lnbits/lnbits/blob/dbb689c5c56017aa2f777327101477864e90c3d5/lnbits/core/views/auth_api.py#L50'>lnbits/core/views/auth_api.py:50:11:</a> RUF029 Function `get_auth_user` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/lnbits/lnbits/blob/dbb689c5c56017aa2f777327101477864e90c3d5/lnbits/core/views/payment_api.py#L257'>lnbits/core/views/payment_api.py:257:11:</a> RUF029 Function `api_payments_fee_reserve` is declared `async`, but doesn't `await` or use `async` features.
... 3 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python-trio/trio">python-trio/trio</a> (+0 -57 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/python-trio/trio/blob/c2df486f95fd29d55f08d0746e9ba79dc2207c3f/src/trio/_core/_tests/test_run.py#L102'>src/trio/_core/_tests/test_run.py:102:15:</a> RUF029 Function `inception` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/python-trio/trio/blob/c2df486f95fd29d55f08d0746e9ba79dc2207c3f/src/trio/_core/_tests/test_run.py#L1209'>src/trio/_core/_tests/test_run.py:1209:15:</a> RUF029 Function `crasher` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/python-trio/trio/blob/c2df486f95fd29d55f08d0746e9ba79dc2207c3f/src/trio/_core/_tests/test_run.py#L1220'>src/trio/_core/_tests/test_run.py:1220:15:</a> RUF029 Function `get_and_check_token` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/python-trio/trio/blob/c2df486f95fd29d55f08d0746e9ba79dc2207c3f/src/trio/_core/_tests/test_run.py#L1249'>src/trio/_core/_tests/test_run.py:1249:15:</a> RUF029 Function `main` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/python-trio/trio/blob/c2df486f95fd29d55f08d0746e9ba79dc2207c3f/src/trio/_core/_tests/test_run.py#L1312'>src/trio/_core/_tests/test_run.py:1312:15:</a> RUF029 Function `main` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/python-trio/trio/blob/c2df486f95fd29d55f08d0746e9ba79dc2207c3f/src/trio/_core/_tests/test_run.py#L138'>src/trio/_core/_tests/test_run.py:138:15:</a> RUF029 Function `erroring` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/python-trio/trio/blob/c2df486f95fd29d55f08d0746e9ba79dc2207c3f/src/trio/_core/_tests/test_run.py#L1441'>src/trio/_core/_tests/test_run.py:1441:15:</a> RUF029 Function `agen` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/python-trio/trio/blob/c2df486f95fd29d55f08d0746e9ba79dc2207c3f/src/trio/_core/_tests/test_run.py#L1555'>src/trio/_core/_tests/test_run.py:1555:15:</a> RUF029 Function `child2` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/python-trio/trio/blob/c2df486f95fd29d55f08d0746e9ba79dc2207c3f/src/trio/_core/_tests/test_run.py#L1593'>src/trio/_core/_tests/test_run.py:1593:15:</a> RUF029 Function `child1` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/python-trio/trio/blob/c2df486f95fd29d55f08d0746e9ba79dc2207c3f/src/trio/_core/_tests/test_run.py#L1610'>src/trio/_core/_tests/test_run.py:1610:15:</a> RUF029 Function `func1` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/python-trio/trio/blob/c2df486f95fd29d55f08d0746e9ba79dc2207c3f/src/trio/_core/_tests/test_run.py#L1617'>src/trio/_core/_tests/test_run.py:1617:15:</a> RUF029 Function `check` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/python-trio/trio/blob/c2df486f95fd29d55f08d0746e9ba79dc2207c3f/src/trio/_core/_tests/test_run.py#L1629'>src/trio/_core/_tests/test_run.py:1629:11:</a> RUF029 Function `test_current_effective_deadline` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/python-trio/trio/blob/c2df486f95fd29d55f08d0746e9ba79dc2207c3f/src/trio/_core/_tests/test_run.py#L1671'>src/trio/_core/_tests/test_run.py:1671:15:</a> RUF029 Function `async_gen` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/python-trio/trio/blob/c2df486f95fd29d55f08d0746e9ba79dc2207c3f/src/trio/_core/_tests/test_run.py#L174'>src/trio/_core/_tests/test_run.py:174:15:</a> RUF029 Function `crasher` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/python-trio/trio/blob/c2df486f95fd29d55f08d0746e9ba79dc2207c3f/src/trio/_core/_tests/test_run.py#L1797'>src/trio/_core/_tests/test_run.py:1797:15:</a> RUF029 Function `double_started` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/python-trio/trio/blob/c2df486f95fd29d55f08d0746e9ba79dc2207c3f/src/trio/_core/_tests/test_run.py#L1809'>src/trio/_core/_tests/test_run.py:1809:15:</a> RUF029 Function `raise_keyerror` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/python-trio/trio/blob/c2df486f95fd29d55f08d0746e9ba79dc2207c3f/src/trio/_core/_tests/test_run.py#L1820'>src/trio/_core/_tests/test_run.py:1820:15:</a> RUF029 Function `nothing` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/python-trio/trio/blob/c2df486f95fd29d55f08d0746e9ba79dc2207c3f/src/trio/_core/_tests/test_run.py#L1849'>src/trio/_core/_tests/test_run.py:1849:15:</a> RUF029 Function `started_with_no_checkpoint` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/python-trio/trio/blob/c2df486f95fd29d55f08d0746e9ba79dc2207c3f/src/trio/_core/_tests/test_run.py#L1863'>src/trio/_core/_tests/test_run.py:1863:15:</a> RUF029 Function `raise_keyerror_after_started` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/python-trio/trio/blob/c2df486f95fd29d55f08d0746e9ba79dc2207c3f/src/trio/_core/_tests/test_run.py#L191'>src/trio/_core/_tests/test_run.py:191:15:</a> RUF029 Function `crasher` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/python-trio/trio/blob/c2df486f95fd29d55f08d0746e9ba79dc2207c3f/src/trio/_core/_tests/test_run.py#L1989'>src/trio/_core/_tests/test_run.py:1989:15:</a> RUF029 Function `fail` is declared `async`, but doesn't `await` or use `async` features.
... 35 additional changes omitted for rule RUF029
- <a href='https://github.com/python-trio/trio/blob/c2df486f95fd29d55f08d0746e9ba79dc2207c3f/src/trio/_core/_tests/test_run.py#L49'>src/trio/_core/_tests/test_run.py:49:32:</a> A004 Import `BaseExceptionGroup` is shadowing a Python builtin
- <a href='https://github.com/python-trio/trio/blob/c2df486f95fd29d55f08d0746e9ba79dc2207c3f/src/trio/_core/_tests/test_run.py#L49'>src/trio/_core/_tests/test_run.py:49:52:</a> A004 Import `ExceptionGroup` is shadowing a Python builtin
... 34 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF029 | 68 | 0 | 68 | 0 | 0 |
| A004 | 2 | 0 | 2 | 0 | 0 |

</p>
</details>




---

_@MichaReiser had review dismissed on 2024-08-16 16:50_

Sorry for the confusion about the option. Let's remove it. 

---

_@AlexWaygood reviewed on 2024-08-17 12:50_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/ruff/RUF029_fastapi.py`:1 on 2024-08-17 12:50_

I think I would just add this snippet to the bottom of the existing fixture file rather than creating a new fixture

---

_@TomerBin reviewed on 2024-08-17 12:52_

---

_Review comment by @TomerBin on `crates/ruff_linter/resources/test/fixtures/ruff/RUF029_fastapi.py`:1 on 2024-08-17 12:52_

Ok :) Where should I put the import section? In the top of the file with the existing imports or in the bottom near the new actual tested function?

---

_@AlexWaygood reviewed on 2024-08-17 12:54_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/ruff/RUF029_fastapi.py`:1 on 2024-08-17 12:54_

At the bottom near the new actual tested function -- that way you'll get a nice clean diff for the generated snapshot :-)

---

_@AlexWaygood approved on 2024-08-17 14:21_

Thanks!

---

_Renamed from "Reduce FastAPI false positives in RUF029" to "[`ruff`] Reduce FastAPI false positives in `unused-async` (`RUF029`)" by @AlexWaygood on 2024-08-17 14:21_

---

_Merged by @AlexWaygood on 2024-08-17 14:25_

---

_Closed by @AlexWaygood on 2024-08-17 14:25_

---
