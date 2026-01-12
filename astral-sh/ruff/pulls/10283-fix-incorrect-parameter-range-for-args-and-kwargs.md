```yaml
number: 10283
title: "Fix incorrect `Parameter` range  for `*args` and `**kwargs` "
type: pull_request
state: merged
author: GtrMo
labels:
  - bug
  - parser
assignees: []
merged: true
base: main
head: fix_starred_parameter_parsing
created_at: 2024-03-07T23:57:31Z
updated_at: 2024-03-08T23:57:53Z
url: https://github.com/astral-sh/ruff/pull/10283
synced_at: 2026-01-12T15:55:31Z
```

# Fix incorrect `Parameter` range  for `*args` and `**kwargs` 

---

_@GtrMo_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fix #10282 
<!-- What's the purpose of the change? What does it do, and why? -->

This PR updates the Python grammar to include the `*` character in `*args` `**kwargs` in the range of the `Parameter`
```
def f(*args, **kwargs): pass
#      ~~~~    ~~~~~~    <-- range before the PR
#     ^^^^^  ^^^^^^^^    <-- range after
```

The invalid syntax `def f(*, **kwargs): ...` is also now correctly reported.

## Test Plan

Test cases were added to `function.rs`.


---

_Review requested from @MichaReiser by @GtrMo on 2024-03-07 23:57_

---

_Review comment by @GtrMo on `crates/ruff_python_parser/src/python.rs`:1 on 2024-03-07 23:58_

Is the diff supposed to be that big? I used the same version of `lalrpop` (0.20.0) to update this file

---

_@GtrMo reviewed on 2024-03-07 23:58_

---

_Label `parser` added by @MichaReiser on 2024-03-08 08:56_

---

