```yaml
number: 16006
title: "[`flake8-builtins`] Match upstream module name comparison (`A005`)"
type: pull_request
state: merged
author: ntBre
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: brent/a005-bugfix
created_at: 2025-02-06T22:50:13Z
updated_at: 2025-02-07T18:55:59Z
url: https://github.com/astral-sh/ruff/pull/16006
synced_at: 2026-01-10T19:57:22Z
```

# [`flake8-builtins`] Match upstream module name comparison (`A005`)

---

_Pull request opened by @ntBre on 2025-02-06 22:50_

See #15951 for the original discussion and reviews. This is just the first half of that PR (reaching parity with `flake8-builtins` without adding any new configuration options) split out for nicer changelog entries.

For posterity, here's a script for generating the module structure that was useful for interactive testing and creating the table  [here](https://github.com/astral-sh/ruff/pull/15951#issuecomment-2640662041). The results for this branch are the same as the `Strict` column there, as expected.

```shell
mkdir abc collections foobar urlparse

for i in */
do
	touch $i/__init__.py
done	

cp -r abc foobar collections/.
cp -r abc collections foobar/.

touch ruff.toml

touch foobar/logging.py
```


---

_Label `bug` added by @ntBre on 2025-02-06 22:50_

---

_Label `rule` added by @ntBre on 2025-02-06 22:50_

---

_@ntBre reviewed on 2025-02-06 22:52_

---

_Review comment by @ntBre on `crates/ruff/tests/lint.rs`:2307 on 2025-02-06 22:52_

I think I want to split these into separate tests. `insta` was having some trouble updating the snapshots in-place like this, and then I can also give them more explanatory names like `a005_module_shadowing_strict`, `a005_module_shadowing_non_strict` and `a005_module_shadowing_default`, but I was planning to handle that in the second PR since the snaps will change again anyway.

---

_@ntBre reviewed on 2025-02-06 22:53_

---

_Review comment by @ntBre on `crates/ruff/tests/lint.rs`:2327 on 2025-02-06 22:53_

And this is for a third PR to change the default behavior to non-strict in a future release, if we still want to do that.

---

