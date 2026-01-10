```yaml
number: 10098
title: "[`pycodestyle`]: Make blank lines in typing stub files optional (`E3*`)"
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: blank-lines-typing-stub
created_at: 2024-02-23T15:12:40Z
updated_at: 2024-03-05T11:48:50Z
url: https://github.com/astral-sh/ruff/pull/10098
synced_at: 2026-01-10T22:47:01Z
```

# [`pycodestyle`]: Make blank lines in typing stub files optional (`E3*`)

---

_Pull request opened by @MichaReiser on 2024-02-23 15:12_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/10039

The [recommendation for typing stub files](https://typing.readthedocs.io/en/latest/source/stubs.html#blank-lines) is to use **one** blank line to group related definitions and
otherwise omit blank lines. 

The newly added blank line rules (`E3*`) didn't account for typing stub files and enforced two empty lines at the top level and one empty line otherwise, making it impossible to group related definitions. 

This PR implements the `E3*` rules to:

* Not enforce blank lines. The use of blank lines in typing definitions is entirely up to the user. 
* Allow at most one empty line, including between top level statements. 

## Test Plan

Added unit tests (It may look odd that many snapshots are empty but the point is that the rule should no longer emit diagnostics)


---

_Label `bug` added by @MichaReiser on 2024-02-23 15:13_

---

_Label `preview` added by @MichaReiser on 2024-02-23 15:13_

---

_Comment by @github-actions[bot] on 2024-02-23 15:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+57 -138 violations, +0 -0 fixes in 6 projects; 37 projects unchanged)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+0 -4 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/RasaHQ/rasa/blob/68ce281aeec352876afb2baf74844e95b0c69ff4/stubs/aio_pika/robust_channel.pyi#L10'>stubs/aio_pika/robust_channel.pyi:10:1:</a> E302 [*] Expected 2 blank lines, found 1
- <a href='https://github.com/RasaHQ/rasa/blob/68ce281aeec352876afb2baf74844e95b0c69ff4/stubs/sanic.pyi#L23'>stubs/sanic.pyi:23:1:</a> E302 [*] Expected 2 blank lines, found 1
- <a href='https://github.com/RasaHQ/rasa/blob/68ce281aeec352876afb2baf74844e95b0c69ff4/stubs/sanic.pyi#L27'>stubs/sanic.pyi:27:1:</a> E302 [*] Expected 2 blank lines, found 1
- <a href='https://github.com/RasaHQ/rasa/blob/68ce281aeec352876afb2baf74844e95b0c69ff4/stubs/socketio.pyi#L49'>stubs/socketio.pyi:49:1:</a> E302 [*] Expected 2 blank lines, found 1
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -25 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/b541f5519c37c9b8b79fb2fe0e2b7105d0b12e6e/airflow/compat/functools.pyi#L27'>airflow/compat/functools.pyi:27:1:</a> E302 [*] Expected 2 blank lines, found 1
- <a href='https://github.com/apache/airflow/blob/b541f5519c37c9b8b79fb2fe0e2b7105d0b12e6e/airflow/decorators/__init__.pyi#L102'>airflow/decorators/__init__.pyi:102:5:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/apache/airflow/blob/b541f5519c37c9b8b79fb2fe0e2b7105d0b12e6e/airflow/decorators/__init__.pyi#L105'>airflow/decorators/__init__.pyi:105:5:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/apache/airflow/blob/b541f5519c37c9b8b79fb2fe0e2b7105d0b12e6e/airflow/decorators/__init__.pyi#L158'>airflow/decorators/__init__.pyi:158:5:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/apache/airflow/blob/b541f5519c37c9b8b79fb2fe0e2b7105d0b12e6e/airflow/decorators/__init__.pyi#L160'>airflow/decorators/__init__.pyi:160:5:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/apache/airflow/blob/b541f5519c37c9b8b79fb2fe0e2b7105d0b12e6e/airflow/decorators/__init__.pyi#L191'>airflow/decorators/__init__.pyi:191:5:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/apache/airflow/blob/b541f5519c37c9b8b79fb2fe0e2b7105d0b12e6e/airflow/decorators/__init__.pyi#L203'>airflow/decorators/__init__.pyi:203:5:</a> E301 [*] Expected 1 blank line, found 0
... 11 additional changes omitted for rule E301
- <a href='https://github.com/apache/airflow/blob/b541f5519c37c9b8b79fb2fe0e2b7105d0b12e6e/airflow/decorators/__init__.pyi#L63'>airflow/decorators/__init__.pyi:63:1:</a> E302 [*] Expected 2 blank lines, found 1
- <a href='https://github.com/apache/airflow/blob/b541f5519c37c9b8b79fb2fe0e2b7105d0b12e6e/airflow/decorators/__init__.pyi#L723'>airflow/decorators/__init__.pyi:723:1:</a> E305 [*] Expected 2 blank lines after class or function definition, found (1)
- <a href='https://github.com/apache/airflow/blob/b541f5519c37c9b8b79fb2fe0e2b7105d0b12e6e/airflow/utils/context.pyi#L107'>airflow/utils/context.pyi:107:1:</a> E302 [*] Expected 2 blank lines, found 1
... 15 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -53 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/IPython/core/getipython.pyi#L3'>src/typings/IPython/core/getipython.pyi:3:1:</a> E302 [*] Expected 2 blank lines, found 1
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/IPython/core/history.pyi#L3'>src/typings/IPython/core/history.pyi:3:1:</a> E302 [*] Expected 2 blank lines, found 1
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/IPython/core/interactiveshell.pyi#L3'>src/typings/IPython/core/interactiveshell.pyi:3:1:</a> E302 [*] Expected 2 blank lines, found 1
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/IPython/display.pyi#L3'>src/typings/IPython/display.pyi:3:1:</a> E302 [*] Expected 2 blank lines, found 1
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/bs4.pyi#L12'>src/typings/bs4.pyi:12:1:</a> E302 [*] Expected 2 blank lines, found 1
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/bs4.pyi#L15'>src/typings/bs4.pyi:15:1:</a> E305 [*] Expected 2 blank lines after class or function definition, found (1)
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/bs4.pyi#L17'>src/typings/bs4.pyi:17:1:</a> E302 [*] Expected 2 blank lines, found 1
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/bs4.pyi#L8'>src/typings/bs4.pyi:8:1:</a> E302 [*] Expected 2 blank lines, found 1
... 46 additional changes omitted for rule E302
... 45 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/freedomofpress/securedrop/blob/6978cf8912f15bcd74105274c2d87f4db0eb6cdc/redwood/redwood/__init__.pyi#L6'>redwood/redwood/__init__.pyi:6:1:</a> E302 [*] Expected 2 blank lines, found 1
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+55 -55 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pandas-dev/pandas/blob/89b286a699b2d023b7a1ebc468abf230d84ad547/pandas/_libs/algos.pyi#L101'>pandas/_libs/algos.pyi:101:5:</a> PLR0917 Too many positional arguments (8/5)
+ <a href='https://github.com/pandas-dev/pandas/blob/89b286a699b2d023b7a1ebc468abf230d84ad547/pandas/_libs/algos.pyi#L108'>pandas/_libs/algos.pyi:108:5:</a> PLR0917 Too many positional arguments (7/5)
- <a href='https://github.com/pandas-dev/pandas/blob/89b286a699b2d023b7a1ebc468abf230d84ad547/pandas/_libs/algos.pyi#L111'>pandas/_libs/algos.pyi:111:5:</a> PLR0917 Too many positional arguments (7/5)
+ <a href='https://github.com/pandas-dev/pandas/blob/89b286a699b2d023b7a1ebc468abf230d84ad547/pandas/_libs/algos.pyi#L15'>pandas/_libs/algos.pyi:15:7:</a> PLW1641 Object does not implement `__hash__` method
- <a href='https://github.com/pandas-dev/pandas/blob/89b286a699b2d023b7a1ebc468abf230d84ad547/pandas/_libs/algos.pyi#L17'>pandas/_libs/algos.pyi:17:7:</a> PLW1641 Object does not implement `__hash__` method
+ <a href='https://github.com/pandas-dev/pandas/blob/89b286a699b2d023b7a1ebc468abf230d84ad547/pandas/_libs/algos.pyi#L7'>pandas/_libs/algos.pyi:7:7:</a> PLW1641 Object does not implement `__hash__` method
- <a href='https://github.com/pandas-dev/pandas/blob/89b286a699b2d023b7a1ebc468abf230d84ad547/pandas/_libs/algos.pyi#L8'>pandas/_libs/algos.pyi:8:7:</a> PLW1641 Object does not implement `__hash__` method
+ <a href='https://github.com/pandas-dev/pandas/blob/89b286a699b2d023b7a1ebc468abf230d84ad547/pandas/_libs/algos.pyi#L98'>pandas/_libs/algos.pyi:98:5:</a> PLR0917 Too many positional arguments (8/5)
- <a href='https://github.com/pandas-dev/pandas/blob/89b286a699b2d023b7a1ebc468abf230d84ad547/pandas/_libs/groupby.pyi#L100'>pandas/_libs/groupby.pyi:100:5:</a> PLR0917 Too many positional arguments (8/5)
+ <a href='https://github.com/pandas-dev/pandas/blob/89b286a699b2d023b7a1ebc468abf230d84ad547/pandas/_libs/groupby.pyi#L109'>pandas/_libs/groupby.pyi:109:5:</a> PLR0917 Too many positional arguments (7/5)
- <a href='https://github.com/pandas-dev/pandas/blob/89b286a699b2d023b7a1ebc468abf230d84ad547/pandas/_libs/groupby.pyi#L110'>pandas/_libs/groupby.pyi:110:5:</a> PLR0917 Too many positional arguments (7/5)
+ <a href='https://github.com/pandas-dev/pandas/blob/89b286a699b2d023b7a1ebc468abf230d84ad547/pandas/_libs/groupby.pyi#L118'>pandas/_libs/groupby.pyi:118:5:</a> PLR0917 Too many positional arguments (10/5)
- <a href='https://github.com/pandas-dev/pandas/blob/89b286a699b2d023b7a1ebc468abf230d84ad547/pandas/_libs/groupby.pyi#L119'>pandas/_libs/groupby.pyi:119:5:</a> PLR0917 Too many positional arguments (10/5)
+ <a href='https://github.com/pandas-dev/pandas/blob/89b286a699b2d023b7a1ebc468abf230d84ad547/pandas/_libs/groupby.pyi#L130'>pandas/_libs/groupby.pyi:130:5:</a> PLR0917 Too many positional arguments (9/5)
... 85 additional changes omitted for rule PLR0917
+ <a href='https://github.com/pandas-dev/pandas/blob/89b286a699b2d023b7a1ebc468abf230d84ad547/pandas/_libs/interval.pyi#L54'>pandas/_libs/interval.pyi:54:1:</a> PLR0904 Too many public methods (29 > 20)
- <a href='https://github.com/pandas-dev/pandas/blob/89b286a699b2d023b7a1ebc468abf230d84ad547/pandas/_libs/interval.pyi#L58'>pandas/_libs/interval.pyi:58:1:</a> PLR0904 Too many public methods (29 > 20)
+ <a href='https://github.com/pandas-dev/pandas/blob/89b286a699b2d023b7a1ebc468abf230d84ad547/pandas/_libs/tslibs/nattype.pyi#L34'>pandas/_libs/tslibs/nattype.pyi:34:1:</a> PLR0904 Too many public methods (63 > 20)
- <a href='https://github.com/pandas-dev/pandas/blob/89b286a699b2d023b7a1ebc468abf230d84ad547/pandas/_libs/tslibs/nattype.pyi#L36'>pandas/_libs/tslibs/nattype.pyi:36:1:</a> PLR0904 Too many public methods (63 > 20)
+ <a href='https://github.com/pandas-dev/pandas/blob/89b286a699b2d023b7a1ebc468abf230d84ad547/pandas/_libs/tslibs/offsets.pyi#L34'>pandas/_libs/tslibs/offsets.pyi:34:1:</a> PLR0904 Too many public methods (35 > 20)
- <a href='https://github.com/pandas-dev/pandas/blob/89b286a699b2d023b7a1ebc468abf230d84ad547/pandas/_libs/tslibs/offsets.pyi#L36'>pandas/_libs/tslibs/offsets.pyi:36:1:</a> PLR0904 Too many public methods (35 > 20)
+ <a href='https://github.com/pandas-dev/pandas/blob/89b286a699b2d023b7a1ebc468abf230d84ad547/pandas/_libs/tslibs/period.pyi#L68'>pandas/_libs/tslibs/period.pyi:68:1:</a> PLR0904 Too many public methods (24 > 20)
- <a href='https://github.com/pandas-dev/pandas/blob/89b286a699b2d023b7a1ebc468abf230d84ad547/pandas/_libs/tslibs/period.pyi#L72'>pandas/_libs/tslibs/period.pyi:72:1:</a> PLR0904 Too many public methods (24 > 20)
+ <a href='https://github.com/pandas-dev/pandas/blob/89b286a699b2d023b7a1ebc468abf230d84ad547/pandas/_libs/tslibs/timedeltas.pyi#L95'>pandas/_libs/tslibs/timedeltas.pyi:95:1:</a> PLR0904 Too many public methods (42 > 20)
- <a href='https://github.com/pandas-dev/pandas/blob/89b286a699b2d023b7a1ebc468abf230d84ad547/pandas/_libs/tslibs/timedeltas.pyi#L97'>pandas/_libs/tslibs/timedeltas.pyi:97:1:</a> PLR0904 Too many public methods (42 > 20)
... 3 additional changes omitted for rule PLR0904
... 86 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/678cfb6061be0a1298470ef983d6f0476e64522b/stubs/Crypto/Random/__init__.pyi#L11'>stubs/Crypto/Random/__init__.pyi:11:1:</a> E303 [*] Too many blank lines (2)
+ <a href='https://github.com/rotki/rotki/blob/678cfb6061be0a1298470ef983d6f0476e64522b/stubs/Crypto/Random/__init__.pyi#L5'>stubs/Crypto/Random/__init__.pyi:5:1:</a> E303 [*] Too many blank lines (2)
</pre>

</p>
</details>
<details><summary>Changes by rule (7 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR0917 | 94 | 47 | 47 | 0 | 0 |
| E302 | 65 | 0 | 65 | 0 | 0 |
| E301 | 16 | 0 | 16 | 0 | 0 |
| PLR0904 | 12 | 6 | 6 | 0 | 0 |
| PLW1641 | 4 | 2 | 2 | 0 | 0 |
| E303 | 2 | 2 | 0 | 0 | 0 |
| E305 | 2 | 0 | 2 | 0 | 0 |

</p>
</details>




---

_@snowsignal approved on 2024-02-23 16:47_

---

_Review requested from @charliermarsh by @MichaReiser on 2024-02-26 15:37_

---

_Renamed from "E3: Adjust expected blank lines for typing stub files" to "[`pycodestyle`]: Make blank lines in typing stub files optional (`E3*`)" by @MichaReiser on 2024-03-01 16:25_

---

_Merged by @MichaReiser on 2024-03-05 11:48_

---

_Closed by @MichaReiser on 2024-03-05 11:48_

---

_Branch deleted on 2024-03-05 11:48_

---
