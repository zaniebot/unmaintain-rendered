```yaml
number: 12535
title: "[pep8-naming] Also check invalid-module-name if file is not in package (N999)"
type: pull_request
state: closed
author: AlexElvers
labels:
  - rule
assignees: []
base: main
head: N999-without-package
created_at: 2024-07-26T16:21:04Z
updated_at: 2024-08-01T21:23:58Z
url: https://github.com/astral-sh/ruff/pull/12535
synced_at: 2026-01-12T15:55:41Z
```

# [pep8-naming] Also check invalid-module-name if file is not in package (N999)

---

_@AlexElvers_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

**Old behavior:**
If `package` is `None` in `invalid_module_name`, checking the module name is skipped. This is different from the https://pypi.org/project/flake8-module-name/ behavior, where module names are checked for all Python files (independent of being in a package).

**New behavior:**
Also check for invalid module names for files that are not in a package. This ensures that all `.py` files can be imported.

## Comparison of ruff and flake8 outputs

```
$ tree
.
├── a-a.py
├── b
│   └── b-b.py
└── c
    ├── __init__.py
    └── c-c.py

3 directories, 4 files

$ flake8 --isolated --select N999 *
a-a.py:0:1: N999 filename should be all lower case
b/b-b.py:0:1: N999 filename should be all lower case
c/c-c.py:0:1: N999 filename should be all lower case

# before change
$ ruff check --isolated --select N999 *
c/c-c.py:1:1: N999 Invalid module name: 'c-c'
Found 1 error.

# after change
$ cargo run -p ruff -- check --isolated --select N999 --no-cache *
...
a-a.py:1:1: N999 Invalid module name: 'a-a'
b/b-b.py:1:1: N999 Invalid module name: 'b-b'
c/c-c.py:1:1: N999 Invalid module name: 'c-c'
Found 3 errors.
```

## Test Plan

New test case in `crates/ruff_linter/src/rules/pep8_naming/mod.rs` and successful execution of `cargo nextest run`.



---

_Comment by @github-actions[bot] on 2024-07-27 18:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+60 -0 violations, +0 -0 fixes in 7 projects; 47 projects unchanged)

