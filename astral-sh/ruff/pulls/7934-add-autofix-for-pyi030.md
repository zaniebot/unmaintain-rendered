```yaml
number: 7934
title: "Add autofix for `PYI030`"
type: pull_request
state: merged
author: diceroll123
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: tweak-PYI030
created_at: 2023-10-13T05:16:02Z
updated_at: 2023-11-30T22:30:19Z
url: https://github.com/astral-sh/ruff/pull/7934
synced_at: 2026-01-12T15:55:25Z
```

# Add autofix for `PYI030`

---

_@diceroll123_

## Summary

Part 2 of implementing the reverted autofix for `PYI030`

Also handles `typing.Union` and `typing_extensions.Literal` etc, uses the first subscript name it finds for each offensive line.

## Test Plan

<!-- How was it tested? -->

`cargo test` and manually

---

_Marked ready for review by @diceroll123 on 2023-10-14 23:24_

---

_@diceroll123 reviewed on 2023-10-14 23:27_

---

_Review comment by @diceroll123 on `crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI030.py`:51 on 2023-10-14 23:27_

The only current outstanding issue is that the test files for `PYI030` suggest that the rule should not emit on this line here, but at the moment it still does on the `main` branch and I haven't added logic to affect this so I'm just gonna ask: What do we want to do here?

---

_Renamed from "Fix PYI030 bug with non-literal unions" to "Add autofix for `PYI030`" by @diceroll123 on 2023-10-14 23:30_

---

_Comment by @github-actions[bot] on 2023-10-14 23:33_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Comment by @zanieb on 2023-10-15 04:49_

Could you gate this fix so it only applies when preview mode is enabled?

---

_Comment by @diceroll123 on 2023-10-15 05:15_

I _believe_ I've done this correctly, doesn't seem like any other fix is gated this way. 🤔 If I've done this wrong, please advise!

---

_Review requested from @zanieb by @zanieb on 2023-10-16 21:28_

---

_Assigned to @zanieb by @zanieb on 2023-10-17 20:38_

---

_Review comment by @zanieb on `crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI030.py`:51 on 2023-10-17 20:39_

👍 no problem

---

_@zanieb reviewed on 2023-10-17 20:39_

---

_@zanieb reviewed on 2023-10-17 20:43_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/flake8_pyi/rules/unnecessary_literal_union.rs`:105 on 2023-10-17 20:43_

It's not clear what this comment refers to. I presume you mean that you'll use the first literal you encounter to construct the fix?

---

_@diceroll123 reviewed on 2023-10-17 20:45_

---

_Review comment by @diceroll123 on `crates/ruff_linter/src/rules/flake8_pyi/rules/unnecessary_literal_union.rs`:105 on 2023-10-17 20:45_

Precisely.

`field25: typing.Union[typing_extensions.Literal[1], typing.Union[Literal[2], typing.Union[Literal[3], Literal[4]]], str] # Error` will use `typing_extensions.Literal` as the only literal after the fix is applied.

Feel free to re-word! 😅 

---

_@zanieb reviewed on 2023-10-17 20:46_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/flake8_pyi/rules/unnecessary_literal_union.rs`:109 on 2023-10-17 20:46_

Should probably include note about `other_exprs` to e.g.
```suggestion
    // Split members into `literal_exprs` if they are a `Literal` annotation  and `other_exprs` otherwise
```

---

_@zanieb reviewed on 2023-10-17 21:02_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/flake8_pyi/rules/unnecessary_literal_union.rs`:125 on 2023-10-17 21:02_

It looks like this could be moved outside of the check for a tuple and deduplicated

---

_@diceroll123 reviewed on 2023-10-17 22:14_

---

_Review comment by @diceroll123 on `crates/ruff_linter/src/rules/flake8_pyi/rules/unnecessary_literal_union.rs`:125 on 2023-10-17 22:14_

woop, silly mistake, will fix.

---

_Comment by @zanieb on 2023-10-28 03:15_

Hey sorry this is on my todo list to review but I got distracted last week – I'll try to get to this on Monday!

---

_Comment by @diceroll123 on 2023-10-28 05:43_

> Hey sorry this is on my todo list to review but I got distracted last week – I'll try to get to this on Monday!

Take your time!

---

