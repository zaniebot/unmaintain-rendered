```yaml
number: 14008
title: "[`flake8-simplify`] Implementation for `split-of-static-string` (SIM905)"
type: pull_request
state: merged
author: sbrugman
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: rule-ruff-split-of-static-string
created_at: 2024-10-30T19:41:34Z
updated_at: 2024-11-03T13:32:37Z
url: https://github.com/astral-sh/ruff/pull/14008
synced_at: 2026-01-10T20:59:37Z
```

# [`flake8-simplify`] Implementation for `split-of-static-string` (SIM905)

---

_Pull request opened by @sbrugman on 2024-10-30 19:41_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Closes https://github.com/astral-sh/ruff/issues/13944

## Test Plan

Standard snapshot testing

flake8-simplify surprisingly only has a single test case

---

_Comment by @github-actions[bot] on 2024-10-30 20:00_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+31 -0 violations, +0 -0 fixes in 6 projects; 48 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/a8921ae57a53d16150f985305a7252222647f15c/airflow/example_dags/example_params_ui_tutorial.py#L101'>airflow/example_dags/example_params_ui_tutorial.py:101:22:</a> SIM905 [*] Consider using a list literal instead of `str.split`
+ <a href='https://github.com/apache/airflow/blob/a8921ae57a53d16150f985305a7252222647f15c/scripts/ci/pre_commit/check_min_python_version.py#L29'>scripts/ci/pre_commit/check_min_python_version.py:29:35:</a> SIM905 [*] Consider using a list literal instead of `str.split`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+9 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/b02d18a39e3ffb7cee2a6abd97a44393e33dc129/superset/utils/date_parser.py#L511'>superset/utils/date_parser.py:511:9:</a> SIM905 [*] Consider using a list literal instead of `str.split`
+ <a href='https://github.com/apache/superset/blob/b02d18a39e3ffb7cee2a6abd97a44393e33dc129/tests/integration_tests/db_engine_specs/hive_tests.py#L31'>tests/integration_tests/db_engine_specs/hive_tests.py:31:11:</a> SIM905 [*] Consider using a list literal instead of `str.split`
+ <a href='https://github.com/apache/superset/blob/b02d18a39e3ffb7cee2a6abd97a44393e33dc129/tests/integration_tests/db_engine_specs/hive_tests.py#L39'>tests/integration_tests/db_engine_specs/hive_tests.py:39:11:</a> SIM905 [*] Consider using a list literal instead of `str.split`
+ <a href='https://github.com/apache/superset/blob/b02d18a39e3ffb7cee2a6abd97a44393e33dc129/tests/integration_tests/db_engine_specs/hive_tests.py#L46'>tests/integration_tests/db_engine_specs/hive_tests.py:46:11:</a> SIM905 [*] Consider using a list literal instead of `str.split`
+ <a href='https://github.com/apache/superset/blob/b02d18a39e3ffb7cee2a6abd97a44393e33dc129/tests/integration_tests/db_engine_specs/hive_tests.py#L54'>tests/integration_tests/db_engine_specs/hive_tests.py:54:11:</a> SIM905 [*] Consider using a list literal instead of `str.split`
+ <a href='https://github.com/apache/superset/blob/b02d18a39e3ffb7cee2a6abd97a44393e33dc129/tests/integration_tests/db_engine_specs/hive_tests.py#L63'>tests/integration_tests/db_engine_specs/hive_tests.py:63:11:</a> SIM905 [*] Consider using a list literal instead of `str.split`
+ <a href='https://github.com/apache/superset/blob/b02d18a39e3ffb7cee2a6abd97a44393e33dc129/tests/integration_tests/db_engine_specs/hive_tests.py#L73'>tests/integration_tests/db_engine_specs/hive_tests.py:73:11:</a> SIM905 [*] Consider using a list literal instead of `str.split`
+ <a href='https://github.com/apache/superset/blob/b02d18a39e3ffb7cee2a6abd97a44393e33dc129/tests/integration_tests/db_engine_specs/hive_tests.py#L84'>tests/integration_tests/db_engine_specs/hive_tests.py:84:11:</a> SIM905 [*] Consider using a list literal instead of `str.split`
+ <a href='https://github.com/apache/superset/blob/b02d18a39e3ffb7cee2a6abd97a44393e33dc129/tests/integration_tests/db_engine_specs/hive_tests.py#L97'>tests/integration_tests/db_engine_specs/hive_tests.py:97:11:</a> SIM905 [*] Consider using a list literal instead of `str.split`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/scripts/hooks/install.py#L5'>scripts/hooks/install.py:5:20:</a> SIM905 [*] Consider using a list literal instead of `str.split`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/scripts/hooks/uninstall.py#L5'>scripts/hooks/uninstall.py:5:20:</a> SIM905 [*] Consider using a list literal instead of `str.split`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/scripts/sri.py#L20'>scripts/sri.py:20:11:</a> SIM905 [*] Consider using a list literal instead of `str.split`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/resources.py#L674'>src/bokeh/resources.py:674:11:</a> SIM905 [*] Consider using a list literal instead of `str.split`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+12 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/eb39e657c552a298c9a91976b88443953a094a6b/admin/tests/test_integration.py#L542'>admin/tests/test_integration.py:542:27:</a> SIM905 [*] Consider using a list literal instead of `str.split`
+ <a href='https://github.com/freedomofpress/securedrop/blob/eb39e657c552a298c9a91976b88443953a094a6b/admin/tests/test_integration.py#L547'>admin/tests/test_integration.py:547:27:</a> SIM905 [*] Consider using a list literal instead of `str.split`
+ <a href='https://github.com/freedomofpress/securedrop/blob/eb39e657c552a298c9a91976b88443953a094a6b/admin/tests/test_integration.py#L642'>admin/tests/test_integration.py:642:27:</a> SIM905 [*] Consider using a list literal instead of `str.split`
+ <a href='https://github.com/freedomofpress/securedrop/blob/eb39e657c552a298c9a91976b88443953a094a6b/admin/tests/test_integration.py#L643'>admin/tests/test_integration.py:643:27:</a> SIM905 [*] Consider using a list literal instead of `str.split`
+ <a href='https://github.com/freedomofpress/securedrop/blob/eb39e657c552a298c9a91976b88443953a094a6b/admin/tests/test_integration.py#L646'>admin/tests/test_integration.py:646:27:</a> SIM905 [*] Consider using a list literal instead of `str.split`
+ <a href='https://github.com/freedomofpress/securedrop/blob/eb39e657c552a298c9a91976b88443953a094a6b/admin/tests/test_integration.py#L675'>admin/tests/test_integration.py:675:27:</a> SIM905 [*] Consider using a list literal instead of `str.split`
+ <a href='https://github.com/freedomofpress/securedrop/blob/eb39e657c552a298c9a91976b88443953a094a6b/securedrop/pretty_bad_protocol/_parsers.py#L1194'>securedrop/pretty_bad_protocol/_parsers.py:1194:16:</a> SIM905 [*] Consider using a list literal instead of `str.split`
+ <a href='https://github.com/freedomofpress/securedrop/blob/eb39e657c552a298c9a91976b88443953a094a6b/securedrop/pretty_bad_protocol/_parsers.py#L1233'>securedrop/pretty_bad_protocol/_parsers.py:1233:16:</a> SIM905 [*] Consider using a list literal instead of `str.split`
+ <a href='https://github.com/freedomofpress/securedrop/blob/eb39e657c552a298c9a91976b88443953a094a6b/securedrop/pretty_bad_protocol/_parsers.py#L1292'>securedrop/pretty_bad_protocol/_parsers.py:1292:24:</a> SIM905 [*] Consider using a list literal instead of `str.split`
+ <a href='https://github.com/freedomofpress/securedrop/blob/eb39e657c552a298c9a91976b88443953a094a6b/securedrop/pretty_bad_protocol/_parsers.py#L1400'>securedrop/pretty_bad_protocol/_parsers.py:1400:24:</a> SIM905 [*] Consider using a list literal instead of `str.split`
+ <a href='https://github.com/freedomofpress/securedrop/blob/eb39e657c552a298c9a91976b88443953a094a6b/securedrop/pretty_bad_protocol/_parsers.py#L671'>securedrop/pretty_bad_protocol/_parsers.py:671:30:</a> SIM905 [*] Consider using a list literal instead of `str.split`
+ <a href='https://github.com/freedomofpress/securedrop/blob/eb39e657c552a298c9a91976b88443953a094a6b/securedrop/pretty_bad_protocol/gnupg.py#L565'>securedrop/pretty_bad_protocol/gnupg.py:565:26:</a> SIM905 [*] Consider using a list literal instead of `str.split`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/f6e60f41e46ce13ddbd344542d1bf45743b74514/tests/test_skbuild_settings.py#L400'>tests/test_skbuild_settings.py:400:12:</a> SIM905 [*] Consider using a list literal instead of `str.split`
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/f6e60f41e46ce13ddbd344542d1bf45743b74514/tests/test_skbuild_settings.py#L440'>tests/test_skbuild_settings.py:440:12:</a> SIM905 [*] Consider using a list literal instead of `str.split`
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/f6e60f41e46ce13ddbd344542d1bf45743b74514/tests/test_skbuild_settings.py#L752'>tests/test_skbuild_settings.py:752:12:</a> SIM905 [*] Consider using a list literal instead of `str.split`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/indico/indico/blob/9cec34cb2c5993241d663a4dd05326256a15dfc7/indico/modules/rb/api.py#L310'>indico/modules/rb/api.py:310:17:</a> SIM905 [*] Consider using a list literal instead of `str.split`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM905 | 31 | 31 | 0 | 0 | 0 |

