```yaml
number: 13585
title: "[`pycodestyle`] Implement continued indentation related rules (`E12x`)"
type: pull_request
state: open
author: LordAro
labels: []
assignees: []
base: main
head: e12x-continued-indentation
created_at: 2024-10-01T10:17:58Z
updated_at: 2025-10-29T15:05:27Z
url: https://github.com/astral-sh/ruff/pull/13585
synced_at: 2026-01-12T15:55:44Z
```

# [`pycodestyle`] Implement continued indentation related rules (`E12x`)

---

_@LordAro_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

I've made a start at implementing the remaining pycodestyle E12x warnings. Largely by translating the implementation in pycodestyle

See #2402

## Test Plan

<!-- How was it tested? -->

Copied E12.py from pycodestyle and added the relevant test cases.

E133 is untested (the same as pycodestyle!)

---

_Comment by @codspeed-hq[bot] on 2024-10-01 10:23_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/LordAro%3Ae12x-continued-indentation?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #13585 will **not alter performance**

<sub>Comparing <code>LordAro:e12x-continued-indentation</code> (f2c0bbe) with <code>main</code> (2272880)</sub>



### Summary

`✅ 32` untouched  





---

_Comment by @github-actions[bot] on 2024-10-01 16:32_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+867 -2788 violations, +0 -0 fixes in 13 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/06f8f8e6082371df603571aa3fde8c763ed4f77e/superset/db_engine_specs/hana.py#L57'>superset/db_engine_specs/hana.py:57:17:</a> E128 Continuation line under-indented for visual indent
+ <a href='https://github.com/apache/superset/blob/06f8f8e6082371df603571aa3fde8c763ed4f77e/superset/db_engine_specs/hive.py#L265'>superset/db_engine_specs/hive.py:265:17:</a> E128 Continuation line under-indented for visual indent
+ <a href='https://github.com/apache/superset/blob/06f8f8e6082371df603571aa3fde8c763ed4f77e/superset/db_engine_specs/oracle.py#L55'>superset/db_engine_specs/oracle.py:55:17:</a> E128 Continuation line under-indented for visual indent
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+7 -202 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/aws/aws-sam-cli/blob/6bd3eb5b36b3da366d52bcb06991929a70953d2e/tests/integration/testdata/buildcmd/terraform/application_outside_root_directory/src/artifacts/layer1/lib.py#L1'>tests/integration/testdata/buildcmd/terraform/application_outside_root_directory/src/artifacts/layer1/lib.py:1:1:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/aws/aws-sam-cli/blob/6bd3eb5b36b3da366d52bcb06991929a70953d2e/tests/integration/testdata/buildcmd/terraform/application_outside_root_directory/src/artifacts/layer1/lib.py#L1'>tests/integration/testdata/buildcmd/terraform/application_outside_root_directory/src/artifacts/layer1/lib.py:1:8:</a> F401 [*] `boto3` imported but unused
- <a href='https://github.com/aws/aws-sam-cli/blob/6bd3eb5b36b3da366d52bcb06991929a70953d2e/tests/integration/testdata/buildcmd/terraform/application_outside_root_directory/src/artifacts/layer1/lib.py#L3'>tests/integration/testdata/buildcmd/terraform/application_outside_root_directory/src/artifacts/layer1/lib.py:3:1:</a> E302 [*] Expected 2 blank lines, found 1
- <a href='https://github.com/aws/aws-sam-cli/blob/6bd3eb5b36b3da366d52bcb06991929a70953d2e/tests/integration/testdata/buildcmd/terraform/application_outside_root_directory_windows/src/artifacts/layer1/lib.py#L1'>tests/integration/testdata/buildcmd/terraform/application_outside_root_directory_windows/src/artifacts/layer1/lib.py:1:1:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/aws/aws-sam-cli/blob/6bd3eb5b36b3da366d52bcb06991929a70953d2e/tests/integration/testdata/buildcmd/terraform/application_outside_root_directory_windows/src/artifacts/layer1/lib.py#L1'>tests/integration/testdata/buildcmd/terraform/application_outside_root_directory_windows/src/artifacts/layer1/lib.py:1:8:</a> F401 [*] `boto3` imported but unused
- <a href='https://github.com/aws/aws-sam-cli/blob/6bd3eb5b36b3da366d52bcb06991929a70953d2e/tests/integration/testdata/buildcmd/terraform/application_outside_root_directory_windows/src/artifacts/layer1/lib.py#L3'>tests/integration/testdata/buildcmd/terraform/application_outside_root_directory_windows/src/artifacts/layer1/lib.py:3:1:</a> E302 [*] Expected 2 blank lines, found 1
- <a href='https://github.com/aws/aws-sam-cli/blob/6bd3eb5b36b3da366d52bcb06991929a70953d2e/tests/integration/testdata/buildcmd/terraform/application_outside_root_directory_windows_container/src/artifacts/layer1/lib.py#L1'>tests/integration/testdata/buildcmd/terraform/application_outside_root_directory_windows_container/src/artifacts/layer1/lib.py:1:1:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/aws/aws-sam-cli/blob/6bd3eb5b36b3da366d52bcb06991929a70953d2e/tests/integration/testdata/buildcmd/terraform/application_outside_root_directory_windows_container/src/artifacts/layer1/lib.py#L1'>tests/integration/testdata/buildcmd/terraform/application_outside_root_directory_windows_container/src/artifacts/layer1/lib.py:1:8:</a> F401 [*] `boto3` imported but unused
- <a href='https://github.com/aws/aws-sam-cli/blob/6bd3eb5b36b3da366d52bcb06991929a70953d2e/tests/integration/testdata/buildcmd/terraform/application_outside_root_directory_windows_container/src/artifacts/layer1/lib.py#L3'>tests/integration/testdata/buildcmd/terraform/application_outside_root_directory_windows_container/src/artifacts/layer1/lib.py:3:1:</a> E302 [*] Expected 2 blank lines, found 1
- <a href='https://github.com/aws/aws-sam-cli/blob/6bd3eb5b36b3da366d52bcb06991929a70953d2e/tests/integration/testdata/buildcmd/terraform/unsupported/lambda_function_with_count_and_invalid_sam_metadata/lambda_src/layer1/lib.py#L1'>tests/integration/testdata/buildcmd/terraform/unsupported/lambda_function_with_count_and_invalid_sam_metadata/lambda_src/layer1/lib.py:1:1:</a> I001 [*] Import block is un-sorted or un-formatted
... 199 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/binary-husky/gpt_academic">binary-husky/gpt_academic</a> (+114 -2500 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/binary-husky/gpt_academic/blob/07ece29c7ce429d6df1c92f74261975c4faedb6a/check_proxy.py#L106'>check_proxy.py:106:11:</a> RUF002 Docstring contains ambiguous `：` (FULLWIDTH COLON). Did you mean `:` (COLON)?
- <a href='https://github.com/binary-husky/gpt_academic/blob/07ece29c7ce429d6df1c92f74261975c4faedb6a/check_proxy.py#L112'>check_proxy.py:112:42:</a> RUF002 Docstring contains ambiguous `，` (FULLWIDTH COMMA). Did you mean `,` (COMMA)?
- <a href='https://github.com/binary-husky/gpt_academic/blob/07ece29c7ce429d6df1c92f74261975c4faedb6a/check_proxy.py#L117'>check_proxy.py:117:17:</a> RUF002 Docstring contains ambiguous `，` (FULLWIDTH COMMA). Did you mean `,` (COMMA)?
- <a href='https://github.com/binary-husky/gpt_academic/blob/07ece29c7ce429d6df1c92f74261975c4faedb6a/check_proxy.py#L119'>check_proxy.py:119:5:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/binary-husky/gpt_academic/blob/07ece29c7ce429d6df1c92f74261975c4faedb6a/check_proxy.py#L128'>check_proxy.py:128:44:</a> RUF001 String contains ambiguous `，` (FULLWIDTH COMMA). Did you mean `,` (COMMA)?
- <a href='https://github.com/binary-husky/gpt_academic/blob/07ece29c7ce429d6df1c92f74261975c4faedb6a/check_proxy.py#L128'>check_proxy.py:128:80:</a> RUF001 String contains ambiguous `，` (FULLWIDTH COMMA). Did you mean `,` (COMMA)?
- <a href='https://github.com/binary-husky/gpt_academic/blob/07ece29c7ce429d6df1c92f74261975c4faedb6a/check_proxy.py#L12'>check_proxy.py:12:37:</a> RUF002 Docstring contains ambiguous `（` (FULLWIDTH LEFT PARENTHESIS). Did you mean `(` (LEFT PARENTHESIS)?
- <a href='https://github.com/binary-husky/gpt_academic/blob/07ece29c7ce429d6df1c92f74261975c4faedb6a/check_proxy.py#L12'>check_proxy.py:12:56:</a> RUF002 Docstring contains ambiguous `）` (FULLWIDTH RIGHT PARENTHESIS). Did you mean `)` (RIGHT PARENTHESIS)?
- <a href='https://github.com/binary-husky/gpt_academic/blob/07ece29c7ce429d6df1c92f74261975c4faedb6a/check_proxy.py#L135'>check_proxy.py:135:18:</a> RUF001 String contains ambiguous `，` (FULLWIDTH COMMA). Did you mean `,` (COMMA)?
- <a href='https://github.com/binary-husky/gpt_academic/blob/07ece29c7ce429d6df1c92f74261975c4faedb6a/check_proxy.py#L136'>check_proxy.py:136:32:</a> E701 Multiple statements on one line (colon)
- <a href='https://github.com/binary-husky/gpt_academic/blob/07ece29c7ce429d6df1c92f74261975c4faedb6a/check_proxy.py#L136'>check_proxy.py:136:47:</a> E702 Multiple statements on one line (semicolon)
- <a href='https://github.com/binary-husky/gpt_academic/blob/07ece29c7ce429d6df1c92f74261975c4faedb6a/check_proxy.py#L141'>check_proxy.py:141:5:</a> E722 Do not use bare `except`
- <a href='https://github.com/binary-husky/gpt_academic/blob/07ece29c7ce429d6df1c92f74261975c4faedb6a/check_proxy.py#L142'>check_proxy.py:142:28:</a> RUF001 String contains ambiguous `，` (FULLWIDTH COMMA). Did you mean `,` (COMMA)?
- <a href='https://github.com/binary-husky/gpt_academic/blob/07ece29c7ce429d6df1c92f74261975c4faedb6a/check_proxy.py#L142'>check_proxy.py:142:85:</a> RUF001 String contains ambiguous `，` (FULLWIDTH COMMA). Did you mean `,` (COMMA)?
- <a href='https://github.com/binary-husky/gpt_academic/blob/07ece29c7ce429d6df1c92f74261975c4faedb6a/check_proxy.py#L144'>check_proxy.py:144:16:</a> RUF001 String contains ambiguous `，` (FULLWIDTH COMMA). Did you mean `,` (COMMA)?
... 356 additional changes omitted for rule RUF001
- <a href='https://github.com/binary-husky/gpt_academic/blob/07ece29c7ce429d6df1c92f74261975c4faedb6a/check_proxy.py#L148'>check_proxy.py:148:32:</a> E701 Multiple statements on one line (colon)
- <a href='https://github.com/binary-husky/gpt_academic/blob/07ece29c7ce429d6df1c92f74261975c4faedb6a/check_proxy.py#L148'>check_proxy.py:148:47:</a> E702 Multiple statements on one line (semicolon)
- <a href='https://github.com/binary-husky/gpt_academic/blob/07ece29c7ce429d6df1c92f74261975c4faedb6a/check_proxy.py#L157'>check_proxy.py:157:30:</a> RUF002 Docstring contains ambiguous `，` (FULLWIDTH COMMA). Did you mean `,` (COMMA)?
... 142 additional changes omitted for rule RUF002
- <a href='https://github.com/binary-husky/gpt_academic/blob/07ece29c7ce429d6df1c92f74261975c4faedb6a/check_proxy.py#L161'>check_proxy.py:161:32:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/07ece29c7ce429d6df1c92f74261975c4faedb6a/check_proxy.py#L163'>check_proxy.py:163:5:</a> E722 Do not use bare `except`
- <a href='https://github.com/binary-husky/gpt_academic/blob/07ece29c7ce429d6df1c92f74261975c4faedb6a/check_proxy.py#L179'>check_proxy.py:179:9:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/binary-husky/gpt_academic/blob/07ece29c7ce429d6df1c92f74261975c4faedb6a/check_proxy.py#L183'>check_proxy.py:183:12:</a> E701 Multiple statements on one line (colon)
- <a href='https://github.com/binary-husky/gpt_academic/blob/07ece29c7ce429d6df1c92f74261975c4faedb6a/check_proxy.py#L184'>check_proxy.py:184:15:</a> E701 Multiple statements on one line (colon)
- <a href='https://github.com/binary-husky/gpt_academic/blob/07ece29c7ce429d6df1c92f74261975c4faedb6a/check_proxy.py#L184'>check_proxy.py:184:9:</a> E722 Do not use bare `except`
- <a href='https://github.com/binary-husky/gpt_academic/blob/07ece29c7ce429d6df1c92f74261975c4faedb6a/check_proxy.py#L191'>check_proxy.py:191:32:</a> UP015 [*] Unnecessary mode argument
- <a href='https://github.com/binary-husky/gpt_academic/blob/07ece29c7ce429d6df1c92f74261975c4faedb6a/check_proxy.py#L194'>check_proxy.py:194:54:</a> E226 [*] Missing whitespace around arithmetic operator
- <a href='https://github.com/binary-husky/gpt_academic/blob/07ece29c7ce429d6df1c92f74261975c4faedb6a/check_proxy.py#L1'>check_proxy.py:1:1:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/binary-husky/gpt_academic/blob/07ece29c7ce429d6df1c92f74261975c4faedb6a/check_proxy.py#L1'>check_proxy.py:1:1:</a> I002 [*] Missing required import: `from __future__ import annotations`
- <a href='https://github.com/binary-husky/gpt_academic/blob/07ece29c7ce429d6df1c92f74261975c4faedb6a/check_proxy.py#L203'>check_proxy.py:203:17:</a> E722 Do not use bare `except`
- <a href='https://github.com/binary-husky/gpt_academic/blob/07ece29c7ce429d6df1c92f74261975c4faedb6a/check_proxy.py#L214'>check_proxy.py:214:5:</a> E722 Do not use bare `except`
- <a href='https://github.com/binary-husky/gpt_academic/blob/07ece29c7ce429d6df1c92f74261975c4faedb6a/check_proxy.py#L221'>check_proxy.py:221:1:</a> E302 [*] Expected 2 blank lines, found 1
- <a href='https://github.com/binary-husky/gpt_academic/blob/07ece29c7ce429d6df1c92f74261975c4faedb6a/check_proxy.py#L226'>check_proxy.py:226:5:</a> I001 [*] Import block is un-sorted or un-formatted
- <a href='https://github.com/binary-husky/gpt_academic/blob/07ece29c7ce429d6df1c92f74261975c4faedb6a/check_proxy.py#L234'>check_proxy.py:234:1:</a> E302 [*] Expected 2 blank lines, found 1
- <a href='https://github.com/binary-husky/gpt_academic/blob/07ece29c7ce429d6df1c92f74261975c4faedb6a/check_proxy.py#L249'>check_proxy.py:249:52:</a> E701 Multiple statements on one line (colon)
- <a href='https://github.com/binary-husky/gpt_academic/blob/07ece29c7ce429d6df1c92f74261975c4faedb6a/check_proxy.py#L39'>check_proxy.py:39:5:</a> E722 Do not use bare `except`
... 36 additional changes omitted for rule E722
... 2579 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+449 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/advanced/extensions/widget.py#L81'>examples/advanced/extensions/widget.py:81:5:</a> E128 Continuation line under-indented for visual indent
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/axes/twin_axes.py#L34'>examples/basic/axes/twin_axes.py:34:5:</a> E128 Continuation line under-indented for visual indent
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/axes/twin_axes.py#L35'>examples/basic/axes/twin_axes.py:35:5:</a> E128 Continuation line under-indented for visual indent
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/axes/twin_axes.py#L36'>examples/basic/axes/twin_axes.py:36:1:</a> E124 Closing bracket does not match visual indentation
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/transform_customjs.py#L36'>examples/basic/data/transform_customjs.py:36:9:</a> E128 Continuation line under-indented for visual indent
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/transform_customjs.py#L38'>examples/basic/data/transform_customjs.py:38:9:</a> E128 Continuation line under-indented for visual indent
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/layouts/sizing_mode_multiple.py#L41'>examples/basic/layouts/sizing_mode_multiple.py:41:2:</a> E128 Continuation line under-indented for visual indent
... 290 additional changes omitted for rule E128
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/js_callbacks/color_sliders.py#L27'>examples/interaction/js_callbacks/color_sliders.py:27:9:</a> E127 Continuation line over-indented for visual indent
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/js_callbacks/customjs_for_selection.py#L41'>examples/interaction/js_callbacks/customjs_for_selection.py:41:1:</a> E124 Closing bracket does not match visual indentation
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/js_callbacks/open_url.py#L11'>examples/interaction/js_callbacks/open_url.py:11:5:</a> E123 Closing bracket does not match indentation of opening bracket's line
... 439 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/fronzbot/blinkpy">fronzbot/blinkpy</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/fronzbot/blinkpy/blob/e2c747b5ad295424b08ff4fb03204129155666fc/blinksync/forms.py#L20'>blinksync/forms.py:20:25:</a> E128 Continuation line under-indented for visual indent
+ <a href='https://github.com/fronzbot/blinkpy/blob/e2c747b5ad295424b08ff4fb03204129155666fc/blinksync/forms.py#L21'>blinksync/forms.py:21:25:</a> E128 Continuation line under-indented for visual indent
+ <a href='https://github.com/fronzbot/blinkpy/blob/e2c747b5ad295424b08ff4fb03204129155666fc/blinksync/forms.py#L22'>blinksync/forms.py:22:25:</a> E124 Closing bracket does not match visual indentation
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/dd8f9fbb1a02b18a5d14dfaebd6b250dc43de553/src/latch_cli/snakemake/serialize.py#L86'>src/latch_cli/snakemake/serialize.py:86:17:</a> E126 Continuation line over-indented for hanging indent
+ <a href='https://github.com/latchbio/latch/blob/dd8f9fbb1a02b18a5d14dfaebd6b250dc43de553/src/latch_cli/snakemake/serialize.py#L87'>src/latch_cli/snakemake/serialize.py:87:13:</a> E121 Continuation line under-indented for hanging indent
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+70 -86 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/768a2dd209a06be909d3b3daa2959603ec7ecadf/examples/alter.py#L22'>examples/alter.py:22:9:</a> E126 Continuation line over-indented for hanging indent
+ <a href='https://github.com/milvus-io/pymilvus/blob/768a2dd209a06be909d3b3daa2959603ec7ecadf/examples/bm25.py#L48'>examples/bm25.py:48:9:</a> E126 Continuation line over-indented for hanging indent
+ <a href='https://github.com/milvus-io/pymilvus/blob/768a2dd209a06be909d3b3daa2959603ec7ecadf/examples/bulk_import/example_bulkinsert_csv.py#L137'>examples/bulk_import/example_bulkinsert_csv.py:137:42:</a> E128 Continuation line under-indented for visual indent
+ <a href='https://github.com/milvus-io/pymilvus/blob/768a2dd209a06be909d3b3daa2959603ec7ecadf/examples/bulk_import/example_bulkinsert_csv.py#L138'>examples/bulk_import/example_bulkinsert_csv.py:138:42:</a> E128 Continuation line under-indented for visual indent
+ <a href='https://github.com/milvus-io/pymilvus/blob/768a2dd209a06be909d3b3daa2959603ec7ecadf/examples/bulk_import/example_bulkinsert_csv.py#L139'>examples/bulk_import/example_bulkinsert_csv.py:139:42:</a> E128 Continuation line under-indented for visual indent
+ <a href='https://github.com/milvus-io/pymilvus/blob/768a2dd209a06be909d3b3daa2959603ec7ecadf/examples/bulk_import/example_bulkinsert_json.py#L180'>examples/bulk_import/example_bulkinsert_json.py:180:42:</a> E128 Continuation line under-indented for visual indent
+ <a href='https://github.com/milvus-io/pymilvus/blob/768a2dd209a06be909d3b3daa2959603ec7ecadf/examples/bulk_import/example_bulkinsert_json.py#L181'>examples/bulk_import/example_bulkinsert_json.py:181:42:</a> E128 Continuation line under-indented for visual indent
+ <a href='https://github.com/milvus-io/pymilvus/blob/768a2dd209a06be909d3b3daa2959603ec7ecadf/examples/bulk_import/example_bulkinsert_withfunction.py#L69'>examples/bulk_import/example_bulkinsert_withfunction.py:69:9:</a> E122 Continuation line missing indentation or outdented
+ <a href='https://github.com/milvus-io/pymilvus/blob/768a2dd209a06be909d3b3daa2959603ec7ecadf/examples/bulk_import/example_bulkinsert_withfunction.py#L70'>examples/bulk_import/example_bulkinsert_withfunction.py:70:9:</a> E122 Continuation line missing indentation or outdented
+ <a href='https://github.com/milvus-io/pymilvus/blob/768a2dd209a06be909d3b3daa2959603ec7ecadf/examples/cert/example_tls1.py#L42'>examples/cert/example_tls1.py:42:29:</a> E128 Continuation line under-indented for visual indent
... 12 additional changes omitted for rule E128
... 146 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/3da2c1c14e8ad55d8cd22efee75e8477e4403997/scripts/validate_rst_title_capitalization.py#L269'>scripts/validate_rst_title_capitalization.py:269:21:</a> E122 Continuation line missing indentation or outdented
+ <a href='https://github.com/pandas-dev/pandas/blob/3da2c1c14e8ad55d8cd22efee75e8477e4403997/scripts/validate_unwanted_patterns.py#L323'>scripts/validate_unwanted_patterns.py:323:13:</a> E128 Continuation line under-indented for visual indent
+ <a href='https://github.com/pandas-dev/pandas/blob/3da2c1c14e8ad55d8cd22efee75e8477e4403997/scripts/validate_unwanted_patterns.py#L325'>scripts/validate_unwanted_patterns.py:325:13:</a> E128 Continuation line under-indented for visual indent
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+138 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/377675308288aafbb32d7d2f9136bb52e3fa6a77/rotkehlchen/accounting/constants.py#L6'>rotkehlchen/accounting/constants.py:6:58:</a> E126 Continuation line over-indented for hanging indent
+ <a href='https://github.com/rotki/rotki/blob/377675308288aafbb32d7d2f9136bb52e3fa6a77/rotkehlchen/api/rest.py#L645'>rotkehlchen/api/rest.py:645:13:</a> E122 Continuation line missing indentation or outdented
+ <a href='https://github.com/rotki/rotki/blob/377675308288aafbb32d7d2f9136bb52e3fa6a77/rotkehlchen/api/v1/schemas.py#L1220'>rotkehlchen/api/v1/schemas.py:1220:9:</a> E125 Continuation line with same indent as next logical line
+ <a href='https://github.com/rotki/rotki/blob/377675308288aafbb32d7d2f9136bb52e3fa6a77/rotkehlchen/api/v1/schemas.py#L704'>rotkehlchen/api/v1/schemas.py:704:17:</a> E126 Continuation line over-indented for hanging indent
+ <a href='https://github.com/rotki/rotki/blob/377675308288aafbb32d7d2f9136bb52e3fa6a77/rotkehlchen/api/v1/schemas.py#L706'>rotkehlchen/api/v1/schemas.py:706:13:</a> E121 Continuation line under-indented for hanging indent
+ <a href='https://github.com/rotki/rotki/blob/377675308288aafbb32d7d2f9136bb52e3fa6a77/rotkehlchen/api/v1/schemas.py#L707'>rotkehlchen/api/v1/schemas.py:707:9:</a> E123 Closing bracket does not match indentation of opening bracket's line
+ <a href='https://github.com/rotki/rotki/blob/377675308288aafbb32d7d2f9136bb52e3fa6a77/rotkehlchen/balances/historical.py#L228'>rotkehlchen/balances/historical.py:228:18:</a> E121 Continuation line under-indented for hanging indent
+ <a href='https://github.com/rotki/rotki/blob/377675308288aafbb32d7d2f9136bb52e3fa6a77/rotkehlchen/chain/arbitrum_one/modules/umami/balances.py#L138'>rotkehlchen/chain/arbitrum_one/modules/umami/balances.py:138:28:</a> E121 Continuation line under-indented for hanging indent
+ <a href='https://github.com/rotki/rotki/blob/377675308288aafbb32d7d2f9136bb52e3fa6a77/rotkehlchen/chain/arbitrum_one/modules/umami/decoder.py#L232'>rotkehlchen/chain/arbitrum_one/modules/umami/decoder.py:232:21:</a> E126 Continuation line over-indented for hanging indent
+ <a href='https://github.com/rotki/rotki/blob/377675308288aafbb32d7d2f9136bb52e3fa6a77/rotkehlchen/chain/base/modules/echo/decoder.py#L53'>rotkehlchen/chain/base/modules/echo/decoder.py:53:17:</a> E126 Continuation line over-indented for hanging indent
... 128 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (104 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E128 | 390 | 390 | 0 | 0 | 0 |
| RUF001 | 361 | 0 | 361 | 0 | 0 |
| RUF003 | 224 | 0 | 224 | 0 | 0 |
| E302 | 220 | 0 | 220 | 0 | 0 |
| I001 | 182 | 0 | 182 | 0 | 0 |
| F401 | 182 | 0 | 182 | 0 | 0 |
| E251 | 162 | 0 | 162 | 0 | 0 |
| E231 | 161 | 0 | 161 | 0 | 0 |
| RUF002 | 147 | 0 | 147 | 0 | 0 |
| E701 | 132 | 0 | 132 | 0 | 0 |
| E261 | 122 | 0 | 122 | 0 | 0 |
| E127 | 116 | 116 | 0 | 0 | 0 |
| E226 | 97 | 0 | 97 | 0 | 0 |
| E126 | 82 | 82 | 0 | 0 | 0 |
| E121 | 77 | 77 | 0 | 0 | 0 |
| E131 | 63 | 63 | 0 | 0 | 0 |
| I002 | 63 | 0 | 63 | 0 | 0 |
| E252 | 60 | 0 | 60 | 0 | 0 |
| E122 | 52 | 52 | 0 | 0 | 0 |
| F541 | 49 | 0 | 49 | 0 | 0 |
| C416 | 49 | 0 | 49 | 0 | 0 |
| E722 | 41 | 0 | 41 | 0 | 0 |
| F841 | 40 | 0 | 40 | 0 | 0 |
| E402 | 40 | 0 | 40 | 0 | 0 |
| E303 | 38 | 0 | 38 | 0 | 0 |
| E401 | 36 | 0 | 36 | 0 | 0 |
| T201 | 35 | 0 | 35 | 0 | 0 |
| E125 | 31 | 31 | 0 | 0 | 0 |
| B007 | 28 | 0 | 28 | 0 | 0 |
| E123 | 27 | 27 | 0 | 0 | 0 |
| E124 | 24 | 24 | 0 | 0 | 0 |
| E702 | 17 | 0 | 17 | 0 | 0 |
| E306 | 16 | 0 | 16 | 0 | 0 |
| B006 | 16 | 0 | 16 | 0 | 0 |
| UP015 | 15 | 0 | 15 | 0 | 0 |
| E305 | 15 | 0 | 15 | 0 | 0 |
| RUF052 | 13 | 0 | 13 | 0 | 0 |
| SIM102 | 12 | 0 | 12 | 0 | 0 |
| E225 | 12 | 0 | 12 | 0 | 0 |
| E262 | 12 | 0 | 12 | 0 | 0 |
| UP039 | 11 | 0 | 11 | 0 | 0 |
| B901 | 9 | 0 | 9 | 0 | 0 |
| F811 | 8 | 0 | 8 | 0 | 0 |
| UP032 | 8 | 0 | 8 | 0 | 0 |
| SIM103 | 8 | 0 | 8 | 0 | 0 |
| E265 | 7 | 0 | 7 | 0 | 0 |
| PIE790 | 6 | 0 | 6 | 0 | 0 |
| SIM108 | 6 | 0 | 6 | 0 | 0 |
| E129 | 5 | 5 | 0 | 0 | 0 |
| E211 | 5 | 0 | 5 | 0 | 0 |
| B904 | 5 | 0 | 5 | 0 | 0 |
| E222 | 5 | 0 | 5 | 0 | 0 |
| E221 | 5 | 0 | 5 | 0 | 0 |
| E116 | 5 | 0 | 5 | 0 | 0 |
| E731 | 5 | 0 | 5 | 0 | 0 |
| E201 | 5 | 0 | 5 | 0 | 0 |
| E111 | 4 | 0 | 4 | 0 | 0 |
| E272 | 4 | 0 | 4 | 0 | 0 |
| E703 | 4 | 0 | 4 | 0 | 0 |
| RUF013 | 4 | 0 | 4 | 0 | 0 |
| PIE810 | 4 | 0 | 4 | 0 | 0 |
| E301 | 3 | 0 | 3 | 0 | 0 |
| E266 | 3 | 0 | 3 | 0 | 0 |
| E228 | 3 | 0 | 3 | 0 | 0 |
| RUF039 | 3 | 0 | 3 | 0 | 0 |
| E741 | 3 | 0 | 3 | 0 | 0 |
| SIM114 | 3 | 0 | 3 | 0 | 0 |
| E117 | 3 | 0 | 3 | 0 | 0 |
| SIM117 | 3 | 0 | 3 | 0 | 0 |
| W292 | 3 | 0 | 3 | 0 | 0 |
| INP001 | 3 | 0 | 3 | 0 | 0 |
| SIM105 | 2 | 0 | 2 | 0 | 0 |
| RUF027 | 2 | 0 | 2 | 0 | 0 |
| RUF010 | 2 | 0 | 2 | 0 | 0 |
| SIM118 | 2 | 0 | 2 | 0 | 0 |
| C419 | 2 | 0 | 2 | 0 | 0 |
| RUF005 | 2 | 0 | 2 | 0 | 0 |
| UP028 | 2 | 0 | 2 | 0 | 0 |
| RUF029 | 2 | 0 | 2 | 0 | 0 |
| S311 | 2 | 0 | 2 | 0 | 0 |
| ANN001 | 2 | 0 | 2 | 0 | 0 |
| PLR2004 | 1 | 0 | 1 | 0 | 0 |
| RUF015 | 1 | 0 | 1 | 0 | 0 |
| SIM115 | 1 | 0 | 1 | 0 | 0 |
| B005 | 1 | 0 | 1 | 0 | 0 |
| SIM300 | 1 | 0 | 1 | 0 | 0 |
| E502 | 1 | 0 | 1 | 0 | 0 |
| SIM223 | 1 | 0 | 1 | 0 | 0 |
| UP031 | 1 | 0 | 1 | 0 | 0 |
| UP034 | 1 | 0 | 1 | 0 | 0 |
| E202 | 1 | 0 | 1 | 0 | 0 |
| PGH003 | 1 | 0 | 1 | 0 | 0 |
| C414 | 1 | 0 | 1 | 0 | 0 |
| C403 | 1 | 0 | 1 | 0 | 0 |
| SIM201 | 1 | 0 | 1 | 0 | 0 |
| UP009 | 1 | 0 | 1 | 0 | 0 |
| UP025 | 1 | 0 | 1 | 0 | 0 |
| C420 | 1 | 0 | 1 | 0 | 0 |
| B020 | 1 | 0 | 1 | 0 | 0 |
| C408 | 1 | 0 | 1 | 0 | 0 |
| B028 | 1 | 0 | 1 | 0 | 0 |
| W291 | 1 | 0 | 1 | 0 | 0 |
| N816 | 1 | 0 | 1 | 0 | 0 |
| RET504 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @LordAro on 2024-10-01 18:15_

+589 new violations is a bit nicer than +56549

---

_Comment by @MichaReiser on 2024-10-07 07:33_

There's some overlap with https://github.com/astral-sh/ruff/pull/11349 and we would have to look into the performance regressions. 

---

_Comment by @LordAro on 2024-10-07 08:10_

That's what I get for not checking existing PRs. I'd imagine much of the performance regression is to do with the 
 `logical_line.lines.locator.line_start` on every token. Not sure how to better do it, I'll see if I can glean anything from the previous PRs (which for my own reference includes #8557)

---

_Comment by @LordAro on 2024-10-31 08:20_

Quick push to show I'm still working on it - I know the tests are still broken, but I'm happy that E12{1,2,3} are correct now. (But probably still slow)

---

_Renamed from "Add support for E12x continued indentation pycodestyle linter errors" to "[`pycodestyle`] Implement continued indentation related rules (`E12x`)" by @LordAro on 2024-11-05 18:12_

---

_Marked ready for review by @LordAro on 2024-11-12 22:50_

---

_Comment by @LordAro on 2024-11-12 22:52_

I'm calling this ready. The output matches pycodestyle for all added warnings (though there's no testing for E133 here or in pycodestyle)

