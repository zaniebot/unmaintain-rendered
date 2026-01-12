```yaml
number: 7601
title: "Improve `B005` documentation to reflect duplicate-character behavior"
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/B005
created_at: 2023-09-22T15:59:30Z
updated_at: 2023-09-22T16:18:31Z
url: https://github.com/astral-sh/ruff/pull/7601
synced_at: 2026-01-12T02:39:10Z
```

# Improve `B005` documentation to reflect duplicate-character behavior

---

_Pull request opened by @charliermarsh on 2023-09-22 15:59_

## Summary

B005 only flags `.strip()` calls for which the argument includes duplicate characters. This is consistent with bugbear, but isn't explained in the documentation.


---

_Marked ready for review by @charliermarsh on 2023-09-22 15:59_

---

_Label `documentation` added by @charliermarsh on 2023-09-22 15:59_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_bugbear/rules/strip_with_multi_characters.rs`:33 on 2023-09-22 16:09_

and at best dead-code...

---

_@MichaReiser approved on 2023-09-22 16:09_

---

_Comment by @codspeed-hq[bot] on 2023-09-22 16:11_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/B005)

### Merging #7601 will **degrade performances by 3.15%**

<sub>Comparing <code>charlie/B005</code> (471a3d8) with <code>main</code> (9d16e46)</sub>



### Summary

`❌ 1` regressions
`✅ 24` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/charlie/B005)._

### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/B005` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/default-rules[large/dataset.py]` | 91.1 ms | 94.1 ms | -3.15% |


---

_Merged by @charliermarsh on 2023-09-22 16:12_

---

_Closed by @charliermarsh on 2023-09-22 16:12_

---

_Branch deleted on 2023-09-22 16:12_

---

_Comment by @github-actions[bot] on 2023-09-22 16:18_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+8, -8, 0 error(s))

<details><summary>airflow (+2, -2)</summary>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/5d2cea4515cd656e85c39e04d148fcb2d6ba516a/airflow/providers/qubole/hooks/qubole.py#L87'>airflow/providers/qubole/hooks/qubole.py:87:21:</a> B005 Using `.strip()` with multi-character strings is misleading
- <a href='https://github.com/apache/airflow/blob/5d2cea4515cd656e85c39e04d148fcb2d6ba516a/airflow/providers/qubole/hooks/qubole.py#L87'>airflow/providers/qubole/hooks/qubole.py:87:21:</a> B005 Using `.strip()` with multi-character strings is misleading the reader
+ <a href='https://github.com/apache/airflow/blob/5d2cea4515cd656e85c39e04d148fcb2d6ba516a/scripts/ci/pre_commit/pre_commit_lint_dockerfile.py#L57'>scripts/ci/pre_commit/pre_commit_lint_dockerfile.py:57:15:</a> B005 Using `.strip()` with multi-character strings is misleading
- <a href='https://github.com/apache/airflow/blob/5d2cea4515cd656e85c39e04d148fcb2d6ba516a/scripts/ci/pre_commit/pre_commit_lint_dockerfile.py#L57'>scripts/ci/pre_commit/pre_commit_lint_dockerfile.py:57:15:</a> B005 Using `.strip()` with multi-character strings is misleading the reader
</pre>

</p>
</details>
<details><summary>bokeh (+2, -2)</summary>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/0bdaa42ce135a88d1476809fe0439b111c8167c7/src/bokeh/plotting/_graph.py#L138'>src/bokeh/plotting/_graph.py:138:20:</a> B005 Using `.strip()` with multi-character strings is misleading
- <a href='https://github.com/bokeh/bokeh/blob/0bdaa42ce135a88d1476809fe0439b111c8167c7/src/bokeh/plotting/_graph.py#L138'>src/bokeh/plotting/_graph.py:138:20:</a> B005 Using `.strip()` with multi-character strings is misleading the reader
+ <a href='https://github.com/bokeh/bokeh/blob/0bdaa42ce135a88d1476809fe0439b111c8167c7/src/bokeh/plotting/_graph.py#L138'>src/bokeh/plotting/_graph.py:138:78:</a> B005 Using `.strip()` with multi-character strings is misleading
- <a href='https://github.com/bokeh/bokeh/blob/0bdaa42ce135a88d1476809fe0439b111c8167c7/src/bokeh/plotting/_graph.py#L138'>src/bokeh/plotting/_graph.py:138:78:</a> B005 Using `.strip()` with multi-character strings is misleading the reader
</pre>

</p>
</details>
<details><summary>content (+4, -4)</summary>
<p>

<pre>
+ <a href='https://github.com/demisto/content/blob/fffe08c72c854dbec7aff6b224f806698219f475/Packs/Cryptocurrency/Integrations/Cryptocurrency/Cryptocurrency.py#L61'>Packs/Cryptocurrency/Integrations/Cryptocurrency/Cryptocurrency.py:61:41:</a> B005 Using `.strip()` with multi-character strings is misleading
- <a href='https://github.com/demisto/content/blob/fffe08c72c854dbec7aff6b224f806698219f475/Packs/Cryptocurrency/Integrations/Cryptocurrency/Cryptocurrency.py#L61'>Packs/Cryptocurrency/Integrations/Cryptocurrency/Cryptocurrency.py:61:41:</a> B005 Using `.strip()` with multi-character strings is misleading the reader
+ <a href='https://github.com/demisto/content/blob/fffe08c72c854dbec7aff6b224f806698219f475/Packs/PcapAnalysis/Scripts/PcapMinerV2/PcapMinerV2.py#L138'>Packs/PcapAnalysis/Scripts/PcapMinerV2/PcapMinerV2.py:138:38:</a> B005 Using `.strip()` with multi-character strings is misleading
- <a href='https://github.com/demisto/content/blob/fffe08c72c854dbec7aff6b224f806698219f475/Packs/PcapAnalysis/Scripts/PcapMinerV2/PcapMinerV2.py#L138'>Packs/PcapAnalysis/Scripts/PcapMinerV2/PcapMinerV2.py:138:38:</a> B005 Using `.strip()` with multi-character strings is misleading the reader
+ <a href='https://github.com/demisto/content/blob/fffe08c72c854dbec7aff6b224f806698219f475/Packs/SimpleDebugger/Scripts/SimpleDebugger/SimpleDebugger.py#L260'>Packs/SimpleDebugger/Scripts/SimpleDebugger/SimpleDebugger.py:260:42:</a> B005 Using `.strip()` with multi-character strings is misleading
- <a href='https://github.com/demisto/content/blob/fffe08c72c854dbec7aff6b224f806698219f475/Packs/SimpleDebugger/Scripts/SimpleDebugger/SimpleDebugger.py#L260'>Packs/SimpleDebugger/Scripts/SimpleDebugger/SimpleDebugger.py:260:42:</a> B005 Using `.strip()` with multi-character strings is misleading the reader
+ <a href='https://github.com/demisto/content/blob/fffe08c72c854dbec7aff6b224f806698219f475/Packs/URLHaus/Integrations/URLHaus/URLHaus.py#L655'>Packs/URLHaus/Integrations/URLHaus/URLHaus.py:655:37:</a> B005 Using `.strip()` with multi-character strings is misleading
- <a href='https://github.com/demisto/content/blob/fffe08c72c854dbec7aff6b224f806698219f475/Packs/URLHaus/Integrations/URLHaus/URLHaus.py#L655'>Packs/URLHaus/Integrations/URLHaus/URLHaus.py:655:37:</a> B005 Using `.strip()` with multi-character strings is misleading the reader
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| B005 | 16 | 8 | 8 |



---