</p>
</details>




---

_Label `rule` added by @MichaReiser on 2024-10-31 07:00_

---

_Label `preview` added by @MichaReiser on 2024-10-31 07:00_

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/ruff/RUF035.py`:10 on 2024-10-31 07:01_

A few more examples would be great:

1. Where the left is an empty string: `"".split()`
1. An all whitespace string: `"     ".split()` ideally where the whitespace is a mixture of spaces, tabs, etc
1. Where the left contains no split points: `"/abc/".split()` (found in the ecosystem results)
1. Multiline with different indents: see https://github.com/scikit-build/scikit-build-core/blob/f6e60f41e46ce13ddbd344542d1bf45743b74514/tests/test_skbuild_settings.py#L440-L443 
1. Strings with unicode flag
1. Examples with comments in various positions 
   ```py
      ("a,b,c"
      # comment
      .split()
      )
   ```


We should also add examples for the following strings where I think it's okay if we don't support them

1. Raw strings
1. Implicit concatenated strings
1. Implicit concatenated strings with commented parts (where there are comments between the parts)
1. Implicit concatenated strings with different prefixes


---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:406 on 2024-10-31 07:05_

Nit
```suggestion
                        } else if matches!(attr, "split" | "rsplit") {
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/split_of_static_string.rs`:40 on 2024-10-31 07:06_

```suggestion
        format!("Consider using a list instead of `str.split`")
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/split_of_static_string.rs`:44 on 2024-10-31 07:06_

```suggestion
        Some(format!("Replace `str.split` with list literal"))
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/split_of_static_string.rs`:133 on 2024-10-31 07:15_

Nit: You could use `Option::and_then` to reduce the nesting

```suggestion
    let maxsplit_value = if let Some(maxsplit) = maxsplit_arg {
        let Some(value) = maxsplit
            .as_number_literal_expr()
            .and_then(|max_split| max_split.value.as_int())
            .and_then(|int_value| int_value.as_usize())
        else {
            return;
        };

        value
    } else {
        0
    };
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/split_of_static_string.rs`:160 on 2024-10-31 07:16_

What's your reasoning for making this an unsafe edit?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/split_of_static_string.rs`:81 on 2024-10-31 07:17_

What makes `maxsplit` with the default separator difficult to implement?

---

_@MichaReiser requested changes on 2024-10-31 07:18_

Nice! This is real good. 

I think this mainly needs more tests .

The touch with `maxsplit` is nice but I haven't seen a single example in the ecosystem results. That's why I'm not sure if its worth the complexity or if we should just bail on providing a fix in this case. 

---

_@AlexWaygood reviewed on 2024-10-31 08:27_

This rule exists in the Flake8-simplify linter as SIM905: https://github.com/MartinThoma/flake8-simplify/issues/86

So I think it would be good for us to put it in our Flake8-simplify category with code SIM905

---

_Comment by @sbrugman on 2024-10-31 08:42_

Thanks for the review @MichaReiser, will make another pass.

Good catch @AlexWaygood, I'll remap to that category.

---

_Renamed from "[`ruff`] Implementation for `split-of-static-string` (RUF035)" to "[`flake8-simplify`] Implementation for `split-of-static-string` (SIM905)" by @sbrugman on 2024-10-31 08:43_

---

_@sbrugman reviewed on 2024-10-31 12:09_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/ruff/rules/split_of_static_string.rs`:81 on 2024-10-31 12:09_

There is no equivalent to `split` for `split_whitespace`. Implementing it is not that much work, but it would be simplified greatly when this experimental API is stabilised (https://doc.rust-lang.org/std/str/struct.SplitWhitespace.html#method.remainder). I don't think it's worth to implement the logic now. I'll add a comment.

---

_@sbrugman reviewed on 2024-10-31 12:19_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/ruff/rules/split_of_static_string.rs`:160 on 2024-10-31 12:19_

I had not yet exhausted all edge cases (comments, implicit string concatenation, etc.), so I marked it unsafe to be on the _safe_ side. Comments within ISC are not preserved, so in that case it's unsafe.

---

_@sbrugman reviewed on 2024-10-31 12:34_

---

_Review comment by @sbrugman on `crates/ruff_linter/resources/test/fixtures/ruff/RUF035.py`:10 on 2024-10-31 12:34_

Added these cases, as well as `maxsize=-1` that I found people also use.

---

_@sbrugman reviewed on 2024-10-31 12:39_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/ruff/rules/split_of_static_string.rs`:133 on 2024-10-31 12:39_

I kept the match statement now that UnaryOp is also matched, but removed one if-else condition with `and_then`.

---

_@sbrugman reviewed on 2024-10-31 12:45_

---

_Review comment by @sbrugman on `crates/ruff_linter/resources/test/fixtures/ruff/RUF035.py`:10 on 2024-10-31 12:45_

Implicit concatenated strings are supported, but the fix is unsafe because of the comments. I could also exclude them from fixing and mark the fix as safe if you prefer.

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/flake8_simplify/rules/split_of_static_string.rs`:88 on 2024-10-31 13:54_

> The touch with maxsplit is nice but I haven't seen a single example in the ecosystem results. That's why I'm not sure if it's worth the complexity or if we should just bail on providing a fix in this case.

Maxsplit=1 is quite common (arguably people should use `str.partition`).

Another reason to keep the logic is that it can be useful to generalise for this pylint rule:
https://pylint.readthedocs.io/en/stable/user_guide/messages/convention/use-maxsplit-arg.html

---

_@sbrugman reviewed on 2024-10-31 13:54_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/codes.rs`:483 on 2024-10-31 17:03_

Nit: How about simplifying it to `SplitStaticString`?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_simplify/rules/split_of_static_string.rs`:122 on 2024-10-31 17:05_

I think we should bail here if `maxplit` is a negative number other than `-1`

---

_@MichaReiser approved on 2024-10-31 17:11_

This is awesome. Thank you. It's up to you if you want to address any of the two comments. I merge whenever you tell me the PR's good to go :)

