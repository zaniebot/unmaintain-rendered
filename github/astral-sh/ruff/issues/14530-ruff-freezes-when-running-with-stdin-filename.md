---
number: 14530
title: ruff freezes when running with --stdin-filename
type: issue
state: closed
author: Guust-Franssens
labels:
  - question
assignees: []
created_at: 2024-11-22T13:56:56Z
updated_at: 2024-11-22T17:00:23Z
url: https://github.com/astral-sh/ruff/issues/14530
synced_at: 2026-01-07T13:12:16-06:00
---

# ruff freezes when running with --stdin-filename

---

_Issue opened by @Guust-Franssens on 2024-11-22 13:56_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
```bash
$ ruff check --stdin-filename src/__init__.py  --verbose --isolated
[2024-11-22][14:49:35][ruff::resolve][DEBUG] Isolated mode, not reading any pyproject.toml
``` 

however running it without specifying --stdin-filename works without any problem
```bash
$ ruff check src/__init__.py --verbose --isolated
[2024-11-22][14:51:18][ruff::resolve][DEBUG] Isolated mode, not reading any pyproject.toml
[2024-11-22][14:51:18][ruff::commands::check][DEBUG] Identified files to lint in: 3.3689ms
[2024-11-22][14:51:18][ruff::diagnostics][DEBUG] Checking: C:\Users\gfranssens\vscode-projects\belgian-journal\src\__init__.py
[2024-11-22][14:51:18][ruff::commands::check][DEBUG] Checked 1 files in: 12.029ms
All checks passed!
```

I have tried running on many versions however all with same problem (minor version 0.5, 0.6, 0.7).  I was currently using the latest version.
```bash
$ ruff version
ruff 0.8.0 (a90e404c3 2024-11-22)
```

the reason that this is a problem is because the VSCode extension runs in the background with --stdin-filename thus no corrections are done on save.

I believe the problem is related to something on my machine, however I cannot figure out what is the problem... 
Hopefully someone knows a fix...

---

_Label `question` added by @MichaReiser on 2024-11-22 14:23_

---

_Comment by @MichaReiser on 2024-11-22 14:25_

Ruff expects that you pass a file via stdin when you specify `--stdin-filename` like this:

```
ruff check --stdin-filename test.py < ../test/c.py
```


---

_Comment by @Guust-Franssens on 2024-11-22 14:54_

@MichaReiser thanks already, make sense....

when I check the VSCode output for the ruff extension I see the following; afterwhich it freezes. I assume I should be this instead on the vscode-ruff extension?

```
2024-11-22 15:51:16.631 [info] Using environment executable: C:\Users\gfranssens\.local\bin\ruff.EXE
2024-11-22 15:51:16.632 [info] Found ruff 0.8.0 at C:\Users\gfranssens\.local\bin\ruff.EXE
2024-11-22 15:51:16.632 [info] Running Ruff with: C:\Users\gfranssens\.local\bin\ruff.EXE ['check', '--force-exclude', '--no-cache', '--no-fix', '--quiet', '--output-format', 'json', '-', '--stdin-filename', 'c:\\Users\\gfranssens\\vscode-projects\\belgian-journal\\src\\spiders\\legal_entities.py']
```

---

_Comment by @MichaReiser on 2024-11-22 14:59_

Can you explain with what you mean by freeze? I don't think ruff prints any other messages after `Running Ruff with`. Do you see violations in your editor? Does the output get updated when you change and save a file?

---

_Closed by @Guust-Franssens on 2024-11-22 16:59_

---

_Comment by @Guust-Franssens on 2024-11-22 17:00_

My VSCode settings for Ruff were improplerly configured....

---