There's still a couple of TODOs in the code, and honestly the code itself is a big blob. Hopefully someone has some better ideas for how to achieve things :)

---

_Comment by @LordAro on 2024-11-21 09:38_

Guess ruff is getting close to the limit of the number of rules - I've bumped `RULESET_SIZE`

---

_Comment by @LordAro on 2024-11-29 10:38_

@MichaReiser et al, do let me know if there's anything more I can do

(sorry, I know how annoying these sort of pings can be!)

---

_Review comment by @LordAro on `crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/continued_indentation.rs`:128 on 2024-11-29 10:39_

flagging myself to actually finish off the docs

---

_@LordAro reviewed on 2024-11-29 10:40_

---

_Review comment by @LordAro on `crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/continued_indentation.rs`:547 on 2024-11-29 11:04_

flagging this ugly line for review

---

_Review comment by @LordAro on `crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/continued_indentation.rs`:710 on 2024-11-29 11:05_

flagging this to see if there's a better way of doing it (and indeed if it's logically correct)

---

_@LordAro reviewed on 2024-11-29 11:06_

---

_@LordAro reviewed on 2024-11-29 11:06_

---

_Review comment by @LordAro on `crates/ruff_linter/src/rules/pycodestyle/rules/logical_lines/continued_indentation.rs`:128 on 2024-11-29 11:06_

Since I had to actually put some work in to get the rebase to work, I've done that.

---

_Comment by @oalfred on 2025-08-13 10:49_

Any updates on how this is progressing? I'm eagerly waiting for these, if they are added to ruff I'm going to ditch pylint completely.

---

_Comment by @LordAro on 2025-08-13 12:42_

I've not thought about this in some time due to an obvious lack of interest from the devs (and it wasn't particularly pressing for me either). Probably wouldn't be difficult to rebase if I tried. I believe the general thinking is "use ruff format", which obviously has other limitations

---

_Comment by @smuuf on 2025-09-30 10:35_

Again got bit today by `E122 continuation line missing indentation or outdented` not being caught by ruff.

Ruff team, pls? 😢 

---