<details><summary><a href="https://github.com/Snowflake-Labs/snowcli">Snowflake-Labs/snowcli</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/740cd4559f0fecc012fea8bae5fd613d931c5871/snyk/dependency-sync.py#L1'>snyk/dependency-sync.py:1:1:</a> N999 Invalid module name: 'dependency-sync'
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/fad5e5e9d494f0cf81f35993f5fd0f6f047a3b75/scripts/tools/generate-integrations-json.py#L1'>scripts/tools/generate-integrations-json.py:1:1:</a> N999 Invalid module name: 'generate-integrations-json'
+ <a href='https://github.com/apache/airflow/blob/fad5e5e9d494f0cf81f35993f5fd0f6f047a3b75/scripts/tools/list-integrations.py#L1'>scripts/tools/list-integrations.py:1:1:</a> N999 Invalid module name: 'list-integrations'
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+36 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/advanced/extensions/font-awesome/font-awesome.py#L1'>examples/advanced/extensions/font-awesome/font-awesome.py:1:1:</a> N999 Invalid module name: 'font-awesome'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/advanced/integration/d3-voronoi.py#L1'>examples/advanced/integration/d3-voronoi.py:1:1:</a> N999 Invalid module name: 'd3-voronoi'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/AnnularWedge.py#L1'>examples/reference/models/AnnularWedge.py:1:1:</a> N999 Invalid module name: 'AnnularWedge'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/Annulus.py#L1'>examples/reference/models/Annulus.py:1:1:</a> N999 Invalid module name: 'Annulus'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/Arc.py#L1'>examples/reference/models/Arc.py:1:1:</a> N999 Invalid module name: 'Arc'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/Bezier.py#L1'>examples/reference/models/Bezier.py:1:1:</a> N999 Invalid module name: 'Bezier'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/Block.py#L1'>examples/reference/models/Block.py:1:1:</a> N999 Invalid module name: 'Block'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/Circle.py#L1'>examples/reference/models/Circle.py:1:1:</a> N999 Invalid module name: 'Circle'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/Ellipse.py#L1'>examples/reference/models/Ellipse.py:1:1:</a> N999 Invalid module name: 'Ellipse'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/HArea.py#L1'>examples/reference/models/HArea.py:1:1:</a> N999 Invalid module name: 'HArea'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/HAreaStep.py#L1'>examples/reference/models/HAreaStep.py:1:1:</a> N999 Invalid module name: 'HAreaStep'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/HBar.py#L1'>examples/reference/models/HBar.py:1:1:</a> N999 Invalid module name: 'HBar'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/HSpan.py#L1'>examples/reference/models/HSpan.py:1:1:</a> N999 Invalid module name: 'HSpan'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/HStrip.py#L1'>examples/reference/models/HStrip.py:1:1:</a> N999 Invalid module name: 'HStrip'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/HexTile.py#L1'>examples/reference/models/HexTile.py:1:1:</a> N999 Invalid module name: 'HexTile'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/ImageURL.py#L1'>examples/reference/models/ImageURL.py:1:1:</a> N999 Invalid module name: 'ImageURL'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/Line.py#L1'>examples/reference/models/Line.py:1:1:</a> N999 Invalid module name: 'Line'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/MathText.py#L1'>examples/reference/models/MathText.py:1:1:</a> N999 Invalid module name: 'MathText'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/MultiLine.py#L1'>examples/reference/models/MultiLine.py:1:1:</a> N999 Invalid module name: 'MultiLine'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/MultiPolygons.py#L1'>examples/reference/models/MultiPolygons.py:1:1:</a> N999 Invalid module name: 'MultiPolygons'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/Patch.py#L1'>examples/reference/models/Patch.py:1:1:</a> N999 Invalid module name: 'Patch'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/Patches.py#L1'>examples/reference/models/Patches.py:1:1:</a> N999 Invalid module name: 'Patches'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/Quad.py#L1'>examples/reference/models/Quad.py:1:1:</a> N999 Invalid module name: 'Quad'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/Quadratic.py#L1'>examples/reference/models/Quadratic.py:1:1:</a> N999 Invalid module name: 'Quadratic'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/Ray.py#L1'>examples/reference/models/Ray.py:1:1:</a> N999 Invalid module name: 'Ray'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/Rect.py#L1'>examples/reference/models/Rect.py:1:1:</a> N999 Invalid module name: 'Rect'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/Scatter.py#L1'>examples/reference/models/Scatter.py:1:1:</a> N999 Invalid module name: 'Scatter'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/Segment.py#L1'>examples/reference/models/Segment.py:1:1:</a> N999 Invalid module name: 'Segment'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/Step.py#L1'>examples/reference/models/Step.py:1:1:</a> N999 Invalid module name: 'Step'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/Text.py#L1'>examples/reference/models/Text.py:1:1:</a> N999 Invalid module name: 'Text'
... 6 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/251a5b112ebf9adfc22b7680c0f96c6801264cde/latch_cli/services/init/example_snakemake/scripts/plot-quals.py#L1'>latch_cli/services/init/example_snakemake/scripts/plot-quals.py:1:1:</a> N999 Invalid module name: 'plot-quals'
</pre>

