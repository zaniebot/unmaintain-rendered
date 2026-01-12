```yaml
number: 14580
title: "[`ruff`] `l.extend([e])` (`RUF043`)"
type: pull_request
state: closed
author: InSyncWithFoo
labels:
  - rule
  - needs-decision
assignees: []
base: main
head: RUF043
created_at: 2024-11-25T06:31:51Z
updated_at: 2024-12-08T00:37:15Z
url: https://github.com/astral-sh/ruff/pull/14580
synced_at: 2026-01-12T15:55:48Z
```

# [`ruff`] `l.extend([e])` (`RUF043`)

---

_@InSyncWithFoo_

## Summary

Resolves #14518.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2024-11-25 06:42_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+23 -0 violations, +0 -0 fixes in 6 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/core/actions/test_forms.py#L364'>tests/core/actions/test_forms.py:364:5:</a> RUF043 [*] Literal iterable with single item in `list.extend()` call
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+16 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/b1a44b4e3db8a3d3c13dfc810ad02f5785f82eca/airflow/models/taskinstance.py#L2062'>airflow/models/taskinstance.py:2062:13:</a> RUF043 [*] Literal iterable with single item in `list.extend()` call
+ <a href='https://github.com/apache/airflow/blob/b1a44b4e3db8a3d3c13dfc810ad02f5785f82eca/airflow/models/taskinstance.py#L2064'>airflow/models/taskinstance.py:2064:13:</a> RUF043 [*] Literal iterable with single item in `list.extend()` call
+ <a href='https://github.com/apache/airflow/blob/b1a44b4e3db8a3d3c13dfc810ad02f5785f82eca/airflow/models/taskinstance.py#L2066'>airflow/models/taskinstance.py:2066:13:</a> RUF043 [*] Literal iterable with single item in `list.extend()` call
+ <a href='https://github.com/apache/airflow/blob/b1a44b4e3db8a3d3c13dfc810ad02f5785f82eca/airflow/models/taskinstance.py#L2072'>airflow/models/taskinstance.py:2072:13:</a> RUF043 [*] Literal iterable with single item in `list.extend()` call
+ <a href='https://github.com/apache/airflow/blob/b1a44b4e3db8a3d3c13dfc810ad02f5785f82eca/airflow/models/taskinstance.py#L2074'>airflow/models/taskinstance.py:2074:13:</a> RUF043 [*] Literal iterable with single item in `list.extend()` call
+ <a href='https://github.com/apache/airflow/blob/b1a44b4e3db8a3d3c13dfc810ad02f5785f82eca/airflow/models/taskinstance.py#L2078'>airflow/models/taskinstance.py:2078:13:</a> RUF043 [*] Literal iterable with single item in `list.extend()` call
+ <a href='https://github.com/apache/airflow/blob/b1a44b4e3db8a3d3c13dfc810ad02f5785f82eca/dev/breeze/src/airflow_breeze/utils/docker_command_utils.py#L346'>dev/breeze/src/airflow_breeze/utils/docker_command_utils.py:346:5:</a> RUF043 [*] Literal iterable with single item in `list.extend()` call
+ <a href='https://github.com/apache/airflow/blob/b1a44b4e3db8a3d3c13dfc810ad02f5785f82eca/dev/breeze/src/airflow_breeze/utils/docker_command_utils.py#L351'>dev/breeze/src/airflow_breeze/utils/docker_command_utils.py:351:5:</a> RUF043 [*] Literal iterable with single item in `list.extend()` call
+ <a href='https://github.com/apache/airflow/blob/b1a44b4e3db8a3d3c13dfc810ad02f5785f82eca/dev/breeze/src/airflow_breeze/utils/docker_command_utils.py#L358'>dev/breeze/src/airflow_breeze/utils/docker_command_utils.py:358:5:</a> RUF043 [*] Literal iterable with single item in `list.extend()` call
+ <a href='https://github.com/apache/airflow/blob/b1a44b4e3db8a3d3c13dfc810ad02f5785f82eca/dev/breeze/src/airflow_breeze/utils/docker_command_utils.py#L415'>dev/breeze/src/airflow_breeze/utils/docker_command_utils.py:415:5:</a> RUF043 [*] Literal iterable with single item in `list.extend()` call
+ <a href='https://github.com/apache/airflow/blob/b1a44b4e3db8a3d3c13dfc810ad02f5785f82eca/dev/breeze/src/airflow_breeze/utils/docker_command_utils.py#L418'>dev/breeze/src/airflow_breeze/utils/docker_command_utils.py:418:5:</a> RUF043 [*] Literal iterable with single item in `list.extend()` call
+ <a href='https://github.com/apache/airflow/blob/b1a44b4e3db8a3d3c13dfc810ad02f5785f82eca/dev/breeze/src/airflow_breeze/utils/docker_command_utils.py#L542'>dev/breeze/src/airflow_breeze/utils/docker_command_utils.py:542:9:</a> RUF043 [*] Literal iterable with single item in `list.extend()` call
+ <a href='https://github.com/apache/airflow/blob/b1a44b4e3db8a3d3c13dfc810ad02f5785f82eca/docs/conf.py#L210'>docs/conf.py:210:5:</a> RUF043 [*] Literal iterable with single item in `list.extend()` call
+ <a href='https://github.com/apache/airflow/blob/b1a44b4e3db8a3d3c13dfc810ad02f5785f82eca/providers/src/airflow/providers/google/cloud/hooks/cloud_sql.py#L697'>providers/src/airflow/providers/google/cloud/hooks/cloud_sql.py:697:9:</a> RUF043 [*] Literal iterable with single item in `list.extend()` call
+ <a href='https://github.com/apache/airflow/blob/b1a44b4e3db8a3d3c13dfc810ad02f5785f82eca/providers/tests/microsoft/psrp/operators/test_psrp.py#L112'>providers/tests/microsoft/psrp/operators/test_psrp.py:112:13:</a> RUF043 [*] Literal iterable with single item in `list.extend()` call
+ <a href='https://github.com/apache/airflow/blob/b1a44b4e3db8a3d3c13dfc810ad02f5785f82eca/scripts/ci/pre_commit/mypy_folder.py#L68'>scripts/ci/pre_commit/mypy_folder.py:68:5:</a> RUF043 [*] Literal iterable with single item in `list.extend()` call
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/util/structure.py#L242'>src/bokeh/models/util/structure.py:242:13:</a> RUF043 [*] Literal iterable with single item in `list.extend()` call
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/util/structure.py#L243'>src/bokeh/models/util/structure.py:243:13:</a> RUF043 [*] Literal iterable with single item in `list.extend()` call
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/aa0dd442345234b1559d61ca264c15c020adaf67/zerver/lib/tex.py#L78'>zerver/lib/tex.py:78:9:</a> RUF043 [*] Literal iterable with single item in `list.extend()` call
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pytest-dev/pytest/blob/acf130311806cb1a627ea0e21369c7d6f586663a/testing/logging/test_reporting.py#L1020'>testing/logging/test_reporting.py:1020:9:</a> RUF043 [*] Literal iterable with single item in `list.extend()` call
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/4e3ad4b94f5183a8c78cc0c6ee636c6a224dc14b/astropy/modeling/fitting.py#L2011'>astropy/modeling/fitting.py:2011:21:</a> RUF043 [*] Literal iterable with single item in `list.extend()` call
+ <a href='https://github.com/astropy/astropy/blob/4e3ad4b94f5183a8c78cc0c6ee636c6a224dc14b/astropy/modeling/fitting.py#L2074'>astropy/modeling/fitting.py:2074:21:</a> RUF043 [*] Literal iterable with single item in `list.extend()` call
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF043 | 23 | 23 | 0 | 0 | 0 |

