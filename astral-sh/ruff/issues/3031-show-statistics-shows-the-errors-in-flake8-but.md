```yaml
number: 3031
title: "--show-statistics shows the errors in flake8 but not in ruff"
type: issue
state: closed
author: cclauss
labels:
  - question
assignees: []
created_at: 2023-02-19T14:29:50Z
updated_at: 2023-02-19T14:54:45Z
url: https://github.com/astral-sh/ruff/issues/3031
synced_at: 2026-01-10T11:09:45Z
```

# --show-statistics shows the errors in flake8 but not in ruff

---

_Issue opened by @cclauss on 2023-02-19 14:29_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
@spaceone
With the repo: https://github.com/pyscript/pyscript
➜  pyscript git:(ruff) `git mv setup.cfg setup.cfg.disabled ` # Disables the flake8 config
➜  pyscript git:(ruff) `flake8 . --max-line-length=100 --statistics`
```
./examples/micrograd_ai.py:18:5: F821 undefined name 'pyscript'
    pyscript.write("micrograd-run-all-fig2-div", result)
    ^
./examples/micrograd_ai.py:24:5: F821 undefined name 'pyscript'
    pyscript.write("micrograd-run-all-print-div", "".join(print_statements))
    ^
./examples/micrograd_ai.py:80:5: F821 undefined name 'pyscript'
    pyscript.write("micrograd-run-all-fig1-div", plt)
    ^
./examples/todo.py:8:17: F821 undefined name 'Element'
task_template = Element("task-template").select(".task", from_content=True)
                ^
./examples/todo.py:9:13: F821 undefined name 'Element'
task_list = Element("list-tasks-container")
            ^
./examples/todo.py:10:20: F821 undefined name 'Element'
new_task_content = Element("new-task-content")
                   ^
./pyscriptjs/tests/integration/support.py:222:9: E731 do not assign a lambda expression, use a def
        pred = lambda msg: msg.text == text
        ^
1     E731 do not assign a lambda expression, use a def
6     F821 undefined name 'pyscript'
7
```
➜  pyscript git:(ruff) ✗ `ruff check . --line-length=100 --statistics`
```
1	E731	[*] Do not assign a `lambda` expression, use a `def`
6	F821	[ ] Undefined name `pyscript`
```
Flake8 shows both the errors and the summary
Ruff only shows the summary


---

_Comment by @charliermarsh on 2023-02-19 14:40_

Ah yeah, this was an intentional deviation. I thought it seemed not-so-useful to include the violations, since I see `--statistics` as a one- or few-time command rather than something that's included consistently. Are you using `--statistics` as a consistent flag on invocations?

---

_Label `question` added by @charliermarsh on 2023-02-19 14:40_

---

_Comment by @cclauss on 2023-02-19 14:54_

> Are you using --statistics as a consistent flag on invocations?

I was but I can live with this change.  Closing.

---

_Closed by @cclauss on 2023-02-19 14:54_

---