_@MichaReiser reviewed on 2024-03-08 09:05_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/python.rs`:1 on 2024-03-08 09:05_

Yeah, lalrpop loves to generate large diffs :(

---

_Comment by @MichaReiser on 2024-03-08 09:20_

Changing the ranges drips over the formatter. There are multiple places where the formatter goes back to introspecting the source to identify the comment placement and I fear that the code has now been written in a way that assumes the incorrect `*arg` and `**kwargs` ranges. 

I've been able to identify a problematic example:

```python
(
    lambda # comment 1
    * # comment 2
    x: # comment 3
    x
)
```

But I haven't yet been successful to narrow it down where the range handling is happening. I suspect that it is related to https://github.com/astral-sh/ruff/issues/10281 (CC: @dhruvmanila) but haven't been able to confirm it yet.

I suspect that the relevant code is 

https://github.com/astral-sh/ruff/blob/0293908b71eeaf610d8ce77d2950f0602fd88dc5/crates/ruff_python_formatter/src/other/parameters.rs#L357-L644

---

_@MichaReiser approved on 2024-03-08 13:35_

I pushed a fix for the formatter. 

Looks good to me. That's a non-trivial parser change. Well done!

Would you mind rebasing and regenerating the parser (after rebase)?

---

_Comment by @charliermarsh on 2024-03-08 19:10_

I can rebase, regen, and merge this later.

---

_Review requested from @dhruvmanila by @MichaReiser on 2024-03-08 19:26_

---

_Assigned to @MichaReiser by @MichaReiser on 2024-03-08 19:27_

---

_Assigned to @dhruvmanila by @MichaReiser on 2024-03-08 19:27_

---

_Unassigned @MichaReiser by @MichaReiser on 2024-03-08 19:27_

---

_Comment by @GtrMo on 2024-03-08 19:28_

Thanks for the formatter fix! I rebased and regenerated the parser

---

_Comment by @github-actions[bot] on 2024-03-08 19:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+3218 -3218 violations, +0 -0 fixes in 2 projects; 41 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2275 -2275 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api/auth/backend/default.py#L38'>airflow/api/auth/backend/default.py:38:19:</a> ANN002 Missing type annotation for `*args`
- <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api/auth/backend/default.py#L38'>airflow/api/auth/backend/default.py:38:20:</a> ANN002 Missing type annotation for `*args`
+ <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api/auth/backend/default.py#L38'>airflow/api/auth/backend/default.py:38:26:</a> ANN003 Missing type annotation for `**kwargs`
- <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api/auth/backend/default.py#L38'>airflow/api/auth/backend/default.py:38:28:</a> ANN003 Missing type annotation for `**kwargs`
+ <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api/auth/backend/deny_all.py#L40'>airflow/api/auth/backend/deny_all.py:40:19:</a> ANN002 Missing type annotation for `*args`
- <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api/auth/backend/deny_all.py#L40'>airflow/api/auth/backend/deny_all.py:40:20:</a> ANN002 Missing type annotation for `*args`
+ <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api/auth/backend/deny_all.py#L40'>airflow/api/auth/backend/deny_all.py:40:26:</a> ANN003 Missing type annotation for `**kwargs`
- <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api/auth/backend/deny_all.py#L40'>airflow/api/auth/backend/deny_all.py:40:28:</a> ANN003 Missing type annotation for `**kwargs`
+ <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api/auth/backend/kerberos_auth.py#L153'>airflow/api/auth/backend/kerberos_auth.py:153:19:</a> ANN002 Missing type annotation for `*args`
- <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api/auth/backend/kerberos_auth.py#L153'>airflow/api/auth/backend/kerberos_auth.py:153:20:</a> ANN002 Missing type annotation for `*args`
+ <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api/auth/backend/kerberos_auth.py#L153'>airflow/api/auth/backend/kerberos_auth.py:153:26:</a> ANN003 Missing type annotation for `**kwargs`
- <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api/auth/backend/kerberos_auth.py#L153'>airflow/api/auth/backend/kerberos_auth.py:153:28:</a> ANN003 Missing type annotation for `**kwargs`
+ <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api/auth/backend/session.py#L41'>airflow/api/auth/backend/session.py:41:19:</a> ANN002 Missing type annotation for `*args`
- <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api/auth/backend/session.py#L41'>airflow/api/auth/backend/session.py:41:20:</a> ANN002 Missing type annotation for `*args`
+ <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api/auth/backend/session.py#L41'>airflow/api/auth/backend/session.py:41:26:</a> ANN003 Missing type annotation for `**kwargs`
- <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api/auth/backend/session.py#L41'>airflow/api/auth/backend/session.py:41:28:</a> ANN003 Missing type annotation for `**kwargs`
+ <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api_connexion/endpoints/forward_to_fab_endpoint.py#L102'>airflow/api_connexion/endpoints/forward_to_fab_endpoint.py:102:14:</a> ANN003 Missing type annotation for `**kwargs`
- <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api_connexion/endpoints/forward_to_fab_endpoint.py#L102'>airflow/api_connexion/endpoints/forward_to_fab_endpoint.py:102:16:</a> ANN003 Missing type annotation for `**kwargs`
+ <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api_connexion/endpoints/forward_to_fab_endpoint.py#L108'>airflow/api_connexion/endpoints/forward_to_fab_endpoint.py:108:15:</a> ANN003 Missing type annotation for `**kwargs`
- <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api_connexion/endpoints/forward_to_fab_endpoint.py#L108'>airflow/api_connexion/endpoints/forward_to_fab_endpoint.py:108:17:</a> ANN003 Missing type annotation for `**kwargs`
+ <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api_connexion/endpoints/forward_to_fab_endpoint.py#L114'>airflow/api_connexion/endpoints/forward_to_fab_endpoint.py:114:15:</a> ANN003 Missing type annotation for `**kwargs`
- <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api_connexion/endpoints/forward_to_fab_endpoint.py#L114'>airflow/api_connexion/endpoints/forward_to_fab_endpoint.py:114:17:</a> ANN003 Missing type annotation for `**kwargs`
+ <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api_connexion/endpoints/forward_to_fab_endpoint.py#L121'>airflow/api_connexion/endpoints/forward_to_fab_endpoint.py:121:16:</a> ANN003 Missing type annotation for `**kwargs`
- <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api_connexion/endpoints/forward_to_fab_endpoint.py#L121'>airflow/api_connexion/endpoints/forward_to_fab_endpoint.py:121:18:</a> ANN003 Missing type annotation for `**kwargs`
+ <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api_connexion/endpoints/forward_to_fab_endpoint.py#L128'>airflow/api_connexion/endpoints/forward_to_fab_endpoint.py:128:17:</a> ANN003 Missing type annotation for `**kwargs`
- <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api_connexion/endpoints/forward_to_fab_endpoint.py#L128'>airflow/api_connexion/endpoints/forward_to_fab_endpoint.py:128:19:</a> ANN003 Missing type annotation for `**kwargs`
... 3595 additional changes omitted for rule ANN003
+ <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api_connexion/endpoints/forward_to_fab_endpoint.py#L39'>airflow/api_connexion/endpoints/forward_to_fab_endpoint.py:39:15:</a> ANN002 Missing type annotation for `*args`
- <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api_connexion/endpoints/forward_to_fab_endpoint.py#L39'>airflow/api_connexion/endpoints/forward_to_fab_endpoint.py:39:16:</a> ANN002 Missing type annotation for `*args`
+ <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api_connexion/parameters.py#L100'>airflow/api_connexion/parameters.py:100:30:</a> ANN002 Missing type annotation for `*args`
- <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api_connexion/parameters.py#L100'>airflow/api_connexion/parameters.py:100:31:</a> ANN002 Missing type annotation for `*args`
+ <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api_connexion/security.py#L118'>airflow/api_connexion/security.py:118:23:</a> ANN002 Missing type annotation for `*args`
- <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api_connexion/security.py#L118'>airflow/api_connexion/security.py:118:24:</a> ANN002 Missing type annotation for `*args`
+ <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api_connexion/security.py#L163'>airflow/api_connexion/security.py:163:23:</a> ANN002 Missing type annotation for `*args`
- <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api_connexion/security.py#L163'>airflow/api_connexion/security.py:163:24:</a> ANN002 Missing type annotation for `*args`
+ <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api_connexion/security.py#L180'>airflow/api_connexion/security.py:180:23:</a> ANN002 Missing type annotation for `*args`
... 4515 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+943 -943 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/ajax_source.py#L34'>examples/basic/data/ajax_source.py:34:26:</a> ANN002 Missing type annotation for `*args`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/ajax_source.py#L34'>examples/basic/data/ajax_source.py:34:27:</a> ANN002 Missing type annotation for `*args`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/ajax_source.py#L34'>examples/basic/data/ajax_source.py:34:33:</a> ANN003 Missing type annotation for `**kwargs`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/ajax_source.py#L34'>examples/basic/data/ajax_source.py:34:35:</a> ANN003 Missing type annotation for `**kwargs`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/server_sent_events_source.py#L36'>examples/basic/data/server_sent_events_source.py:36:26:</a> ANN002 Missing type annotation for `*args`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/server_sent_events_source.py#L36'>examples/basic/data/server_sent_events_source.py:36:27:</a> ANN002 Missing type annotation for `*args`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/server_sent_events_source.py#L36'>examples/basic/data/server_sent_events_source.py:36:33:</a> ANN003 Missing type annotation for `**kwargs`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/server_sent_events_source.py#L36'>examples/basic/data/server_sent_events_source.py:36:35:</a> ANN003 Missing type annotation for `**kwargs`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/graphs.py#L22'>examples/models/graphs.py:22:78:</a> ANN003 Missing type annotation for `**kwargs`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/graphs.py#L22'>examples/models/graphs.py:22:80:</a> ANN003 Missing type annotation for `**kwargs`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/struct.py#L58'>src/bokeh/core/property/struct.py:58:24:</a> ANN003 Missing type annotation for `**fields`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/struct.py#L58'>src/bokeh/core/property/struct.py:58:26:</a> ANN003 Missing type annotation for `**fields`
... 971 additional changes omitted for rule ANN003
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/validation.py#L93'>src/bokeh/core/property/validation.py:93:14:</a> ANN002 Missing type annotation for `*args`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/validation.py#L93'>src/bokeh/core/property/validation.py:93:15:</a> ANN002 Missing type annotation for `*args`
... 1872 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| ANN003 | 4590 | 2295 | 2295 | 0 | 0 |
| ANN002 | 1846 | 923 | 923 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+3218 -3218 violations, +0 -0 fixes in 2 projects; 41 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2275 -2275 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api/auth/backend/default.py#L38'>airflow/api/auth/backend/default.py:38:19:</a> ANN002 Missing type annotation for `*args`
- <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api/auth/backend/default.py#L38'>airflow/api/auth/backend/default.py:38:20:</a> ANN002 Missing type annotation for `*args`
+ <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api/auth/backend/default.py#L38'>airflow/api/auth/backend/default.py:38:26:</a> ANN003 Missing type annotation for `**kwargs`
- <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api/auth/backend/default.py#L38'>airflow/api/auth/backend/default.py:38:28:</a> ANN003 Missing type annotation for `**kwargs`
+ <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api/auth/backend/deny_all.py#L40'>airflow/api/auth/backend/deny_all.py:40:19:</a> ANN002 Missing type annotation for `*args`
- <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api/auth/backend/deny_all.py#L40'>airflow/api/auth/backend/deny_all.py:40:20:</a> ANN002 Missing type annotation for `*args`
+ <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api/auth/backend/deny_all.py#L40'>airflow/api/auth/backend/deny_all.py:40:26:</a> ANN003 Missing type annotation for `**kwargs`
- <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api/auth/backend/deny_all.py#L40'>airflow/api/auth/backend/deny_all.py:40:28:</a> ANN003 Missing type annotation for `**kwargs`
+ <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api/auth/backend/kerberos_auth.py#L153'>airflow/api/auth/backend/kerberos_auth.py:153:19:</a> ANN002 Missing type annotation for `*args`
- <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api/auth/backend/kerberos_auth.py#L153'>airflow/api/auth/backend/kerberos_auth.py:153:20:</a> ANN002 Missing type annotation for `*args`
+ <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api/auth/backend/kerberos_auth.py#L153'>airflow/api/auth/backend/kerberos_auth.py:153:26:</a> ANN003 Missing type annotation for `**kwargs`
- <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api/auth/backend/kerberos_auth.py#L153'>airflow/api/auth/backend/kerberos_auth.py:153:28:</a> ANN003 Missing type annotation for `**kwargs`
+ <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api/auth/backend/session.py#L41'>airflow/api/auth/backend/session.py:41:19:</a> ANN002 Missing type annotation for `*args`
- <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api/auth/backend/session.py#L41'>airflow/api/auth/backend/session.py:41:20:</a> ANN002 Missing type annotation for `*args`
+ <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api/auth/backend/session.py#L41'>airflow/api/auth/backend/session.py:41:26:</a> ANN003 Missing type annotation for `**kwargs`
- <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api/auth/backend/session.py#L41'>airflow/api/auth/backend/session.py:41:28:</a> ANN003 Missing type annotation for `**kwargs`
+ <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api_connexion/endpoints/forward_to_fab_endpoint.py#L102'>airflow/api_connexion/endpoints/forward_to_fab_endpoint.py:102:14:</a> ANN003 Missing type annotation for `**kwargs`
- <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api_connexion/endpoints/forward_to_fab_endpoint.py#L102'>airflow/api_connexion/endpoints/forward_to_fab_endpoint.py:102:16:</a> ANN003 Missing type annotation for `**kwargs`
+ <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api_connexion/endpoints/forward_to_fab_endpoint.py#L108'>airflow/api_connexion/endpoints/forward_to_fab_endpoint.py:108:15:</a> ANN003 Missing type annotation for `**kwargs`
- <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api_connexion/endpoints/forward_to_fab_endpoint.py#L108'>airflow/api_connexion/endpoints/forward_to_fab_endpoint.py:108:17:</a> ANN003 Missing type annotation for `**kwargs`
+ <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api_connexion/endpoints/forward_to_fab_endpoint.py#L114'>airflow/api_connexion/endpoints/forward_to_fab_endpoint.py:114:15:</a> ANN003 Missing type annotation for `**kwargs`
- <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api_connexion/endpoints/forward_to_fab_endpoint.py#L114'>airflow/api_connexion/endpoints/forward_to_fab_endpoint.py:114:17:</a> ANN003 Missing type annotation for `**kwargs`
+ <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api_connexion/endpoints/forward_to_fab_endpoint.py#L121'>airflow/api_connexion/endpoints/forward_to_fab_endpoint.py:121:16:</a> ANN003 Missing type annotation for `**kwargs`
- <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api_connexion/endpoints/forward_to_fab_endpoint.py#L121'>airflow/api_connexion/endpoints/forward_to_fab_endpoint.py:121:18:</a> ANN003 Missing type annotation for `**kwargs`
+ <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api_connexion/endpoints/forward_to_fab_endpoint.py#L128'>airflow/api_connexion/endpoints/forward_to_fab_endpoint.py:128:17:</a> ANN003 Missing type annotation for `**kwargs`
- <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api_connexion/endpoints/forward_to_fab_endpoint.py#L128'>airflow/api_connexion/endpoints/forward_to_fab_endpoint.py:128:19:</a> ANN003 Missing type annotation for `**kwargs`
... 3595 additional changes omitted for rule ANN003
+ <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api_connexion/endpoints/forward_to_fab_endpoint.py#L39'>airflow/api_connexion/endpoints/forward_to_fab_endpoint.py:39:15:</a> ANN002 Missing type annotation for `*args`
- <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api_connexion/endpoints/forward_to_fab_endpoint.py#L39'>airflow/api_connexion/endpoints/forward_to_fab_endpoint.py:39:16:</a> ANN002 Missing type annotation for `*args`
+ <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api_connexion/parameters.py#L100'>airflow/api_connexion/parameters.py:100:30:</a> ANN002 Missing type annotation for `*args`
- <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api_connexion/parameters.py#L100'>airflow/api_connexion/parameters.py:100:31:</a> ANN002 Missing type annotation for `*args`
+ <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api_connexion/security.py#L118'>airflow/api_connexion/security.py:118:23:</a> ANN002 Missing type annotation for `*args`
- <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api_connexion/security.py#L118'>airflow/api_connexion/security.py:118:24:</a> ANN002 Missing type annotation for `*args`
+ <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api_connexion/security.py#L163'>airflow/api_connexion/security.py:163:23:</a> ANN002 Missing type annotation for `*args`
- <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api_connexion/security.py#L163'>airflow/api_connexion/security.py:163:24:</a> ANN002 Missing type annotation for `*args`
+ <a href='https://github.com/apache/airflow/blob/47348ce66c1f4a90aec87215b8a237c4bffcddca/airflow/api_connexion/security.py#L180'>airflow/api_connexion/security.py:180:23:</a> ANN002 Missing type annotation for `*args`
... 4515 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+943 -943 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/ajax_source.py#L34'>examples/basic/data/ajax_source.py:34:26:</a> ANN002 Missing type annotation for `*args`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/ajax_source.py#L34'>examples/basic/data/ajax_source.py:34:27:</a> ANN002 Missing type annotation for `*args`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/ajax_source.py#L34'>examples/basic/data/ajax_source.py:34:33:</a> ANN003 Missing type annotation for `**kwargs`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/ajax_source.py#L34'>examples/basic/data/ajax_source.py:34:35:</a> ANN003 Missing type annotation for `**kwargs`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/server_sent_events_source.py#L36'>examples/basic/data/server_sent_events_source.py:36:26:</a> ANN002 Missing type annotation for `*args`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/server_sent_events_source.py#L36'>examples/basic/data/server_sent_events_source.py:36:27:</a> ANN002 Missing type annotation for `*args`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/server_sent_events_source.py#L36'>examples/basic/data/server_sent_events_source.py:36:33:</a> ANN003 Missing type annotation for `**kwargs`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/server_sent_events_source.py#L36'>examples/basic/data/server_sent_events_source.py:36:35:</a> ANN003 Missing type annotation for `**kwargs`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/graphs.py#L22'>examples/models/graphs.py:22:78:</a> ANN003 Missing type annotation for `**kwargs`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/graphs.py#L22'>examples/models/graphs.py:22:80:</a> ANN003 Missing type annotation for `**kwargs`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/struct.py#L58'>src/bokeh/core/property/struct.py:58:24:</a> ANN003 Missing type annotation for `**fields`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/struct.py#L58'>src/bokeh/core/property/struct.py:58:26:</a> ANN003 Missing type annotation for `**fields`
... 971 additional changes omitted for rule ANN003
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/validation.py#L93'>src/bokeh/core/property/validation.py:93:14:</a> ANN002 Missing type annotation for `*args`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/validation.py#L93'>src/bokeh/core/property/validation.py:93:15:</a> ANN002 Missing type annotation for `*args`
... 1872 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| ANN003 | 4590 | 2295 | 2295 | 0 | 0 |
| ANN002 | 1846 | 923 | 923 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Merged by @charliermarsh on 2024-03-08 23:57_

---

_Closed by @charliermarsh on 2024-03-08 23:57_

---

_Label `bug` added by @charliermarsh on 2024-03-08 23:57_

---
