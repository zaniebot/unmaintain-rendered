```yaml
number: 17454
title: "[`pylint`] Implement `missing-maxsplit-arg` (`PLC0207`)"
type: pull_request
state: merged
author: vjurczenia
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: vjurczenia/missing_maxsplit_arg
created_at: 2025-04-17T22:09:17Z
updated_at: 2025-05-28T20:49:49Z
url: https://github.com/astral-sh/ruff/pull/17454
synced_at: 2026-01-10T18:45:04Z
```

# [`pylint`] Implement `missing-maxsplit-arg` (`PLC0207`)

---

_Pull request opened by @vjurczenia on 2025-04-17 22:09_

## Summary

Implements  `use-maxsplit-arg` (`PLC0207`)
https://pylint.readthedocs.io/en/latest/user_guide/messages/convention/use-maxsplit-arg.html
> Emitted when accessing only the first or last element of str.split().
The first and last element can be accessed by using str.split(sep, maxsplit=1)[0] or str.rsplit(sep, maxsplit=1)[-1] instead.

This is part of https://github.com/astral-sh/ruff/issues/970

## Test Plan

`cargo test`

Additionally compared Ruff output to Pylint:
```
pylint --disable=all --enable=use-maxsplit-arg crates/ruff_linter/resources/test/fixtures/pylint/missing_maxsplit_arg.py

cargo run -p ruff -- check crates/ruff_linter/resources/test/fixtures/pylint/missing_maxsplit_arg.py --no-cache --select PLC0207
```

---

_Comment by @Skylion007 on 2025-04-18 16:13_

Does the original pylint handle slicing too (ie calling split but only using the first n elements)? If not, that would be a great generalization of this rule, maybe one that can be made ruff specific

---

_Comment by @vjurczenia on 2025-04-18 18:29_

No, [it only checks for indexes -1 and 0](https://github.com/pylint-dev/pylint/blob/main/pylint/checkers/refactoring/recommendation_checker.py#L169), but making it more general is a very neat idea.

Having looked into the Pylint source just now, I also realize that this PR is missing the following functionality:
- checking the value of a variable used as the index
- checking whether a variable used as the index might be mutated at runtime

I'm happy to add this functionality to stay aligned with Pylint and would appreciate maintainer input on whether or not to implement the generalized rule.

---

_Label `rule` added by @ntBre on 2025-04-18 18:33_

---

_@ntBre reviewed on 2025-04-18 18:36_

---

_Review comment by @ntBre on `crates/ruff_linter/src/codes.rs`:192 on 2025-04-18 18:36_

New rules should be added to `RuleGroup::Preview`

---

_Comment by @ntBre on 2025-04-18 18:38_

I think making the rule more general (both by handling slices and by handling variables) sounds nice, but it's also okay to add a simpler form of a rule first and extend it in follow-up PRs.

---

_Label `preview` added by @ntBre on 2025-04-18 18:38_

---

_Comment by @github-actions[bot] on 2025-04-18 18:43_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+47 -0 violations, +0 -0 fixes in 14 projects; 41 projects unchanged)

