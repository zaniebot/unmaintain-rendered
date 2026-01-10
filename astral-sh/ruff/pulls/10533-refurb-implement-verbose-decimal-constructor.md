```yaml
number: 10533
title: "[`refurb`] Implement `verbose-decimal-constructor` (`FURB157`)"
type: pull_request
state: merged
author: hikaru-kajita
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: simplify-decimal-ctor
created_at: 2024-03-23T06:30:32Z
updated_at: 2024-03-25T02:32:42Z
url: https://github.com/astral-sh/ruff/pull/10533
synced_at: 2026-01-10T22:47:02Z
```

# [`refurb`] Implement `verbose-decimal-constructor` (`FURB157`)

---

_Pull request opened by @hikaru-kajita on 2024-03-23 06:30_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Implement FURB157 in the issue #1348.
Relevant Refurb docs is here: https://github.com/dosisod/refurb/blob/master/docs/checks.md#furb157-simplify-decimal-ctor

## Test Plan

<!-- How was it tested? -->

I've written it in the `FURB157.py`.

---

_Comment by @github-actions[bot] on 2024-03-23 06:49_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+14 -0 violations, +0 -0 fixes in 4 projects; 40 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/20cb9f1770c819f31e48748fc9fb86527f739d6e/tests/providers/teradata/transfers/test_teradata_to_teradata.py#L86'>tests/providers/teradata/transfers/test_teradata_to_teradata.py:86:33:</a> FURB157 [*] Verbose expression in `Decimal` constructor
+ <a href='https://github.com/apache/airflow/blob/20cb9f1770c819f31e48748fc9fb86527f739d6e/tests/providers/teradata/transfers/test_teradata_to_teradata.py#L86'>tests/providers/teradata/transfers/test_teradata_to_teradata.py:86:58:</a> FURB157 [*] Verbose expression in `Decimal` constructor
+ <a href='https://github.com/apache/airflow/blob/20cb9f1770c819f31e48748fc9fb86527f739d6e/tests/providers/teradata/transfers/test_teradata_to_teradata.py#L86'>tests/providers/teradata/transfers/test_teradata_to_teradata.py:86:83:</a> FURB157 [*] Verbose expression in `Decimal` constructor
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+7 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/75bce14e93352af68318452acecfbe1af4a23be4/rotkehlchen/fval.py#L63'>rotkehlchen/fval.py:63:68:</a> FURB157 [*] Verbose expression in `Decimal` constructor
+ <a href='https://github.com/rotki/rotki/blob/75bce14e93352af68318452acecfbe1af4a23be4/rotkehlchen/fval.py#L67'>rotkehlchen/fval.py:67:68:</a> FURB157 [*] Verbose expression in `Decimal` constructor
+ <a href='https://github.com/rotki/rotki/blob/75bce14e93352af68318452acecfbe1af4a23be4/rotkehlchen/fval.py#L71'>rotkehlchen/fval.py:71:69:</a> FURB157 [*] Verbose expression in `Decimal` constructor
+ <a href='https://github.com/rotki/rotki/blob/75bce14e93352af68318452acecfbe1af4a23be4/rotkehlchen/fval.py#L71'>rotkehlchen/fval.py:71:84:</a> FURB157 [*] Verbose expression in `Decimal` constructor
+ <a href='https://github.com/rotki/rotki/blob/75bce14e93352af68318452acecfbe1af4a23be4/rotkehlchen/fval.py#L75'>rotkehlchen/fval.py:75:69:</a> FURB157 [*] Verbose expression in `Decimal` constructor
+ <a href='https://github.com/rotki/rotki/blob/75bce14e93352af68318452acecfbe1af4a23be4/rotkehlchen/fval.py#L75'>rotkehlchen/fval.py:75:83:</a> FURB157 [*] Verbose expression in `Decimal` constructor
+ <a href='https://github.com/rotki/rotki/blob/75bce14e93352af68318452acecfbe1af4a23be4/rotkehlchen/fval.py#L86'>rotkehlchen/fval.py:86:68:</a> FURB157 [*] Verbose expression in `Decimal` constructor
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/e5d50d9787f735a1fe5e9c8ff080ccd8b54d35f3/corporate/tests/test_stripe.py#L5525'>corporate/tests/test_stripe.py:5525:82:</a> FURB157 [*] Verbose expression in `Decimal` constructor
+ <a href='https://github.com/zulip/zulip/blob/e5d50d9787f735a1fe5e9c8ff080ccd8b54d35f3/corporate/tests/test_stripe.py#L5587'>corporate/tests/test_stripe.py:5587:41:</a> FURB157 [*] Verbose expression in `Decimal` constructor
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/indico/indico/blob/bd40933e8357b0674af92e2f33fd4d75d19f9ca0/indico/modules/events/registration/models/registrations.py#L510'>indico/modules/events/registration/models/registrations.py:510:49:</a> FURB157 [*] Verbose expression in `Decimal` constructor
+ <a href='https://github.com/indico/indico/blob/bd40933e8357b0674af92e2f33fd4d75d19f9ca0/indico/modules/events/registration/models/registrations.py#L511'>indico/modules/events/registration/models/registrations.py:511:61:</a> FURB157 [*] Verbose expression in `Decimal` constructor
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FURB157 | 14 | 14 | 0 | 0 | 0 |

</p>
</details>




---

_Review requested from @charliermarsh by @charliermarsh on 2024-03-24 20:15_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-24 20:15_

---

_Label `rule` added by @charliermarsh on 2024-03-24 20:15_

---

_Label `preview` added by @charliermarsh on 2024-03-24 20:15_

---

_@charliermarsh approved on 2024-03-25 02:08_

---

_Renamed from "[`Refurb`] Implement `simplify-decimal-ctor` (`FURB157`)" to "[`Refurb`] Implement `verbose-decimal-constructor` (`FURB157`)" by @charliermarsh on 2024-03-25 02:09_

---

_Renamed from "[`Refurb`] Implement `verbose-decimal-constructor` (`FURB157`)" to "[`refurb`] Implement `verbose-decimal-constructor` (`FURB157`)" by @charliermarsh on 2024-03-25 02:09_

---

_Comment by @codspeed-hq[bot] on 2024-03-25 02:16_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/boolean-light:simplify-decimal-ctor)

### Merging #10533 will **degrade performances by 5.31%**

<sub>Comparing <code>boolean-light:simplify-decimal-ctor</code> (655821b) with <code>main</code> (22f237f)</sub>



### Summary

`❌ 2` regressions
`✅ 28` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/boolean-light:simplify-decimal-ctor)._

### Benchmarks breakdown

|     | Benchmark | `main` | `boolean-light:simplify-decimal-ctor` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/default-rules[large/dataset.py]` | 19.3 ms | 20.3 ms | -5.31% |
| ❌ | `linter/default-rules[unicode/pypinyin.py]` | 1.5 ms | 1.6 ms | -4.51% |


---

_Comment by @charliermarsh on 2024-03-25 02:18_

That looks like a false positive given that the default rule set didn't change here... I'll re-run though.

---

_Merged by @charliermarsh on 2024-03-25 02:28_

---

_Closed by @charliermarsh on 2024-03-25 02:28_

---