_Comment by @github-actions[bot] on 2023-11-24 20:48_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +6 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +6 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/a9132f389163ec6adcf6187def521e04a25685da/airflow/cli/commands/task_command.py#L80'>airflow/cli/commands/task_command.py:80:21:</a> PYI030 Multiple literal members in a union. Use a single literal, e.g. `Literal[False, "db", "memory"]`
+ <a href='https://github.com/apache/airflow/blob/a9132f389163ec6adcf6187def521e04a25685da/airflow/cli/commands/task_command.py#L80'>airflow/cli/commands/task_command.py:80:21:</a> PYI030 [*] Multiple literal members in a union. Use a single literal, e.g. `Literal[False, "db", "memory"]`
- <a href='https://github.com/apache/airflow/blob/a9132f389163ec6adcf6187def521e04a25685da/airflow/models/mappedoperator.py#L88'>airflow/models/mappedoperator.py:88:20:</a> PYI030 Multiple literal members in a union. Use a single literal, e.g. `Literal["expand", "partial"]`
+ <a href='https://github.com/apache/airflow/blob/a9132f389163ec6adcf6187def521e04a25685da/airflow/models/mappedoperator.py#L88'>airflow/models/mappedoperator.py:88:20:</a> PYI030 [*] Multiple literal members in a union. Use a single literal, e.g. `Literal["expand", "partial"]`
- <a href='https://github.com/apache/airflow/blob/a9132f389163ec6adcf6187def521e04a25685da/airflow/providers_manager.py#L195'>airflow/providers_manager.py:195:24:</a> PYI030 Multiple literal members in a union. Use a single literal, e.g. `Literal["source", "package"]`
+ <a href='https://github.com/apache/airflow/blob/a9132f389163ec6adcf6187def521e04a25685da/airflow/providers_manager.py#L195'>airflow/providers_manager.py:195:24:</a> PYI030 [*] Multiple literal members in a union. Use a single literal, e.g. `Literal["source", "package"]`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PYI030 | 6 | 0 | 0 | 6 | 0 |

</p>
</details>




---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/flake8_pyi/rules/unnecessary_literal_union.rs`:105 on 2023-11-30 22:11_

```suggestion
    // for the sake of consistency and correctness, we'll use the first `Literal` subscript attribute
    // to construct the fix
```

---

_@zanieb reviewed on 2023-11-30 22:11_

---

_@zanieb approved on 2023-11-30 22:11_

This lgtm! Thanks for your patience.

---

_Merged by @zanieb on 2023-11-30 22:16_

---

_Closed by @zanieb on 2023-11-30 22:16_

---

_Comment by @codspeed-hq[bot] on 2023-11-30 22:21_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/diceroll123:tweak-PYI030)

### Merging #7934 will **improve performances by 12.27%**

<sub>Comparing <code>diceroll123:tweak-PYI030</code> (2f6dd84) with <code>main</code> (2590aa3)</sub>



### Summary

`⚡ 5` improvements
`✅ 20` untouched benchmarks

`🆕 5` new benchmarks



### Benchmarks breakdown

|     | Benchmark | `main` | `diceroll123:tweak-PYI030` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/all-rules[pydantic/types.py]` | 80.5 ms | 72.1 ms | +11.63% |
| ⚡ | `linter/all-rules[unicode/pypinyin.py]` | 17.2 ms | 16.2 ms | +6.12% |
| 🆕 | `linter/all-with-preview-rules[unicode/pypinyin.py]` | N/A | 17.2 ms | N/A |
| ⚡ | `linter/all-rules[numpy/ctypeslib.py]` | 37 ms | 34.2 ms | +8.23% |
| 🆕 | `linter/all-with-preview-rules[pydantic/types.py]` | N/A | 78.8 ms | N/A |
| ⚡ | `linter/all-rules[large/dataset.py]` | 185.7 ms | 165.4 ms | +12.27% |
| 🆕 | `linter/all-with-preview-rules[large/dataset.py]` | N/A | 185.8 ms | N/A |
| ⚡ | `linter/all-rules[numpy/globals.py]` | 4.1 ms | 3.9 ms | +5.9% |
| 🆕 | `linter/all-with-preview-rules[numpy/ctypeslib.py]` | N/A | 37.3 ms | N/A |
| 🆕 | `linter/all-with-preview-rules[numpy/globals.py]` | N/A | 4.2 ms | N/A |


---

_Label `rule` added by @zanieb on 2023-11-30 22:30_

---

_Label `preview` added by @zanieb on 2023-11-30 22:30_

---
