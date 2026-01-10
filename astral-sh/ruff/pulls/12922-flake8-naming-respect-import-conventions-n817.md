```yaml
number: 12922
title: "[`flake8-naming`]: Respect import conventions (`N817`)"
type: pull_request
state: merged
author: MichaReiser
labels:
  - incompatibility
assignees: []
merged: true
base: main
head: fix-icn001-n817-incompatibility
created_at: 2024-08-16T09:55:57Z
updated_at: 2024-08-16T15:38:14Z
url: https://github.com/astral-sh/ruff/pull/12922
synced_at: 2026-01-10T21:38:32Z
```

# [`flake8-naming`]: Respect import conventions (`N817`)

---

_Pull request opened by @MichaReiser on 2024-08-16 09:55_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/12916 by respecting the configured import conventions in `N817`. 

I decided to respect the conventions regardless on whether `ICN001` is enabled or not because it makes the change easier to explain (fewer conditions when the behavior applies) 
and seems like a reasonable default.

## Test Plan

Added tests

## Breaking change

One could argue that this is a breaking change because `ET` imports flagged before will now no-longer be flagged, requiring users to adopt their configuration if they want to keep flagging `ET`. 
I think it's fine but interested in more opinions.



---

_Label `incompatibility` added by @MichaReiser on 2024-08-16 09:56_

---

_@MichaReiser reviewed on 2024-08-16 09:57_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pep8_naming/snapshots/ruff_linter__rules__pep8_naming__tests__camelcase_imported_as_incorrect_convention.snap`:18 on 2024-08-16 09:57_

The configuration for this test run specifies that the convention for importing `ElementTree` is `XET`. That's why `ET` gets flagged.

---

_Comment by @github-actions[bot] on 2024-08-16 10:23_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -2 violations, +0 -0 fixes in 1 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/e35ae2db7c56a23b2f7abbd7c4269221ed98ead6/scripts/ci/testing/summarize_junit_failures.py#L23'>scripts/ci/testing/summarize_junit_failures.py:23:8:</a> N817 CamelCase `ElementTree` imported as acronym `ET`
- <a href='https://github.com/apache/airflow/blob/e35ae2db7c56a23b2f7abbd7c4269221ed98ead6/scripts/in_container/check_junitxml_result.py#L21'>scripts/in_container/check_junitxml_result.py:21:8:</a> N817 CamelCase `ElementTree` imported as acronym `ET`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| N817 | 2 | 0 | 2 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -2 violations, +0 -0 fixes in 1 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/e35ae2db7c56a23b2f7abbd7c4269221ed98ead6/scripts/ci/testing/summarize_junit_failures.py#L23'>scripts/ci/testing/summarize_junit_failures.py:23:8:</a> N817 CamelCase `ElementTree` imported as acronym `ET`
- <a href='https://github.com/apache/airflow/blob/e35ae2db7c56a23b2f7abbd7c4269221ed98ead6/scripts/in_container/check_junitxml_result.py#L21'>scripts/in_container/check_junitxml_result.py:21:8:</a> N817 CamelCase `ElementTree` imported as acronym `ET`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| N817 | 2 | 0 | 2 | 0 | 0 |

</p>
</details>




---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pep8_naming/rules/camelcase_imported_as_acronym.rs`:28 on 2024-08-16 13:22_

```suggestion
/// [`lint.flake8-import-conventions.banned-aliases`] option are allowed.
///
```

Also, I notice you used square brackets but didn't add a link at the end. Was that deliberate -- do we have a mechanism that adds links automatically to settings?
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pep8_naming/snapshots/ruff_linter__rules__pep8_naming__tests__camelcase_imported_as_incorrect_convention.snap`:18 on 2024-08-16 13:25_

And the pre-existing test ensures that `import ElementTree as ET` is _not_ flagged with the default settings, correct?

---

_@AlexWaygood approved on 2024-08-16 13:26_

LGTM. I think this is what users would intuitively expect and is a reduction in scope, so I don't see it as particularly breaking

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pep8_naming/snapshots/ruff_linter__rules__pep8_naming__tests__camelcase_imported_as_incorrect_convention.snap`:18 on 2024-08-16 15:19_

Yes

---

_@MichaReiser reviewed on 2024-08-16 15:19_

---

_Renamed from "[`flake8-naming`]: Respect import conventions (`N8171`)" to "[`flake8-naming`]: Respect import conventions (`N817`)" by @MichaReiser on 2024-08-16 15:22_

---

_@MichaReiser reviewed on 2024-08-16 15:23_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pep8_naming/rules/camelcase_imported_as_acronym.rs`:28 on 2024-08-16 15:23_

Yes, we have some fancy script that resolves settings (and validates them)

![image](https://github.com/user-attachments/assets/5f8ddb96-b9bd-4945-83ed-2d3aa8025f99)


---

_@AlexWaygood reviewed on 2024-08-16 15:28_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pep8_naming/rules/camelcase_imported_as_acronym.rs`:28 on 2024-08-16 15:28_

Fancy script or no fancy script, you're still referring to the wrong setting here: the `lint.flake8-boolean-trap.extend-allowed-calls` setting has nothing to do with this rule

---

_Merged by @MichaReiser on 2024-08-16 15:28_

---

_Closed by @MichaReiser on 2024-08-16 15:28_

---

_Branch deleted on 2024-08-16 15:28_

---

_@AlexWaygood reviewed on 2024-08-16 15:33_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pep8_naming/rules/camelcase_imported_as_acronym.rs`:28 on 2024-08-16 15:33_

I filed #12935 to address this

---
