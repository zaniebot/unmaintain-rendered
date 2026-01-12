```yaml
number: 17941
title: "[`airflow`] Get rid of `Replacement::Name` and replace them with `Replacement::AutoImport` for enabling auto fixing (`AIR301`, `AIR311`)"
type: pull_request
state: merged
author: Lee-W
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: refactor-AIR301-AIR311
created_at: 2025-05-08T10:12:15Z
updated_at: 2025-05-14T15:10:15Z
url: https://github.com/astral-sh/ruff/pull/17941
synced_at: 2026-01-12T15:56:08Z
```

# [`airflow`] Get rid of `Replacement::Name` and replace them with `Replacement::AutoImport` for enabling auto fixing (`AIR301`, `AIR311`)

---

_@Lee-W_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Similiar to https://github.com/astral-sh/ruff/pull/17941.

`Replacement::Name` was designed for linting only. Now, we also want to fix the user code. It would be easier to replace it with a better AutoImport struct whenever possible.

On the other hand, `AIR301` and `AIR311` contain attribute changes that can still use a struct like `Replacement::Name`. To reduce the confusion, I also updated it as `Replacement::AttrName`

Some of the original `Replacement::Name` has been replaced as `Replacement::Message` as they're not directly mapping and the message has now been moved to `help`


## Test Plan

<!-- How was it tested? -->

The test fixtures have been updated


---

_Comment by @github-actions[bot] on 2025-05-08 10:19_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+2 -2 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/8bf7986db4c8db81bc26374e56d5fa11b2793bdf/providers/fab/tests/unit/fab/auth_manager/api_fastapi/conftest.py#L27'>providers/fab/tests/unit/fab/auth_manager/api_fastapi/conftest.py:27:26:</a> AIR301 `appbuilder` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/8bf7986db4c8db81bc26374e56d5fa11b2793bdf/providers/fab/tests/unit/fab/auth_manager/api_fastapi/conftest.py#L27'>providers/fab/tests/unit/fab/auth_manager/api_fastapi/conftest.py:27:26:</a> AIR301 `appbuilder` is removed in Airflow 3.0; The constructor takes no parameter now
+ <a href='https://github.com/apache/airflow/blob/8bf7986db4c8db81bc26374e56d5fa11b2793bdf/providers/fab/tests/unit/fab/auth_manager/test_fab_auth_manager.py#L99'>providers/fab/tests/unit/fab/auth_manager/test_fab_auth_manager.py:99:26:</a> AIR301 `appbuilder` is removed in Airflow 3.0
- <a href='https://github.com/apache/airflow/blob/8bf7986db4c8db81bc26374e56d5fa11b2793bdf/providers/fab/tests/unit/fab/auth_manager/test_fab_auth_manager.py#L99'>providers/fab/tests/unit/fab/auth_manager/test_fab_auth_manager.py:99:26:</a> AIR301 `appbuilder` is removed in Airflow 3.0; The constructor takes no parameter now
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| AIR301 | 4 | 2 | 2 | 0 | 0 |

</p>
</details>




---

_Renamed from "Refactor air301 air311" to "[`airflow`] Get rid of `Replacement::Name` and replace them with `Replacement::AutoImport` for enabling auto fixing (`AIR302`, `AIR312`)" by @Lee-W on 2025-05-08 12:00_

---

_Marked ready for review by @Lee-W on 2025-05-08 12:04_

---

_Renamed from "[`airflow`] Get rid of `Replacement::Name` and replace them with `Replacement::AutoImport` for enabling auto fixing (`AIR302`, `AIR312`)" to "[`airflow`] Get rid of `Replacement::Name` and replace them with `Replacement::AutoImport` for enabling auto fixing (`AIR301`, `AIR311`)" by @Lee-W on 2025-05-09 14:26_

---

_Review requested from @ntBre by @MichaReiser on 2025-05-12 07:42_

---

_Label `rule` added by @ntBre on 2025-05-13 21:52_

---

_Label `preview` added by @ntBre on 2025-05-13 21:52_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:74 on 2025-05-13 21:58_

Do these need  to be separate? It looks like all of the `Replacement::Message`s down below include `Use ... instead` in their messages, so I think you could at least combine these `match` arms if not the entire variants.

```suggestion
            Replacement::AttrName(name) | Replacement::Message(name) => Some(format!("Use `{name}` instead")),
```

Or do you have plans to make the messages more different later on?

---

_@ntBre approved on 2025-05-13 22:02_

This looks good to me, just one question!

---

_@Lee-W reviewed on 2025-05-14 01:36_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:74 on 2025-05-14 01:36_

There are one or two exceptions now. So it's still needed

https://github.com/astral-sh/ruff/blob/8cbd433a3130c692ea8f78894fdfacd6e4a50c27/crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs#L717-L719

I'm thinking of merging the message into other types later on so that we can provide extra information for them as well.

---

_@ntBre reviewed on 2025-05-14 15:10_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:74 on 2025-05-14 15:10_

Ah thanks, I missed that one. I'll merge this!

---

_Merged by @ntBre on 2025-05-14 15:10_

---

_Closed by @ntBre on 2025-05-14 15:10_

---
