```yaml
number: 15889
title: "[`flake8-pyi`] Make `PYI019` autofixable for `.py` files in preview mode as well as stubs"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - fixes
  - preview
assignees: []
merged: true
base: main
head: alex/pyi019-non-stub-fix
created_at: 2025-02-02T23:17:07Z
updated_at: 2025-02-04T16:43:00Z
url: https://github.com/astral-sh/ruff/pull/15889
synced_at: 2026-01-12T15:55:53Z
```

# [`flake8-pyi`] Make `PYI019` autofixable for `.py` files in preview mode as well as stubs

---

_@AlexWaygood_

(This PR is stacked on top of https://github.com/astral-sh/ruff/pull/15885 and https://github.com/astral-sh/ruff/pull/15888; review those first.)

## Summary

Now that this is a bindings-based rule, I think the autofix can be safely applied to `.py` files as well as stubs. All we need to do is search the method body for references we might need to replace as well as searching the method header.

@InSyncWithFoo mentioned in https://github.com/astral-sh/ruff/pull/15821 that there might be some edge cases to do with stringized type expressions where this wouldn't work well. I've made the rule unfixable in these edge cases by `bail`ing out of the fix if we see that any references in the source code do not exactly match the original TypeVar name. 

## Test Plan

New snippets added to the fixture files.


---

_Label `fixes` added by @AlexWaygood on 2025-02-02 23:17_

---

_Label `preview` added by @AlexWaygood on 2025-02-02 23:17_

---

_@InSyncWithFoo reviewed on 2025-02-02 23:23_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/flake8_pyi/snapshots/ruff_linter__rules__flake8_pyi__tests__preview_PYI019_PYI019_0.py.snap`:132 on 2025-02-02 23:23_

This line can be removed now. Same for its twin in `PYI019_0.pyi`.

---

_Comment by @github-actions[bot] on 2025-02-02 23:23_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +26 -0 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -0 violations, +2 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/53d944d0130da23b4f0d02e35c4e204c813ce314/superset/sql/parse.py#L167'>superset/sql/parse.py:167:21:</a> PYI019 Use `Self` instead of custom TypeVar `TBaseSQLStatement`
+ <a href='https://github.com/apache/superset/blob/53d944d0130da23b4f0d02e35c4e204c813ce314/superset/sql/parse.py#L167'>superset/sql/parse.py:167:21:</a> PYI019 [*] Use `Self` instead of custom TypeVar `TBaseSQLStatement`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+0 -0 violations, +20 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/rotki/rotki/blob/696982031d32a0a9cd7c540a65451d0c03c12b87/rotkehlchen/db/filtering.py#L1138'>rotkehlchen/db/filtering.py:1138:13:</a> PYI019 Use `Self` instead of custom TypeVar `T_EthSTakingFilterQ`
+ <a href='https://github.com/rotki/rotki/blob/696982031d32a0a9cd7c540a65451d0c03c12b87/rotkehlchen/db/filtering.py#L1138'>rotkehlchen/db/filtering.py:1138:13:</a> PYI019 [*] Use `Self` instead of custom TypeVar `T_EthSTakingFilterQ`
- <a href='https://github.com/rotki/rotki/blob/696982031d32a0a9cd7c540a65451d0c03c12b87/rotkehlchen/db/filtering.py#L373'>rotkehlchen/db/filtering.py:373:15:</a> PYI019 Use `Self` instead of custom TypeVar `T_FilterQ`
+ <a href='https://github.com/rotki/rotki/blob/696982031d32a0a9cd7c540a65451d0c03c12b87/rotkehlchen/db/filtering.py#L373'>rotkehlchen/db/filtering.py:373:15:</a> PYI019 [*] Use `Self` instead of custom TypeVar `T_FilterQ`
- <a href='https://github.com/rotki/rotki/blob/696982031d32a0a9cd7c540a65451d0c03c12b87/rotkehlchen/db/filtering.py#L866'>rotkehlchen/db/filtering.py:866:13:</a> PYI019 Use `Self` instead of custom TypeVar `T_HistoryBaseEntryFilterQ`
+ <a href='https://github.com/rotki/rotki/blob/696982031d32a0a9cd7c540a65451d0c03c12b87/rotkehlchen/db/filtering.py#L866'>rotkehlchen/db/filtering.py:866:13:</a> PYI019 [*] Use `Self` instead of custom TypeVar `T_HistoryBaseEntryFilterQ`
- <a href='https://github.com/rotki/rotki/blob/696982031d32a0a9cd7c540a65451d0c03c12b87/rotkehlchen/history/events/structures/base.py#L372'>rotkehlchen/history/events/structures/base.py:372:39:</a> PYI019 Use `Self` instead of custom TypeVar `T`
+ <a href='https://github.com/rotki/rotki/blob/696982031d32a0a9cd7c540a65451d0c03c12b87/rotkehlchen/history/events/structures/base.py#L372'>rotkehlchen/history/events/structures/base.py:372:39:</a> PYI019 [*] Use `Self` instead of custom TypeVar `T`
- <a href='https://github.com/rotki/rotki/blob/696982031d32a0a9cd7c540a65451d0c03c12b87/rotkehlchen/utils/mixins/enums.py#L101'>rotkehlchen/utils/mixins/enums.py:101:20:</a> PYI019 Use `Self` instead of custom TypeVar `S`
+ <a href='https://github.com/rotki/rotki/blob/696982031d32a0a9cd7c540a65451d0c03c12b87/rotkehlchen/utils/mixins/enums.py#L101'>rotkehlchen/utils/mixins/enums.py:101:20:</a> PYI019 [*] Use `Self` instead of custom TypeVar `S`
- <a href='https://github.com/rotki/rotki/blob/696982031d32a0a9cd7c540a65451d0c03c12b87/rotkehlchen/utils/mixins/enums.py#L126'>rotkehlchen/utils/mixins/enums.py:126:20:</a> PYI019 Use `Self` instead of custom TypeVar `S`
+ <a href='https://github.com/rotki/rotki/blob/696982031d32a0a9cd7c540a65451d0c03c12b87/rotkehlchen/utils/mixins/enums.py#L126'>rotkehlchen/utils/mixins/enums.py:126:20:</a> PYI019 [*] Use `Self` instead of custom TypeVar `S`
- <a href='https://github.com/rotki/rotki/blob/696982031d32a0a9cd7c540a65451d0c03c12b87/rotkehlchen/utils/mixins/enums.py#L12'>rotkehlchen/utils/mixins/enums.py:12:16:</a> PYI019 Use `Self` instead of custom TypeVar `T`
+ <a href='https://github.com/rotki/rotki/blob/696982031d32a0a9cd7c540a65451d0c03c12b87/rotkehlchen/utils/mixins/enums.py#L12'>rotkehlchen/utils/mixins/enums.py:12:16:</a> PYI019 [*] Use `Self` instead of custom TypeVar `T`
- <a href='https://github.com/rotki/rotki/blob/696982031d32a0a9cd7c540a65451d0c03c12b87/rotkehlchen/utils/mixins/enums.py#L151'>rotkehlchen/utils/mixins/enums.py:151:28:</a> PYI019 Use `Self` instead of custom TypeVar `D`
+ <a href='https://github.com/rotki/rotki/blob/696982031d32a0a9cd7c540a65451d0c03c12b87/rotkehlchen/utils/mixins/enums.py#L151'>rotkehlchen/utils/mixins/enums.py:151:28:</a> PYI019 [*] Use `Self` instead of custom TypeVar `D`
- <a href='https://github.com/rotki/rotki/blob/696982031d32a0a9cd7c540a65451d0c03c12b87/rotkehlchen/utils/mixins/enums.py#L172'>rotkehlchen/utils/mixins/enums.py:172:28:</a> PYI019 Use `Self` instead of custom TypeVar `D`
+ <a href='https://github.com/rotki/rotki/blob/696982031d32a0a9cd7c540a65451d0c03c12b87/rotkehlchen/utils/mixins/enums.py#L172'>rotkehlchen/utils/mixins/enums.py:172:28:</a> PYI019 [*] Use `Self` instead of custom TypeVar `D`
- <a href='https://github.com/rotki/rotki/blob/696982031d32a0a9cd7c540a65451d0c03c12b87/rotkehlchen/utils/mixins/enums.py#L77'>rotkehlchen/utils/mixins/enums.py:77:20:</a> PYI019 Use `Self` instead of custom TypeVar `S`
+ <a href='https://github.com/rotki/rotki/blob/696982031d32a0a9cd7c540a65451d0c03c12b87/rotkehlchen/utils/mixins/enums.py#L77'>rotkehlchen/utils/mixins/enums.py:77:20:</a> PYI019 [*] Use `Self` instead of custom TypeVar `S`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -0 violations, +4 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/410ae119d42d987e649116e5d795b506497b6784/zerver/data_import/slack.py#L109'>zerver/data_import/slack.py:109:18:</a> PYI019 Use `Self` instead of custom TypeVar `SlackBotEmailT`
+ <a href='https://github.com/zulip/zulip/blob/410ae119d42d987e649116e5d795b506497b6784/zerver/data_import/slack.py#L109'>zerver/data_import/slack.py:109:18:</a> PYI019 [*] Use `Self` instead of custom TypeVar `SlackBotEmailT`
- <a href='https://github.com/zulip/zulip/blob/410ae119d42d987e649116e5d795b506497b6784/zerver/lib/thumbnail.py#L55'>zerver/lib/thumbnail.py:55:20:</a> PYI019 Use `Self` instead of custom TypeVar `T`
+ <a href='https://github.com/zulip/zulip/blob/410ae119d42d987e649116e5d795b506497b6784/zerver/lib/thumbnail.py#L55'>zerver/lib/thumbnail.py:55:20:</a> PYI019 [*] Use `Self` instead of custom TypeVar `T`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PYI019 | 26 | 0 | 0 | 26 | 0 |

</p>
</details>




---

_@AlexWaygood reviewed on 2025-02-02 23:34_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/snapshots/ruff_linter__rules__flake8_pyi__tests__preview_PYI019_PYI019_0.py.snap`:132 on 2025-02-02 23:34_

Great catch, thanks! Will fix in the morning 

---

_@InSyncWithFoo reviewed on 2025-02-03 04:46_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/flake8_pyi/rules/custom_type_var_for_self.rs`:55 on 2025-02-03 04:46_

Please, don't use `.md` links. These don't play nice with language clients/IDEs, which haven't the slightest idea where they point to. An unclickable link results in nothing but terrible UX.

Full URLs (`https://docs.astral.sh/ruff/rules/rule-name/`) would be best.

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI019_0.py`:156 on 2025-02-04 09:58_

Could you add a test where `S` gets rebound (or deleted) inside the method body? Just to make sure that these references don't get rewritten.

---

_@MichaReiser approved on 2025-02-04 09:58_

---

_Merged by @AlexWaygood on 2025-02-04 16:41_

---

_Closed by @AlexWaygood on 2025-02-04 16:41_

---

_Branch deleted on 2025-02-04 16:41_

---
