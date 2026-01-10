```yaml
number: 15247
title: "[`ruff`] Recognize more expressions (`RUF027`)"
type: pull_request
state: closed
author: InSyncWithFoo
labels:
  - rule
  - preview
assignees: []
base: main
head: RUF027
created_at: 2025-01-04T04:05:59Z
updated_at: 2025-04-28T07:08:21Z
url: https://github.com/astral-sh/ruff/pull/15247
synced_at: 2026-01-10T19:03:00Z
```

# [`ruff`] Recognize more expressions (`RUF027`)

---

_Pull request opened by @InSyncWithFoo on 2025-01-04 04:05_

## Summary

Resolves #15227.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2025-01-04 04:11_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+29 -0 violations, +0 -0 fixes in 9 projects; 46 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/013401b4a7c1c9fe7b3dd669d40f546ffd895314/dev/breeze/src/airflow_breeze/utils/kubernetes_utils.py#L327'>dev/breeze/src/airflow_breeze/utils/kubernetes_utils.py:327:13:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/apache/airflow/blob/013401b4a7c1c9fe7b3dd669d40f546ffd895314/providers/src/airflow/providers/apache/spark/hooks/spark_connect.py#L76'>providers/src/airflow/providers/apache/spark/hooks/spark_connect.py:76:30:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/apache/airflow/blob/013401b4a7c1c9fe7b3dd669d40f546ffd895314/scripts/ci/pre_commit/check_min_python_version.py#L38'>scripts/ci/pre_commit/check_min_python_version.py:38:17:</a> RUF027 Possible f-string without an `f` prefix
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/b6e9ef11ea61dd2f8b1002654f495d9f1caa3342/ibis/backends/__init__.py#L919'>ibis/backends/__init__.py:919:33:</a> RUF027 Possible f-string without an `f` prefix
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch_cli/nextflow/config.py#L148'>src/latch_cli/nextflow/config.py:148:35:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch_cli/snakemake/config/parser.py#L116'>src/latch_cli/snakemake/config/parser.py:116:20:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch_cli/snakemake/config/parser.py#L248'>src/latch_cli/snakemake/config/parser.py:248:13:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch_cli/snakemake/serialize_utils.py#L179'>src/latch_cli/snakemake/serialize_utils.py:179:13:</a> RUF027 Possible f-string without an `f` prefix
</pre>

</p>
</details>
<details><summary><a href="https://github.com/lnbits/lnbits">lnbits/lnbits</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/helpers.py#L85'>lnbits/core/helpers.py:85:30:</a> RUF027 Possible f-string without an `f` prefix
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/1be26374dd7ef43bc709c4bc6db2daf7bfd606c8/pandas/core/generic.py#L12613'>pandas/core/generic.py:12613:22:</a> RUF027 Possible f-string without an `f` prefix
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/e11c61e13d09c99ac13b1e69677cf83e6eecb198/rotkehlchen/tests/api/blockchain/test_base.py#L949'>rotkehlchen/tests/api/blockchain/test_base.py:949:33:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/rotki/rotki/blob/e11c61e13d09c99ac13b1e69677cf83e6eecb198/rotkehlchen/tests/exchanges/test_bitfinex.py#L70'>rotkehlchen/tests/exchanges/test_bitfinex.py:70:22:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/rotki/rotki/blob/e11c61e13d09c99ac13b1e69677cf83e6eecb198/rotkehlchen/tests/exchanges/test_bitfinex.py#L80'>rotkehlchen/tests/exchanges/test_bitfinex.py:80:22:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/rotki/rotki/blob/e11c61e13d09c99ac13b1e69677cf83e6eecb198/rotkehlchen/tests/exchanges/test_bitfinex.py#L89'>rotkehlchen/tests/exchanges/test_bitfinex.py:89:22:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/rotki/rotki/blob/e11c61e13d09c99ac13b1e69677cf83e6eecb198/rotkehlchen/tests/utils/checks.py#L15'>rotkehlchen/tests/utils/checks.py:15:33:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/rotki/rotki/blob/e11c61e13d09c99ac13b1e69677cf83e6eecb198/rotkehlchen/tests/utils/checks.py#L16'>rotkehlchen/tests/utils/checks.py:16:33:</a> RUF027 Possible f-string without an `f` prefix
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/01cb7867156ecbd6f0b4efc4b7440ea2c2ca80a0/corporate/tests/test_stripe.py#L1650'>corporate/tests/test_stripe.py:1650:46:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/zulip/zulip/blob/01cb7867156ecbd6f0b4efc4b7440ea2c2ca80a0/zerver/lib/event_schema.py#L457'>zerver/lib/event_schema.py:457:30:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/zulip/zulip/blob/01cb7867156ecbd6f0b4efc4b7440ea2c2ca80a0/zerver/management/commands/export_search.py#L129'>zerver/management/commands/export_search.py:129:17:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/zulip/zulip/blob/01cb7867156ecbd6f0b4efc4b7440ea2c2ca80a0/zerver/tests/test_signup.py#L3140'>zerver/tests/test_signup.py:3140:13:</a> RUF027 Possible f-string without an `f` prefix
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pytest-dev/pytest/blob/e8c2082d751999fa0ebf1ba335cfec3b022f0a88/testing/test_pathlib.py#L1365'>testing/test_pathlib.py:1365:13:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/pytest-dev/pytest/blob/e8c2082d751999fa0ebf1ba335cfec3b022f0a88/testing/test_terminal.py#L1103'>testing/test_terminal.py:1103:13:</a> RUF027 Possible f-string without an `f` prefix
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+7 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/80aa67d2cc7ddb41a63d00d231b2792aafca77fa/astropy/coordinates/representation/geodetic.py#L18'>astropy/coordinates/representation/geodetic.py:18:21:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/astropy/astropy/blob/80aa67d2cc7ddb41a63d00d231b2792aafca77fa/astropy/coordinates/transformations/affine.py#L68'>astropy/coordinates/transformations/affine.py:68:17:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/astropy/astropy/blob/80aa67d2cc7ddb41a63d00d231b2792aafca77fa/astropy/table/table.py#L98'>astropy/table/table.py:98:16:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/astropy/astropy/blob/80aa67d2cc7ddb41a63d00d231b2792aafca77fa/astropy/uncertainty/core.py#L579'>astropy/uncertainty/core.py:579:17:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/astropy/astropy/blob/80aa67d2cc7ddb41a63d00d231b2792aafca77fa/astropy/utils/tests/test_decorators.py#L725'>astropy/utils/tests/test_decorators.py:725:18:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/astropy/astropy/blob/80aa67d2cc7ddb41a63d00d231b2792aafca77fa/astropy/utils/tests/test_decorators.py#L821'>astropy/utils/tests/test_decorators.py:821:17:</a> RUF027 Possible f-string without an `f` prefix
+ <a href='https://github.com/astropy/astropy/blob/80aa67d2cc7ddb41a63d00d231b2792aafca77fa/astropy/wcs/utils.py#L253'>astropy/wcs/utils.py:253:17:</a> RUF027 Possible f-string without an `f` prefix
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF027 | 29 | 29 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @InSyncWithFoo on 2025-01-04 10:21_

