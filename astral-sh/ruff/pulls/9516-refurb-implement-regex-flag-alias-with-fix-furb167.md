```yaml
number: 9516
title: "[`refurb`] Implement `regex-flag-alias` with fix (`FURB167`)"
type: pull_request
state: merged
author: diceroll123
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: add-FURB167
created_at: 2024-01-14T20:52:12Z
updated_at: 2024-01-14T23:47:29Z
url: https://github.com/astral-sh/ruff/pull/9516
synced_at: 2026-01-10T22:57:09Z
```

# [`refurb`] Implement `regex-flag-alias` with fix (`FURB167`)

---

_Pull request opened by @diceroll123 on 2024-01-14 20:52_

## Summary

add [`FURB167`/`use-long-regex-flag`](https://github.com/dosisod/refurb/blob/master/refurb/checks/regex/use_long_flag.py) with autofix

See: #1348 

## Test Plan

`cargo test`

---

_Comment by @github-actions[bot] on 2024-01-14 21:05_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+15 -0 violations, +0 -0 fixes in 3 projects; 40 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/4883c38f3b2e55c6e2acbc0ed690907d7b725e77/airflow/providers/apache/spark/hooks/spark_submit.py#L306'>airflow/providers/apache/spark/hooks/spark_submit.py:306:19:</a> FURB167 [*] Use of regular expression alias `re.I`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/core/property/visual.py#L109'>src/bokeh/core/property/visual.py:109:116:</a> FURB167 [*] Use of regular expression alias `re.I`
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/embed/util.py#L363'>src/bokeh/embed/util.py:363:60:</a> FURB167 [*] Use of regular expression alias `re.S`
+ <a href='https://github.com/bokeh/bokeh/blob/1e932c157d9e3e973617e39065759a2dae4147a9/src/bokeh/embed/util.py#L378'>src/bokeh/embed/util.py:378:60:</a> FURB167 [*] Use of regular expression alias `re.S`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+11 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/79ec61b373c0e027627fe13f877665a1a2b893b3/zerver/lib/compatibility.py#L46'>zerver/lib/compatibility.py:46:60:</a> FURB167 [*] Use of regular expression alias `re.X`
+ <a href='https://github.com/zulip/zulip/blob/79ec61b373c0e027627fe13f877665a1a2b893b3/zerver/lib/compatibility.py#L97'>zerver/lib/compatibility.py:97:48:</a> FURB167 [*] Use of regular expression alias `re.I`
+ <a href='https://github.com/zulip/zulip/blob/79ec61b373c0e027627fe13f877665a1a2b893b3/zerver/lib/compatibility.py#L97'>zerver/lib/compatibility.py:97:55:</a> FURB167 [*] Use of regular expression alias `re.X`
+ <a href='https://github.com/zulip/zulip/blob/79ec61b373c0e027627fe13f877665a1a2b893b3/zerver/lib/compatibility.py#L99'>zerver/lib/compatibility.py:99:61:</a> FURB167 [*] Use of regular expression alias `re.I`
+ <a href='https://github.com/zulip/zulip/blob/79ec61b373c0e027627fe13f877665a1a2b893b3/zerver/lib/compatibility.py#L99'>zerver/lib/compatibility.py:99:68:</a> FURB167 [*] Use of regular expression alias `re.X`
+ <a href='https://github.com/zulip/zulip/blob/79ec61b373c0e027627fe13f877665a1a2b893b3/zerver/lib/markdown/include.py#L30'>zerver/lib/markdown/include.py:30:48:</a> FURB167 [*] Use of regular expression alias `re.M`
+ <a href='https://github.com/zulip/zulip/blob/79ec61b373c0e027627fe13f877665a1a2b893b3/zerver/lib/user_agent.py#L12'>zerver/lib/user_agent.py:12:5:</a> FURB167 [*] Use of regular expression alias `re.X`
+ <a href='https://github.com/zulip/zulip/blob/79ec61b373c0e027627fe13f877665a1a2b893b3/zerver/migrations/0041_create_attachments_for_old_messages.py#L14'>zerver/migrations/0041_create_attachments_for_old_messages.py:14:51:</a> FURB167 [*] Use of regular expression alias `re.M`
+ <a href='https://github.com/zulip/zulip/blob/79ec61b373c0e027627fe13f877665a1a2b893b3/zerver/openapi/markdown_extension.py#L170'>zerver/openapi/markdown_extension.py:170:42:</a> FURB167 [*] Use of regular expression alias `re.M`
+ <a href='https://github.com/zulip/zulip/blob/79ec61b373c0e027627fe13f877665a1a2b893b3/zerver/openapi/markdown_extension.py#L170'>zerver/openapi/markdown_extension.py:170:49:</a> FURB167 [*] Use of regular expression alias `re.S`
+ <a href='https://github.com/zulip/zulip/blob/79ec61b373c0e027627fe13f877665a1a2b893b3/zerver/tornado/sharding.py#L20'>zerver/tornado/sharding.py:20:32:</a> FURB167 [*] Use of regular expression alias `re.I`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FURB167 | 15 | 15 | 0 | 0 | 0 |

</p>
</details>




---

_@charliermarsh approved on 2024-01-14 23:35_

Great, thanks!

---

_Label `rule` added by @charliermarsh on 2024-01-14 23:35_

---

_Label `preview` added by @charliermarsh on 2024-01-14 23:35_

---

_Renamed from "[refurb] - add `FURB167`/`use-long-regex-flag` with autofix" to "[`refurb`] Implement `regex-flag-alias` with fix (`FURB167`)" by @charliermarsh on 2024-01-14 23:36_

---

_Merged by @charliermarsh on 2024-01-14 23:40_

---

_Closed by @charliermarsh on 2024-01-14 23:40_

---

_Comment by @codspeed-hq[bot] on 2024-01-14 23:41_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/diceroll123:add-FURB167)

### Merging #9516 will **improve performances by 4.55%**

<sub>Comparing <code>diceroll123:add-FURB167</code> (de5f663) with <code>main</code> (0c0d3db)</sub>



### Summary

`⚡ 1` improvements
`✅ 29` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `diceroll123:add-FURB167` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `parser[numpy/ctypeslib.py]` | 12.8 ms | 12.2 ms | +4.55% |


---