</p>
</details>




---

_Label `rule` added by @MichaReiser on 2024-11-25 07:26_

---

_Label `needs-decision` added by @MichaReiser on 2024-11-25 07:26_

---

_@MichaReiser reviewed on 2024-11-27 10:10_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/mod.rs`:409 on 2024-11-27 10:10_

Nit: I would prefer a slightly longer name, maybe `list-extend-single-item-iterable`

---

_@MichaReiser reviewed on 2024-11-27 10:10_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/list_extend_single.rs`:74 on 2024-11-27 10:10_

```suggestion
fn list_extend_call_with_one_argument<'a>(
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/list_extend_single.rs`:63 on 2024-11-27 10:10_

```suggestion
    let list_ref_expr = locator.slice(list_ref);
    let item_expr = locator.slice(item);
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/list_extend_single.rs`:67 on 2024-11-27 10:11_

Nit: We should mark this fix as unsafe if the edit range contains comments. 

---

_@MichaReiser reviewed on 2024-11-27 10:13_

This overall looks good to me. I also tested if there would be many more violations when removing the type-checking guard, which isn't the case.


The original issue also mentions that they would like to handle the following cases

```
retval.extend([x, y])
retval.append(v)
retval.extend([z, w])
```

Is it an intentional decision to exclude those from this rule and if so, what's your reasoning?

---

_@InSyncWithFoo reviewed on 2024-11-27 12:35_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/ruff/mod.rs`:409 on 2024-11-27 12:35_

Done. This rule's name is similar to [`RUF042`](https://github.com/astral-sh/ruff/pull/14564)'s. I'll rename that one too.

---

_Comment by @InSyncWithFoo on 2024-11-27 12:41_

@MichaReiser This rule is call-based, whereas that pattern requires a statements-based rule. There are also tricky cases like this one:

```python
# Should this first line be included in the diagnostic?
l.extend({a, b})  # Append only unique elements
l.append([c])
l.extend([d, e])

# What about this?
l.append([a])
l.extend({b, c})  # Append only unique elements
l.extend([d, e])
```

---

_Comment by @MichaReiser on 2024-11-27 13:07_

That makes sense. But I don't think the scope of a rule should mainly be defined by how it's implemented.

---

_Comment by @InSyncWithFoo on 2024-11-27 13:18_

Also consider this case, where there are no `.extend([single])` at all:

```python
l.extend([0, 1])
l.extend([2, 3])
l.extend([4, 5])
```

I would prefer to keep this rule simple and defer the consecutive-calls pattern to another rule.

---

_Closed by @InSyncWithFoo on 2024-12-08 00:37_

---

_Branch deleted on 2024-12-08 00:37_

---
