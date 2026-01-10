```yaml
number: 11757
title: allow human-friendly rule names in noqa directives
type: pull_request
state: closed
author: sd-collins
labels: []
assignees: []
draft: true
base: main
head: human-friendly-names
created_at: 2024-06-05T16:49:06Z
updated_at: 2024-12-30T10:08:05Z
url: https://github.com/astral-sh/ruff/pull/11757
synced_at: 2026-01-10T20:42:26Z
```

# allow human-friendly rule names in noqa directives

---

_Pull request opened by @sd-collins on 2024-06-05 16:49_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Works towards #1773.

Allows users to write noqa directives with human-friendly rule names. Users can write noqa directives with a comma-separated list of rule names (`# noqa: ambiguous-variable-name , unused-variable`), or even a mix of codes and names (`# noqa: E741, unused-variable`).

Additionally, rules have been added for enforcing the exclusive use of either codes or names, with fixes defined for both.

This is a changeset we are looking to use internally at Pexip. We have been migrating from pylint to ruff, but had pushback due to codes being required for noqa directives (pylint allows human-friendly rule names here). We'd like to upstream these changes, but are aware you may wish to implement some or all of this differently, so we're submitting this as a draft PR for feedback.

## Caveats

We made some compromises with the parsing rules for noqa directives that reference rules by name to avoid confusing comments with rule names. The most significant one is that lists of rule names must be comma separated, which allows us to determine if the next word is supposed to be a rule name or is just a comment. This stricter lexing rule does not apply to lists of rule codes. (so `# noqa: E741 F841` is still fine).

We also require a rule name to be followed by either a comma or a whitespace character (or the end of the line). This prevents us from seeing e.g. `some_rule_name` and interpreting it as a rule named `some` followed by a comment `_rule_name`.

## Test Plan

<!-- How was it tested? -->

- Updated existing noqa snapshot tests and added new ones to cover using rule names.
- Added snapshot tests for new `RUF102` (`noqa-by-code`) and `RUF103` (`noqa-by-name`) lints.

---

_Comment by @MichaelOultram-pexip on 2024-06-05 22:28_

We've triaged the ecosystem test failure. The job is timing out in CI, but we believe this is due to the python script and not a regression in ruff performance. 

The ecosystem python script will produce the diff between baseline ruff and this PR's ruff, and we believe it's this diff which is taking so much time. The diff will be extremely large as the rule name will now be printed everywhere the rule code is printed. The ruff executable runs in the same amount of time from our investigation.

---

_Comment by @kaddkaka on 2024-11-20 17:39_

Does this PR support multiple names per rule as suggested in https://github.com/astral-sh/ruff/issues/1773#issuecomment-2163951669 ?

---

_Comment by @MichaelOultram-pexip on 2024-11-20 17:44_

@kaddkaka the PR in it's current state does not have this functionality. I will look into extending `crates/ruff_linter/src/rule_redirects.rs` to support rule names. This will take me a few days due to other commitments.

---

