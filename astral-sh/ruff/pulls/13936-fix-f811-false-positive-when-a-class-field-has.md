```yaml
number: 13936
title: Fix F811 false positive when a class field has the same name as an unused import
type: pull_request
state: closed
author: MichaReiser
labels:
  - bug
assignees: []
base: main
head: micha/fix-class-field-redefinition
created_at: 2024-10-26T09:03:03Z
updated_at: 2024-10-28T07:02:34Z
url: https://github.com/astral-sh/ruff/pull/13936
synced_at: 2026-01-12T15:55:46Z
```

# Fix F811 false positive when a class field has the same name as an unused import

---

_@MichaReiser_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/13930

Ruff incorrectly reports class fields as redefinitions when there's an unused import with the same name:

```python
from queue import Empty

class Types:
    INVALID = 0
    UINT = 1
    HEX = 2
    Empty = 3  # <-- reported as redefinition

# q = Empty() <- This disables the error if uncommented
```

This PR excludes fields from the redefinition checks. 

I'm not sure if this is the proper fix. Or maybe the diagnostic is even intentional? 

## Test Plan

Added test. 

I verified that Ruff doesn't detect redefined class fields today:

```python
class Types:
    INVALID = 0
    UINT = 1
    HEX = 2
    HEX = 4  # not raised
```

Ruff doesn't report a violation for the redeclared HEX field


---

_Label `bug` added by @MichaReiser on 2024-10-26 09:03_

---

_Review requested from @charliermarsh by @MichaReiser on 2024-10-26 09:03_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-10-26 09:03_

---

_Comment by @github-actions[bot] on 2024-10-26 09:19_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+1 -2 violations, +0 -0 fixes in 3 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/299cea060128ade18a8b22f0f79acdc2c8d00af5/superset/commands/importers/v1/__init__.py#L82'>superset/commands/importers/v1/__init__.py:82:34:</a> RUF100 [*] Unused `noqa` directive (unused: `F811`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/binary-husky/gpt_academic">binary-husky/gpt_academic</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/binary-husky/gpt_academic/blob/69f3755682e8fb5c07d12a78e40fc06e5ac9d233/crazy_functions/agent_fns/python_comment_agent.py#L205'>crazy_functions/agent_fns/python_comment_agent.py:205:9:</a> F811 Redefinition of unused `dedent` from line 5
- <a href='https://github.com/binary-husky/gpt_academic/blob/69f3755682e8fb5c07d12a78e40fc06e5ac9d233/tests/test_python_auto_docstring.py#L194'>tests/test_python_auto_docstring.py:194:9:</a> F811 Redefinition of unused `dedent` from line 8
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| F811 | 2 | 0 | 2 | 0 | 0 |
| RUF100 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -10 violations, +0 -0 fixes in 3 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/299cea060128ade18a8b22f0f79acdc2c8d00af5/superset/commands/importers/v1/__init__.py#L82'>superset/commands/importers/v1/__init__.py:82:34:</a> RUF100 [*] Unused `noqa` directive (unused: `F811`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/binary-husky/gpt_academic">binary-husky/gpt_academic</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/binary-husky/gpt_academic/blob/69f3755682e8fb5c07d12a78e40fc06e5ac9d233/crazy_functions/agent_fns/python_comment_agent.py#L205'>crazy_functions/agent_fns/python_comment_agent.py:205:9:</a> F811 Redefinition of unused `dedent` from line 5
- <a href='https://github.com/binary-husky/gpt_academic/blob/69f3755682e8fb5c07d12a78e40fc06e5ac9d233/tests/test_python_auto_docstring.py#L194'>tests/test_python_auto_docstring.py:194:9:</a> F811 Redefinition of unused `dedent` from line 8
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+0 -8 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
- <a href='https://github.com/python/typeshed/blob/efccd7455a942ab4e9fdeda2885fcab15c093b10/stubs/psutil/psutil/_pswindows.pyi#L41'>stubs/psutil/psutil/_pswindows.pyi:41:5:</a> F811 Redefinition of unused `ABOVE_NORMAL_PRIORITY_CLASS` from line 23
- <a href='https://github.com/python/typeshed/blob/efccd7455a942ab4e9fdeda2885fcab15c093b10/stubs/psutil/psutil/_pswindows.pyi#L42'>stubs/psutil/psutil/_pswindows.pyi:42:5:</a> F811 Redefinition of unused `BELOW_NORMAL_PRIORITY_CLASS` from line 24
- <a href='https://github.com/python/typeshed/blob/efccd7455a942ab4e9fdeda2885fcab15c093b10/stubs/psutil/psutil/_pswindows.pyi#L43'>stubs/psutil/psutil/_pswindows.pyi:43:5:</a> F811 Redefinition of unused `HIGH_PRIORITY_CLASS` from line 25
- <a href='https://github.com/python/typeshed/blob/efccd7455a942ab4e9fdeda2885fcab15c093b10/stubs/psutil/psutil/_pswindows.pyi#L44'>stubs/psutil/psutil/_pswindows.pyi:44:5:</a> F811 Redefinition of unused `IDLE_PRIORITY_CLASS` from line 26
- <a href='https://github.com/python/typeshed/blob/efccd7455a942ab4e9fdeda2885fcab15c093b10/stubs/psutil/psutil/_pswindows.pyi#L45'>stubs/psutil/psutil/_pswindows.pyi:45:5:</a> F811 Redefinition of unused `NORMAL_PRIORITY_CLASS` from line 27
- <a href='https://github.com/python/typeshed/blob/efccd7455a942ab4e9fdeda2885fcab15c093b10/stubs/psutil/psutil/_pswindows.pyi#L46'>stubs/psutil/psutil/_pswindows.pyi:46:5:</a> F811 Redefinition of unused `REALTIME_PRIORITY_CLASS` from line 28
- <a href='https://github.com/python/typeshed/blob/efccd7455a942ab4e9fdeda2885fcab15c093b10/stubs/pysftp/pysftp/__init__.pyi#L105'>stubs/pysftp/pysftp/__init__.pyi:105:9:</a> F811 Redefinition of unused `walktree` from line 24
- <a href='https://github.com/python/typeshed/blob/efccd7455a942ab4e9fdeda2885fcab15c093b10/stubs/pysftp/pysftp/__init__.pyi#L79'>stubs/pysftp/pysftp/__init__.pyi:79:9:</a> F811 Redefinition of unused `cd` from line 18
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| F811 | 10 | 0 | 10 | 0 | 0 |
| RUF100 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>




---

_Closed by @MichaReiser on 2024-10-28 07:02_

---