</p>
</details>
<details><summary><a href="https://github.com/lnbits/lnbits">lnbits/lnbits</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/lnbits/lnbits/blob/dbb689c5c56017aa2f777327101477864e90c3d5/tools/i18n-ai-tool.py#L1'>tools/i18n-ai-tool.py:1:1:</a> N999 Invalid module name: 'i18n-ai-tool'
+ <a href='https://github.com/lnbits/lnbits/blob/dbb689c5c56017aa2f777327101477864e90c3d5/tools/i18n-check.py#L1'>tools/i18n-check.py:1:1:</a> N999 Invalid module name: 'i18n-check'
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+11 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/ac3dd44bcedbd13b385e2c3473c1f23d187c8e97/tools/pyinstaller_hooks/hook-coincurve.py#L1'>tools/pyinstaller_hooks/hook-coincurve.py:1:1:</a> N999 Invalid module name: 'hook-coincurve'
+ <a href='https://github.com/rotki/rotki/blob/ac3dd44bcedbd13b385e2c3473c1f23d187c8e97/tools/pyinstaller_hooks/hook-content_hash.py#L1'>tools/pyinstaller_hooks/hook-content_hash.py:1:1:</a> N999 Invalid module name: 'hook-content_hash'
+ <a href='https://github.com/rotki/rotki/blob/ac3dd44bcedbd13b385e2c3473c1f23d187c8e97/tools/pyinstaller_hooks/hook-eth_abi.py#L1'>tools/pyinstaller_hooks/hook-eth_abi.py:1:1:</a> N999 Invalid module name: 'hook-eth_abi'
+ <a href='https://github.com/rotki/rotki/blob/ac3dd44bcedbd13b385e2c3473c1f23d187c8e97/tools/pyinstaller_hooks/hook-eth_hash.py#L1'>tools/pyinstaller_hooks/hook-eth_hash.py:1:1:</a> N999 Invalid module name: 'hook-eth_hash'
+ <a href='https://github.com/rotki/rotki/blob/ac3dd44bcedbd13b385e2c3473c1f23d187c8e97/tools/pyinstaller_hooks/hook-eth_keyfile.py#L1'>tools/pyinstaller_hooks/hook-eth_keyfile.py:1:1:</a> N999 Invalid module name: 'hook-eth_keyfile'
+ <a href='https://github.com/rotki/rotki/blob/ac3dd44bcedbd13b385e2c3473c1f23d187c8e97/tools/pyinstaller_hooks/hook-eth_utils.py#L1'>tools/pyinstaller_hooks/hook-eth_utils.py:1:1:</a> N999 Invalid module name: 'hook-eth_utils'
+ <a href='https://github.com/rotki/rotki/blob/ac3dd44bcedbd13b385e2c3473c1f23d187c8e97/tools/pyinstaller_hooks/hook-multiformats_config.py#L1'>tools/pyinstaller_hooks/hook-multiformats_config.py:1:1:</a> N999 Invalid module name: 'hook-multiformats_config'
+ <a href='https://github.com/rotki/rotki/blob/ac3dd44bcedbd13b385e2c3473c1f23d187c8e97/tools/pyinstaller_hooks/hook-pysqlcipher3.py#L1'>tools/pyinstaller_hooks/hook-pysqlcipher3.py:1:1:</a> N999 Invalid module name: 'hook-pysqlcipher3'
+ <a href='https://github.com/rotki/rotki/blob/ac3dd44bcedbd13b385e2c3473c1f23d187c8e97/tools/pyinstaller_hooks/hook-scalecodec.py#L1'>tools/pyinstaller_hooks/hook-scalecodec.py:1:1:</a> N999 Invalid module name: 'hook-scalecodec'
+ <a href='https://github.com/rotki/rotki/blob/ac3dd44bcedbd13b385e2c3473c1f23d187c8e97/tools/pyinstaller_hooks/hook-web3.py#L1'>tools/pyinstaller_hooks/hook-web3.py:1:1:</a> N999 Invalid module name: 'hook-web3'
... 1 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+7 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/indico/indico/blob/5167d8c065e04ca468764babeffd9f5287a3af60/bin/maintenance/build-assets.py#L1'>bin/maintenance/build-assets.py:1:1:</a> N999 Invalid module name: 'build-assets'
+ <a href='https://github.com/indico/indico/blob/5167d8c065e04ca468764babeffd9f5287a3af60/bin/maintenance/build-wheel.py#L1'>bin/maintenance/build-wheel.py:1:1:</a> N999 Invalid module name: 'build-wheel'
+ <a href='https://github.com/indico/indico/blob/5167d8c065e04ca468764babeffd9f5287a3af60/bin/maintenance/compile-python-deps.py#L1'>bin/maintenance/compile-python-deps.py:1:1:</a> N999 Invalid module name: 'compile-python-deps'
+ <a href='https://github.com/indico/indico/blob/5167d8c065e04ca468764babeffd9f5287a3af60/bin/maintenance/dump-template.py#L1'>bin/maintenance/dump-template.py:1:1:</a> N999 Invalid module name: 'dump-template'
+ <a href='https://github.com/indico/indico/blob/5167d8c065e04ca468764babeffd9f5287a3af60/bin/maintenance/make-changelog.py#L1'>bin/maintenance/make-changelog.py:1:1:</a> N999 Invalid module name: 'make-changelog'
+ <a href='https://github.com/indico/indico/blob/5167d8c065e04ca468764babeffd9f5287a3af60/bin/maintenance/make-release.py#L1'>bin/maintenance/make-release.py:1:1:</a> N999 Invalid module name: 'make-release'
+ <a href='https://github.com/indico/indico/blob/5167d8c065e04ca468764babeffd9f5287a3af60/bin/utils/apiProxy.py#L1'>bin/utils/apiProxy.py:1:1:</a> N999 Invalid module name: 'apiProxy'
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| N999 | 60 | 60 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+60 -0 violations, +0 -0 fixes in 7 projects; 47 projects unchanged)

