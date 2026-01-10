```yaml
number: 12579
title: Improve handling of metaclasses in various linter rules
type: pull_request
state: merged
author: AlexWaygood
labels:
  - linter
assignees: []
merged: true
base: main
head: alex/is-metaclass-centralize
created_at: 2024-07-30T12:56:12Z
updated_at: 2024-07-30T13:48:38Z
url: https://github.com/astral-sh/ruff/pull/12579
synced_at: 2026-01-10T21:47:02Z
```

# Improve handling of metaclasses in various linter rules

---

_Pull request opened by @AlexWaygood on 2024-07-30 12:56_

## Summary

This PR does several interrelated things:
1. The first thing it does is move an `is_metaclass` function out of `crates/ruff_linter/src/rules/flake8_pyi/rules/non_self_return_type.rs` and into the `ruff_python_semantic` crate. It's a generally useful function, and we were doing similar analysis elsewhere (but we were doing it incorrectly). It's better to centralize it in one place.
2. The second thing it does is that it changes the `classify()` function in `ruff_python_semantic::analyze::function_type` so that it no longer reports that metaclass instance methods are classmethods. For our `pep8-naming` rules, it makes sense to treat metaclass instance methods like classmethods, since metaclass instance methods usually use `cls` for the name of the first parameter, like classmethods on non-metaclasses. However, the `classify()` function is used by many other linter rules, not just our `pep8-naming` rules. For those rules, it doesn't make sense for metaclass instance methods to be considered classmethods, since, well, they're not. An example is B019, which I've added a test for in this PR: I'm not sure it makes sense to treat instance methods on metaclasses any differently to instance methods on regular classes for that rule.
3. Since `ruff_python_semantic::analyze::function_type::classify()` no longer considers instance methods on metaclasses to be classmethods, I adjusted the `pep8-naming` rules so that they now do their own check to see whether an instance method is a method on a metaclass, in order to determine what the name of the first parameter should be.

FWIW, there were several buggy things in the check for metaclasses that `ruff_python_semantic::analyze::function_type::classify()` was doing:
- It was using `map_callable()` when it should have been using `map_subscript()`
- It was missing the `enum`-module metaclasses that the stdlib provides
- It was only looking at the direct bases on the class, instead of iterating up through the bases of all superclasses

## Test Plan

`cargo test -p ruff_linter`


---

_Label `linter` added by @AlexWaygood on 2024-07-30 12:56_

---

_Review requested from @charliermarsh by @AlexWaygood on 2024-07-30 12:56_

---

_Comment by @github-actions[bot] on 2024-07-30 13:09_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+4 -4 violations, +0 -0 fixes in 1 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+4 -4 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/41508f23ad87bdf410978ff96e62d8057d17d9d0/airflow/stats.py#L46'>airflow/stats.py:46:24:</a> ARG002 Unused method argument: `args`
- <a href='https://github.com/apache/airflow/blob/41508f23ad87bdf410978ff96e62d8057d17d9d0/airflow/stats.py#L46'>airflow/stats.py:46:24:</a> ARG003 Unused class method argument: `args`
+ <a href='https://github.com/apache/airflow/blob/41508f23ad87bdf410978ff96e62d8057d17d9d0/airflow/stats.py#L46'>airflow/stats.py:46:32:</a> ARG002 Unused method argument: `kwargs`
- <a href='https://github.com/apache/airflow/blob/41508f23ad87bdf410978ff96e62d8057d17d9d0/airflow/stats.py#L46'>airflow/stats.py:46:32:</a> ARG003 Unused class method argument: `kwargs`
+ <a href='https://github.com/apache/airflow/blob/41508f23ad87bdf410978ff96e62d8057d17d9d0/airflow/traces/tracer.py#L259'>airflow/traces/tracer.py:259:24:</a> ARG002 Unused method argument: `args`
- <a href='https://github.com/apache/airflow/blob/41508f23ad87bdf410978ff96e62d8057d17d9d0/airflow/traces/tracer.py#L259'>airflow/traces/tracer.py:259:24:</a> ARG003 Unused class method argument: `args`
+ <a href='https://github.com/apache/airflow/blob/41508f23ad87bdf410978ff96e62d8057d17d9d0/airflow/traces/tracer.py#L259'>airflow/traces/tracer.py:259:32:</a> ARG002 Unused method argument: `kwargs`
- <a href='https://github.com/apache/airflow/blob/41508f23ad87bdf410978ff96e62d8057d17d9d0/airflow/traces/tracer.py#L259'>airflow/traces/tracer.py:259:32:</a> ARG003 Unused class method argument: `kwargs`
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| ARG002 | 4 | 4 | 0 | 0 | 0 |
| ARG003 | 4 | 0 | 4 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+4 -4 violations, +0 -0 fixes in 1 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+4 -4 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/41508f23ad87bdf410978ff96e62d8057d17d9d0/airflow/stats.py#L46'>airflow/stats.py:46:24:</a> ARG002 Unused method argument: `args`
- <a href='https://github.com/apache/airflow/blob/41508f23ad87bdf410978ff96e62d8057d17d9d0/airflow/stats.py#L46'>airflow/stats.py:46:24:</a> ARG003 Unused class method argument: `args`
+ <a href='https://github.com/apache/airflow/blob/41508f23ad87bdf410978ff96e62d8057d17d9d0/airflow/stats.py#L46'>airflow/stats.py:46:32:</a> ARG002 Unused method argument: `kwargs`
- <a href='https://github.com/apache/airflow/blob/41508f23ad87bdf410978ff96e62d8057d17d9d0/airflow/stats.py#L46'>airflow/stats.py:46:32:</a> ARG003 Unused class method argument: `kwargs`
+ <a href='https://github.com/apache/airflow/blob/41508f23ad87bdf410978ff96e62d8057d17d9d0/airflow/traces/tracer.py#L259'>airflow/traces/tracer.py:259:24:</a> ARG002 Unused method argument: `args`
- <a href='https://github.com/apache/airflow/blob/41508f23ad87bdf410978ff96e62d8057d17d9d0/airflow/traces/tracer.py#L259'>airflow/traces/tracer.py:259:24:</a> ARG003 Unused class method argument: `args`
+ <a href='https://github.com/apache/airflow/blob/41508f23ad87bdf410978ff96e62d8057d17d9d0/airflow/traces/tracer.py#L259'>airflow/traces/tracer.py:259:32:</a> ARG002 Unused method argument: `kwargs`
- <a href='https://github.com/apache/airflow/blob/41508f23ad87bdf410978ff96e62d8057d17d9d0/airflow/traces/tracer.py#L259'>airflow/traces/tracer.py:259:32:</a> ARG003 Unused class method argument: `kwargs`
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| ARG002 | 4 | 4 | 0 | 0 | 0 |
| ARG003 | 4 | 0 | 4 | 0 | 0 |

</p>
</details>




---

_Comment by @AlexWaygood on 2024-07-30 13:18_

The ecosystem changes look good, in my opinion. I think those are all improvements in error messages.

---

_@charliermarsh approved on 2024-07-30 13:45_

---

_Merged by @AlexWaygood on 2024-07-30 13:48_

---

_Closed by @AlexWaygood on 2024-07-30 13:48_

---

_Branch deleted on 2024-07-30 13:48_

---