<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/5c17b8f3343a5cd403651201cbfa07bb1edcea1f/noxfile.py#L445'>noxfile.py:445:16:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+9 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/9d31c74ec8b40275d11beb1a790268ebf633827e/dev/backport/update_backport_status.py#L78'>dev/backport/update_backport_status.py:78:21:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/apache/airflow/blob/9d31c74ec8b40275d11beb1a790268ebf633827e/dev/breeze/src/airflow_breeze/commands/common_options.py#L442'>dev/breeze/src/airflow_breeze/commands/common_options.py:442:26:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/apache/airflow/blob/9d31c74ec8b40275d11beb1a790268ebf633827e/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L2625'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:2625:50:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/apache/airflow/blob/9d31c74ec8b40275d11beb1a790268ebf633827e/dev/breeze/src/airflow_breeze/prepare_providers/provider_documentation.py#L658'>dev/breeze/src/airflow_breeze/prepare_providers/provider_documentation.py:658:34:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/apache/airflow/blob/9d31c74ec8b40275d11beb1a790268ebf633827e/dev/breeze/src/airflow_breeze/utils/run_utils.py#L118'>dev/breeze/src/airflow_breeze/utils/run_utils.py:118:25:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/apache/airflow/blob/9d31c74ec8b40275d11beb1a790268ebf633827e/dev/breeze/src/airflow_breeze/utils/version_utils.py#L44'>dev/breeze/src/airflow_breeze/utils/version_utils.py:44:16:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/apache/airflow/blob/9d31c74ec8b40275d11beb1a790268ebf633827e/providers/amazon/tests/system/amazon/aws/example_bedrock.py#L66'>providers/amazon/tests/system/amazon/aws/example_bedrock.py:66:24:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/apache/airflow/blob/9d31c74ec8b40275d11beb1a790268ebf633827e/providers/amazon/tests/system/amazon/aws/example_glue.py#L76'>providers/amazon/tests/system/amazon/aws/example_glue.py:76:12:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/apache/airflow/blob/9d31c74ec8b40275d11beb1a790268ebf633827e/scripts/in_container/install_airflow_and_providers.py#L32'>scripts/in_container/install_airflow_and_providers.py:32:21:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/ea5a609d0b7014f264a54dab1381d3dac6e494ff/tests/common/query_context_generator.py#L214'>tests/common/query_context_generator.py:214:29:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/apache/superset/blob/ea5a609d0b7014f264a54dab1381d3dac6e494ff/tests/common/query_context_generator.py#L255'>tests/common/query_context_generator.py:255:22:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/io/notebook.py#L663'>src/bokeh/io/notebook.py:663:26:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/token.py#L142'>src/bokeh/util/token.py:142:41:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/token.py#L155'>src/bokeh/util/token.py:155:41:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/db43b77698ff53df9386cd9e822840597f57ba32/securedrop/store.py#L345'>securedrop/store.py:345:49:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/28fb8d5a24501faf648acf55c649a4df72c8b876/ibis/examples/gen_registry.py#L125'>ibis/examples/gen_registry.py:125:52:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+9 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/b808d272849516d1c0b902421edf1933a87d9afa/libs/core/langchain_core/_api/deprecation.py#L362'>libs/core/langchain_core/_api/deprecation.py:362:17:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/langchain-ai/langchain/blob/b808d272849516d1c0b902421edf1933a87d9afa/libs/core/langchain_core/_api/deprecation.py#L362'>libs/core/langchain_core/_api/deprecation.py:362:56:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/langchain-ai/langchain/blob/b808d272849516d1c0b902421edf1933a87d9afa/libs/core/langchain_core/_api/deprecation.py#L370'>libs/core/langchain_core/_api/deprecation.py:370:17:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/langchain-ai/langchain/blob/b808d272849516d1c0b902421edf1933a87d9afa/libs/core/langchain_core/_api/deprecation.py#L371'>libs/core/langchain_core/_api/deprecation.py:371:16:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/langchain-ai/langchain/blob/b808d272849516d1c0b902421edf1933a87d9afa/libs/core/langchain_core/_api/deprecation.py#L475'>libs/core/langchain_core/_api/deprecation.py:475:24:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/langchain-ai/langchain/blob/b808d272849516d1c0b902421edf1933a87d9afa/libs/core/langchain_core/_api/deprecation.py#L494'>libs/core/langchain_core/_api/deprecation.py:494:27:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/langchain-ai/langchain/blob/b808d272849516d1c0b902421edf1933a87d9afa/libs/core/langchain_core/runnables/graph_mermaid.py#L157'>libs/core/langchain_core/runnables/graph_mermaid.py:157:24:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/langchain-ai/langchain/blob/b808d272849516d1c0b902421edf1933a87d9afa/libs/core/langchain_core/utils/mustache.py#L85'>libs/core/langchain_core/utils/mustache.py:85:19:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/langchain-ai/langchain/blob/b808d272849516d1c0b902421edf1933a87d9afa/libs/core/langchain_core/utils/utils.py#L140'>libs/core/langchain_core/utils/utils.py:140:32:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/f2f24a9d4396da43fad1d6f5145f85f6e1f5386c/pandas/compat/_optional.py#L166'>pandas/compat/_optional.py:166:14:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/cibuildwheel">pypa/cibuildwheel</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pypa/cibuildwheel/blob/7e099863cdbe9f0c241aa5ef52747f2db6b6f310/bin/update_pythons.py#L150'>bin/update_pythons.py:150:22:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/pypa/cibuildwheel/blob/7e099863cdbe9f0c241aa5ef52747f2db6b6f310/cibuildwheel/platforms/macos.py#L153'>cibuildwheel/platforms/macos.py:153:31:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/pypa/cibuildwheel/blob/7e099863cdbe9f0c241aa5ef52747f2db6b6f310/cibuildwheel/selector.py#L78'>cibuildwheel/selector.py:78:26:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/9970528f5ce011c0db45926b43711d6e402e32a6/rotkehlchen/assets/converters.py#L417'>rotkehlchen/assets/converters.py:417:17:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+11 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/66c123dd437cdfc3711cb111e184667d2e307ecb/zerver/lib/send_email.py#L287'>zerver/lib/send_email.py:287:16:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/zulip/zulip/blob/66c123dd437cdfc3711cb111e184667d2e307ecb/zerver/lib/send_email.py#L429'>zerver/lib/send_email.py:429:21:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/zulip/zulip/blob/66c123dd437cdfc3711cb111e184667d2e307ecb/zerver/lib/upload/local.py#L67'>zerver/lib/upload/local.py:67:17:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/zulip/zulip/blob/66c123dd437cdfc3711cb111e184667d2e307ecb/zerver/lib/upload/s3.py#L176'>zerver/lib/upload/s3.py:176:25:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/zulip/zulip/blob/66c123dd437cdfc3711cb111e184667d2e307ecb/zerver/lib/webhooks/common.py#L242'>zerver/lib/webhooks/common.py:242:26:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/zulip/zulip/blob/66c123dd437cdfc3711cb111e184667d2e307ecb/zerver/lib/webhooks/common.py#L269'>zerver/lib/webhooks/common.py:269:16:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/zulip/zulip/blob/66c123dd437cdfc3711cb111e184667d2e307ecb/zerver/middleware.py#L228'>zerver/middleware.py:228:49:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/zulip/zulip/blob/66c123dd437cdfc3711cb111e184667d2e307ecb/zerver/views/upload.py#L374'>zerver/views/upload.py:374:16:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/zulip/zulip/blob/66c123dd437cdfc3711cb111e184667d2e307ecb/zerver/webhooks/gitlab/view.py#L46'>zerver/webhooks/gitlab/view.py:46:36:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/zulip/zulip/blob/66c123dd437cdfc3711cb111e184667d2e307ecb/zerver/webhooks/stripe/view.py#L356'>zerver/webhooks/stripe/view.py:356:39:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/zulip/zulip/blob/66c123dd437cdfc3711cb111e184667d2e307ecb/zerver/webhooks/wekan/view.py#L15'>zerver/webhooks/wekan/view.py:15:12:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/wntrblm/nox">wntrblm/nox</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/wntrblm/nox/blob/382fc8c36292056d315929dad59cb12e74bdfb92/tests/test_sessions.py#L1199'>tests/test_sessions.py:1199:44:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pytest-dev/pytest/blob/f59e1ad661832f18bce28a9522be2c6d37485d09/src/_pytest/config/findpaths.py#L160'>src/_pytest/config/findpaths.py:160:16:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/pytest-dev/pytest/blob/f59e1ad661832f18bce28a9522be2c6d37485d09/src/_pytest/terminal.py#L1007'>src/_pytest/terminal.py:1007:40:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
+ <a href='https://github.com/pytest-dev/pytest/blob/f59e1ad661832f18bce28a9522be2c6d37485d09/src/_pytest/terminal.py#L459'>src/_pytest/terminal.py:459:41:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/863b40e2e18d0ffe0374a7379d6f7828c89ecabe/astropy/io/ascii/latex.py#L443'>astropy/io/ascii/latex.py:443:16:</a> PLC0207 Accessing only the first or last element of `str.split()` without setting `maxsplit=1`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLC0207 | 47 | 47 | 0 | 0 | 0 |