<details><summary><a href="https://github.com/Snowflake-Labs/snowcli">Snowflake-Labs/snowcli</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/Snowflake-Labs/snowcli/blob/740cd4559f0fecc012fea8bae5fd613d931c5871/snyk/dependency-sync.py#L1'>snyk/dependency-sync.py:1:1:</a> N999 Invalid module name: 'dependency-sync'
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/fad5e5e9d494f0cf81f35993f5fd0f6f047a3b75/scripts/tools/generate-integrations-json.py#L1'>scripts/tools/generate-integrations-json.py:1:1:</a> N999 Invalid module name: 'generate-integrations-json'
+ <a href='https://github.com/apache/airflow/blob/fad5e5e9d494f0cf81f35993f5fd0f6f047a3b75/scripts/tools/list-integrations.py#L1'>scripts/tools/list-integrations.py:1:1:</a> N999 Invalid module name: 'list-integrations'
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+36 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/advanced/extensions/font-awesome/font-awesome.py#L1'>examples/advanced/extensions/font-awesome/font-awesome.py:1:1:</a> N999 Invalid module name: 'font-awesome'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/advanced/integration/d3-voronoi.py#L1'>examples/advanced/integration/d3-voronoi.py:1:1:</a> N999 Invalid module name: 'd3-voronoi'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/AnnularWedge.py#L1'>examples/reference/models/AnnularWedge.py:1:1:</a> N999 Invalid module name: 'AnnularWedge'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/Annulus.py#L1'>examples/reference/models/Annulus.py:1:1:</a> N999 Invalid module name: 'Annulus'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/Arc.py#L1'>examples/reference/models/Arc.py:1:1:</a> N999 Invalid module name: 'Arc'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/Bezier.py#L1'>examples/reference/models/Bezier.py:1:1:</a> N999 Invalid module name: 'Bezier'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/Block.py#L1'>examples/reference/models/Block.py:1:1:</a> N999 Invalid module name: 'Block'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/Circle.py#L1'>examples/reference/models/Circle.py:1:1:</a> N999 Invalid module name: 'Circle'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/Ellipse.py#L1'>examples/reference/models/Ellipse.py:1:1:</a> N999 Invalid module name: 'Ellipse'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/HArea.py#L1'>examples/reference/models/HArea.py:1:1:</a> N999 Invalid module name: 'HArea'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/HAreaStep.py#L1'>examples/reference/models/HAreaStep.py:1:1:</a> N999 Invalid module name: 'HAreaStep'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/HBar.py#L1'>examples/reference/models/HBar.py:1:1:</a> N999 Invalid module name: 'HBar'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/HSpan.py#L1'>examples/reference/models/HSpan.py:1:1:</a> N999 Invalid module name: 'HSpan'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/HStrip.py#L1'>examples/reference/models/HStrip.py:1:1:</a> N999 Invalid module name: 'HStrip'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/HexTile.py#L1'>examples/reference/models/HexTile.py:1:1:</a> N999 Invalid module name: 'HexTile'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/ImageURL.py#L1'>examples/reference/models/ImageURL.py:1:1:</a> N999 Invalid module name: 'ImageURL'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/Line.py#L1'>examples/reference/models/Line.py:1:1:</a> N999 Invalid module name: 'Line'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/MathText.py#L1'>examples/reference/models/MathText.py:1:1:</a> N999 Invalid module name: 'MathText'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/MultiLine.py#L1'>examples/reference/models/MultiLine.py:1:1:</a> N999 Invalid module name: 'MultiLine'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/MultiPolygons.py#L1'>examples/reference/models/MultiPolygons.py:1:1:</a> N999 Invalid module name: 'MultiPolygons'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/Patch.py#L1'>examples/reference/models/Patch.py:1:1:</a> N999 Invalid module name: 'Patch'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/Patches.py#L1'>examples/reference/models/Patches.py:1:1:</a> N999 Invalid module name: 'Patches'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/Quad.py#L1'>examples/reference/models/Quad.py:1:1:</a> N999 Invalid module name: 'Quad'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/Quadratic.py#L1'>examples/reference/models/Quadratic.py:1:1:</a> N999 Invalid module name: 'Quadratic'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/Ray.py#L1'>examples/reference/models/Ray.py:1:1:</a> N999 Invalid module name: 'Ray'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/Rect.py#L1'>examples/reference/models/Rect.py:1:1:</a> N999 Invalid module name: 'Rect'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/Scatter.py#L1'>examples/reference/models/Scatter.py:1:1:</a> N999 Invalid module name: 'Scatter'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/Segment.py#L1'>examples/reference/models/Segment.py:1:1:</a> N999 Invalid module name: 'Segment'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/Step.py#L1'>examples/reference/models/Step.py:1:1:</a> N999 Invalid module name: 'Step'
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/Text.py#L1'>examples/reference/models/Text.py:1:1:</a> N999 Invalid module name: 'Text'
... 6 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/251a5b112ebf9adfc22b7680c0f96c6801264cde/latch_cli/services/init/example_snakemake/scripts/plot-quals.py#L1'>latch_cli/services/init/example_snakemake/scripts/plot-quals.py:1:1:</a> N999 Invalid module name: 'plot-quals'
</pre>

