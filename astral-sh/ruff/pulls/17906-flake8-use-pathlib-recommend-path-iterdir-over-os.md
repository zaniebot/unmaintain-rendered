```yaml
number: 17906
title: "[`flake8-use-pathlib`] Recommend `Path.iterdir()` over `os.scandir()` (`PTH209`)"
type: pull_request
state: closed
author: InSyncWithFoo
labels:
  - rule
assignees: []
base: main
head: PTH209
created_at: 2025-05-07T03:48:38Z
updated_at: 2025-05-07T12:47:07Z
url: https://github.com/astral-sh/ruff/pull/17906
synced_at: 2026-01-10T18:57:03Z
```

# [`flake8-use-pathlib`] Recommend `Path.iterdir()` over `os.scandir()` (`PTH209`)

---

_Pull request opened by @InSyncWithFoo on 2025-05-07 03:48_

## Summary

Per discussion at #14509; redo of #14623, part of #15506.

`PTH209` reports any and all usages of `os.scandir()` and recommends replacing it with `Path.iterdir()`. This has performance implications, as have been discussed in #14623.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2025-05-07 03:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+4 -0 violations, +0 -0 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/8c0dcc8760444470203057087966bbbe3259162e/dev/breeze/src/airflow_breeze/commands/release_candidate_command.py#L299'>dev/breeze/src/airflow_breeze/commands/release_candidate_command.py:299:18:</a> PTH209 Use `pathlib.Path.iterdir()` instead.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_ext.py#L69'>tests/unit/bokeh/test_ext.py:69:19:</a> PTH209 Use `pathlib.Path.iterdir()` instead.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/e47a651f5c9795b75bf7a2dd1cbda21a041334d4/src/scikit_build_core/_shutil.py#L93'>src/scikit_build_core/_shutil.py:93:10:</a> PTH209 Use `pathlib.Path.iterdir()` instead.
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/e47a651f5c9795b75bf7a2dd1cbda21a041334d4/src/scikit_build_core/build/_pathutil.py#L23'>src/scikit_build_core/build/_pathutil.py:23:18:</a> PTH209 Use `pathlib.Path.iterdir()` instead.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PTH209 | 4 | 4 | 0 | 0 | 0 |

</p>
</details>




---

_Label `rule` added by @MichaReiser on 2025-05-07 06:34_

---

_Comment by @MichaReiser on 2025-05-07 06:34_

I think I'm missing something but how's that different from the other PR? Shouldn't we recomment `Path.scandir` instead of `listdir` as pointed out in https://github.com/astral-sh/ruff/pull/14623#issuecomment-2503719352

---

_Comment by @InSyncWithFoo on 2025-05-07 10:11_

@MichaReiser It is the same; I submitted this one because I couldn't reopen the other. Also, there's no `Path.scandir()` (probably just a typo), only `Path.iterdir()`.

---

_Comment by @MichaReiser on 2025-05-07 10:40_

> When Python 3.14 enters the beta period, we can consider adding a rule suggesting to replace os.scandir with Path.scandir, assuming the new API is still present when the beta period starts.

I assumed a `Path.scandir` exists but couldn't find it when doing a quick search

>  It is the same

I then, don't understand what's different to when we closed the other PR. What has changed that addresses our concerns from back then?


---

_Comment by @InSyncWithFoo on 2025-05-07 12:47_

Oops, sorry, Alex did mean `Path.scandir` and not `Path.iterdir`. It appears `scandir` [was made private](https://github.com/python/cpython/pull/127377) a while ago.

(What a mistake I have made.) Closing.

---

_Closed by @InSyncWithFoo on 2025-05-07 12:47_

---