_Comment by @github-actions[bot] on 2025-02-06 22:57_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+26 -8 violations, +0 -0 fixes in 4 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+20 -8 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/airflow/api_fastapi/logging/__init__.py#L1'>airflow/api_fastapi/logging/__init__.py:1:1:</a> A005 Module `logging` shadows a Python standard-library module
+ <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/airflow/io/__init__.py#L1'>airflow/io/__init__.py:1:1:</a> A005 Module `io` shadows a Python standard-library module
+ <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/airflow/secrets/__init__.py#L1'>airflow/secrets/__init__.py:1:1:</a> A005 Module `secrets` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/providers/common/io/src/airflow/providers/common/io/assets/__init__.py#L1'>providers/common/io/src/airflow/providers/common/io/assets/__init__.py:1:1:</a> A005 Module `io` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/providers/common/io/src/airflow/providers/common/io/assets/assets/__init__.py#L1'>providers/common/io/src/airflow/providers/common/io/assets/assets/__init__.py:1:1:</a> A005 Module `io` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/providers/common/io/src/airflow/providers/common/io/operators/__init__.py#L1'>providers/common/io/src/airflow/providers/common/io/operators/__init__.py:1:1:</a> A005 Module `io` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/providers/common/io/src/airflow/providers/common/io/xcom/__init__.py#L1'>providers/common/io/src/airflow/providers/common/io/xcom/__init__.py:1:1:</a> A005 Module `io` shadows a Python standard-library module
+ <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/providers/common/io/tests/provider_tests/common/io/__init__.py#L1'>providers/common/io/tests/provider_tests/common/io/__init__.py:1:1:</a> A005 Module `io` shadows a Python standard-library module
+ <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/providers/google/src/airflow/providers/google/cloud/secrets/__init__.py#L1'>providers/google/src/airflow/providers/google/cloud/secrets/__init__.py:1:1:</a> A005 Module `secrets` shadows a Python standard-library module
+ <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/providers/google/tests/provider_tests/google/cloud/secrets/__init__.py#L1'>providers/google/tests/provider_tests/google/cloud/secrets/__init__.py:1:1:</a> A005 Module `secrets` shadows a Python standard-library module
+ <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/providers/hashicorp/src/airflow/providers/hashicorp/secrets/__init__.py#L1'>providers/hashicorp/src/airflow/providers/hashicorp/secrets/__init__.py:1:1:</a> A005 Module `secrets` shadows a Python standard-library module
+ <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/providers/hashicorp/tests/provider_tests/hashicorp/secrets/__init__.py#L1'>providers/hashicorp/tests/provider_tests/hashicorp/secrets/__init__.py:1:1:</a> A005 Module `secrets` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/providers/http/src/airflow/providers/http/hooks/__init__.py#L1'>providers/http/src/airflow/providers/http/hooks/__init__.py:1:1:</a> A005 Module `http` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/providers/http/src/airflow/providers/http/operators/__init__.py#L1'>providers/http/src/airflow/providers/http/operators/__init__.py:1:1:</a> A005 Module `http` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/providers/http/src/airflow/providers/http/sensors/__init__.py#L1'>providers/http/src/airflow/providers/http/sensors/__init__.py:1:1:</a> A005 Module `http` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/providers/http/src/airflow/providers/http/triggers/__init__.py#L1'>providers/http/src/airflow/providers/http/triggers/__init__.py:1:1:</a> A005 Module `http` shadows a Python standard-library module
+ <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/providers/http/tests/provider_tests/http/__init__.py#L1'>providers/http/tests/provider_tests/http/__init__.py:1:1:</a> A005 Module `http` shadows a Python standard-library module
+ <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/providers/opsgenie/src/airflow/providers/opsgenie/typing/__init__.py#L1'>providers/opsgenie/src/airflow/providers/opsgenie/typing/__init__.py:1:1:</a> A005 Module `typing` shadows a Python standard-library module
+ <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/providers/opsgenie/tests/provider_tests/opsgenie/typing/__init__.py#L1'>providers/opsgenie/tests/provider_tests/opsgenie/typing/__init__.py:1:1:</a> A005 Module `typing` shadows a Python standard-library module
+ <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/providers/src/airflow/providers/amazon/aws/secrets/__init__.py#L1'>providers/src/airflow/providers/amazon/aws/secrets/__init__.py:1:1:</a> A005 Module `secrets` shadows a Python standard-library module
+ <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/providers/src/airflow/providers/microsoft/azure/secrets/__init__.py#L1'>providers/src/airflow/providers/microsoft/azure/secrets/__init__.py:1:1:</a> A005 Module `secrets` shadows a Python standard-library module
+ <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/providers/tests/amazon/aws/secrets/__init__.py#L1'>providers/tests/amazon/aws/secrets/__init__.py:1:1:</a> A005 Module `secrets` shadows a Python standard-library module
+ <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/providers/tests/email/__init__.py#L1'>providers/tests/email/__init__.py:1:1:</a> A005 Module `email` shadows a Python standard-library module
+ <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/providers/tests/microsoft/azure/secrets/__init__.py#L1'>providers/tests/microsoft/azure/secrets/__init__.py:1:1:</a> A005 Module `secrets` shadows a Python standard-library module
+ <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/providers/yandex/src/airflow/providers/yandex/secrets/__init__.py#L1'>providers/yandex/src/airflow/providers/yandex/secrets/__init__.py:1:1:</a> A005 Module `secrets` shadows a Python standard-library module
+ <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/providers/yandex/tests/provider_tests/yandex/secrets/__init__.py#L1'>providers/yandex/tests/provider_tests/yandex/secrets/__init__.py:1:1:</a> A005 Module `secrets` shadows a Python standard-library module
+ <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/tests/io/__init__.py#L1'>tests/io/__init__.py:1:1:</a> A005 Module `io` shadows a Python standard-library module
+ <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/tests/secrets/__init__.py#L1'>tests/secrets/__init__.py:1:1:</a> A005 Module `secrets` shadows a Python standard-library module
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/widgets/fileinput.py#L1'>examples/interaction/widgets/fileinput.py:1:1:</a> A005 Module `fileinput` shadows a Python standard-library module
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/io/__init__.py#L1'>src/bokeh/io/__init__.py:1:1:</a> A005 Module `io` shadows a Python standard-library module
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/annotations/html/__init__.py#L1'>src/bokeh/models/annotations/html/__init__.py:1:1:</a> A005 Module `html` shadows a Python standard-library module
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/5493f7e481a656ea98619f98bcd2acd4139f8536/src/latch/idl/core/types.py#L1'>src/latch/idl/core/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
+ <a href='https://github.com/latchbio/latch/blob/5493f7e481a656ea98619f98bcd2acd4139f8536/src/latch/types/__init__.py#L1'>src/latch/types/__init__.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/4deb0a46a3a59a290733e1f980edf264a6142db3/zerver/webhooks/json/__init__.py#L1'>zerver/webhooks/json/__init__.py:1:1:</a> A005 Module `json` shadows a Python standard-library module
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| A005 | 34 | 26 | 8 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+26 -8 violations, +0 -0 fixes in 4 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+20 -8 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/airflow/api_fastapi/logging/__init__.py#L1'>airflow/api_fastapi/logging/__init__.py:1:1:</a> A005 Module `logging` shadows a Python standard-library module
+ <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/airflow/io/__init__.py#L1'>airflow/io/__init__.py:1:1:</a> A005 Module `io` shadows a Python standard-library module
+ <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/airflow/secrets/__init__.py#L1'>airflow/secrets/__init__.py:1:1:</a> A005 Module `secrets` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/providers/common/io/src/airflow/providers/common/io/assets/__init__.py#L1'>providers/common/io/src/airflow/providers/common/io/assets/__init__.py:1:1:</a> A005 Module `io` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/providers/common/io/src/airflow/providers/common/io/assets/assets/__init__.py#L1'>providers/common/io/src/airflow/providers/common/io/assets/assets/__init__.py:1:1:</a> A005 Module `io` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/providers/common/io/src/airflow/providers/common/io/operators/__init__.py#L1'>providers/common/io/src/airflow/providers/common/io/operators/__init__.py:1:1:</a> A005 Module `io` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/providers/common/io/src/airflow/providers/common/io/xcom/__init__.py#L1'>providers/common/io/src/airflow/providers/common/io/xcom/__init__.py:1:1:</a> A005 Module `io` shadows a Python standard-library module
+ <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/providers/common/io/tests/provider_tests/common/io/__init__.py#L1'>providers/common/io/tests/provider_tests/common/io/__init__.py:1:1:</a> A005 Module `io` shadows a Python standard-library module
+ <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/providers/google/src/airflow/providers/google/cloud/secrets/__init__.py#L1'>providers/google/src/airflow/providers/google/cloud/secrets/__init__.py:1:1:</a> A005 Module `secrets` shadows a Python standard-library module
+ <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/providers/google/tests/provider_tests/google/cloud/secrets/__init__.py#L1'>providers/google/tests/provider_tests/google/cloud/secrets/__init__.py:1:1:</a> A005 Module `secrets` shadows a Python standard-library module
+ <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/providers/hashicorp/src/airflow/providers/hashicorp/secrets/__init__.py#L1'>providers/hashicorp/src/airflow/providers/hashicorp/secrets/__init__.py:1:1:</a> A005 Module `secrets` shadows a Python standard-library module
+ <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/providers/hashicorp/tests/provider_tests/hashicorp/secrets/__init__.py#L1'>providers/hashicorp/tests/provider_tests/hashicorp/secrets/__init__.py:1:1:</a> A005 Module `secrets` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/providers/http/src/airflow/providers/http/hooks/__init__.py#L1'>providers/http/src/airflow/providers/http/hooks/__init__.py:1:1:</a> A005 Module `http` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/providers/http/src/airflow/providers/http/operators/__init__.py#L1'>providers/http/src/airflow/providers/http/operators/__init__.py:1:1:</a> A005 Module `http` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/providers/http/src/airflow/providers/http/sensors/__init__.py#L1'>providers/http/src/airflow/providers/http/sensors/__init__.py:1:1:</a> A005 Module `http` shadows a Python standard-library module
- <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/providers/http/src/airflow/providers/http/triggers/__init__.py#L1'>providers/http/src/airflow/providers/http/triggers/__init__.py:1:1:</a> A005 Module `http` shadows a Python standard-library module
+ <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/providers/http/tests/provider_tests/http/__init__.py#L1'>providers/http/tests/provider_tests/http/__init__.py:1:1:</a> A005 Module `http` shadows a Python standard-library module
+ <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/providers/opsgenie/src/airflow/providers/opsgenie/typing/__init__.py#L1'>providers/opsgenie/src/airflow/providers/opsgenie/typing/__init__.py:1:1:</a> A005 Module `typing` shadows a Python standard-library module
+ <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/providers/opsgenie/tests/provider_tests/opsgenie/typing/__init__.py#L1'>providers/opsgenie/tests/provider_tests/opsgenie/typing/__init__.py:1:1:</a> A005 Module `typing` shadows a Python standard-library module
+ <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/providers/src/airflow/providers/amazon/aws/secrets/__init__.py#L1'>providers/src/airflow/providers/amazon/aws/secrets/__init__.py:1:1:</a> A005 Module `secrets` shadows a Python standard-library module
+ <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/providers/src/airflow/providers/microsoft/azure/secrets/__init__.py#L1'>providers/src/airflow/providers/microsoft/azure/secrets/__init__.py:1:1:</a> A005 Module `secrets` shadows a Python standard-library module
+ <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/providers/tests/amazon/aws/secrets/__init__.py#L1'>providers/tests/amazon/aws/secrets/__init__.py:1:1:</a> A005 Module `secrets` shadows a Python standard-library module
+ <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/providers/tests/email/__init__.py#L1'>providers/tests/email/__init__.py:1:1:</a> A005 Module `email` shadows a Python standard-library module
+ <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/providers/tests/microsoft/azure/secrets/__init__.py#L1'>providers/tests/microsoft/azure/secrets/__init__.py:1:1:</a> A005 Module `secrets` shadows a Python standard-library module
+ <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/providers/yandex/src/airflow/providers/yandex/secrets/__init__.py#L1'>providers/yandex/src/airflow/providers/yandex/secrets/__init__.py:1:1:</a> A005 Module `secrets` shadows a Python standard-library module
+ <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/providers/yandex/tests/provider_tests/yandex/secrets/__init__.py#L1'>providers/yandex/tests/provider_tests/yandex/secrets/__init__.py:1:1:</a> A005 Module `secrets` shadows a Python standard-library module
+ <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/tests/io/__init__.py#L1'>tests/io/__init__.py:1:1:</a> A005 Module `io` shadows a Python standard-library module
+ <a href='https://github.com/apache/airflow/blob/764bf20cb624f16d15481804ad72d4c6ca1e3316/tests/secrets/__init__.py#L1'>tests/secrets/__init__.py:1:1:</a> A005 Module `secrets` shadows a Python standard-library module
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/widgets/fileinput.py#L1'>examples/interaction/widgets/fileinput.py:1:1:</a> A005 Module `fileinput` shadows a Python standard-library module
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/io/__init__.py#L1'>src/bokeh/io/__init__.py:1:1:</a> A005 Module `io` shadows a Python standard-library module
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/annotations/html/__init__.py#L1'>src/bokeh/models/annotations/html/__init__.py:1:1:</a> A005 Module `html` shadows a Python standard-library module
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/5493f7e481a656ea98619f98bcd2acd4139f8536/src/latch/idl/core/types.py#L1'>src/latch/idl/core/types.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
+ <a href='https://github.com/latchbio/latch/blob/5493f7e481a656ea98619f98bcd2acd4139f8536/src/latch/types/__init__.py#L1'>src/latch/types/__init__.py:1:1:</a> A005 Module `types` shadows a Python standard-library module
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/4deb0a46a3a59a290733e1f980edf264a6142db3/zerver/webhooks/json/__init__.py#L1'>zerver/webhooks/json/__init__.py:1:1:</a> A005 Module `json` shadows a Python standard-library module
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| A005 | 34 | 26 | 8 | 0 | 0 |