</p>
</details>
<details><summary><a href="https://github.com/lnbits/lnbits">lnbits/lnbits</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/lnbits/lnbits/blob/dbb689c5c56017aa2f777327101477864e90c3d5/tools/i18n-ai-tool.py#L1'>tools/i18n-ai-tool.py:1:1:</a> N999 Invalid module name: 'i18n-ai-tool'
+ <a href='https://github.com/lnbits/lnbits/blob/dbb689c5c56017aa2f777327101477864e90c3d5/tools/i18n-check.py#L1'>tools/i18n-check.py:1:1:</a> N999 Invalid module name: 'i18n-check'
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+11 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/ac3dd44bcedbd13b385e2c3473c1f23d187c8e97/tools/pyinstaller_hooks/hook-coincurve.py#L1'>tools/pyinstaller_hooks/hook-coincurve.py:1:1:</a> N999 Invalid module name: 'hook-coincurve'
+ <a href='https://github.com/rotki/rotki/blob/ac3dd44bcedbd13b385e2c3473c1f23d187c8e97/tools/pyinstaller_hooks/hook-content_hash.py#L1'>tools/pyinstaller_hooks/hook-content_hash.py:1:1:</a> N999 Invalid module name: 'hook-content_hash'
+ <a href='https://github.com/rotki/rotki/blob/ac3dd44bcedbd13b385e2c3473c1f23d187c8e97/tools/pyinstaller_hooks/hook-eth_abi.py#L1'>tools/pyinstaller_hooks/hook-eth_abi.py:1:1:</a> N999 Invalid module name: 'hook-eth_abi'
+ <a href='https://github.com/rotki/rotki/blob/ac3dd44bcedbd13b385e2c3473c1f23d187c8e97/tools/pyinstaller_hooks/hook-eth_hash.py#L1'>tools/pyinstaller_hooks/hook-eth_hash.py:1:1:</a> N999 Invalid module name: 'hook-eth_hash'
+ <a href='https://github.com/rotki/rotki/blob/ac3dd44bcedbd13b385e2c3473c1f23d187c8e97/tools/pyinstaller_hooks/hook-eth_keyfile.py#L1'>tools/pyinstaller_hooks/hook-eth_keyfile.py:1:1:</a> N999 Invalid module name: 'hook-eth_keyfile'
+ <a href='https://github.com/rotki/rotki/blob/ac3dd44bcedbd13b385e2c3473c1f23d187c8e97/tools/pyinstaller_hooks/hook-eth_utils.py#L1'>tools/pyinstaller_hooks/hook-eth_utils.py:1:1:</a> N999 Invalid module name: 'hook-eth_utils'
+ <a href='https://github.com/rotki/rotki/blob/ac3dd44bcedbd13b385e2c3473c1f23d187c8e97/tools/pyinstaller_hooks/hook-multiformats_config.py#L1'>tools/pyinstaller_hooks/hook-multiformats_config.py:1:1:</a> N999 Invalid module name: 'hook-multiformats_config'
+ <a href='https://github.com/rotki/rotki/blob/ac3dd44bcedbd13b385e2c3473c1f23d187c8e97/tools/pyinstaller_hooks/hook-pysqlcipher3.py#L1'>tools/pyinstaller_hooks/hook-pysqlcipher3.py:1:1:</a> N999 Invalid module name: 'hook-pysqlcipher3'
+ <a href='https://github.com/rotki/rotki/blob/ac3dd44bcedbd13b385e2c3473c1f23d187c8e97/tools/pyinstaller_hooks/hook-scalecodec.py#L1'>tools/pyinstaller_hooks/hook-scalecodec.py:1:1:</a> N999 Invalid module name: 'hook-scalecodec'
+ <a href='https://github.com/rotki/rotki/blob/ac3dd44bcedbd13b385e2c3473c1f23d187c8e97/tools/pyinstaller_hooks/hook-web3.py#L1'>tools/pyinstaller_hooks/hook-web3.py:1:1:</a> N999 Invalid module name: 'hook-web3'
... 1 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+7 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/indico/indico/blob/5167d8c065e04ca468764babeffd9f5287a3af60/bin/maintenance/build-assets.py#L1'>bin/maintenance/build-assets.py:1:1:</a> N999 Invalid module name: 'build-assets'
+ <a href='https://github.com/indico/indico/blob/5167d8c065e04ca468764babeffd9f5287a3af60/bin/maintenance/build-wheel.py#L1'>bin/maintenance/build-wheel.py:1:1:</a> N999 Invalid module name: 'build-wheel'
+ <a href='https://github.com/indico/indico/blob/5167d8c065e04ca468764babeffd9f5287a3af60/bin/maintenance/compile-python-deps.py#L1'>bin/maintenance/compile-python-deps.py:1:1:</a> N999 Invalid module name: 'compile-python-deps'
+ <a href='https://github.com/indico/indico/blob/5167d8c065e04ca468764babeffd9f5287a3af60/bin/maintenance/dump-template.py#L1'>bin/maintenance/dump-template.py:1:1:</a> N999 Invalid module name: 'dump-template'
+ <a href='https://github.com/indico/indico/blob/5167d8c065e04ca468764babeffd9f5287a3af60/bin/maintenance/make-changelog.py#L1'>bin/maintenance/make-changelog.py:1:1:</a> N999 Invalid module name: 'make-changelog'
+ <a href='https://github.com/indico/indico/blob/5167d8c065e04ca468764babeffd9f5287a3af60/bin/maintenance/make-release.py#L1'>bin/maintenance/make-release.py:1:1:</a> N999 Invalid module name: 'make-release'
+ <a href='https://github.com/indico/indico/blob/5167d8c065e04ca468764babeffd9f5287a3af60/bin/utils/apiProxy.py#L1'>bin/utils/apiProxy.py:1:1:</a> N999 Invalid module name: 'apiProxy'
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| N999 | 60 | 60 | 0 | 0 | 0 |