---

_@sbrugman reviewed on 2024-10-31 18:14_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/flake8_simplify/rules/split_of_static_string.rs`:122 on 2024-10-31 18:14_

It's rare, but `maxsplit=-2` behaves as if maxsplit is omitted.
From that perspective, any negative number could be flagged as non-idiomatic default, but that would be a different rule.

Working code in the wild:

https://github.com/localstack/localstack/blob/master/localstack-core/localstack/services/kms/utils.py#L17

(This is valid code, although likely a mistake)

---

_@sbrugman reviewed on 2024-10-31 18:17_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/codes.rs`:483 on 2024-10-31 18:17_

Will do. I inserted "of"  when I saw "to" in "static-join-to-f-string", but in hindsight that name does not even adhere to the naming convention (name the thing the rule detects)

---

_Comment by @sbrugman on 2024-10-31 18:49_

@MichaReiser processed the comments, thanks for the pointers

---

_@MichaReiser reviewed on 2024-11-01 09:08_

Could we add a test for `maxsplit=0`. I suspect that we're handling that incorrectly to be the same as `maxsplit=-1` where it isn't. 

```py
>>> "a,b,c".split(',', maxsplit=0)
['a,b,c']
>>> "a,b,c".split(',', maxsplit=-1)
['a', 'b', 'c']
>>> "a,b,c".split(',', maxsplit=-2)
```

* negative values => Same as `maxsplit` None
* 0 => No split

---

_Comment by @MichaReiser on 2024-11-01 14:51_

To make things more interesting:

```
>>> "a,b,c".split(',', maxsplit=-0)
['a,b,c']
>>> "a,b,c".split(',', maxsplit=-1)
['a', 'b', 'c']
```

---

_Comment by @sbrugman on 2024-11-02 10:03_

You're right, the `maxsplit=0` case wasn't handled correctly. (And nice touch on `-0` )

---

_@charliermarsh approved on 2024-11-02 17:02_

---

_Comment by @charliermarsh on 2024-11-02 17:10_

I decided to make it safe _unless_ there are comments in the range (in which case, we still fix, but as unsafe).

---

_Merged by @charliermarsh on 2024-11-02 17:15_

---

_Closed by @charliermarsh on 2024-11-02 17:15_

---

_Branch deleted on 2024-11-03 13:32_

---
