```yaml
number: 14105
title: "[`refurb`] Implement `subclass-builtin` (`FURB189`)"
type: pull_request
state: merged
author: sbrugman
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: refurb-no-subclass-builtins
created_at: 2024-11-05T12:21:22Z
updated_at: 2024-11-07T11:59:23Z
url: https://github.com/astral-sh/ruff/pull/14105
synced_at: 2026-01-12T15:55:46Z
```

# [`refurb`] Implement `subclass-builtin` (`FURB189`)

---

_@sbrugman_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Implementation for one of the rules in https://github.com/astral-sh/ruff/issues/1348
Refurb only deals only with classes with a single base, however the rule is valid for any base.
(`str, Enum` is common prior to `StrEnum`)

## Test Plan

`cargo test`

---

_Comment by @github-actions[bot] on 2024-11-05 12:51_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+12 -0 violations, +0 -0 fixes in 5 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/a51487d89ab62ea32284bab26232349e0c7c070e/airflow/decorators/setup_teardown.py#L58'>airflow/decorators/setup_teardown.py:58:22:</a> FURB189 Subclassing `list` can be error prone, use `collections.UserList` instead
+ <a href='https://github.com/apache/airflow/blob/a51487d89ab62ea32284bab26232349e0c7c070e/airflow/io/utils/stat.py#L22'>airflow/io/utils/stat.py:22:19:</a> FURB189 Subclassing `dict` can be error prone, use `collections.UserDict` instead
+ <a href='https://github.com/apache/airflow/blob/a51487d89ab62ea32284bab26232349e0c7c070e/kubernetes_tests/test_base.py#L45'>kubernetes_tests/test_base.py:45:26:</a> FURB189 Subclassing `str` can be error prone, use `collections.UserStr` instead
+ <a href='https://github.com/apache/airflow/blob/a51487d89ab62ea32284bab26232349e0c7c070e/providers/src/airflow/providers/openlineage/utils/utils.py#L170'>providers/src/airflow/providers/openlineage/utils/utils.py:170:25:</a> FURB189 Subclassing `dict` can be error prone, use `collections.UserDict` instead
+ <a href='https://github.com/apache/airflow/blob/a51487d89ab62ea32284bab26232349e0c7c070e/providers/tests/openlineage/plugins/test_utils.py#L67'>providers/tests/openlineage/plugins/test_utils.py:67:19:</a> FURB189 Subclassing `dict` can be error prone, use `collections.UserDict` instead
+ <a href='https://github.com/apache/airflow/blob/a51487d89ab62ea32284bab26232349e0c7c070e/tests/utils/log/test_secrets_masker.py#L317'>tests/utils/log/test_secrets_masker.py:317:54:</a> FURB189 Subclassing `str` can be error prone, use `collections.UserStr` instead
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/plots.py#L903'>src/bokeh/models/plots.py:903:24:</a> FURB189 Subclassing `list` can be error prone, use `collections.UserList` instead
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/2cb39270ecd920adb93451c6edecc6e5b2efab30/libs/core/tests/unit_tests/stubs.py#L7'>libs/core/tests/unit_tests/stubs.py:7:14:</a> FURB189 Subclassing `str` can be error prone, use `collections.UserStr` instead
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/f6e60f41e46ce13ddbd344542d1bf45743b74514/src/scikit_build_core/settings/skbuild_model.py#L31'>src/scikit_build_core/settings/skbuild_model.py:31:27:</a> FURB189 Subclassing `str` can be error prone, use `collections.UserStr` instead
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/f6e60f41e46ce13ddbd344542d1bf45743b74514/src/scikit_build_core/setuptools/build_cmake.py#L214'>src/scikit_build_core/setuptools/build_cmake.py:214:24:</a> FURB189 Subclassing `list` can be error prone, use `collections.UserList` instead
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/indico/indico/blob/5b70ccf74a5125d22325406c52ce43c82b364170/indico/legacy/pdfinterface/latex.py#L169'>indico/legacy/pdfinterface/latex.py:169:16:</a> FURB189 Subclassing `str` can be error prone, use `collections.UserStr` instead
+ <a href='https://github.com/indico/indico/blob/5b70ccf74a5125d22325406c52ce43c82b364170/indico/modules/events/abstracts/util.py#L141'>indico/modules/events/abstracts/util.py:141:24:</a> FURB189 Subclassing `dict` can be error prone, use `collections.UserDict` instead
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FURB189 | 12 | 12 | 0 | 0 | 0 |

</p>
</details>




---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/refurb/rules/subclass_builtin.rs`:72 on 2024-11-06 04:18_

I'd prefer to pass in the `ast::StmtClass` here instead of `Arguments` as it's then clear that we're only interested in the arguments of a class and not the function.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/refurb/rules/subclass_builtin.rs`:93 on 2024-11-06 04:19_

