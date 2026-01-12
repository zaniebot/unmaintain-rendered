```yaml
number: 10117
title: Fix E301 not triggering on decorated methods.
type: pull_request
state: merged
author: hoel-bagard
labels:
  - bug
  - rule
  - preview
assignees: []
merged: true
base: main
head: hoel/fix-blank-lines-classmethods
created_at: 2024-02-25T10:47:05Z
updated_at: 2024-03-01T09:37:09Z
url: https://github.com/astral-sh/ruff/pull/10117
synced_at: 2026-01-12T15:55:31Z
```

# Fix E301 not triggering on decorated methods.

---

_@hoel-bagard_

Stacked on top of  #10098.

## Summary

This PR fixes  #10114.

## Test Plan

The example given in the issue has been added to the fixtures, and the results compared with pycodestyle.

---

_Marked ready for review by @hoel-bagard on 2024-02-25 11:01_

---

_Comment by @github-actions[bot] on 2024-02-25 11:04_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+14 -83 violations, +0 -0 fixes in 4 projects; 39 projects unchanged)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/RasaHQ/rasa/blob/68ce281aeec352876afb2baf74844e95b0c69ff4/stubs/sanic.pyi#L25'>stubs/sanic.pyi:25:5:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/RasaHQ/rasa/blob/68ce281aeec352876afb2baf74844e95b0c69ff4/stubs/sanic.pyi#L30'>stubs/sanic.pyi:30:5:</a> E301 [*] Expected 1 blank line, found 0
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/9dac90a42e0ceadd09f6d21fa9810d40a5dbc395/airflow/utils/context.pyi#L53'>airflow/utils/context.pyi:53:5:</a> E301 [*] Expected 1 blank line, found 0
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+2 -68 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/compiler.py#L127'>src/bokeh/util/compiler.py:127:5:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/selenium/webdriver/common/action_chains.pyi#L10'>src/typings/selenium/webdriver/common/action_chains.pyi:10:5:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/selenium/webdriver/common/action_chains.pyi#L11'>src/typings/selenium/webdriver/common/action_chains.pyi:11:5:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/selenium/webdriver/common/action_chains.pyi#L12'>src/typings/selenium/webdriver/common/action_chains.pyi:12:5:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/selenium/webdriver/common/action_chains.pyi#L13'>src/typings/selenium/webdriver/common/action_chains.pyi:13:5:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/selenium/webdriver/common/action_chains.pyi#L14'>src/typings/selenium/webdriver/common/action_chains.pyi:14:5:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/selenium/webdriver/common/action_chains.pyi#L15'>src/typings/selenium/webdriver/common/action_chains.pyi:15:5:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/selenium/webdriver/common/action_chains.pyi#L16'>src/typings/selenium/webdriver/common/action_chains.pyi:16:5:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/selenium/webdriver/common/action_chains.pyi#L17'>src/typings/selenium/webdriver/common/action_chains.pyi:17:5:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/selenium/webdriver/common/action_chains.pyi#L18'>src/typings/selenium/webdriver/common/action_chains.pyi:18:5:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/selenium/webdriver/common/action_chains.pyi#L19'>src/typings/selenium/webdriver/common/action_chains.pyi:19:5:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/selenium/webdriver/common/action_chains.pyi#L20'>src/typings/selenium/webdriver/common/action_chains.pyi:20:5:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/selenium/webdriver/common/action_chains.pyi#L21'>src/typings/selenium/webdriver/common/action_chains.pyi:21:5:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/selenium/webdriver/common/action_chains.pyi#L22'>src/typings/selenium/webdriver/common/action_chains.pyi:22:5:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/selenium/webdriver/common/action_chains.pyi#L23'>src/typings/selenium/webdriver/common/action_chains.pyi:23:5:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/selenium/webdriver/common/action_chains.pyi#L24'>src/typings/selenium/webdriver/common/action_chains.pyi:24:5:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/selenium/webdriver/common/action_chains.pyi#L25'>src/typings/selenium/webdriver/common/action_chains.pyi:25:5:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/selenium/webdriver/common/action_chains.pyi#L26'>src/typings/selenium/webdriver/common/action_chains.pyi:26:5:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/selenium/webdriver/firefox/options.pyi#L3'>src/typings/selenium/webdriver/firefox/options.pyi:3:5:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/selenium/webdriver/remote/webdriver.pyi#L35'>src/typings/selenium/webdriver/remote/webdriver.pyi:35:5:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/selenium/webdriver/remote/webelement.pyi#L27'>src/typings/selenium/webdriver/remote/webelement.pyi:27:5:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/selenium/webdriver/remote/webelement.pyi#L28'>src/typings/selenium/webdriver/remote/webelement.pyi:28:5:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/selenium/webdriver/remote/webelement.pyi#L29'>src/typings/selenium/webdriver/remote/webelement.pyi:29:5:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/selenium/webdriver/remote/webelement.pyi#L30'>src/typings/selenium/webdriver/remote/webelement.pyi:30:5:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/selenium/webdriver/remote/webelement.pyi#L31'>src/typings/selenium/webdriver/remote/webelement.pyi:31:5:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/selenium/webdriver/remote/webelement.pyi#L32'>src/typings/selenium/webdriver/remote/webelement.pyi:32:5:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/selenium/webdriver/remote/webelement.pyi#L33'>src/typings/selenium/webdriver/remote/webelement.pyi:33:5:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/selenium/webdriver/remote/webelement.pyi#L34'>src/typings/selenium/webdriver/remote/webelement.pyi:34:5:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/selenium/webdriver/remote/webelement.pyi#L35'>src/typings/selenium/webdriver/remote/webelement.pyi:35:5:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/selenium/webdriver/remote/webelement.pyi#L36'>src/typings/selenium/webdriver/remote/webelement.pyi:36:5:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/selenium/webdriver/remote/webelement.pyi#L37'>src/typings/selenium/webdriver/remote/webelement.pyi:37:5:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/selenium/webdriver/remote/webelement.pyi#L42'>src/typings/selenium/webdriver/remote/webelement.pyi:42:5:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/selenium/webdriver/remote/webelement.pyi#L51'>src/typings/selenium/webdriver/remote/webelement.pyi:51:5:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/selenium/webdriver/support/expected_conditions.pyi#L100'>src/typings/selenium/webdriver/support/expected_conditions.pyi:100:5:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/selenium/webdriver/support/expected_conditions.pyi#L104'>src/typings/selenium/webdriver/support/expected_conditions.pyi:104:5:</a> E301 [*] Expected 1 blank line, found 0
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/selenium/webdriver/support/expected_conditions.pyi#L108'>src/typings/selenium/webdriver/support/expected_conditions.pyi:108:5:</a> E301 [*] Expected 1 blank line, found 0
... 34 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+12 -12 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/301c5c719bc13c20d44b488d987a05fa5fd605a0/pandas/_libs/algos.pyi#L100'>pandas/_libs/algos.pyi:100:5:</a> PLR0917 Too many positional arguments (8/5)
+ <a href='https://github.com/pandas-dev/pandas/blob/301c5c719bc13c20d44b488d987a05fa5fd605a0/pandas/_libs/algos.pyi#L110'>pandas/_libs/algos.pyi:110:5:</a> PLR0917 Too many positional arguments (7/5)
- <a href='https://github.com/pandas-dev/pandas/blob/301c5c719bc13c20d44b488d987a05fa5fd605a0/pandas/_libs/algos.pyi#L110'>pandas/_libs/algos.pyi:110:5:</a> PLR0917 Too many positional arguments (8/5)
- <a href='https://github.com/pandas-dev/pandas/blob/301c5c719bc13c20d44b488d987a05fa5fd605a0/pandas/_libs/algos.pyi#L120'>pandas/_libs/algos.pyi:120:5:</a> PLR0917 Too many positional arguments (7/5)
+ <a href='https://github.com/pandas-dev/pandas/blob/301c5c719bc13c20d44b488d987a05fa5fd605a0/pandas/_libs/algos.pyi#L17'>pandas/_libs/algos.pyi:17:7:</a> PLW1641 Object does not implement `__hash__` method
- <a href='https://github.com/pandas-dev/pandas/blob/301c5c719bc13c20d44b488d987a05fa5fd605a0/pandas/_libs/algos.pyi#L22'>pandas/_libs/algos.pyi:22:7:</a> PLW1641 Object does not implement `__hash__` method
+ <a href='https://github.com/pandas-dev/pandas/blob/301c5c719bc13c20d44b488d987a05fa5fd605a0/pandas/_libs/hashtable.pyi#L206'>pandas/_libs/hashtable.pyi:206:9:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/pandas-dev/pandas/blob/301c5c719bc13c20d44b488d987a05fa5fd605a0/pandas/_libs/hashtable.pyi#L247'>pandas/_libs/hashtable.pyi:247:9:</a> PLR0917 Too many positional arguments (6/5)
... 13 additional changes omitted for rule PLR0917
+ <a href='https://github.com/pandas-dev/pandas/blob/301c5c719bc13c20d44b488d987a05fa5fd605a0/pandas/_libs/interval.pyi#L58'>pandas/_libs/interval.pyi:58:1:</a> PLR0904 Too many public methods (29 > 20)
- <a href='https://github.com/pandas-dev/pandas/blob/301c5c719bc13c20d44b488d987a05fa5fd605a0/pandas/_libs/interval.pyi#L59'>pandas/_libs/interval.pyi:59:1:</a> PLR0904 Too many public methods (29 > 20)
+ <a href='https://github.com/pandas-dev/pandas/blob/301c5c719bc13c20d44b488d987a05fa5fd605a0/pandas/_libs/tslibs/period.pyi#L72'>pandas/_libs/tslibs/period.pyi:72:1:</a> PLR0904 Too many public methods (24 > 20)
- <a href='https://github.com/pandas-dev/pandas/blob/301c5c719bc13c20d44b488d987a05fa5fd605a0/pandas/_libs/tslibs/period.pyi#L73'>pandas/_libs/tslibs/period.pyi:73:1:</a> PLR0904 Too many public methods (24 > 20)
... 12 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (4 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E301 | 73 | 2 | 71 | 0 | 0 |
| PLR0917 | 18 | 9 | 9 | 0 | 0 |
| PLR0904 | 4 | 2 | 2 | 0 | 0 |
| PLW1641 | 2 | 1 | 1 | 0 | 0 |

</p>
</details>




---

_Label `bug` added by @MichaReiser on 2024-02-26 08:54_

---

_Label `rule` added by @MichaReiser on 2024-02-26 08:54_

---

_Label `preview` added by @MichaReiser on 2024-02-26 08:54_

---

_@MichaReiser approved on 2024-02-26 09:15_

That was quick! Thank you

---

_Comment by @hoel-bagard on 2024-02-27 02:04_

@MichaReiser Since it was a really small fix, should I rebase it on main so that the fix can be merged quicker ?

---

_Comment by @MichaReiser on 2024-02-27 08:19_

@hoel-bagard yeah that might be worth it. I thought the other PR's are quick wins but that's not the case :(

---

_Comment by @hoel-bagard on 2024-02-27 08:23_

@MichaReiser Ok, I'll do that.

Is there a way I can help with the other PR ?

---

_Comment by @MichaReiser on 2024-02-27 08:36_

That's very kind of you! The main struggle is finding a good way to make `E301` and `I001` compatible. See https://github.com/astral-sh/ruff/pull/10096 and @charliermarsh's comment

---

_Review requested from @MichaReiser by @hoel-bagard on 2024-02-27 09:45_

---

_Closed by @MichaReiser on 2024-02-27 12:19_

---

_Reopened by @MichaReiser on 2024-02-27 12:19_

---

_@MichaReiser approved on 2024-03-01 09:24_

---

_Merged by @MichaReiser on 2024-03-01 09:30_

---

_Closed by @MichaReiser on 2024-03-01 09:30_

---
