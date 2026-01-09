---
number: 8139
title: "Problem of install black[jupyter] as uv tool."
type: issue
state: closed
author: partrita
labels:
  - question
assignees: []
created_at: 2024-10-12T01:28:52Z
updated_at: 2024-10-13T00:41:43Z
url: https://github.com/astral-sh/uv/issues/8139
synced_at: 2026-01-07T13:12:17-06:00
---

# Problem of install black[jupyter] as uv tool.

---

_Issue opened by @partrita on 2024-10-12 01:28_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

```bash
➜  ipynb git:(main) ✗ uv tool run black .
Skipping .ipynb files as Jupyter dependencies are not installed.
You can fix this by running ``pip install "black[jupyter]"``
No Python files are present to be formatted. Nothing to do 😴
➜  ipynb git:(main) ✗ uv tool install black[jupyter]
zsh: no matches found: black[jupyter]
```

I'm trying to use the black[jupyter] lint tool for `ipynb` files using the uv tool functionality. However, it's not installing properly. 
I would appreciate if you could provide a solution. 
Thank you in advance.


```bash
➜  ipynb git:(main) ✗ uv --version
uv 0.4.18 (Homebrew 2024-10-01)
```


---

_Comment by @charliermarsh on 2024-10-12 03:44_

Can you try `uv tool install "black[jupyter]"`? Your shell likely wants it wrapped in quotes.

---

_Label `question` added by @charliermarsh on 2024-10-12 03:44_

---

_Comment by @partrita on 2024-10-13 00:41_

> Can you try `uv tool install "black[jupyter]"`? Your shell likely wants it wrapped in quotes.

Thanks, it works!

```bash
➜  ~ uv tool install "black[jupyter]"
Resolved 23 packages in 342ms
Prepared 1 package in 34ms
Installed 23 packages in 91ms
 + asttokens==2.4.1
 + black==24.10.0
 + click==8.1.7
 + decorator==5.1.1
 + executing==2.1.0
 + ipython==8.28.0
 + jedi==0.19.1
 + matplotlib-inline==0.1.7
 + mypy-extensions==1.0.0
 + packaging==24.1
 + parso==0.8.4
 + pathspec==0.12.1
 + pexpect==4.9.0
 + platformdirs==4.3.6
 + prompt-toolkit==3.0.48
 + ptyprocess==0.7.0
 + pure-eval==0.2.3
 + pygments==2.18.0
 + six==1.16.0
 + stack-data==0.6.3
 + tokenize-rt==6.0.0
 + traitlets==5.14.3
 + wcwidth==0.2.13
Installed 2 executables: black, blackd
```

---

_Closed by @partrita on 2024-10-13 00:41_

---