nit: let's implement these directly as methods on `Builtins` like `symbol` would return `dict`/`list`/`str` and `user_symbol` would return `UserDict`/`UserList`/`UserStr`.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/refurb/rules/subclass_builtin.rs`:65 on 2024-11-06 04:22_

nit: maybe `SupportedBuiltins` to highlight that these are the ones that are supported by this specific rule. There's some precedence for it at https://github.com/astral-sh/ruff/blob/4ece8e5c1e674eb0d751fa55b5a72950aa9d20f0/crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_comprehension_in_call.rs#L197-L204

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/refurb/rules/subclass_builtin.rs`:93 on 2024-11-06 04:34_

Can we restructure this to avoid the double loop? Something like the following:

```rs
for base in args {
	let Some(symbol) = checker.semantic().resolve_builtin_symbol(base) else {
		continue;
	};

	let Some(supported_builtin) = SupportedBuiltins::from_str(symbol) else {
		continue;
	};

	let mut diagnostic = ...
}
```

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/refurb/rules/subclass_builtin.rs`:53 on 2024-11-06 04:39_

I think we should expand the diagnostic message to be more informative, maybe something like "Subclassing `{subclass}` can be error prone, use `collections.{replacement}` instead"

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/refurb/rules/subclass_builtin.rs`:61 on 2024-11-06 04:39_

```suggestion
        format!("Replace with `collections.{replacement}`")
```

---

_@dhruvmanila requested changes on 2024-11-06 04:42_

---

_Label `rule` added by @dhruvmanila on 2024-11-06 04:42_

---

_Label `preview` added by @dhruvmanila on 2024-11-06 04:42_

---

_Comment by @sbrugman on 2024-11-06 15:53_

Thanks for the helpful pointers @dhruvmanila! 

---

_@dhruvmanila approved on 2024-11-07 04:40_

Thanks! I made a couple of minor changes:

* Moved the utilities below the rule logic function; we do this for all rules
* Avoid the eager `.to_string` call for `user_symbol` method and only allocate it when creating the `Diagnostic`

---

_Comment by @dhruvmanila on 2024-11-07 04:45_

Looking at the ecosystem changes, we might want to special case `(str, Enum)`.

> Refurb only deals only with classes with a single base, however the rule is valid for any base.

Sorry, I missed this earlier but can you expand on why do we want to diverge from this behavior.

Edit: I think we _should_ keep the same behavior as `refurb` and check only when there's one base class.

---

_Comment by @sbrugman on 2024-11-07 09:41_

Fair point. After looking at the ecosystem results, I think we indeed should stick with refurb's behaviour (at least until type inference is available).

Only considering classes with a single base is the least error prone.

It also has some false negatives which can be detected by considering multiple bases, e.g. https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/wrappers.py#L306. 

However there are false positives for: 
- `str, Enum` is a special case already handled by another rule: https://docs.astral.sh/ruff/rules/replace-str-enum.
-  At the moment we cannot detect cases such as `str, SubclassOfEnum`: https://github.com/indico/indico/blob/5b70ccf74a5125d22325406c52ce43c82b364170/indico/modules/events/timetable/reschedule.py#L27

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/refurb/rules/subclass_builtin.rs`:77 on 2024-11-07 10:32_

```suggestion
    let [base] = &**bases else {
        return;
    }
```

---

_@dhruvmanila reviewed on 2024-11-07 10:32_

---

_Comment by @dhruvmanila on 2024-11-07 10:34_

> It also has some false negatives which can be detected by considering multiple bases, e.g. [bokeh/bokeh@`829b2a7`/src/bokeh/core/property/wrappers.py#L306](https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/wrappers.py#L306).
> 
> However there are false positives for:
> 
> * `str, Enum` is a special case already handled by another rule: [docs.astral.sh/ruff/rules/replace-str-enum](https://docs.astral.sh/ruff/rules/replace-str-enum).
> * At the moment we cannot detect cases such as `str, SubclassOfEnum`: [indico/indico@`5b70ccf`/indico/modules/events/timetable/reschedule.py#L27](https://github.com/indico/indico/blob/5b70ccf74a5125d22325406c52ce43c82b364170/indico/modules/events/timetable/reschedule.py#L27)

I think the listed limitations are a good trade-off.

---

_@dhruvmanila approved on 2024-11-07 11:55_

---

_Renamed from "[`refurb`] Implement `subclass-builtin` (FURB189)" to "[`refurb`] Implement `subclass-builtin` (`FURB189`)" by @dhruvmanila on 2024-11-07 11:56_

---

_Merged by @dhruvmanila on 2024-11-07 11:56_

---

_Closed by @dhruvmanila on 2024-11-07 11:56_

---

_Branch deleted on 2024-11-07 11:59_

---