</p>
</details>




---

_@ntBre reviewed on 2025-04-18 18:49_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/missing_maxsplit_arg.rs`:113 on 2025-04-18 18:49_

The ecosystem check turned up some results like this:

```py
ext = file_path.rsplit(".", 2)[-1].lower()
```

The `maxsplit` argument doesn't have to be passed as a keyword. We have a `find_argument_value` helper that should handle this and also extract the value for your comparison to 1:

https://github.com/astral-sh/ruff/blob/1108f662e47e8ff5456f00e5a0742f1cd9e95852/crates/ruff_python_ast/src/nodes.rs#L2854-L2859



---

_@ntBre reviewed on 2025-04-18 18:50_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/missing_maxsplit_arg.rs`:113 on 2025-04-18 18:50_

Oh I see, you're doing the positional comparison below, and this is a true positive because the `maxsplit` value also needs to be 1. In any case, this should help to combine the keyword and positional checks!

---

_@vjurczenia reviewed on 2025-04-20 18:28_

---

_Review comment by @vjurczenia on `crates/ruff_linter/src/codes.rs`:192 on 2025-04-20 18:28_

Updated [here](https://github.com/astral-sh/ruff/pull/17454/commits/38a9c81675e083775917671c47128af0ba297110).

---

_@vjurczenia reviewed on 2025-04-20 18:40_

---

_Review comment by @vjurczenia on `crates/ruff_linter/src/rules/pylint/rules/missing_maxsplit_arg.rs`:113 on 2025-04-20 18:40_

It's way neater with `find_argument_value`, so thanks for that!

However, this ecosystem check failure had me re-evaluate the test included, so I compared it to the Pylint one and noticed that `maxsplit=2` should be a [passing case](https://github.com/pylint-dev/pylint/blob/7d6f3f230e038eabee2efc75628329fe74aa943e/tests/functional/u/use/use_maxsplit_arg.py#L25). The relevant Pylint source is [here](https://github.com/pylint-dev/pylint/blob/main/pylint/checkers/refactoring/recommendation_checker.py#L139).

Following that discovery, I've updated the test cases to closer match the Pylint ones, which left me with [a few that require additional implementation](https://github.com/vjurczenia/ruff/blob/vjurczenia/missing_maxsplit_arg/crates/ruff_linter/resources/test/fixtures/pylint/missing_maxsplit_arg.py#L62). These will require identifying:

- when split is called on slices of strings
- kwargs set via unpacked dicts
- if index is set via variable
- if the index variable is modified in a loop (that includes the split call)
- if split is called on a static class member that happens to be a string (I'm uncertain about this one, but think it should be possible.)
- if split is called on a method that returns a string (I'm uncertain that this one is possible.)

As before, any advice is appreciated.

---

_Converted to draft by @vjurczenia on 2025-04-23 00:56_

---

_@vjurczenia reviewed on 2025-04-23 00:57_

---

_Review comment by @vjurczenia on `crates/ruff_linter/src/rules/pylint/rules/missing_maxsplit_arg.rs`:113 on 2025-04-23 00:57_

> if split is called on a static class member that happens to be a string

I've just managed to figure this one out, and its given me a bit of a confidence boost on the rest of them, so I've converted this PR to a Draft until they're done.

---

_@ntBre reviewed on 2025-04-23 15:06_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/missing_maxsplit_arg.rs`:113 on 2025-04-23 15:06_

Awesome, thanks for the update and for your work on this. Ping me again if you end up needing help. I'm also happy merging a simpler first draft of the rule that we can expand later, if these end up being trickier than you want to deal with!

If it comes to that, let's just include tests for the unhandled cases with clear TODO comments for future work.

---

_@vjurczenia reviewed on 2025-05-12 21:22_

---

_Review comment by @vjurczenia on `crates/ruff_linter/src/rules/pylint/rules/missing_maxsplit_arg.rs`:113 on 2025-05-12 21:22_

Hi @ntBre , hope all is well. Sorry for the delay on getting back to this.

As you can see from my most recent commits, I've managed to get through a few of the cases I had listed. However, I think I've hit a bit of a wall with the rest. The remaining ones are:

- if index is set via a variable
- if the index variable is modified in a loop (that includes the split call)
- if split is called on a method that returns a string
- if kwargs are set via an unpacked dict variable

I've documented these as TODO in the test, and am happy to go ahead with merging this simpler first draft if you are.

---

_Marked ready for review by @vjurczenia on 2025-05-12 21:22_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/missing_maxsplit_arg.rs`:17 on 2025-05-14 17:07_

```suggestion
/// Calling `str.split()` without `maxsplit` set splits on every delimiter in the
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/missing_maxsplit_arg.rs`:78 on 2025-05-14 17:14_

Should we use `let-else` here since the else branch is just `return`?

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/missing_maxsplit_arg.rs`:88 on 2025-05-14 17:21_

Could you add a comment here explaining what this is for? It looks like you're recursing into nested subscripts, but I didn't see any tests that would exercise that. I could be missing something here, though.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/missing_maxsplit_arg.rs`:111 on 2025-05-14 17:24_

This might be nice as a little `is_string` helper function here.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/missing_maxsplit_arg.rs`:117 on 2025-05-14 17:53_

I think it would be pretty unusual to pass an unpacked literal dict, and we don't handle this case in a somewhat similar rule [split-static-string (SIM905)](https://docs.astral.sh/ruff/rules/split-static-string/#split-static-string-sim905).

Actually we currently mishandle that case in SIM905, so maybe it is worth keeping here. I opened an issue to track this as a broader problem: https://github.com/astral-sh/ruff/issues/18101.

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/pylint/missing_maxsplit_arg.py`:132 on 2025-05-14 18:04_

It would be fine to uncomment these just to confirm that they aren't currently handled. You could also update your nice `# [missing-maxsplit-arg]` comments to look like `# TODO: [missing-maxsplit-arg]`. That's not special here in ruff, but that's similar to what we do for our ty mdtests, and it looks nice in the diff when removing the TODOs eventually.

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/pylint/missing_maxsplit_arg.py`:142 on 2025-05-14 18:06_

I think there was a recent change that _might_ allow `typing::is_string` to detect these cases, actually! No worries if not though, it's more a limitation of the helper function than the rule implementation. (It might be worth noting something like that for some of the TODO cases).

---

_@ntBre reviewed on 2025-05-14 18:13_

Thanks, this is looking great! I think this would be a good place to finish this PR and leave other extensions as follow-ups, as you said. It could be fun to add an auto-fix for this rule, as another follow-up, if you're interested :)

---

_@vjurczenia reviewed on 2025-05-15 23:38_

---

_Review comment by @vjurczenia on `crates/ruff_linter/src/rules/pylint/rules/missing_maxsplit_arg.rs`:17 on 2025-05-15 23:38_

Done in https://github.com/astral-sh/ruff/pull/17454/commits/b67bc213675034a4e1d2c54a73cd564619dac15e

---

_@vjurczenia reviewed on 2025-05-15 23:39_

---

_Review comment by @vjurczenia on `crates/ruff_linter/src/rules/pylint/rules/missing_maxsplit_arg.rs`:78 on 2025-05-15 23:39_

Absolutely. Done in https://github.com/astral-sh/ruff/pull/17454/commits/b67bc213675034a4e1d2c54a73cd564619dac15e

---

_@vjurczenia reviewed on 2025-05-15 23:45_

---

_Review comment by @vjurczenia on `crates/ruff_linter/src/rules/pylint/rules/missing_maxsplit_arg.rs`:88 on 2025-05-15 23:45_

Sure, I''ve added a comment here https://github.com/astral-sh/ruff/pull/17454/commits/b67bc213675034a4e1d2c54a73cd564619dac15e

The test case for this is `"1,2,3"[::-1][::-1].split(",")[0]` since while theoretically you could slice a slice ad infinitum, slicing a slice just once should be enough to demonstrate the functionality.

---

_@vjurczenia reviewed on 2025-05-15 23:45_

---

_Review comment by @vjurczenia on `crates/ruff_linter/src/rules/pylint/rules/missing_maxsplit_arg.rs`:111 on 2025-05-15 23:45_

Agreed it's a bit neater with the helper function. Added here https://github.com/astral-sh/ruff/pull/17454/commits/b67bc213675034a4e1d2c54a73cd564619dac15e

---

_@vjurczenia reviewed on 2025-05-15 23:52_

---

_Review comment by @vjurczenia on `crates/ruff_linter/resources/test/fixtures/pylint/missing_maxsplit_arg.py`:132 on 2025-05-15 23:52_

I like that idea, so I've actioned it here https://github.com/astral-sh/ruff/pull/17454/commits/b67bc213675034a4e1d2c54a73cd564619dac15e

One thing to note is that, uncommented, these test cases give us false positive errors:
```rust
## Test unpacked dict instance kwargs 
# Errors
kwargs_without_maxsplit = {"seq": ","}
"1,2,3".split(**kwargs_without_maxsplit)[0]  # TODO: [missing-maxsplit-arg]
# OK
kwargs_with_maxsplit = {"maxsplit": 1}
"1,2,3".split(",", **kwargs_with_maxsplit)[0]
kwargs_with_maxsplit = {"sep": ",", "maxsplit": 1}
"1,2,3".split(**kwargs_with_maxsplit)[0]
```

I think this is fine, it's very clear in the test file that these are TODO.

---

_@vjurczenia reviewed on 2025-05-16 00:00_

---

_Review comment by @vjurczenia on `crates/ruff_linter/src/rules/pylint/rules/missing_maxsplit_arg.rs`:117 on 2025-05-16 00:00_

Neat. I agree it's a bit of a silly case, but handling it brings us closer to pylint's behavior. 

Thanks for the shoutout in that other issue!

---

_@vjurczenia reviewed on 2025-05-16 00:16_

---

_Review comment by @vjurczenia on `crates/ruff_linter/resources/test/fixtures/pylint/missing_maxsplit_arg.py`:142 on 2025-05-16 00:16_

I went ahead and rebased locally to get the most recent implementation of the helper and couldn't get it to recognize the return of my test method `def get_string(self) -> str:` as a string.

However, I did add some more comments to the tests here https://github.com/astral-sh/ruff/pull/17454/commits/378de24b52341eb06ec8fea67c3a8cc53cffc6a4

---

_@ntBre reviewed on 2025-05-19 21:11_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/missing_maxsplit_arg.rs`:88 on 2025-05-19 21:11_

That makes sense, thanks!

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/missing_maxsplit_arg.rs`:66 on 2025-05-19 21:25_

```suggestion
fn is_string(expr: &Expr, semantic: &SemanticModel) -> bool {
    if let Expr::Name(name) = expr {
        semantic
            .only_binding(name)
            .is_some_and(|binding_id| typing::is_string(semantic.binding(binding_id), semantic))
    } else if let Some(binding_id) = semantic.lookup_attribute(expr) {
        typing::is_string(semantic.binding(binding_id), semantic)
    } else {
        expr.is_string_literal_expr()
    }
}
```

Something like this might be a little neater.

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/pylint/missing_maxsplit_arg.py`:184 on 2025-05-19 21:31_

It might be good to have a `# TODO: false positive` comment here just to make it totally clear without seeing the heading (like in the snapshots).

---

_@ntBre approved on 2025-05-19 21:33_

Thank you, this looks great!

I had two minor nits but this is otherwise good to go.

---

_@vjurczenia reviewed on 2025-05-19 23:40_

---

_Review comment by @vjurczenia on `crates/ruff_linter/src/rules/pylint/rules/missing_maxsplit_arg.rs`:66 on 2025-05-19 23:40_

It absolutely is. The original looks downright goofy now that I'm looking back on it.

Applied suggestion as part of https://github.com/astral-sh/ruff/pull/17454/commits/5997aa77f4c3c15c5909dd31165903fdbded9124

---

_@vjurczenia reviewed on 2025-05-19 23:41_

---

_Review comment by @vjurczenia on `crates/ruff_linter/resources/test/fixtures/pylint/missing_maxsplit_arg.py`:184 on 2025-05-19 23:41_

Sounds good to me, added in https://github.com/astral-sh/ruff/pull/17454/commits/5997aa77f4c3c15c5909dd31165903fdbded9124

---

_@ntBre approved on 2025-05-28 20:30_

Thanks again, this is great!

---

_Comment by @ntBre on 2025-05-28 20:39_

I merged `main` to pick up our rust version updates and then ran `cargo fmt` to make CI happy

---

_Renamed from "`[pylint]` Implement `missing-maxsplit-arg` (`PLC0207`)" to "[`pylint`] Implement `missing-maxsplit-arg` (`PLC0207`)" by @ntBre on 2025-05-28 20:43_

---

_Merged by @ntBre on 2025-05-28 20:46_

---

_Closed by @ntBre on 2025-05-28 20:46_

---
