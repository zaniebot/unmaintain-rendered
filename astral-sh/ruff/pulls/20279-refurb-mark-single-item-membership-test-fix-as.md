```yaml
number: 20279
title: "[`refurb`] Mark `single-item-membership-test` fix as always unsafe (`FURB171`)"
type: pull_request
state: merged
author: TaKO8Ki
labels:
  - fixes
  - preview
assignees: []
merged: true
base: main
head: furb171-always-unsafe
created_at: 2025-09-06T09:28:48Z
updated_at: 2025-09-18T15:32:23Z
url: https://github.com/astral-sh/ruff/pull/20279
synced_at: 2026-01-10T17:40:28Z
```

# [`refurb`] Mark `single-item-membership-test` fix as always unsafe (`FURB171`)

---

_Pull request opened by @TaKO8Ki on 2025-09-06 09:28_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fixes #20255

Mark single-item-membership-test fixes as always unsafe

- Always set `Applicability::Unsafe` for FURB171 fixes
- Update “Fix safety” docs to reflect always-unsafe behavior
- Expand tests (not in, nested set/frozenset, commented args)

## Test Plan

<!-- How was it tested? -->

I have added new test cases to `crates/ruff_linter/resources/test/fixtures/refurb/FURB171_0.py` and `crates/ruff_linter/resources/test/fixtures/refurb/FURB171_1.py`.


---

_@TaKO8Ki reviewed on 2025-09-06 09:34_

---

_Review comment by @TaKO8Ki on `crates/ruff_linter/src/rules/refurb/rules/single_item_membership_test.rs`:102 on 2025-09-06 09:34_

Previously, fixes were marked unsafe only for string literals and when comments intersected the edit range, but single‑item list/tuple/set/frozenset cases can also
  change behavior (e.g., NaN identity differences and non‑boolean eq), so we now mark all FURB171 fixes as unsafe.

---