_Comment by @github-actions[bot] on 2024-11-20 17:46_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+3 -0 violations, +0 -0 fixes in 1 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/47cdb84b351bd75c8232406d5673405dcb7efd64/tests/decorators/test_setup_teardown.py#L129'>tests/decorators/test_setup_teardown.py:129:34:</a> RUF100 [*] Unused `noqa` directive (unknown: `check`)
+ <a href='https://github.com/apache/airflow/blob/47cdb84b351bd75c8232406d5673405dcb7efd64/tests/decorators/test_setup_teardown.py#L147'>tests/decorators/test_setup_teardown.py:147:34:</a> RUF100 [*] Unused `noqa` directive (unknown: `check`)
+ <a href='https://github.com/apache/airflow/blob/47cdb84b351bd75c8232406d5673405dcb7efd64/tests/decorators/test_setup_teardown.py#L992'>tests/decorators/test_setup_teardown.py:992:30:</a> RUF100 [*] Unused `noqa` directive (unknown: `check`)
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF100 | 3 | 3 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+9665 -338 violations, +0 -0 fixes in 20 projects; 34 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+36 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/ac5a936d187ff89d6756e5f33bf0b2965e7b7685/disnake/enums.py#L839'>disnake/enums.py:839:26:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: RUF001 (ambiguous-unicode-character-string)
+ <a href='https://github.com/DisnakeDev/disnake/blob/ac5a936d187ff89d6756e5f33bf0b2965e7b7685/disnake/ext/commands/bot_base.py#L122'>disnake/ext/commands/bot_base.py:122:83:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: TRY004 (type-check-without-type-error)
+ <a href='https://github.com/DisnakeDev/disnake/blob/ac5a936d187ff89d6756e5f33bf0b2965e7b7685/disnake/ext/commands/core.py#L512'>disnake/ext/commands/core.py:512:25:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: B012 (jump-statement-in-finally)
+ <a href='https://github.com/DisnakeDev/disnake/blob/ac5a936d187ff89d6756e5f33bf0b2965e7b7685/disnake/ext/commands/ctx_menus_core.py#L121'>disnake/ext/commands/ctx_menus_core.py:121:25:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: B012 (jump-statement-in-finally)
+ <a href='https://github.com/DisnakeDev/disnake/blob/ac5a936d187ff89d6756e5f33bf0b2965e7b7685/disnake/ext/commands/ctx_menus_core.py#L221'>disnake/ext/commands/ctx_menus_core.py:221:25:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: B012 (jump-statement-in-finally)
+ <a href='https://github.com/DisnakeDev/disnake/blob/ac5a936d187ff89d6756e5f33bf0b2965e7b7685/disnake/ext/commands/slash_core.py#L654'>disnake/ext/commands/slash_core.py:654:25:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: B012 (jump-statement-in-finally)
+ <a href='https://github.com/DisnakeDev/disnake/blob/ac5a936d187ff89d6756e5f33bf0b2965e7b7685/disnake/ext/commands/view.py#L18'>disnake/ext/commands/view.py:18:16:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: RUF001 (ambiguous-unicode-character-string)
+ <a href='https://github.com/DisnakeDev/disnake/blob/ac5a936d187ff89d6756e5f33bf0b2965e7b7685/disnake/ext/commands/view.py#L21'>disnake/ext/commands/view.py:21:16:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: RUF001 (ambiguous-unicode-character-string)
+ <a href='https://github.com/DisnakeDev/disnake/blob/ac5a936d187ff89d6756e5f33bf0b2965e7b7685/disnake/ext/commands/view.py#L8'>disnake/ext/commands/view.py:8:16:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: RUF001 (ambiguous-unicode-character-string)
+ <a href='https://github.com/DisnakeDev/disnake/blob/ac5a936d187ff89d6756e5f33bf0b2965e7b7685/disnake/ext/commands/view.py#L9'>disnake/ext/commands/view.py:9:16:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: RUF001 (ambiguous-unicode-character-string)
... 26 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+103 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/.github/tests/test_download_pretrained.py#L102'>.github/tests/test_download_pretrained.py:102:19:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: RUF015 (unnecessary-iterable-allocation-for-first-element)
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/.github/tests/test_download_pretrained.py#L10'>.github/tests/test_download_pretrained.py:10:29:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: E402 (module-import-not-at-top-of-file)
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/.github/tests/test_download_pretrained.py#L26'>.github/tests/test_download_pretrained.py:26:19:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: RUF015 (unnecessary-iterable-allocation-for-first-element)
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/.github/tests/test_download_pretrained.py#L46'>.github/tests/test_download_pretrained.py:46:19:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: RUF015 (unnecessary-iterable-allocation-for-first-element)
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/.github/tests/test_download_pretrained.py#L63'>.github/tests/test_download_pretrained.py:63:19:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: RUF015 (unnecessary-iterable-allocation-for-first-element)
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/.github/tests/test_download_pretrained.py#L83'>.github/tests/test_download_pretrained.py:83:19:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: RUF015 (unnecessary-iterable-allocation-for-first-element)
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/.github/tests/test_mr_generate_summary.py#L4'>.github/tests/test_mr_generate_summary.py:4:49:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: E402 (module-import-not-at-top-of-file)
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/.github/tests/test_mr_publish_results.py#L7'>.github/tests/test_mr_publish_results.py:7:35:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: E402 (module-import-not-at-top-of-file)
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/.github/tests/test_validate_gpus.py#L8'>.github/tests/test_validate_gpus.py:8:22:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: E402 (module-import-not-at-top-of-file)
+ <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/.github/tests/test_validate_gpus.py#L9'>.github/tests/test_validate_gpus.py:9:23:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: E402 (module-import-not-at-top-of-file)
... 93 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+512 -15 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/47cdb84b351bd75c8232406d5673405dcb7efd64/airflow/cli/commands/dag_processor_command.py#L42'>airflow/cli/commands/dag_processor_command.py:42:81:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: F401 (unused-import)
+ <a href='https://github.com/apache/airflow/blob/47cdb84b351bd75c8232406d5673405dcb7efd64/airflow/cli/commands/dag_processor_command.py#L43'>airflow/cli/commands/dag_processor_command.py:43:53:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: F401 (unused-import)
+ <a href='https://github.com/apache/airflow/blob/47cdb84b351bd75c8232406d5673405dcb7efd64/airflow/cli/commands/task_command.py#L326'>airflow/cli/commands/task_command.py:326:81:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: F401 (unused-import)
+ <a href='https://github.com/apache/airflow/blob/47cdb84b351bd75c8232406d5673405dcb7efd64/airflow/cli/commands/task_command.py#L327'>airflow/cli/commands/task_command.py:327:53:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: F401 (unused-import)
+ <a href='https://github.com/apache/airflow/blob/47cdb84b351bd75c8232406d5673405dcb7efd64/airflow/configuration.py#L1'>airflow/configuration.py:1:1:</a> D100 Missing docstring in public module
- <a href='https://github.com/apache/airflow/blob/47cdb84b351bd75c8232406d5673405dcb7efd64/airflow/configuration.py#L1'>airflow/configuration.py:1:1:</a> D100 Missing docstring in public module
+ <a href='https://github.com/apache/airflow/blob/47cdb84b351bd75c8232406d5673405dcb7efd64/airflow/configuration.py#L303'>airflow/configuration.py:303:65:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: UP006 (non-pep585-annotation)
+ <a href='https://github.com/apache/airflow/blob/47cdb84b351bd75c8232406d5673405dcb7efd64/airflow/macros/__init__.py#L20'>airflow/macros/__init__.py:20:14:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: F401 (unused-import)
+ <a href='https://github.com/apache/airflow/blob/47cdb84b351bd75c8232406d5673405dcb7efd64/airflow/macros/__init__.py#L21'>airflow/macros/__init__.py:21:14:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: F401 (unused-import)
... 56 additional changes omitted for rule F401
+ <a href='https://github.com/apache/airflow/blob/47cdb84b351bd75c8232406d5673405dcb7efd64/airflow/models/baseoperator.py#L119'>airflow/models/baseoperator.py:119:49:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: TC001 (typing-only-first-party-import)
... 517 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1696 -170 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/1102d41842fa7136c464ce550980ea6c21db2bb4/scripts/build_docker.py#L143'>scripts/build_docker.py:143:15:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: F841 (unused-variable)
+ <a href='https://github.com/apache/superset/blob/1102d41842fa7136c464ce550980ea6c21db2bb4/scripts/build_docker.py#L287'>scripts/build_docker.py:287:35:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: F841 (unused-variable)
+ <a href='https://github.com/apache/superset/blob/1102d41842fa7136c464ce550980ea6c21db2bb4/scripts/build_docker.py#L85'>scripts/build_docker.py:85:44:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: E741 (ambiguous-variable-name)
+ <a href='https://github.com/apache/superset/blob/1102d41842fa7136c464ce550980ea6c21db2bb4/superset/__init__.py#L20'>superset/__init__.py:20:38:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: F401 (unused-import)
+ <a href='https://github.com/apache/superset/blob/1102d41842fa7136c464ce550980ea6c21db2bb4/superset/__init__.py#L22'>superset/__init__.py:22:18:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: F401 (unused-import)
+ <a href='https://github.com/apache/superset/blob/1102d41842fa7136c464ce550980ea6c21db2bb4/superset/__init__.py#L24'>superset/__init__.py:24:10:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: F401 (unused-import)
+ <a href='https://github.com/apache/superset/blob/1102d41842fa7136c464ce550980ea6c21db2bb4/superset/__init__.py#L25'>superset/__init__.py:25:20:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: F401 (unused-import)
+ <a href='https://github.com/apache/superset/blob/1102d41842fa7136c464ce550980ea6c21db2bb4/superset/__init__.py#L29'>superset/__init__.py:29:24:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: F401 (unused-import)
+ <a href='https://github.com/apache/superset/blob/1102d41842fa7136c464ce550980ea6c21db2bb4/superset/__init__.py#L30'>superset/__init__.py:30:16:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: F401 (unused-import)
... 399 additional changes omitted for rule F401
+ <a href='https://github.com/apache/superset/blob/1102d41842fa7136c464ce550980ea6c21db2bb4/superset/commands/importers/v1/__init__.py#L82'>superset/commands/importers/v1/__init__.py:82:34:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: F811 (redefined-while-unused)
... 1856 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+17 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/color_mappers.py#L9'>examples/basic/data/color_mappers.py:9:5:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: E501 (line-too-long)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/js_callbacks/color_sliders.py#L9'>examples/interaction/js_callbacks/color_sliders.py:9:5:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: E501 (line-too-long)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/buttons.py#L15'>examples/models/buttons.py:15:5:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: E501 (line-too-long)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/calendars.py#L14'>examples/models/calendars.py:14:5:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: E501 (line-too-long)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/sliders.py#L9'>examples/models/sliders.py:9:5:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: E501 (line-too-long)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/widgets.py#L11'>examples/models/widgets.py:11:5:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: E501 (line-too-long)
... 2 additional changes omitted for rule E501
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/pd.py#L31'>src/bokeh/core/property/pd.py:31:35:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: F401 (unused-import)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/pd.py#L32'>src/bokeh/core/property/pd.py:32:54:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: F401 (unused-import)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/model/util.py#L167'>src/bokeh/model/util.py:167:28:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: F401 (unused-import)
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/model/util.py#L168'>src/bokeh/model/util.py:168:30:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: F401 (unused-import)
... 8 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+251 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/7595ca6ba4db48a4abd30996f1824bcb26ee6ee5/.github/workflows/algolia/configure-algolia-api.py#L1'>.github/workflows/algolia/configure-algolia-api.py:1:37:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: INP001 (implicit-namespace-package)
+ <a href='https://github.com/ibis-project/ibis/blob/7595ca6ba4db48a4abd30996f1824bcb26ee6ee5/.github/workflows/algolia/upload-algolia-api.py#L164'>.github/workflows/algolia/upload-algolia-api.py:164:54:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: T201 (print)
+ <a href='https://github.com/ibis-project/ibis/blob/7595ca6ba4db48a4abd30996f1824bcb26ee6ee5/.github/workflows/algolia/upload-algolia-api.py#L178'>.github/workflows/algolia/upload-algolia-api.py:178:56:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: T201 (print)
+ <a href='https://github.com/ibis-project/ibis/blob/7595ca6ba4db48a4abd30996f1824bcb26ee6ee5/.github/workflows/algolia/upload-algolia-api.py#L190'>.github/workflows/algolia/upload-algolia-api.py:190:66:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: T201 (print)
+ <a href='https://github.com/ibis-project/ibis/blob/7595ca6ba4db48a4abd30996f1824bcb26ee6ee5/.github/workflows/algolia/upload-algolia-api.py#L1'>.github/workflows/algolia/upload-algolia-api.py:1:37:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: INP001 (implicit-namespace-package)
+ <a href='https://github.com/ibis-project/ibis/blob/7595ca6ba4db48a4abd30996f1824bcb26ee6ee5/.github/workflows/algolia/upload-algolia-api.py#L203'>.github/workflows/algolia/upload-algolia-api.py:203:53:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: T201 (print)
+ <a href='https://github.com/ibis-project/ibis/blob/7595ca6ba4db48a4abd30996f1824bcb26ee6ee5/.github/workflows/algolia/upload-algolia-api.py#L208'>.github/workflows/algolia/upload-algolia-api.py:208:66:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: T201 (print)
+ <a href='https://github.com/ibis-project/ibis/blob/7595ca6ba4db48a4abd30996f1824bcb26ee6ee5/.github/workflows/algolia/upload-algolia.py#L1'>.github/workflows/algolia/upload-algolia.py:1:37:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: INP001 (implicit-namespace-package)
+ <a href='https://github.com/ibis-project/ibis/blob/7595ca6ba4db48a4abd30996f1824bcb26ee6ee5/ci/make_geography_db.py#L98'>ci/make_geography_db.py:98:21:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: T201 (print)
... 16 additional changes omitted for rule T201
+ <a href='https://github.com/ibis-project/ibis/blob/7595ca6ba4db48a4abd30996f1824bcb26ee6ee5/ibis/__init__.py#L140'>ibis/__init__.py:140:24:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: F405 (undefined-local-with-import-star-usage)
... 241 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/lnbits/lnbits">lnbits/lnbits</a> (+12 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/bolt11.py#L2'>lnbits/bolt11.py:2:25:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: F401 (unused-import)
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/bolt11.py#L5'>lnbits/bolt11.py:5:14:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: F401 (unused-import)
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/bolt11.py#L6'>lnbits/bolt11.py:6:14:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: F401 (unused-import)
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/views/api.py#L44'>lnbits/core/views/api.py:44:52:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: F401 (unused-import)
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/views/public_api.py#L54'>lnbits/core/views/public_api.py:54:51:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: RUF006 (asyncio-dangling-task)
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/nodes/lndrest.py#L179'>lnbits/nodes/lndrest.py:179:65:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: RUF006 (asyncio-dangling-task)
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/wallets/boltz.py#L75'>lnbits/wallets/boltz.py:75:112:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: E501 (line-too-long)
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/tests/wallets/test_blink.py#L135'>tests/wallets/test_blink.py:135:92:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: E501 (line-too-long)
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/tests/wallets/test_blink.py#L46'>tests/wallets/test_blink.py:46:317:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: E501 (line-too-long)
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/tests/wallets/test_blink.py#L97'>tests/wallets/test_blink.py:97:100:</a> RUF102 [*] Noqa directive lists rule codes instead of rule names: E501 (line-too-long)
... 2 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -88 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pandas-dev/pandas/blob/0e2099089406c6d5616bf9e8872154fee4960ea7/asv_bench/benchmarks/pandas_vb_common.py#L18'>asv_bench/benchmarks/pandas_vb_common.py:18:31:</a> F401 `pandas._testing` imported but unused; consider using `importlib.util.find_spec` to test for availability
- <a href='https://github.com/pandas-dev/pandas/blob/0e2099089406c6d5616bf9e8872154fee4960ea7/pandas/api/types/__init__.py#L7'>pandas/api/types/__init__.py:7:1:</a> F403 `from pandas.core.dtypes.api import *` used; unable to detect undefined names
- <a href='https://github.com/pandas-dev/pandas/blob/0e2099089406c6d5616bf9e8872154fee4960ea7/pandas/conftest.py#L1969'>pandas/conftest.py:1969:89:</a> E501 Line too long (156 > 88)
- <a href='https://github.com/pandas-dev/pandas/blob/0e2099089406c6d5616bf9e8872154fee4960ea7/pandas/core/api.py#L44'>pandas/core/api.py:44:38:</a> ICN001 `pandas.core.construction.array` should be imported as `pd_array`
- <a href='https://github.com/pandas-dev/pandas/blob/0e2099089406c6d5616bf9e8872154fee4960ea7/pandas/core/dtypes/cast.py#L1257'>pandas/core/dtypes/cast.py:1257:89:</a> E501 Line too long (104 > 88)
- <a href='https://github.com/pandas-dev/pandas/blob/0e2099089406c6d5616bf9e8872154fee4960ea7/pandas/core/dtypes/cast.py#L1262'>pandas/core/dtypes/cast.py:1262:89:</a> E501 Line too long (106 > 88)
- <a href='https://github.com/pandas-dev/pandas/blob/0e2099089406c6d5616bf9e8872154fee4960ea7/pandas/core/groupby/groupby.py#L5300'>pandas/core/groupby/groupby.py:5300:89:</a> E501 Line too long (106 > 88)
- <a href='https://github.com/pandas-dev/pandas/blob/0e2099089406c6d5616bf9e8872154fee4960ea7/pandas/io/common.py#L284'>pandas/io/common.py:284:12:</a> TID251 `urllib.request.urlopen` is banned: Use pandas.io.common.urlopen instead of urllib.request.urlopen
- <a href='https://github.com/pandas-dev/pandas/blob/0e2099089406c6d5616bf9e8872154fee4960ea7/pandas/io/formats/html.py#L98'>pandas/io/formats/html.py:98:30:</a> RUF003 Comment contains ambiguous `×` (MULTIPLICATION SIGN). Did you mean `x` (LATIN SMALL LETTER X)?
- <a href='https://github.com/pandas-dev/pandas/blob/0e2099089406c6d5616bf9e8872154fee4960ea7/pandas/tests/copy_view/index/test_index.py#L78'>pandas/tests/copy_view/index/test_index.py:78:5:</a> F841 Local variable `idx` is assigned to but never used
... 78 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (151 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E501 | 6666 | 6627 | 39 | 0 | 0 |
| E402 | 1084 | 1084 | 0 | 0 | 0 |
| F401 | 563 | 561 | 2 | 0 | 0 |
| ANN201 | 238 | 119 | 119 | 0 | 0 |
| F811 | 124 | 110 | 14 | 0 | 0 |
| F405 | 124 | 124 | 0 | 0 | 0 |
| F403 | 113 | 112 | 1 | 0 | 0 |
| D100 | 86 | 43 | 43 | 0 | 0 |
| F841 | 78 | 75 | 3 | 0 | 0 |
| F541 | 58 | 58 | 0 | 0 | 0 |
| E711 | 44 | 38 | 6 | 0 | 0 |
| RUF001 | 39 | 9 | 30 | 0 | 0 |
| PLR6301 | 38 | 19 | 19 | 0 | 0 |
| TC001 | 37 | 37 | 0 | 0 | 0 |
| E712 | 28 | 27 | 1 | 0 | 0 |
| T201 | 28 | 28 | 0 | 0 | 0 |
| PT009 | 26 | 26 | 0 | 0 | 0 |
| F821 | 22 | 22 | 0 | 0 | 0 |
| S610 | 22 | 22 | 0 | 0 | 0 |
| C901 | 20 | 19 | 1 | 0 | 0 |
| B015 | 19 | 19 | 0 | 0 | 0 |
| D102 | 18 | 9 | 9 | 0 | 0 |
| B018 | 18 | 16 | 2 | 0 | 0 |
| UP031 | 17 | 17 | 0 | 0 | 0 |
| UP032 | 17 | 17 | 0 | 0 | 0 |
| F822 | 17 | 0 | 17 | 0 | 0 |
| RUF012 | 15 | 15 | 0 | 0 | 0 |
| N802 | 15 | 15 | 0 | 0 | 0 |
| S608 | 14 | 14 | 0 | 0 | 0 |
| RUF018 | 13 | 13 | 0 | 0 | 0 |
| PLW2901 | 13 | 13 | 0 | 0 | 0 |
| RUF015 | 12 | 12 | 0 | 0 | 0 |
| ANN001 | 12 | 6 | 6 | 0 | 0 |
| E741 | 11 | 11 | 0 | 0 | 0 |
| TID251 | 10 | 7 | 3 | 0 | 0 |
| D400 | 10 | 6 | 4 | 0 | 0 |
| SIM115 | 10 | 10 | 0 | 0 | 0 |
| UP006 | 9 | 9 | 0 | 0 | 0 |
| PT012 | 8 | 8 | 0 | 0 | 0 |
| BLE001 | 8 | 8 | 0 | 0 | 0 |
| N801 | 8 | 8 | 0 | 0 | 0 |
| PERF401 | 8 | 8 | 0 | 0 | 0 |
| S102 | 7 | 7 | 0 | 0 | 0 |
| TC003 | 7 | 7 | 0 | 0 | 0 |
| ARG002 | 7 | 7 | 0 | 0 | 0 |
| B012 | 6 | 6 | 0 | 0 | 0 |
| PLW0603 | 6 | 6 | 0 | 0 | 0 |
| B007 | 6 | 6 | 0 | 0 | 0 |
| CPY001 | 6 | 3 | 3 | 0 | 0 |
| E731 | 6 | 6 | 0 | 0 | 0 |
| E722 | 6 | 6 | 0 | 0 | 0 |
| PLC1901 | 6 | 6 | 0 | 0 | 0 |
| N815 | 6 | 6 | 0 | 0 | 0 |
| E241 | 6 | 6 | 0 | 0 | 0 |
| S307 | 5 | 5 | 0 | 0 | 0 |
| DTZ005 | 5 | 5 | 0 | 0 | 0 |
| UP007 | 5 | 5 | 0 | 0 | 0 |
| S105 | 5 | 5 | 0 | 0 | 0 |
| S404 | 5 | 5 | 0 | 0 | 0 |
| PT017 | 5 | 5 | 0 | 0 | 0 |
| PLE0704 | 5 | 5 | 0 | 0 | 0 |
| B023 | 4 | 4 | 0 | 0 | 0 |
| E721 | 4 | 4 | 0 | 0 | 0 |
| D212 | 4 | 2 | 2 | 0 | 0 |
| F722 | 4 | 4 | 0 | 0 | 0 |
| INP001 | 4 | 4 | 0 | 0 | 0 |
| SIM201 | 4 | 4 | 0 | 0 | 0 |
| B008 | 4 | 4 | 0 | 0 | 0 |
| ARG001 | 4 | 4 | 0 | 0 | 0 |
| PLW1641 | 4 | 4 | 0 | 0 | 0 |
| S301 | 4 | 4 | 0 | 0 | 0 |
| N805 | 4 | 4 | 0 | 0 | 0 |
| N806 | 4 | 4 | 0 | 0 | 0 |
| TRY002 | 3 | 3 | 0 | 0 | 0 |
| S101 | 3 | 3 | 0 | 0 | 0 |
| TC004 | 3 | 2 | 1 | 0 | 0 |
| D205 | 3 | 2 | 1 | 0 | 0 |
| RUF100 | 3 | 3 | 0 | 0 | 0 |
| F402 | 3 | 3 | 0 | 0 | 0 |
| S310 | 3 | 3 | 0 | 0 | 0 |
| B027 | 3 | 3 | 0 | 0 | 0 |
| B017 | 3 | 3 | 0 | 0 | 0 |
| UP036 | 3 | 3 | 0 | 0 | 0 |
| S403 | 3 | 3 | 0 | 0 | 0 |
| PLR1714 | 3 | 3 | 0 | 0 | 0 |
| D101 | 2 | 2 | 0 | 0 | 0 |
| ANN204 | 2 | 1 | 1 | 0 | 0 |
| D104 | 2 | 1 | 1 | 0 | 0 |
| D202 | 2 | 1 | 1 | 0 | 0 |
| D415 | 2 | 1 | 1 | 0 | 0 |
| PLR0912 | 2 | 1 | 1 | 0 | 0 |
| D200 | 2 | 1 | 1 | 0 | 0 |
| E713 | 2 | 2 | 0 | 0 | 0 |
| ICN001 | 2 | 1 | 1 | 0 | 0 |
| RET503 | 2 | 2 | 0 | 0 | 0 |
| S106 | 2 | 2 | 0 | 0 | 0 |
| S603 | 2 | 2 | 0 | 0 | 0 |
| SIM202 | 2 | 2 | 0 | 0 | 0 |
| RUF006 | 2 | 2 | 0 | 0 | 0 |
| S108 | 2 | 2 | 0 | 0 | 0 |
| B024 | 2 | 2 | 0 | 0 | 0 |
| S308 | 2 | 2 | 0 | 0 | 0 |
| B006 | 2 | 2 | 0 | 0 | 0 |
| SIM118 | 2 | 2 | 0 | 0 | 0 |
| FURB118 | 2 | 2 | 0 | 0 | 0 |
| N811 | 2 | 2 | 0 | 0 | 0 |
| S501 | 2 | 2 | 0 | 0 | 0 |
| RUF003 | 2 | 0 | 2 | 0 | 0 |
| TRY004 | 1 | 1 | 0 | 0 | 0 |
| PLC0205 | 1 | 1 | 0 | 0 | 0 |
| B035 | 1 | 1 | 0 | 0 | 0 |
| I001 | 1 | 1 | 0 | 0 | 0 |
| PGH005 | 1 | 1 | 0 | 0 | 0 |
| PLE0604 | 1 | 1 | 0 | 0 | 0 |
| RUF017 | 1 | 1 | 0 | 0 | 0 |
| T203 | 1 | 1 | 0 | 0 | 0 |
| UP035 | 1 | 1 | 0 | 0 | 0 |
| B009 | 1 | 1 | 0 | 0 | 0 |
| RUF002 | 1 | 1 | 0 | 0 | 0 |
| D417 | 1 | 1 | 0 | 0 | 0 |
| RUF027 | 1 | 1 | 0 | 0 | 0 |
| ERA001 | 1 | 1 | 0 | 0 | 0 |
| SIM102 | 1 | 1 | 0 | 0 | 0 |
| S607 | 1 | 1 | 0 | 0 | 0 |
| TRY300 | 1 | 1 | 0 | 0 | 0 |
| PTH101 | 1 | 1 | 0 | 0 | 0 |
| PTH123 | 1 | 1 | 0 | 0 | 0 |
| ARG004 | 1 | 1 | 0 | 0 | 0 |
| UP038 | 1 | 1 | 0 | 0 | 0 |
| DTZ007 | 1 | 1 | 0 | 0 | 0 |
| N999 | 1 | 1 | 0 | 0 | 0 |
| S606 | 1 | 1 | 0 | 0 | 0 |
| S112 | 1 | 1 | 0 | 0 | 0 |
| B019 | 1 | 1 | 0 | 0 | 0 |
| S406 | 1 | 1 | 0 | 0 | 0 |
| S110 | 1 | 1 | 0 | 0 | 0 |
| S701 | 1 | 1 | 0 | 0 | 0 |
| PLR5501 | 1 | 1 | 0 | 0 | 0 |
| RET504 | 1 | 1 | 0 | 0 | 0 |
| N813 | 1 | 1 | 0 | 0 | 0 |
| N803 | 1 | 1 | 0 | 0 | 0 |
| PERF402 | 1 | 1 | 0 | 0 | 0 |
| PLC0414 | 1 | 1 | 0 | 0 | 0 |
| SIM103 | 1 | 1 | 0 | 0 | 0 |
| SIM117 | 1 | 1 | 0 | 0 | 0 |
| PYI024 | 1 | 1 | 0 | 0 | 0 |
| E701 | 1 | 1 | 0 | 0 | 0 |
| PLE0307 | 1 | 1 | 0 | 0 | 0 |
| PT014 | 1 | 0 | 1 | 0 | 0 |
| C416 | 1 | 0 | 1 | 0 | 0 |
| RUF029 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---

_Comment by @kaddkaka on 2024-11-20 17:55_

does all rules already have names today?

---

_Comment by @kaddkaka on 2024-11-20 17:57_

- `RUF102` (noqa-by-code) warns if code is used.  
- `RUF103` (noqa-by-name) warns if name is used.  

Is there any request to have RUF103? Is it valuable to have?

---

_Comment by @sbrugman on 2024-11-21 13:18_

For review I'd suggest to split this PR into one for extending `noqa` comments to support human-friendly rule names and one that introduces the new `noqa` ruff rules.

---

_Comment by @MichaReiser on 2024-12-30 09:13_

Thanks, @Humandoodlebug, for working on this. I'll close this PR because of inactivity, and I also think it requires some design work: For example, do we want to extend `noqa` comments, or should we introduce a ruff-specific ignore comment (e.g., `ruff: ignore[code]`) that allows human-readable names? I also don't think that we should allow mixing both codes and rule names.
Thanks again for exploring adding human readable names. This PR is a great inspiration.

---

_Closed by @MichaReiser on 2024-12-30 09:13_

---

_Comment by @MichaelOultram on 2024-12-30 10:08_

I was working to rebase in the background for @Humandoodlebug, but work/life commitments kept getting in the way.

As we mentioned in the first post, we are perfectly happy for the ruff project to take a different approach here. We at Pexip developed this patch for our specific internal needs, and decided to share based on the interest in #1773.

I'm glad the PR has given at least some inspiration, and closing seems like the right choice given the design work required.

We at Pexip will continue to use the patch internally for now. When this feature is reworked and added to ruff properly, we will migrate over and ditch our patch.

---
