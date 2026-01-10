```yaml
number: 9047
title: "Allow `sys.path` modifications between imports"
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: charlie/E402
created_at: 2023-12-07T16:23:14Z
updated_at: 2023-12-14T18:24:52Z
url: https://github.com/astral-sh/ruff/pull/9047
synced_at: 2026-01-10T23:31:11Z
```

# Allow `sys.path` modifications between imports

---

_Pull request opened by @charliermarsh on 2023-12-07 16:23_

## Summary

It's common to interleave a `sys.path` modification between imports at the top of a file. This is a frequent cause of `# noqa: E402` false positives, as seen in the ecosystem checks. This PR modifies E402 to omit such modifications when determining the "import boundary".

(We could consider linting against `sys.path` modifications, but that should be a separate rule.)

Closes: https://github.com/astral-sh/ruff/issues/5557.


---

_Comment by @github-actions[bot] on 2023-12-07 16:38_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+23 -2 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/.github/tests/test_download_pretrained.py#L10'>.github/tests/test_download_pretrained.py:10:29:</a> RUF100 [*] Unused `noqa` directive (unused: `E402`)
+ <a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/.github/tests/test_mr_generate_summary.py#L4'>.github/tests/test_mr_generate_summary.py:4:49:</a> RUF100 [*] Unused `noqa` directive (unused: `E402`)
+ <a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/.github/tests/test_mr_publish_results.py#L7'>.github/tests/test_mr_publish_results.py:7:35:</a> RUF100 [*] Unused `noqa` directive (unused: `E402`)
+ <a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/.github/tests/test_validate_gpus.py#L8'>.github/tests/test_validate_gpus.py:8:22:</a> RUF100 [*] Unused `noqa` directive (unused: `E402`)
+ <a href='https://github.com/RasaHQ/rasa/blob/cca30d4e06af5aba177e916d64c60313fc537005/.github/tests/test_validate_gpus.py#L9'>.github/tests/test_validate_gpus.py:9:23:</a> RUF100 [*] Unused `noqa` directive (unused: `E402`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/3904206b69428525db31ff7813daa0322f7b83e8/docs/conf.py#L50'>docs/conf.py:50:69:</a> RUF100 [*] Unused `noqa` directive (unused: `E402`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+15 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/f6553036b9cc8dd7dc0db3ac6eab1dc05aa8ccbb/securedrop/manage.py#L22'>securedrop/manage.py:22:16:</a> RUF100 [*] Unused `noqa` directive (unused: `E402`)
+ <a href='https://github.com/freedomofpress/securedrop/blob/f6553036b9cc8dd7dc0db3ac6eab1dc05aa8ccbb/securedrop/manage.py#L23'>securedrop/manage.py:23:47:</a> RUF100 [*] Unused `noqa` directive (unused: `E402`)
+ <a href='https://github.com/freedomofpress/securedrop/blob/f6553036b9cc8dd7dc0db3ac6eab1dc05aa8ccbb/securedrop/manage.py#L29'>securedrop/manage.py:29:20:</a> RUF100 [*] Unused `noqa` directive (unused: `E402`)
+ <a href='https://github.com/freedomofpress/securedrop/blob/f6553036b9cc8dd7dc0db3ac6eab1dc05aa8ccbb/securedrop/manage.py#L30'>securedrop/manage.py:30:55:</a> RUF100 [*] Unused `noqa` directive (unused: `E402`)
+ <a href='https://github.com/freedomofpress/securedrop/blob/f6553036b9cc8dd7dc0db3ac6eab1dc05aa8ccbb/securedrop/manage.py#L31'>securedrop/manage.py:31:33:</a> RUF100 [*] Unused `noqa` directive (unused: `E402`)
+ <a href='https://github.com/freedomofpress/securedrop/blob/f6553036b9cc8dd7dc0db3ac6eab1dc05aa8ccbb/securedrop/manage.py#L32'>securedrop/manage.py:32:56:</a> RUF100 [*] Unused `noqa` directive (unused: `E402`)
+ <a href='https://github.com/freedomofpress/securedrop/blob/f6553036b9cc8dd7dc0db3ac6eab1dc05aa8ccbb/securedrop/manage.py#L33'>securedrop/manage.py:33:39:</a> RUF100 [*] Unused `noqa` directive (unused: `E402`)
+ <a href='https://github.com/freedomofpress/securedrop/blob/f6553036b9cc8dd7dc0db3ac6eab1dc05aa8ccbb/securedrop/manage.py#L42'>securedrop/manage.py:42:80:</a> RUF100 [*] Unused `noqa` directive (unused: `E402`)
+ <a href='https://github.com/freedomofpress/securedrop/blob/f6553036b9cc8dd7dc0db3ac6eab1dc05aa8ccbb/securedrop/scripts/rqrequeue#L11'>securedrop/scripts/rqrequeue:11:40:</a> RUF100 [*] Unused `noqa` directive (unused: `E402`)
+ <a href='https://github.com/freedomofpress/securedrop/blob/f6553036b9cc8dd7dc0db3ac6eab1dc05aa8ccbb/securedrop/scripts/rqrequeue#L12'>securedrop/scripts/rqrequeue:12:46:</a> RUF100 [*] Unused `noqa` directive (unused: `E402`)
+ <a href='https://github.com/freedomofpress/securedrop/blob/f6553036b9cc8dd7dc0db3ac6eab1dc05aa8ccbb/securedrop/scripts/shredder#L14'>securedrop/scripts/shredder:14:24:</a> RUF100 [*] Unused `noqa` directive (unused: `E402`)
+ <a href='https://github.com/freedomofpress/securedrop/blob/f6553036b9cc8dd7dc0db3ac6eab1dc05aa8ccbb/securedrop/scripts/shredder#L15'>securedrop/scripts/shredder:15:40:</a> RUF100 [*] Unused `noqa` directive (unused: `E402`)
+ <a href='https://github.com/freedomofpress/securedrop/blob/f6553036b9cc8dd7dc0db3ac6eab1dc05aa8ccbb/securedrop/scripts/shredder#L16'>securedrop/scripts/shredder:16:28:</a> RUF100 [*] Unused `noqa` directive (unused: `E402`)
+ <a href='https://github.com/freedomofpress/securedrop/blob/f6553036b9cc8dd7dc0db3ac6eab1dc05aa8ccbb/securedrop/scripts/source_deleter#L14'>securedrop/scripts/source_deleter:14:24:</a> RUF100 [*] Unused `noqa` directive (unused: `E402`)
+ <a href='https://github.com/freedomofpress/securedrop/blob/f6553036b9cc8dd7dc0db3ac6eab1dc05aa8ccbb/securedrop/scripts/source_deleter#L15'>securedrop/scripts/source_deleter:15:40:</a> RUF100 [*] Unused `noqa` directive (unused: `E402`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/model-bakers/model_bakery">model-bakers/model_bakery</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/model-bakers/model_bakery/blob/adba9fce630a20290b59a487f52a8a88a4a77da5/docs/conf.py#L6'>docs/conf.py:6:37:</a> RUF100 [*] Unused blanket `noqa` directive
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/pip">pypa/pip</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pypa/pip/blob/a15dd75d98884c94a77d349b800c7c755d8c34e4/noxfile.py#L16'>noxfile.py:16:42:</a> RUF100 [*] Unused blanket `noqa` directive
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/1987894a2d0e3948cd6433bb880cd0ad1b1895db/docs/conf.py#L11'>docs/conf.py:11:1:</a> E402 Module level import not at top of file
- <a href='https://github.com/zulip/zulip/blob/1987894a2d0e3948cd6433bb880cd0ad1b1895db/zerver/lib/test_fixtures.py#L21'>zerver/lib/test_fixtures.py:21:1:</a> E402 Module level import not at top of file
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF100 | 23 | 23 | 0 | 0 | 0 |
| E402 | 2 | 0 | 2 | 0 | 0 |

</p>
</details>




---

_Comment by @charliermarsh on 2023-12-07 16:45_

I think these are all improvements -- it's nice to see so many `# noqa` comments being removed.

---

_Label `rule` added by @charliermarsh on 2023-12-07 17:08_

---

_Label `preview` added by @charliermarsh on 2023-12-07 17:08_

---

_Renamed from "Allow sys.path modifications between imports" to "Allow `sys.path` modifications between imports" by @charliermarsh on 2023-12-07 17:08_

---

_Review requested from @zanieb by @charliermarsh on 2023-12-07 17:09_

---

_Review requested from @konstin by @charliermarsh on 2023-12-07 17:09_

---

_Marked ready for review by @charliermarsh on 2023-12-07 17:09_

---

_Comment by @charliermarsh on 2023-12-07 17:11_

I gated this to preview for now (ecosystem checks are still updating at time of writing), though honestly I'd be okay with shipping it to stable...

---

_@zanieb approved on 2023-12-07 18:30_

Nice! I think preview makes sense for this change.

---

_Merged by @charliermarsh on 2023-12-07 18:35_

---

_Closed by @charliermarsh on 2023-12-07 18:35_

---

_Branch deleted on 2023-12-07 18:35_

---

_Comment by @konstin on 2023-12-07 20:12_

Looking at the ecosystem checks with entrypoint scripts and tests, this makes even more sense

---

_Comment by @tooruu on 2023-12-14 12:51_

Why modify `sys.path`? This sounds like a dirty fix for bad design, not something to encourage.

---

_Comment by @zanieb on 2023-12-14 18:24_

Hi @tooruu I'd recommend looking through the ecosystem checks as some of this usage is entirely valid.

We're not encouraging this behavior, we're simply fixing a false positive for a different lint rule. We can introduce a separate rule to suggest _not_ making `sys.path` modifications to properly discourage this behavior.

---
