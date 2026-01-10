```yaml
number: 10626
title: "ruff.toml parse error - unknown field \"lint\""
type: issue
state: closed
author: leitcode
labels: []
assignees: []
created_at: 2024-03-27T11:24:36Z
updated_at: 2024-03-27T19:11:21Z
url: https://github.com/astral-sh/ruff/issues/10626
synced_at: 2026-01-10T11:09:52Z
```

# ruff.toml parse error - unknown field "lint"

---

_Issue opened by @leitcode on 2024-03-27 11:24_

I have the following ruffl.toml file:
```
[lint]
ignore = ["E501", "E402"]
```

I keep getting the error: 
```
Ruff: Lint failed (ruff failed
  Cause: Failed to parse `c:\<PathToProject>\ruff.toml`: TOML parse error at line 1, column 2
  |
1 | [lint]
  |  ^^^^
unknown field `lint`, expected one of `allowed-confusables`, `builtins`, `cache-dir`, `dummy-variable-rgx`, `exclude`, `extend`, `extend-exclude`, `extend-include`, `extend-ignore`, `extend-select`, `extend-fixable`, `extend-unfixable`, `external`, `fix`, `fix-only`, `fixable`, `output-format`, `force-exclude`, `ignore`, `ignore-init-module-imports`, `include`, `line-length`, `tab-size`, `logger-objects`, `required-version`, `respect-gitignore`, `select`, `show-source`, `show-fixes`, `src`, `namespace-packages`, `target-version`, `preview`, `task-tags`, `typing-modules`, `unfixable`, `flake8-annotations`, `flake8-bandit`, `flake8-bugbear`, `flake8-builtins`, `flake8-comprehensions`, `flake8-copyright`, `flake8-errmsg`, `flake8-quotes`, `flake8-self`, `flake8-tidy-imports`, `flake8-type-checking`, `flake8-gettext`, `flake8-implicit-str-concat`, `flake8-import-conventions`, `flake8-pytest-style`, `flake8-unused-arguments`, `isort`, `mccabe`, `pep8-naming`, `pycodestyle`, `pydocstyle`, `pyflakes`, `pylint`, `pyupgrade`, `format`, `per-file-ignores`, `extend-per-file-ignores`

)
```

I am using VSCode with the Ruff extension (v2024.16.0, shipping with ruff 0.3.1).

According to the documentation (https://docs.astral.sh/ruff/configuration/) the `ruff.toml` file seems correct, any idea why the `lint` field is not supported?

---

_Comment by @MichaReiser on 2024-03-27 11:46_

That looks correct to me. I wonder if the extension picks up some older ruff version from your global path or your venv. 

Would you mind sharing the output of: *Show output channels:...*, Select *ruff*. It should be something like

```
2024-03-27 12:42:00.995 [info] Name: Ruff
2024-03-27 12:42:00.995 [info] Module: ruff
2024-03-27 12:42:00.995 [info] Python extension loading
2024-03-27 12:42:00.995 [info] Waiting for interpreter from python extension.
2024-03-27 12:42:00.995 [info] Python extension loaded
2024-03-27 12:42:00.995 [info] Server run command: /home/micha/.pyenv/versions/3.13-dev/bin/python /home/micha/.vscode/extensions/charliermarsh.ruff-2024.16.0-linux-x64/bundled/tool/server.py
2024-03-27 12:42:00.995 [info] Server: Start requested.
2024-03-27 12:42:00.995 [info] [Trace - 12:42:00 PM] Sending request 'initialize - (0)'.
...
...
2024-03-27 12:42:01.972 [info] Running Ruff with: /home/micha/astral/ruff/target/debug/ruff ['check', '--force-exclude', '--no-cache', '--no-fix', '--quiet', '--output-format', 'json', '-', '--stdin-filename', '/home/micha/astral/isort/isort/core.py']
```

---

_Comment by @leitcode on 2024-03-27 12:38_

Thanks, that was the issue.
The extension picked up some old ruff installation which was installed as python package.

```
...
Found ruff 0.0.291 at C:\Users\<User>\AppData\Roaming\Python\Python311\Scripts\ruff.EXE
...
```

`ruff.toml` is now parsed successfully after updating via `pip install ruff -U`


---

_Comment by @MichaReiser on 2024-03-27 12:40_

I'm glad to hear that. One improvement to our VS code extension could be showing the Ruff version in the toolbar (similar to Python) to ease debugging. CC: @snowsignal 

---

_Closed by @MichaReiser on 2024-03-27 12:40_

---

_Comment by @snowsignal on 2024-03-27 19:11_

@MichaReiser I like that idea! I'll open an issue for it.

---