The true-positive-turned-false-negative:

```python
raise ValueError(
    "Scale {scale!r} is not in the allowed scales {sorted(self.SCALES)}"
)
```

#9849 has a bit more context on why strings whose placeholders' names are the same as that of built-ins are ignored. This interacted poorly with the new logic; in addition, it fails to recognize dunder names:

```python
print('{__path__}')  # Currently a false negative
```

---

_Comment by @InSyncWithFoo on 2025-01-04 10:37_

The new changes introduced a few true positives, most notably:

[@latchbio/latch](https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch_cli/nextflow/config.py#L148):

```py
params_path.write_text(dedent(r"""
        ...

        generated_parameters = {
        __params__
        }

        """).replace("__params__", reindent("".join(params), 1)))
```

[@astropy/astropy](https://github.com/astropy/astropy/blob/80aa67d2cc7ddb41a63d00d231b2792aafca77fa/astropy/coordinates/representation/geodetic.py#L18):

```py
geodetic_base_doc = """{__doc__}
    ...
"""


@format_doc(geodetic_base_doc)
class BaseGeodeticRepresentation(BaseRepresentation): ...
```

The former is embedded code, while the latter is dynamically interpolated. These cannot be detected reliably in the first place, so I think the false positives are acceptable.

---

_Comment by @AlexWaygood on 2025-01-04 10:44_

I haven't looked at this PR yet, but note that RUF027 already has too many false positives for us to consider stabilising it. I definitely don't think we should be making changes that introduce new false positives here, honestly

---

_Comment by @InSyncWithFoo on 2025-01-04 10:52_

If so, this PR might as well be closed, as this is now considered an error:

[@apache/airflow](https://github.com/apache/airflow/blob/013401b4a7c1c9fe7b3dd669d40f546ffd895314/dev/breeze/src/airflow_breeze/utils/kubernetes_utils.py#L327):

```python
python_major_minor_version = run_command(
    [
        str(PYTHON_BIN_PATH),
        "-c",
        "import sys; print(f'{sys.version_info.major}.{sys.version_info.minor}')",
    ],
).stdout.strip()
```

---

_Comment by @MichaReiser on 2025-01-04 12:09_

It does find some new actual violations but we should look into if we can find a way to reduce the false positives. 

---

_Label `rule` added by @dhruvmanila on 2025-01-20 15:00_

---

_Label `preview` added by @dhruvmanila on 2025-01-20 15:00_

---

_Comment by @MichaReiser on 2025-04-28 07:08_

> It does find some new actual violations but we should look into if we can find a way to reduce the false positives.

Closing as this PR hasn't seen any activity in a while. Feel free to open a new PR that takes this feedback into account. Thanks @InSyncWithFoo for your work on this.

---

_Closed by @MichaReiser on 2025-04-28 07:08_

---
