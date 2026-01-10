```yaml
number: 15231
title: Misc. clean up to rounding rules
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/random
created_at: 2025-01-02T22:45:47Z
updated_at: 2025-01-02T22:51:42Z
url: https://github.com/astral-sh/ruff/pull/15231
synced_at: 2026-01-10T20:42:27Z
```

# Misc. clean up to rounding rules

---

_Pull request opened by @charliermarsh on 2025-01-02 22:45_

_No description provided._

---

_Label `internal` added by @charliermarsh on 2025-01-02 22:45_

---

_Merged by @charliermarsh on 2025-01-02 22:51_

---

_Closed by @charliermarsh on 2025-01-02 22:51_

---

_Branch deleted on 2025-01-02 22:51_

---

_Comment by @github-actions[bot] on 2025-01-02 22:51_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+15 -15 violations, +0 -0 fixes in 5 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/shared/core/training_data/loading.py#L86'>rasa/shared/core/training_data/loading.py:86:15:</a> RUF046 Value being cast to `int` is already an integer
- <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/shared/core/training_data/loading.py#L86'>rasa/shared/core/training_data/loading.py:86:15:</a> RUF046 Value being casted is already an integer
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/7c903236498281f34970012e701250edef1b1493/superset/migrations/versions/2018-07-22_11-59_bebcf3fed1fe_convert_dashboard_v1_positions.py#L197'>superset/migrations/versions/2018-07-22_11-59_bebcf3fed1fe_convert_dashboard_v1_positions.py:197:40:</a> RUF046 Value being cast to `int` is already an integer
- <a href='https://github.com/apache/superset/blob/7c903236498281f34970012e701250edef1b1493/superset/migrations/versions/2018-07-22_11-59_bebcf3fed1fe_convert_dashboard_v1_positions.py#L197'>superset/migrations/versions/2018-07-22_11-59_bebcf3fed1fe_convert_dashboard_v1_positions.py:197:40:</a> RUF046 Value being casted is already an integer
+ <a href='https://github.com/apache/superset/blob/7c903236498281f34970012e701250edef1b1493/superset/migrations/versions/2018-07-22_11-59_bebcf3fed1fe_convert_dashboard_v1_positions.py#L199'>superset/migrations/versions/2018-07-22_11-59_bebcf3fed1fe_convert_dashboard_v1_positions.py:199:29:</a> RUF046 Value being cast to `int` is already an integer
- <a href='https://github.com/apache/superset/blob/7c903236498281f34970012e701250edef1b1493/superset/migrations/versions/2018-07-22_11-59_bebcf3fed1fe_convert_dashboard_v1_positions.py#L199'>superset/migrations/versions/2018-07-22_11-59_bebcf3fed1fe_convert_dashboard_v1_positions.py:199:29:</a> RUF046 Value being casted is already an integer
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+5 -5 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/spectrogram/main.py#L107'>examples/server/app/spectrogram/main.py:107:13:</a> RUF046 Value being cast to `int` is already an integer
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/spectrogram/main.py#L107'>examples/server/app/spectrogram/main.py:107:13:</a> RUF046 Value being casted is already an integer
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L338'>src/bokeh/colors/color.py:338:60:</a> RUF046 Value being cast to `int` is already an integer
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L338'>src/bokeh/colors/color.py:338:60:</a> RUF046 Value being casted is already an integer
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/palettes.py#L1530'>src/bokeh/palettes.py:1530:27:</a> RUF046 Value being cast to `int` is already an integer
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/palettes.py#L1530'>src/bokeh/palettes.py:1530:27:</a> RUF046 Value being casted is already an integer
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/palettes.py#L1568'>src/bokeh/palettes.py:1568:10:</a> RUF046 Value being cast to `int` is already an integer
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/palettes.py#L1568'>src/bokeh/palettes.py:1568:10:</a> RUF046 Value being casted is already an integer
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/palettes.py#L1569'>src/bokeh/palettes.py:1569:10:</a> RUF046 Value being cast to `int` is already an integer
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/palettes.py#L1569'>src/bokeh/palettes.py:1569:10:</a> RUF046 Value being casted is already an integer
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/d6030ffeef5610f176efca6e9f6fc907de90677b/rotkehlchen/tests/utils/ethereum.py#L231'>rotkehlchen/tests/utils/ethereum.py:231:15:</a> RUF046 [*] Value being cast to `int` is already an integer
- <a href='https://github.com/rotki/rotki/blob/d6030ffeef5610f176efca6e9f6fc907de90677b/rotkehlchen/tests/utils/ethereum.py#L231'>rotkehlchen/tests/utils/ethereum.py:231:15:</a> RUF046 [*] Value being casted is already an integer
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+6 -6 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/80aa67d2cc7ddb41a63d00d231b2792aafca77fa/astropy/io/fits/hdu/compressed/tests/test_compressed.py#L375'>astropy/io/fits/hdu/compressed/tests/test_compressed.py:375:26:</a> RUF046 Value being cast to `int` is already an integer
- <a href='https://github.com/astropy/astropy/blob/80aa67d2cc7ddb41a63d00d231b2792aafca77fa/astropy/io/fits/hdu/compressed/tests/test_compressed.py#L375'>astropy/io/fits/hdu/compressed/tests/test_compressed.py:375:26:</a> RUF046 Value being casted is already an integer
+ <a href='https://github.com/astropy/astropy/blob/80aa67d2cc7ddb41a63d00d231b2792aafca77fa/astropy/io/fits/hdu/compressed/utils.py#L102'>astropy/io/fits/hdu/compressed/utils.py:102:13:</a> RUF046 Value being cast to `int` is already an integer
- <a href='https://github.com/astropy/astropy/blob/80aa67d2cc7ddb41a63d00d231b2792aafca77fa/astropy/io/fits/hdu/compressed/utils.py#L102'>astropy/io/fits/hdu/compressed/utils.py:102:13:</a> RUF046 Value being casted is already an integer
+ <a href='https://github.com/astropy/astropy/blob/80aa67d2cc7ddb41a63d00d231b2792aafca77fa/astropy/io/fits/tests/test_image.py#L1003'>astropy/io/fits/tests/test_image.py:1003:26:</a> RUF046 Value being cast to `int` is already an integer
- <a href='https://github.com/astropy/astropy/blob/80aa67d2cc7ddb41a63d00d231b2792aafca77fa/astropy/io/fits/tests/test_image.py#L1003'>astropy/io/fits/tests/test_image.py:1003:26:</a> RUF046 Value being casted is already an integer
+ <a href='https://github.com/astropy/astropy/blob/80aa67d2cc7ddb41a63d00d231b2792aafca77fa/astropy/io/votable/validator/html.py#L246'>astropy/io/votable/validator/html.py:246:14:</a> RUF046 Value being cast to `int` is already an integer
- <a href='https://github.com/astropy/astropy/blob/80aa67d2cc7ddb41a63d00d231b2792aafca77fa/astropy/io/votable/validator/html.py#L246'>astropy/io/votable/validator/html.py:246:14:</a> RUF046 Value being casted is already an integer
+ <a href='https://github.com/astropy/astropy/blob/80aa67d2cc7ddb41a63d00d231b2792aafca77fa/astropy/modeling/_fitting_parallel.py#L194'>astropy/modeling/_fitting_parallel.py:194:22:</a> RUF046 Value being cast to `int` is already an integer
- <a href='https://github.com/astropy/astropy/blob/80aa67d2cc7ddb41a63d00d231b2792aafca77fa/astropy/modeling/_fitting_parallel.py#L194'>astropy/modeling/_fitting_parallel.py:194:22:</a> RUF046 Value being casted is already an integer
+ <a href='https://github.com/astropy/astropy/blob/80aa67d2cc7ddb41a63d00d231b2792aafca77fa/astropy/utils/console.py#L365'>astropy/utils/console.py:365:21:</a> RUF046 Value being cast to `int` is already an integer
- <a href='https://github.com/astropy/astropy/blob/80aa67d2cc7ddb41a63d00d231b2792aafca77fa/astropy/utils/console.py#L365'>astropy/utils/console.py:365:21:</a> RUF046 Value being casted is already an integer
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF046 | 30 | 15 | 15 | 0 | 0 |

</p>
</details>




---