</p>
</details>




---

_Label `rule` added by @MichaReiser on 2024-07-29 06:47_

---

_Comment by @MichaReiser on 2024-07-30 06:39_

This looks reasonable to me. I do think that we should gate the change behind preview mode. A minor release is required when

> The scope of a stable rule is significantly increased

Whether this is a *significant* increase is debatable. I'm leaning towards being safe. @AlexWaygood what do you think?

---

_@MichaReiser approved on 2024-07-30 06:40_

---

_Comment by @charliermarsh on 2024-07-30 12:15_

I'm torn on this, since it's not that uncommon to use invalid module names for standalone scripts. We even do it in uv.

---

_Comment by @AlexWaygood on 2024-07-30 12:23_

> I'm torn on this, since it's not that uncommon to use invalid module names for standalone scripts. We even do it in uv.

Yeah. I'm -1 on this being part of N999. We already have https://github.com/astral-sh/ruff/issues/12475 complaining (reasonably, in my view) that this rule is too broad. I don't think we should broaden it further.

Possibly this could be added as a separate rule. But it would be a very opinionated rule, in my opinion. As @charliermarsh says, it's pretty common for single-file scripts to have names with hyphens in them or whatever. And that's fine, if you never intend to import them from other modules.

---

_Comment by @MichaReiser on 2024-08-01 20:48_

From reading the above, it seems we have no support for this rule change. 

Thanks @AlexElvers for putting the time into this and I'm sorry this has not a more positive outcome.

---

_Closed by @MichaReiser on 2024-08-01 20:48_

---

_Comment by @AlexElvers on 2024-08-01 21:23_

Thanks for the debate! I'm now hoping for a rule split as suggested in #12475 too. :slightly_smiling_face: 

---
