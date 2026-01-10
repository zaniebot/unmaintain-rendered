```yaml
number: 11370
title: "fix(F822): Enable F822 in __init__.py files by default"
type: pull_request
state: merged
author: hassec
labels:
  - preview
assignees: []
merged: true
base: main
head: enable_f822_in_init
created_at: 2024-05-11T17:32:26Z
updated_at: 2024-06-03T12:04:08Z
url: https://github.com/astral-sh/ruff/pull/11370
synced_at: 2026-01-10T21:55:59Z
```

# fix(F822): Enable F822 in __init__.py files by default

---

_Pull request opened by @hassec on 2024-05-11 17:32_

**NOTE**
The actual implementation of this PR changed during review, the below description is outdated.

## Summary

This PR aims to close #10095 by adding an option `init-allow-undef-export` to the `pyflakes` settings. This option is currently set to `true` such that behavior is kept identical.
But setting this option to `false` will lead to `F822` warnings to be shown in all files, **including** `__init__.py` files.

As I've mentioned on #10095, I think `init-allow-undef-export=false` would be the more user-friendly default option, as it creates fewer surprises. @charliermarsh what do you think about making that the default? 

With this option in place, it's a single line fix for people that rely on the old behavior.

And thinking longer term, for future major releases, one could probably consider deprecating the option and eventually having people just `noqa` these warnings if they are not wanted.


## Test Plan

I've added a `test_init_f822_enabled` test which repeats the test that is done in the `init` test but this time with `init-allow-undef-export=false` and the snap file correctly shows that ruff will then trigger the otherwise suppressed F822 warning.


closes #10095

---

_Comment by @github-actions[bot] on 2024-05-11 17:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+12 -0 violations, +0 -0 fixes in 5 projects; 45 projects unchanged)

<details><summary><a href="https://github.com/PostHog/HouseWatch">PostHog/HouseWatch</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/PostHog/HouseWatch/blob/849261caaa4ac78aeddb64c4bcae23088efb6181/housewatch/tasks/__init__.py#L5'>housewatch/tasks/__init__.py:5:43:</a> F822 Undefined name `customer_report` in `__all__`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/0b9232e54d61357930c71737fb51657c834fc6d7/airflow/providers/cncf/kubernetes/utils/__init__.py#L19'>airflow/providers/cncf/kubernetes/utils/__init__.py:19:12:</a> F822 Undefined name `xcom_sidecar` in `__all__`
+ <a href='https://github.com/apache/airflow/blob/0b9232e54d61357930c71737fb51657c834fc6d7/airflow/providers/cncf/kubernetes/utils/__init__.py#L19'>airflow/providers/cncf/kubernetes/utils/__init__.py:19:28:</a> F822 Undefined name `pod_manager` in `__all__`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/mlflow/mlflow/blob/89c44d9e2468cc083a7fd6b3cb6d9921982fd6aa/mlflow/metrics/__init__.py#L444'>mlflow/metrics/__init__.py:444:5:</a> F822 Undefined name `accuracy` in `__all__`
+ <a href='https://github.com/mlflow/mlflow/blob/89c44d9e2468cc083a7fd6b3cb6d9921982fd6aa/mlflow/metrics/__init__.py#L456'>mlflow/metrics/__init__.py:456:5:</a> F822 Undefined name `binary_recall` in `__all__`
+ <a href='https://github.com/mlflow/mlflow/blob/89c44d9e2468cc083a7fd6b3cb6d9921982fd6aa/mlflow/metrics/__init__.py#L457'>mlflow/metrics/__init__.py:457:5:</a> F822 Undefined name `binary_precision` in `__all__`
+ <a href='https://github.com/mlflow/mlflow/blob/89c44d9e2468cc083a7fd6b3cb6d9921982fd6aa/mlflow/metrics/__init__.py#L458'>mlflow/metrics/__init__.py:458:5:</a> F822 Undefined name `binary_f1_score` in `__all__`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/prefecthq/prefect">prefecthq/prefect</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/prefecthq/prefect/blob/0b359a7a03daab3021402869c2cd7dc79049f5d5/src/prefect/blocks/__init__.py#L7'>src/prefect/blocks/__init__.py:7:12:</a> F822 Undefined name `notifications` in `__all__`
+ <a href='https://github.com/prefecthq/prefect/blob/0b359a7a03daab3021402869c2cd7dc79049f5d5/src/prefect/blocks/__init__.py#L7'>src/prefect/blocks/__init__.py:7:29:</a> F822 Undefined name `system` in `__all__`
+ <a href='https://github.com/prefecthq/prefect/blob/0b359a7a03daab3021402869c2cd7dc79049f5d5/src/prefect/blocks/__init__.py#L7'>src/prefect/blocks/__init__.py:7:39:</a> F822 Undefined name `webhook` in `__all__`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/indico/indico/blob/105617f251881a9b1a139cc260e26df12f264b34/indico/modules/api/__init__.py#L20'>indico/modules/api/__init__.py:20:12:</a> F822 Undefined name `settings` in `__all__`
+ <a href='https://github.com/indico/indico/blob/105617f251881a9b1a139cc260e26df12f264b34/indico/modules/logs/__init__.py#L19'>indico/modules/logs/__init__.py:19:60:</a> F822 Undefined name `CategoryLogRealm` in `__all__`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| F822 | 12 | 12 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @charliermarsh on 2024-05-16 02:11_