</p>
</details>




---

_Review requested from @MichaReiser by @ntBre on 2025-02-06 23:04_

---

_Review comment by @MichaReiser on `crates/ruff/tests/lint.rs`:2307 on 2025-02-07 08:00_

What's the difference between those tests? I'm probably blind but it seems the arguments are all the same? 

Is the idea to put the snapshot into place for your next PR so that it's "evident" what introducing the new setting changes?

---

_Review comment by @MichaReiser on `crates/ruff/tests/lint.rs`:2327 on 2025-02-07 08:02_

I'd probably move this test to your next PR. Adding tests that currently all assert the same behavior can be somewhat confusing. Took me a while to figure out why they exist. I do think it's fine to leave it as is, considering that you'll land those PR roughly add the same time.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_builtins/rules/stdlib_module_shadowing.rs`:113 on 2025-02-07 08:03_

I think we should use natural ordering here rather than just comparing by byte length. 
```suggestion
        let len = dir.as_os_str().len();
        if path.starts_with(dir) && prefix.is_none_or(|existing| existing < dir) {
            prefix = Some(dir);
        }
```

---

_@MichaReiser approved on 2025-02-07 08:04_

Thanks for taking the time to split this into its own PR! 

---

_@ntBre reviewed on 2025-02-07 14:04_

