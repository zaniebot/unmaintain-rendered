---
number: 12030
title: How to make Pylance recognize the declared dependencies in a script
type: issue
state: closed
author: CrendKing
labels:
  - question
assignees: []
created_at: 2025-03-07T02:51:17Z
updated_at: 2025-11-29T14:16:35Z
url: https://github.com/astral-sh/uv/issues/12030
synced_at: 2026-01-10T01:25:14Z
---

# How to make Pylance recognize the declared dependencies in a script

---

_Issue opened by @CrendKing on 2025-03-07 02:51_

### Question

I don't know if this has been asked before. If yes, please let me know and close this.

Many people love the [inline declared dependencies feature](https://docs.astral.sh/uv/guides/scripts/#declaring-script-dependencies) of uv. However, when one works on the script in VSCode with Pylance or I assume any other Python language server, it won't be able to find the dependencies, since uv dynamically creates the environment. This always ends with errors like `Import "<dependency>" could not be resolved`, not to mention the broken Intellisense for these dependencies.

I know if I explicitly creates a venv and change the interpreter to the venv python binary it will fix the issue, but I tries to move away from venv approach for standalone scripts. Is there some black magic I'm not aware?

![Image](https://github.com/user-attachments/assets/7cb0a7ee-336b-4b2f-90ae-f8073b90a180)

### Platform

Windows 11

### Version

0.6.5

---

_Label `question` added by @CrendKing on 2025-03-07 02:51_

---

_Comment by @jromal on 2025-03-12 16:12_

I think you could use on the terminal `uv run my_script.py` or set the "launch.json" to `uv run --script".


But because "venv" can be created so easily, probably not many tried it. 

But just thinking out loud, without a computer in front of me. 

---

_Comment by @konstin on 2025-03-12 16:28_

There is a feature request in pylance here: https://github.com/microsoft/pylance-release/discussions/6522. So no hidden tricks involved, but it will take a bit until we can replace the venv workarounds.

Inline script metadata is still comparatively young, I expect it will take some time until it is universally supported across IDEs.

---

_Comment by @CrendKing on 2025-03-12 19:28_

I see. Use venv until the tools start to support the feature. Thanks!

---

_Closed by @CrendKing on 2025-03-12 19:28_

---

_Comment by @m0awie on 2025-05-13 08:14_

For anyone looking for a quicker solution in the meantime, you can point VS Code to a cached UV virtual environment, using 
`uv python find --script script.py`
This will print the path to the cached venv, which you can then enter as an interpreter path using the select interpreter command.
You may need to run with `uv run --script script.py` to update the cache if you add a dependency. 
The cache name should stay the same when adding a dependency from my cursory testing, but the window will need to be reloaded; using command palette; Developer: Reload Window

---

_Comment by @Jamie-BitFlight on 2025-11-27 04:34_

For anyone landing here looking for a solution for **CI/pre-commit** (not IDE support), I built a wrapper tool that auto-installs PEP 723 dependencies before running linters:

```bash
pip install pep723-loader
pep723-loader mypy your_script.py
pep723-loader basedpyright your_script.py
```

Or in pre-commit:
```yaml
- repo: local
  hooks:
    - id: mypy
      name: mypy
      entry: uv run -q --no-sync --with pep723-loader --with mypy pep723-loader mypy
      language: system
      types: [python]
      pass_filenames: true
```

It scans files for `# /// script` metadata, extracts deps via `uv export --script`, installs them, then runs your linter. Useful until native IDE/LSP support arrives.

Repo: https://github.com/bitflight-devops/pre-commit-pep723-linter-wrapper

---