I'm wondering if we should instead always enable this, and ask users to add `F822` to `per-file-ignores` if they want it disabled in that context. Could you try always enabling this, and see if the ecosystem checks surface any violations?

---

_Comment by @charliermarsh on 2024-05-16 02:12_

I.e., if we think tis should be the default, maybe we just make it the default and use the existing `per-file-ignores` mechanism for those that need to turn it off?

---

_Comment by @hassec on 2024-05-16 02:32_

Thanks for taking a look at this @charliermarsh. 
I like your suggestion, I'd be happy to have this be the default :blush: 

I've changed the PR to simply remove the special handling of F822 warnings from `__init__.py` files.


---

_Comment by @hassec on 2024-05-26 00:53_

@charliermarsh just a gentle reminder 😊 
To me, the ecosystem checks actually show that it would be beneficial to make this the default, and I bet most people assume that their `__init__.py` files are getting checked already.
And I like your suggestion of just having people use `per-file-ignore` for the few cases where it would be a false positive.

---

_Comment by @zanieb on 2024-05-26 14:46_

Could you make this change only apply when preview mode is enabled so we don't increase the scope of the rule without a stabilization period? I think it'd be `checker.settings.preview.is_enabled()`

---

_Comment by @hassec on 2024-05-30 00:15_

@zanieb apologies for the slow reply, it's been a busy work week so far. 

I've made the suggested change and added a test for the preview to make sure the preview enabled scenario is tested too.

How long do changes like this usually have to be in preview, before they can become stable?

---

_Comment by @charliermarsh on 2024-05-30 00:52_

Thanks @hassec. Will take a look shortly. We tend to leave a change in preview for one minor release (so this would be elevated in v0.6.0, since v0.5.0 will go out in the next few weeks).

---

_Label `preview` added by @charliermarsh on 2024-05-30 03:11_

---

_Merged by @charliermarsh on 2024-05-30 03:15_

---

_Closed by @charliermarsh on 2024-05-30 03:15_

---

_Comment by @codspeed-hq[bot] on 2024-05-30 03:16_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/hassec:enable_f822_in_init)

### Merging #11370 will **improve performances by 7.9%**

<sub>Comparing <code>hassec:enable_f822_in_init</code> (164d4cf) with <code>hassec:enable_f822_in_init</code> (686ef15)</sub>



### Summary

`⚡ 1` improvements
`✅ 29` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `hassec:enable_f822_in_init` | `hassec:enable_f822_in_init` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `parser[numpy/ctypeslib.py]` | 1.2 ms | 1.1 ms | +7.9% |


---

_Comment by @Avasam on 2024-06-01 17:46_

Hey thanks for the change! Just a nitpick as a user about the changelog entry, it says
> [pyflakes] Add option to enable F822 in __init__.py files (https://github.com/astral-sh/ruff/pull/11370)

So I went looking for the new option, even reading this PR's description and looking for `init-allow-undef-export`, but turns out this is just enabled by default, with an appropriate `lint.per-file-ignores` recommendation to disable ^^"

---

_Comment by @charliermarsh on 2024-06-01 18:04_

Thanks, good call!

---

_Renamed from "fix(F822): add option to enable F822 in __init__.py files" to "fix(F822): Enable F822 in __init__.py files by default" by @hassec on 2024-06-03 12:02_

---
