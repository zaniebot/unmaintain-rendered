```yaml
number: 18505
title: "[`pyupgrade`] Stabilize `non-pep604-annotation-optional` (`UP045`) and preview behavior for `non-pep604-annotation-union` (`UP007`)"
type: pull_request
state: merged
author: dylwil3
labels:
  - rule
assignees: []
merged: true
base: brent/release-0.12.0
head: dylan/stabilize-optional-union-up
created_at: 2025-06-06T17:17:41Z
updated_at: 2025-06-09T22:25:15Z
url: https://github.com/astral-sh/ruff/pull/18505
synced_at: 2026-01-12T15:56:20Z
```

# [`pyupgrade`] Stabilize `non-pep604-annotation-optional` (`UP045`) and preview behavior for `non-pep604-annotation-union` (`UP007`)

---

_@dylwil3_

_No description provided._

---

_Added to milestone `v0.12` by @dylwil3 on 2025-06-06 17:17_

---

_Label `rule` added by @dylwil3 on 2025-06-06 17:17_

---

_Comment by @dylwil3 on 2025-06-06 17:18_

(@ntBre heads up this also does a rule stabilization - stepping on your toes here 😄 )

---

_Comment by @github-actions[bot] on 2025-06-06 17:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+3085 -857 violations, +0 -0 fixes in 6 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/b5e19cbe31475d63a5b33dd4a59d765da6857cd5/airflow-core/src/airflow/utils/log/logging_mixin.py#L75'>airflow-core/src/airflow/utils/log/logging_mixin.py:75:30:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/apache/airflow/blob/b5e19cbe31475d63a5b33dd4a59d765da6857cd5/airflow-core/src/airflow/utils/log/logging_mixin.py#L75'>airflow-core/src/airflow/utils/log/logging_mixin.py:75:52:</a> RUF100 [*] Unused `noqa` directive (unused: `UP007`)
+ <a href='https://github.com/apache/airflow/blob/b5e19cbe31475d63a5b33dd4a59d765da6857cd5/airflow-core/src/airflow/utils/log/logging_mixin.py#L77'>airflow-core/src/airflow/utils/log/logging_mixin.py:77:19:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/apache/airflow/blob/b5e19cbe31475d63a5b33dd4a59d765da6857cd5/airflow-core/src/airflow/utils/log/logging_mixin.py#L77'>airflow-core/src/airflow/utils/log/logging_mixin.py:77:41:</a> RUF100 [*] Unused `noqa` directive (unused: `UP007`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+833 -833 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/2f007bf7a5a110631ce48a3fdd595289642aa18d/RELEASING/changelog.py#L106'>RELEASING/changelog.py:106:53:</a> UP007 [*] Use `X | Y` for type annotations
+ <a href='https://github.com/apache/superset/blob/2f007bf7a5a110631ce48a3fdd595289642aa18d/RELEASING/changelog.py#L106'>RELEASING/changelog.py:106:53:</a> UP045 [*] Use `X | None` for type annotations
- <a href='https://github.com/apache/superset/blob/2f007bf7a5a110631ce48a3fdd595289642aa18d/RELEASING/changelog.py#L72'>RELEASING/changelog.py:72:23:</a> UP007 [*] Use `X | Y` for type annotations
+ <a href='https://github.com/apache/superset/blob/2f007bf7a5a110631ce48a3fdd595289642aa18d/RELEASING/changelog.py#L72'>RELEASING/changelog.py:72:23:</a> UP045 [*] Use `X | None` for type annotations
- <a href='https://github.com/apache/superset/blob/2f007bf7a5a110631ce48a3fdd595289642aa18d/RELEASING/changelog.py#L73'>RELEASING/changelog.py:73:15:</a> UP007 [*] Use `X | Y` for type annotations
+ <a href='https://github.com/apache/superset/blob/2f007bf7a5a110631ce48a3fdd595289642aa18d/RELEASING/changelog.py#L73'>RELEASING/changelog.py:73:15:</a> UP045 [*] Use `X | None` for type annotations
- <a href='https://github.com/apache/superset/blob/2f007bf7a5a110631ce48a3fdd595289642aa18d/RELEASING/changelog.py#L78'>RELEASING/changelog.py:78:45:</a> UP007 [*] Use `X | Y` for type annotations
+ <a href='https://github.com/apache/superset/blob/2f007bf7a5a110631ce48a3fdd595289642aa18d/RELEASING/changelog.py#L78'>RELEASING/changelog.py:78:45:</a> UP045 [*] Use `X | None` for type annotations
- <a href='https://github.com/apache/superset/blob/2f007bf7a5a110631ce48a3fdd595289642aa18d/RELEASING/verify_release.py#L59'>RELEASING/verify_release.py:59:42:</a> UP007 [*] Use `X | Y` for type annotations
+ <a href='https://github.com/apache/superset/blob/2f007bf7a5a110631ce48a3fdd595289642aa18d/RELEASING/verify_release.py#L59'>RELEASING/verify_release.py:59:42:</a> UP045 [*] Use `X | None` for type annotations
- <a href='https://github.com/apache/superset/blob/2f007bf7a5a110631ce48a3fdd595289642aa18d/RELEASING/verify_release.py#L59'>RELEASING/verify_release.py:59:57:</a> UP007 [*] Use `X | Y` for type annotations
+ <a href='https://github.com/apache/superset/blob/2f007bf7a5a110631ce48a3fdd595289642aa18d/RELEASING/verify_release.py#L59'>RELEASING/verify_release.py:59:57:</a> UP045 [*] Use `X | None` for type annotations
- <a href='https://github.com/apache/superset/blob/2f007bf7a5a110631ce48a3fdd595289642aa18d/RELEASING/verify_release.py#L91'>RELEASING/verify_release.py:91:33:</a> UP007 [*] Use `X | Y` for type annotations
+ <a href='https://github.com/apache/superset/blob/2f007bf7a5a110631ce48a3fdd595289642aa18d/RELEASING/verify_release.py#L91'>RELEASING/verify_release.py:91:33:</a> UP045 [*] Use `X | None` for type annotations
- <a href='https://github.com/apache/superset/blob/2f007bf7a5a110631ce48a3fdd595289642aa18d/scripts/cancel_github_workflows.py#L156'>scripts/cancel_github_workflows.py:156:21:</a> UP007 [*] Use `X | Y` for type annotations
+ <a href='https://github.com/apache/superset/blob/2f007bf7a5a110631ce48a3fdd595289642aa18d/scripts/cancel_github_workflows.py#L156'>scripts/cancel_github_workflows.py:156:21:</a> UP045 [*] Use `X | None` for type annotations
- <a href='https://github.com/apache/superset/blob/2f007bf7a5a110631ce48a3fdd595289642aa18d/scripts/cancel_github_workflows.py#L65'>scripts/cancel_github_workflows.py:65:13:</a> UP007 [*] Use `X | Y` for type annotations
+ <a href='https://github.com/apache/superset/blob/2f007bf7a5a110631ce48a3fdd595289642aa18d/scripts/cancel_github_workflows.py#L65'>scripts/cancel_github_workflows.py:65:13:</a> UP045 [*] Use `X | None` for type annotations
- <a href='https://github.com/apache/superset/blob/2f007bf7a5a110631ce48a3fdd595289642aa18d/scripts/cancel_github_workflows.py#L95'>scripts/cancel_github_workflows.py:95:13:</a> UP007 [*] Use `X | Y` for type annotations
+ <a href='https://github.com/apache/superset/blob/2f007bf7a5a110631ce48a3fdd595289642aa18d/scripts/cancel_github_workflows.py#L95'>scripts/cancel_github_workflows.py:95:13:</a> UP045 [*] Use `X | None` for type annotations
- <a href='https://github.com/apache/superset/blob/2f007bf7a5a110631ce48a3fdd595289642aa18d/scripts/cancel_github_workflows.py#L96'>scripts/cancel_github_workflows.py:96:11:</a> UP007 [*] Use `X | Y` for type annotations
... 823 additional changes omitted for rule UP007
... 1645 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/binary-husky/gpt_academic">binary-husky/gpt_academic</a> (+11 -11 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/binary-husky/gpt_academic/blob/8c2143229113ad0953b4e4dfd2a543292ce65b14/crazy_functions/doc_fns/read_fns/excel_reader.py#L186'>crazy_functions/doc_fns/read_fns/excel_reader.py:186:22:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/binary-husky/gpt_academic/blob/8c2143229113ad0953b4e4dfd2a543292ce65b14/crazy_functions/doc_fns/read_fns/excel_reader.py#L186'>crazy_functions/doc_fns/read_fns/excel_reader.py:186:22:</a> UP045 Use `X | None` for type annotations
- <a href='https://github.com/binary-husky/gpt_academic/blob/8c2143229113ad0953b4e4dfd2a543292ce65b14/crazy_functions/doc_fns/read_fns/excel_reader.py#L40'>crazy_functions/doc_fns/read_fns/excel_reader.py:40:32:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/binary-husky/gpt_academic/blob/8c2143229113ad0953b4e4dfd2a543292ce65b14/crazy_functions/doc_fns/read_fns/excel_reader.py#L40'>crazy_functions/doc_fns/read_fns/excel_reader.py:40:32:</a> UP045 Use `X | None` for type annotations
- <a href='https://github.com/binary-husky/gpt_academic/blob/8c2143229113ad0953b4e4dfd2a543292ce65b14/crazy_functions/doc_fns/read_fns/excel_reader.py#L96'>crazy_functions/doc_fns/read_fns/excel_reader.py:96:60:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/binary-husky/gpt_academic/blob/8c2143229113ad0953b4e4dfd2a543292ce65b14/crazy_functions/doc_fns/read_fns/excel_reader.py#L96'>crazy_functions/doc_fns/read_fns/excel_reader.py:96:60:</a> UP045 Use `X | None` for type annotations
- <a href='https://github.com/binary-husky/gpt_academic/blob/8c2143229113ad0953b4e4dfd2a543292ce65b14/crazy_functions/doc_fns/read_fns/markitdown/markdown_reader.py#L171'>crazy_functions/doc_fns/read_fns/markitdown/markdown_reader.py:171:26:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/binary-husky/gpt_academic/blob/8c2143229113ad0953b4e4dfd2a543292ce65b14/crazy_functions/doc_fns/read_fns/markitdown/markdown_reader.py#L171'>crazy_functions/doc_fns/read_fns/markitdown/markdown_reader.py:171:26:</a> UP045 Use `X | None` for type annotations
- <a href='https://github.com/binary-husky/gpt_academic/blob/8c2143229113ad0953b4e4dfd2a543292ce65b14/crazy_functions/doc_fns/read_fns/markitdown/markdown_reader.py#L46'>crazy_functions/doc_fns/read_fns/markitdown/markdown_reader.py:46:17:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/binary-husky/gpt_academic/blob/8c2143229113ad0953b4e4dfd2a543292ce65b14/crazy_functions/doc_fns/read_fns/markitdown/markdown_reader.py#L46'>crazy_functions/doc_fns/read_fns/markitdown/markdown_reader.py:46:17:</a> UP045 Use `X | None` for type annotations
... 12 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+105 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/7671bda9852a1585cd725c10711642ecd75aa1f4/ibis/backends/bigquery/__init__.py#L890'>ibis/backends/bigquery/__init__.py:890:48:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/ibis-project/ibis/blob/7671bda9852a1585cd725c10711642ecd75aa1f4/ibis/common/graph.py#L171'>ibis/common/graph.py:171:46:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/ibis-project/ibis/blob/7671bda9852a1585cd725c10711642ecd75aa1f4/ibis/common/graph.py#L203'>ibis/common/graph.py:203:50:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/ibis-project/ibis/blob/7671bda9852a1585cd725c10711642ecd75aa1f4/ibis/common/graph.py#L275'>ibis/common/graph.py:275:41:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/ibis-project/ibis/blob/7671bda9852a1585cd725c10711642ecd75aa1f4/ibis/common/graph.py#L310'>ibis/common/graph.py:310:47:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/ibis-project/ibis/blob/7671bda9852a1585cd725c10711642ecd75aa1f4/ibis/common/graph.py#L359'>ibis/common/graph.py:359:47:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/ibis-project/ibis/blob/7671bda9852a1585cd725c10711642ecd75aa1f4/ibis/common/graph.py#L379'>ibis/common/graph.py:379:17:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/ibis-project/ibis/blob/7671bda9852a1585cd725c10711642ecd75aa1f4/ibis/common/graph.py#L380'>ibis/common/graph.py:380:18:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/ibis-project/ibis/blob/7671bda9852a1585cd725c10711642ecd75aa1f4/ibis/common/graph.py#L418'>ibis/common/graph.py:418:17:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/ibis-project/ibis/blob/7671bda9852a1585cd725c10711642ecd75aa1f4/ibis/common/graph.py#L419'>ibis/common/graph.py:419:18:</a> UP045 Use `X | None` for type annotations
... 95 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+2119 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/9ce974247ce454dcdae1904489c65063f5c2af09/libs/core/langchain_core/caches.py#L100'>libs/core/langchain_core/caches.py:100:62:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/9ce974247ce454dcdae1904489c65063f5c2af09/libs/core/langchain_core/caches.py#L152'>libs/core/langchain_core/caches.py:152:36:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/9ce974247ce454dcdae1904489c65063f5c2af09/libs/core/langchain_core/caches.py#L170'>libs/core/langchain_core/caches.py:170:55:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/9ce974247ce454dcdae1904489c65063f5c2af09/libs/core/langchain_core/caches.py#L204'>libs/core/langchain_core/caches.py:204:62:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/9ce974247ce454dcdae1904489c65063f5c2af09/libs/core/langchain_core/caches.py#L55'>libs/core/langchain_core/caches.py:55:55:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/9ce974247ce454dcdae1904489c65063f5c2af09/libs/core/langchain_core/callbacks/base.py#L107'>libs/core/langchain_core/callbacks/base.py:107:24:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/9ce974247ce454dcdae1904489c65063f5c2af09/libs/core/langchain_core/callbacks/base.py#L128'>libs/core/langchain_core/callbacks/base.py:128:24:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/9ce974247ce454dcdae1904489c65063f5c2af09/libs/core/langchain_core/callbacks/base.py#L145'>libs/core/langchain_core/callbacks/base.py:145:24:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/9ce974247ce454dcdae1904489c65063f5c2af09/libs/core/langchain_core/callbacks/base.py#L162'>libs/core/langchain_core/callbacks/base.py:162:24:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/9ce974247ce454dcdae1904489c65063f5c2af09/libs/core/langchain_core/callbacks/base.py#L179'>libs/core/langchain_core/callbacks/base.py:179:24:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/9ce974247ce454dcdae1904489c65063f5c2af09/libs/core/langchain_core/callbacks/base.py#L200'>libs/core/langchain_core/callbacks/base.py:200:24:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/9ce974247ce454dcdae1904489c65063f5c2af09/libs/core/langchain_core/callbacks/base.py#L217'>libs/core/langchain_core/callbacks/base.py:217:24:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/9ce974247ce454dcdae1904489c65063f5c2af09/libs/core/langchain_core/callbacks/base.py#L239'>libs/core/langchain_core/callbacks/base.py:239:24:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/9ce974247ce454dcdae1904489c65063f5c2af09/libs/core/langchain_core/callbacks/base.py#L240'>libs/core/langchain_core/callbacks/base.py:240:15:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/9ce974247ce454dcdae1904489c65063f5c2af09/libs/core/langchain_core/callbacks/base.py#L241'>libs/core/langchain_core/callbacks/base.py:241:19:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/9ce974247ce454dcdae1904489c65063f5c2af09/libs/core/langchain_core/callbacks/base.py#L266'>libs/core/langchain_core/callbacks/base.py:266:24:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/9ce974247ce454dcdae1904489c65063f5c2af09/libs/core/langchain_core/callbacks/base.py#L267'>libs/core/langchain_core/callbacks/base.py:267:15:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/9ce974247ce454dcdae1904489c65063f5c2af09/libs/core/langchain_core/callbacks/base.py#L268'>libs/core/langchain_core/callbacks/base.py:268:19:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/9ce974247ce454dcdae1904489c65063f5c2af09/libs/core/langchain_core/callbacks/base.py#L296'>libs/core/langchain_core/callbacks/base.py:296:24:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/9ce974247ce454dcdae1904489c65063f5c2af09/libs/core/langchain_core/callbacks/base.py#L297'>libs/core/langchain_core/callbacks/base.py:297:15:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/9ce974247ce454dcdae1904489c65063f5c2af09/libs/core/langchain_core/callbacks/base.py#L298'>libs/core/langchain_core/callbacks/base.py:298:19:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/9ce974247ce454dcdae1904489c65063f5c2af09/libs/core/langchain_core/callbacks/base.py#L319'>libs/core/langchain_core/callbacks/base.py:319:24:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/9ce974247ce454dcdae1904489c65063f5c2af09/libs/core/langchain_core/callbacks/base.py#L320'>libs/core/langchain_core/callbacks/base.py:320:15:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/9ce974247ce454dcdae1904489c65063f5c2af09/libs/core/langchain_core/callbacks/base.py#L321'>libs/core/langchain_core/callbacks/base.py:321:19:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/9ce974247ce454dcdae1904489c65063f5c2af09/libs/core/langchain_core/callbacks/base.py#L32'>libs/core/langchain_core/callbacks/base.py:32:24:</a> UP045 Use `X | None` for type annotations
+ <a href='https://github.com/langchain-ai/langchain/blob/9ce974247ce454dcdae1904489c65063f5c2af09/libs/core/langchain_core/callbacks/base.py#L342'>libs/core/langchain_core/callbacks/base.py:342:24:</a> UP045 Use `X | None` for type annotations
... 2093 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+13 -13 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/fe0d3801e44c64448f01372f01c9dc6574f805a6/src/latch/registry/record.py#L188'>src/latch/registry/record.py:188:57:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/latchbio/latch/blob/fe0d3801e44c64448f01372f01c9dc6574f805a6/src/latch/registry/record.py#L188'>src/latch/registry/record.py:188:57:</a> UP045 Use `X | None` for type annotations
- <a href='https://github.com/latchbio/latch/blob/fe0d3801e44c64448f01372f01c9dc6574f805a6/src/latch/registry/record.py#L190'>src/latch/registry/record.py:190:64:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/latchbio/latch/blob/fe0d3801e44c64448f01372f01c9dc6574f805a6/src/latch/registry/record.py#L190'>src/latch/registry/record.py:190:64:</a> UP045 Use `X | None` for type annotations
- <a href='https://github.com/latchbio/latch/blob/fe0d3801e44c64448f01372f01c9dc6574f805a6/src/latch/registry/record.py#L215'>src/latch/registry/record.py:215:53:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/latchbio/latch/blob/fe0d3801e44c64448f01372f01c9dc6574f805a6/src/latch/registry/record.py#L215'>src/latch/registry/record.py:215:53:</a> UP045 Use `X | None` for type annotations
- <a href='https://github.com/latchbio/latch/blob/fe0d3801e44c64448f01372f01c9dc6574f805a6/src/latch/registry/record.py#L217'>src/latch/registry/record.py:217:60:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/latchbio/latch/blob/fe0d3801e44c64448f01372f01c9dc6574f805a6/src/latch/registry/record.py#L217'>src/latch/registry/record.py:217:60:</a> UP045 Use `X | None` for type annotations
- <a href='https://github.com/latchbio/latch/blob/fe0d3801e44c64448f01372f01c9dc6574f805a6/src/latch/registry/record.py#L250'>src/latch/registry/record.py:250:10:</a> UP007 Use `X | Y` for type annotations
+ <a href='https://github.com/latchbio/latch/blob/fe0d3801e44c64448f01372f01c9dc6574f805a6/src/latch/registry/record.py#L250'>src/latch/registry/record.py:250:10:</a> UP045 Use `X | None` for type annotations
... 16 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (3 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| UP045 | 3083 | 3083 | 0 | 0 | 0 |
| UP007 | 857 | 0 | 857 | 0 | 0 |
| RUF100 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @dylwil3 on 2025-06-06 17:35_

Despite the alarming number of changes in the ecosystem check, this is correct since some repos have `UP007` ignored - see https://github.com/astral-sh/ruff/pull/15313#issuecomment-2574840504

---

_Comment by @ntBre on 2025-06-06 18:35_

Keep stepping! There are plenty of toes to go around in this release :laughing: 

In this case, we may need to hold off in light of https://github.com/astral-sh/ruff/issues/18508

---

_Comment by @AlexWaygood on 2025-06-06 19:53_

> In this case, we may need to hold off in light of #18508

IMO: I don't think that issue should block stabilisation on its own. It's a valid bug report and we should fix it, but it's also an extreme edge case that would almost never come up in real code (the only situations I can think of are generated code, or a very confused user 😄)

---

_@ntBre approved on 2025-06-09 21:44_

---

_Merged by @dylwil3 on 2025-06-09 22:25_

---

_Closed by @dylwil3 on 2025-06-09 22:25_

---

_Branch deleted on 2025-06-09 22:25_

---