_Comment by @github-actions[bot] on 2025-09-06 09:40_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +0 -64 fixes in 5 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +0 -44 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/6a10bc6bf9446ae1ceeaeda95e9b8ee0d867bf99/airflow-core/src/airflow/utils/db.py#L1419'>airflow-core/src/airflow/utils/db.py:1419:8:</a> FURB171 Membership test against single-item container
- <a href='https://github.com/apache/airflow/blob/6a10bc6bf9446ae1ceeaeda95e9b8ee0d867bf99/airflow-core/src/airflow/utils/db.py#L1419'>airflow-core/src/airflow/utils/db.py:1419:8:</a> FURB171 [*] Membership test against single-item container
+ <a href='https://github.com/apache/airflow/blob/6a10bc6bf9446ae1ceeaeda95e9b8ee0d867bf99/airflow-core/tests/unit/cli/commands/test_variable_command.py#L516'>airflow-core/tests/unit/cli/commands/test_variable_command.py:516:16:</a> FURB171 Membership test against single-item container
- <a href='https://github.com/apache/airflow/blob/6a10bc6bf9446ae1ceeaeda95e9b8ee0d867bf99/airflow-core/tests/unit/cli/commands/test_variable_command.py#L516'>airflow-core/tests/unit/cli/commands/test_variable_command.py:516:16:</a> FURB171 [*] Membership test against single-item container
+ <a href='https://github.com/apache/airflow/blob/6a10bc6bf9446ae1ceeaeda95e9b8ee0d867bf99/airflow-core/tests/unit/cli/commands/test_variable_command.py#L533'>airflow-core/tests/unit/cli/commands/test_variable_command.py:533:16:</a> FURB171 Membership test against single-item container
- <a href='https://github.com/apache/airflow/blob/6a10bc6bf9446ae1ceeaeda95e9b8ee0d867bf99/airflow-core/tests/unit/cli/commands/test_variable_command.py#L533'>airflow-core/tests/unit/cli/commands/test_variable_command.py:533:16:</a> FURB171 [*] Membership test against single-item container
+ <a href='https://github.com/apache/airflow/blob/6a10bc6bf9446ae1ceeaeda95e9b8ee0d867bf99/dev/breeze/src/airflow_breeze/utils/run_tests.py#L427'>dev/breeze/src/airflow_breeze/utils/run_tests.py:427:8:</a> FURB171 Membership test against single-item container
- <a href='https://github.com/apache/airflow/blob/6a10bc6bf9446ae1ceeaeda95e9b8ee0d867bf99/dev/breeze/src/airflow_breeze/utils/run_tests.py#L427'>dev/breeze/src/airflow_breeze/utils/run_tests.py:427:8:</a> FURB171 [*] Membership test against single-item container
+ <a href='https://github.com/apache/airflow/blob/6a10bc6bf9446ae1ceeaeda95e9b8ee0d867bf99/dev/react-plugin-tools/bootstrap.py#L59'>dev/react-plugin-tools/bootstrap.py:59:10:</a> FURB171 Membership test against single-item container
- <a href='https://github.com/apache/airflow/blob/6a10bc6bf9446ae1ceeaeda95e9b8ee0d867bf99/dev/react-plugin-tools/bootstrap.py#L59'>dev/react-plugin-tools/bootstrap.py:59:10:</a> FURB171 [*] Membership test against single-item container
+ <a href='https://github.com/apache/airflow/blob/6a10bc6bf9446ae1ceeaeda95e9b8ee0d867bf99/dev/react-plugin-tools/bootstrap.py#L62'>dev/react-plugin-tools/bootstrap.py:62:10:</a> FURB171 Membership test against single-item container
- <a href='https://github.com/apache/airflow/blob/6a10bc6bf9446ae1ceeaeda95e9b8ee0d867bf99/dev/react-plugin-tools/bootstrap.py#L62'>dev/react-plugin-tools/bootstrap.py:62:10:</a> FURB171 [*] Membership test against single-item container
+ <a href='https://github.com/apache/airflow/blob/6a10bc6bf9446ae1ceeaeda95e9b8ee0d867bf99/devel-common/src/docs/provider_conf.py#L270'>devel-common/src/docs/provider_conf.py:270:4:</a> FURB171 Membership test against single-item container
- <a href='https://github.com/apache/airflow/blob/6a10bc6bf9446ae1ceeaeda95e9b8ee0d867bf99/devel-common/src/docs/provider_conf.py#L270'>devel-common/src/docs/provider_conf.py:270:4:</a> FURB171 [*] Membership test against single-item container
+ <a href='https://github.com/apache/airflow/blob/6a10bc6bf9446ae1ceeaeda95e9b8ee0d867bf99/providers/common/sql/tests/unit/common/sql/sensors/test_sql.py#L140'>providers/common/sql/tests/unit/common/sql/sensors/test_sql.py:140:103:</a> FURB171 Membership test against single-item container
- <a href='https://github.com/apache/airflow/blob/6a10bc6bf9446ae1ceeaeda95e9b8ee0d867bf99/providers/common/sql/tests/unit/common/sql/sensors/test_sql.py#L140'>providers/common/sql/tests/unit/common/sql/sensors/test_sql.py:140:103:</a> FURB171 [*] Membership test against single-item container
+ <a href='https://github.com/apache/airflow/blob/6a10bc6bf9446ae1ceeaeda95e9b8ee0d867bf99/providers/common/sql/tests/unit/common/sql/sensors/test_sql.py#L164'>providers/common/sql/tests/unit/common/sql/sensors/test_sql.py:164:31:</a> FURB171 Membership test against single-item container
- <a href='https://github.com/apache/airflow/blob/6a10bc6bf9446ae1ceeaeda95e9b8ee0d867bf99/providers/common/sql/tests/unit/common/sql/sensors/test_sql.py#L164'>providers/common/sql/tests/unit/common/sql/sensors/test_sql.py:164:31:</a> FURB171 [*] Membership test against single-item container
+ <a href='https://github.com/apache/airflow/blob/6a10bc6bf9446ae1ceeaeda95e9b8ee0d867bf99/providers/common/sql/tests/unit/common/sql/sensors/test_sql.py#L186'>providers/common/sql/tests/unit/common/sql/sensors/test_sql.py:186:31:</a> FURB171 Membership test against single-item container
- <a href='https://github.com/apache/airflow/blob/6a10bc6bf9446ae1ceeaeda95e9b8ee0d867bf99/providers/common/sql/tests/unit/common/sql/sensors/test_sql.py#L186'>providers/common/sql/tests/unit/common/sql/sensors/test_sql.py:186:31:</a> FURB171 [*] Membership test against single-item container
+ <a href='https://github.com/apache/airflow/blob/6a10bc6bf9446ae1ceeaeda95e9b8ee0d867bf99/providers/common/sql/tests/unit/common/sql/sensors/test_sql.py#L187'>providers/common/sql/tests/unit/common/sql/sensors/test_sql.py:187:31:</a> FURB171 Membership test against single-item container
- <a href='https://github.com/apache/airflow/blob/6a10bc6bf9446ae1ceeaeda95e9b8ee0d867bf99/providers/common/sql/tests/unit/common/sql/sensors/test_sql.py#L187'>providers/common/sql/tests/unit/common/sql/sensors/test_sql.py:187:31:</a> FURB171 [*] Membership test against single-item container
+ <a href='https://github.com/apache/airflow/blob/6a10bc6bf9446ae1ceeaeda95e9b8ee0d867bf99/providers/common/sql/tests/unit/common/sql/sensors/test_sql.py#L209'>providers/common/sql/tests/unit/common/sql/sensors/test_sql.py:209:31:</a> FURB171 Membership test against single-item container
- <a href='https://github.com/apache/airflow/blob/6a10bc6bf9446ae1ceeaeda95e9b8ee0d867bf99/providers/common/sql/tests/unit/common/sql/sensors/test_sql.py#L209'>providers/common/sql/tests/unit/common/sql/sensors/test_sql.py:209:31:</a> FURB171 [*] Membership test against single-item container
+ <a href='https://github.com/apache/airflow/blob/6a10bc6bf9446ae1ceeaeda95e9b8ee0d867bf99/providers/common/sql/tests/unit/common/sql/sensors/test_sql.py#L210'>providers/common/sql/tests/unit/common/sql/sensors/test_sql.py:210:31:</a> FURB171 Membership test against single-item container
- <a href='https://github.com/apache/airflow/blob/6a10bc6bf9446ae1ceeaeda95e9b8ee0d867bf99/providers/common/sql/tests/unit/common/sql/sensors/test_sql.py#L210'>providers/common/sql/tests/unit/common/sql/sensors/test_sql.py:210:31:</a> FURB171 [*] Membership test against single-item container
+ <a href='https://github.com/apache/airflow/blob/6a10bc6bf9446ae1ceeaeda95e9b8ee0d867bf99/providers/common/sql/tests/unit/common/sql/sensors/test_sql.py#L267'>providers/common/sql/tests/unit/common/sql/sensors/test_sql.py:267:31:</a> FURB171 Membership test against single-item container
- <a href='https://github.com/apache/airflow/blob/6a10bc6bf9446ae1ceeaeda95e9b8ee0d867bf99/providers/common/sql/tests/unit/common/sql/sensors/test_sql.py#L267'>providers/common/sql/tests/unit/common/sql/sensors/test_sql.py:267:31:</a> FURB171 [*] Membership test against single-item container
+ <a href='https://github.com/apache/airflow/blob/6a10bc6bf9446ae1ceeaeda95e9b8ee0d867bf99/providers/common/sql/tests/unit/common/sql/sensors/test_sql.py#L268'>providers/common/sql/tests/unit/common/sql/sensors/test_sql.py:268:31:</a> FURB171 Membership test against single-item container
- <a href='https://github.com/apache/airflow/blob/6a10bc6bf9446ae1ceeaeda95e9b8ee0d867bf99/providers/common/sql/tests/unit/common/sql/sensors/test_sql.py#L268'>providers/common/sql/tests/unit/common/sql/sensors/test_sql.py:268:31:</a> FURB171 [*] Membership test against single-item container
+ <a href='https://github.com/apache/airflow/blob/6a10bc6bf9446ae1ceeaeda95e9b8ee0d867bf99/providers/common/sql/tests/unit/common/sql/sensors/test_sql.py#L278'>providers/common/sql/tests/unit/common/sql/sensors/test_sql.py:278:31:</a> FURB171 Membership test against single-item container
- <a href='https://github.com/apache/airflow/blob/6a10bc6bf9446ae1ceeaeda95e9b8ee0d867bf99/providers/common/sql/tests/unit/common/sql/sensors/test_sql.py#L278'>providers/common/sql/tests/unit/common/sql/sensors/test_sql.py:278:31:</a> FURB171 [*] Membership test against single-item container
+ <a href='https://github.com/apache/airflow/blob/6a10bc6bf9446ae1ceeaeda95e9b8ee0d867bf99/providers/common/sql/tests/unit/common/sql/sensors/test_sql.py#L279'>providers/common/sql/tests/unit/common/sql/sensors/test_sql.py:279:31:</a> FURB171 Membership test against single-item container
- <a href='https://github.com/apache/airflow/blob/6a10bc6bf9446ae1ceeaeda95e9b8ee0d867bf99/providers/common/sql/tests/unit/common/sql/sensors/test_sql.py#L279'>providers/common/sql/tests/unit/common/sql/sensors/test_sql.py:279:31:</a> FURB171 [*] Membership test against single-item container
... 10 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -0 violations, +0 -4 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/b60be9655fcd3b79906719a54e87174f05e9f1fd/superset/connectors/sqla/models.py#L796'>superset/connectors/sqla/models.py:796:62:</a> FURB171 Membership test against single-item container
- <a href='https://github.com/apache/superset/blob/b60be9655fcd3b79906719a54e87174f05e9f1fd/superset/connectors/sqla/models.py#L796'>superset/connectors/sqla/models.py:796:62:</a> FURB171 [*] Membership test against single-item container
+ <a href='https://github.com/apache/superset/blob/b60be9655fcd3b79906719a54e87174f05e9f1fd/superset/models/helpers.py#L2120'>superset/models/helpers.py:2120:26:</a> FURB171 Membership test against single-item container
- <a href='https://github.com/apache/superset/blob/b60be9655fcd3b79906719a54e87174f05e9f1fd/superset/models/helpers.py#L2120'>superset/models/helpers.py:2120:26:</a> FURB171 [*] Membership test against single-item container
</pre>

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+0 -0 violations, +0 -2 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/reflex-dev/reflex/blob/8661f7e8c2fb9c28c3f4489cf52e12f7859a42fe/reflex/utils/templates.py#L388'>reflex/utils/templates.py:388:34:</a> FURB171 Membership test against single-item container
- <a href='https://github.com/reflex-dev/reflex/blob/8661f7e8c2fb9c28c3f4489cf52e12f7859a42fe/reflex/utils/templates.py#L388'>reflex/utils/templates.py:388:34:</a> FURB171 [*] Membership test against single-item container
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (+0 -0 violations, +0 -2 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/326490975510b2af888e0f319292fc4a9084a033/tests/test_settings_overrides.py#L441'>tests/test_settings_overrides.py:441:8:</a> FURB171 Membership test against single-item container
- <a href='https://github.com/scikit-build/scikit-build-core/blob/326490975510b2af888e0f319292fc4a9084a033/tests/test_settings_overrides.py#L441'>tests/test_settings_overrides.py:441:8:</a> FURB171 [*] Membership test against single-item container
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -0 violations, +0 -12 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/58e0d6fc124c24c088c721767258212a996a1f31/tools/lib/provision.py#L155'>tools/lib/provision.py:155:28:</a> FURB171 Membership test against single-item container
- <a href='https://github.com/zulip/zulip/blob/58e0d6fc124c24c088c721767258212a996a1f31/tools/lib/provision.py#L155'>tools/lib/provision.py:155:28:</a> FURB171 [*] Membership test against single-item container
+ <a href='https://github.com/zulip/zulip/blob/58e0d6fc124c24c088c721767258212a996a1f31/zerver/actions/presence.py#L87'>zerver/actions/presence.py:87:8:</a> FURB171 Membership test against single-item container
- <a href='https://github.com/zulip/zulip/blob/58e0d6fc124c24c088c721767258212a996a1f31/zerver/actions/presence.py#L87'>zerver/actions/presence.py:87:8:</a> FURB171 [*] Membership test against single-item container
+ <a href='https://github.com/zulip/zulip/blob/58e0d6fc124c24c088c721767258212a996a1f31/zerver/lib/emoji.py#L107'>zerver/lib/emoji.py:107:12:</a> FURB171 Membership test against single-item container
- <a href='https://github.com/zulip/zulip/blob/58e0d6fc124c24c088c721767258212a996a1f31/zerver/lib/emoji.py#L107'>zerver/lib/emoji.py:107:12:</a> FURB171 [*] Membership test against single-item container
+ <a href='https://github.com/zulip/zulip/blob/58e0d6fc124c24c088c721767258212a996a1f31/zerver/lib/narrow_predicate.py#L60'>zerver/lib/narrow_predicate.py:60:39:</a> FURB171 Membership test against single-item container
- <a href='https://github.com/zulip/zulip/blob/58e0d6fc124c24c088c721767258212a996a1f31/zerver/lib/narrow_predicate.py#L60'>zerver/lib/narrow_predicate.py:60:39:</a> FURB171 [*] Membership test against single-item container
+ <a href='https://github.com/zulip/zulip/blob/58e0d6fc124c24c088c721767258212a996a1f31/zerver/lib/url_decoding.py#L136'>zerver/lib/url_decoding.py:136:30:</a> FURB171 Membership test against single-item container
- <a href='https://github.com/zulip/zulip/blob/58e0d6fc124c24c088c721767258212a996a1f31/zerver/lib/url_decoding.py#L136'>zerver/lib/url_decoding.py:136:30:</a> FURB171 [*] Membership test against single-item container
... 2 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FURB171 | 64 | 0 | 0 | 0 | 64 |

</p>
</details>




---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/single_item_membership_test.rs`:104 on 2025-09-11 19:27_

We could just use `Fix::unsafe_edit` now that it's always unsafe instead of a separate `applicability`.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/single_item_membership_test.rs`:34 on 2025-09-11 19:30_

I think we should probably preserve the details about strings from the original docs. The `in` vs `==` thing seems pretty niche, while the string case seems more likely, so it feels worth mentioning both.

---

_@ntBre reviewed on 2025-09-11 19:32_

Thanks, this looks good! I just had a couple of minor suggestions. I agree with marking the fix as always unsafe instead of trying to look for the few special safe cases mentioned in the issue. I think that could get pretty complicated.

---

_Label `fixes` added by @ntBre on 2025-09-11 19:33_

---

_Label `preview` added by @ntBre on 2025-09-11 19:33_

---

_Renamed from "[`refurb`] Mark single-item-membership-test fixes as always unsafe" to "[`refurb`] Mark `single-item-membership-test` fix as always unsafe (`FURB171`)" by @ntBre on 2025-09-11 19:33_

---

_@TaKO8Ki reviewed on 2025-09-12 21:01_

---

_Review comment by @TaKO8Ki on `crates/ruff_linter/src/rules/refurb/rules/single_item_membership_test.rs`:104 on 2025-09-12 21:01_

I've replaced it with `unsafe_edit`.

---

_@TaKO8Ki reviewed on 2025-09-12 21:01_

---

_Review comment by @TaKO8Ki on `crates/ruff_linter/src/rules/refurb/rules/single_item_membership_test.rs`:34 on 2025-09-12 21:01_

I have revived a part of original docs.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/single_item_membership_test.rs`:37 on 2025-09-18 15:11_

```suggestion
/// Additionally, converting `in`/`not in` against a single-item container to `==`/`!=` can
/// change runtime behavior: `in` may consider identity (e.g., `NaN`) and always
/// yields a `bool`. 
///
/// Comments within the replacement range will also be removed.
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/single_item_membership_test.rs`:33 on 2025-09-18 15:11_

```suggestion
/// This is because `c in "a"` is true both when `c` is `"a"` and when `c` is the empty string.
```

---

_@ntBre approved on 2025-09-18 15:12_

Thank you! Just a couple of small docs nits that I'll apply and then merge this :)

---

_Merged by @ntBre on 2025-09-18 15:24_

---

_Closed by @ntBre on 2025-09-18 15:24_

---