---

_Review comment by @ntBre on `crates/ruff/tests/lint.rs`:2307 on 2025-02-07 14:04_

Sorry for the confusion, you're exactly right, all three are identical right now. They used to have 
* `lint.flake8-builtins.builtins-strict-checking = true`
* `lint.flake8-builtins.builtins-strict-checking = false`
* and the default

but I had to delete those when I removed the config option. I think it makes much more sense only to include one copy here and add the other two back in the next PR.

Although your snapshot idea is good too, would that make the next one easier for you to review?

---

_@ntBre reviewed on 2025-02-07 14:07_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_builtins/rules/stdlib_module_shadowing.rs`:113 on 2025-02-07 14:07_

Oh good catch, and now I can get rid of the separate `len` tracking!

---

_@ntBre reviewed on 2025-02-07 14:09_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_builtins/rules/stdlib_module_shadowing.rs`:113 on 2025-02-07 14:09_

Oh, I just got an MSRV warning that `is_none_or` is in 1.82 but our MSRV is 1.80. Is that right, or is something in my local setup out of date?

---

_@MichaReiser reviewed on 2025-02-07 14:16_

---

_Review comment by @MichaReiser on `crates/ruff/tests/lint.rs`:2307 on 2025-02-07 14:16_

I'm fine with either. What ever's less work for you

---

_Comment by @ntBre on 2025-02-07 14:24_

I just did a manual `is_none_or` to avoid any MSRV problems, and I'm currently leaning toward just leaving the tests as they are. Once this is merged, I will rebase my other PR onto it!

---

_Merged by @ntBre on 2025-02-07 18:55_

---

_Closed by @ntBre on 2025-02-07 18:55_

---

_Branch deleted on 2025-02-07 18:55_

---
