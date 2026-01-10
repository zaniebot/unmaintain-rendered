```yaml
number: 10713
title: "[`pyupgrade`] Replace str,Enum with StrEnum `UP042`"
type: pull_request
state: merged
author: Warchant
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: main
created_at: 2024-04-01T15:17:01Z
updated_at: 2024-04-06T01:59:16Z
url: https://github.com/astral-sh/ruff/pull/10713
synced_at: 2026-01-10T22:47:02Z
```

# [`pyupgrade`] Replace str,Enum with StrEnum `UP042`

---

_Pull request opened by @Warchant on 2024-04-01 15:17_

<sup>Disclaimer: I am first time contributor to Ruff, relatively new to Rust. Doing this to learn the language, study how ruff project is structured and to pay back for using my favourite python tool :)</sup>

## Summary

Add new rule `pyupgrade - UP042` (I picked next available number).
Closes https://github.com/astral-sh/ruff/discussions/3867
Closes https://github.com/astral-sh/ruff/issues/9569

It should warn + provide a fix `class A(str, Enum)` -> `class A(StrEnum)` for py311+.

## Test Plan

Added UP042.py test.

## Notes

I did not find a way to call `remove_argument` 2 times consecutively, so the automatic fixing works only for classes that inherit exactly `str, Enum` (regardless of the order).

I also plan to extend this rule to support IntEnum in next PR.

---

_Comment by @codspeed-hq[bot] on 2024-04-01 15:22_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/Warchant:main)

### Merging #10713 will **not alter performance**

<sub>Comparing <code>Warchant:main</code> (d677f25) with <code>main</code> (323264d)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_Marked ready for review by @Warchant on 2024-04-01 20:45_

---

_Comment by @github-actions[bot] on 2024-04-01 20:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +0 -0 fixes in 1 projects; 43 projects unchanged)

<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/indico/indico/blob/367d9abbec32c3aba1d13901ac67b04923d21144/indico/modules/admin/notices.py#L40'>indico/modules/admin/notices.py:40:1:</a> UP042 Class NoticeSeverity inherits from both `str` and `enum.Enum`. Prefer `enum.StrEnum` instead.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| UP042 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -1 violations, +0 -0 fixes in 1 projects; 43 projects unchanged)

<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/indico/indico/blob/367d9abbec32c3aba1d13901ac67b04923d21144/indico/modules/admin/notices.py#L40'>indico/modules/admin/notices.py:40:1:</a> UP042 Class NoticeSeverity inherits from both `str` and `enum.Enum`. Prefer `enum.StrEnum` instead.
+ <a href='https://github.com/indico/indico/blob/367d9abbec32c3aba1d13901ac67b04923d21144/indico/modules/admin/notices.py#L40'>indico/modules/admin/notices.py:40:7:</a> UP042 Class NoticeSeverity inherits from both `str` and `enum.Enum`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| UP042 | 2 | 1 | 1 | 0 | 0 |

</p>
</details>




---

_Label `rule` added by @MichaReiser on 2024-04-02 09:12_

---

_Comment by @Warchant on 2024-04-05 20:42_

Anyone? 

---

_Comment by @charliermarsh on 2024-04-05 20:57_

I’ll take a look today :)

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-05 20:57_

---

_@charliermarsh approved on 2024-04-06 01:41_

---

_Comment by @charliermarsh on 2024-04-06 01:43_

This is great! I made some changes to the documentation and rule messages just to better align with our style elsewhere.

---

_@charliermarsh reviewed on 2024-04-06 01:43_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pyupgrade/rules/replace_str_enum.rs`:113 on 2024-04-06 01:43_

I changed this to a `match` over the segments, rather than an `if`-`else`.

---

_@charliermarsh reviewed on 2024-04-06 01:43_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pyupgrade/rules/replace_str_enum.rs`:93 on 2024-04-06 01:43_

We prefer to omit the _fix_ in the rule message.

---

_@charliermarsh reviewed on 2024-04-06 01:44_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pyupgrade/rules/replace_str_enum.rs`:135 on 2024-04-06 01:44_

The use of `.identifier()` here is nice because it confines the range of the diagnostic to the class name, rather than the _entire_ class definition (which would include the class body).


---

_@charliermarsh reviewed on 2024-04-06 01:44_

---

_Review comment by @charliermarsh on `crates/ruff_linter/resources/test/fixtures/pyupgrade/UP042.py`:13 on 2024-04-06 01:44_

(I ran `ruff format` over the fixture. It's not a hard rule, since fixtures are often intentionally unformatted, but it's nice when we don't rely on the formatting anywhere.)

---

_Label `preview` added by @charliermarsh on 2024-04-06 01:46_

---

_Comment by @charliermarsh on 2024-04-06 01:47_

Left a few clarifying comments on changes that I made, but please ask questions if you'd like.

---

_Merged by @charliermarsh on 2024-04-06 01:56_

---

_Closed by @charliermarsh on 2024-04-06 01:56_

---
