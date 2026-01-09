---
number: 10289
title: Using jupyter notebooks with uv causes circular imports
type: issue
state: closed
author: anuroop18
labels:
  - question
assignees: []
created_at: 2025-01-03T15:28:18Z
updated_at: 2025-01-03T17:42:00Z
url: https://github.com/astral-sh/uv/issues/10289
synced_at: 2026-01-07T13:12:18-06:00
---

# Using jupyter notebooks with uv causes circular imports

---

_Issue opened by @anuroop18 on 2025-01-03 15:28_

I did following

- `uv init`
- `uv venv`
- `source .venv/bin/activate`
- `uv add jupyterlab`  (same problem if using ipykernel or marimo)
- Open some notebook and try to run some cell
- Shows circular import error like following:

```
The kernel failed to start as 'get_ipython' could not be imported from 'most likely due to a circular import'.
```
<details>
<summary>This is the detailed error i am getting.</summary>

```
15:21:43.265 [info] Starting Kernel (Python Path: ~/projects/worksheet_automation/.venv/bin/python, Venv, 3.13.0) for '~/projects/worksheet_automation/src/scripts/experiments.ipynb' (disableUI=false)
15:21:43.286 [info] Process Execution: ~/projects/worksheet_automation/.venv/bin/python -c "import ipykernel; print(ipykernel.__version__); print("5dc3a68c-e34e-4080-9c3e-2a532b2ccb4d"); print(ipykernel.__file__)"
15:21:43.312 [info] Process Execution: ~/projects/worksheet_automation/.venv/bin/python -m ipykernel_launcher --f=/home/~/.local/share/jupyter/runtime/kernel-v33885fbf97197ca0b83610954ce18c5d90e11ea7d.json
    > cwd: //home/~/projects/worksheet_automation/src/scripts
15:21:43.672 [error] Disposing kernel process due to an error Error: The kernel died. Error: Traceback (most recent call last):
  File "<frozen runpy>", line 198, in _run_module_as_main
  File "<frozen runpy>", line 88, in _run_code
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/ipykernel_launcher.py", line 16, in <module>
    from ipykernel import kernelapp as app
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/ipykernel/kernelapp.py", line 21, in <module>
    from IPython.core.application import (  # type:ignore[attr-defined]
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/__init__.py", line 53, in <module>
    from .core.application import Application
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/core/application.py", line 37, in <module>
    from IPython.core import crashhandler, release
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/core/crashhandler.py", line 30, in <module>
    from IPython.core import ultratb
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/core/ultratb.py", line 111, in <module>
    from IPython import get_ipython
ImportError: cannot import name 'get_ipython' from partially initialized module 'IPython' (most likely due to a circular import) (/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/__init__.py)... View Jupyter [log](command:jupyter.viewOutput) for further details.
    > Kernel Id = .jvsc74a57bd04b8b6175ebb045c6837643fdeeb2af16a8fcb612fd1ee157725d0c80dd9e4020./home/~/projects/worksheet_automation/.venv/python./home/~/projects/worksheet_automation/.venv/python.-m#ipykernel_launcher
    > Interpreter Id = ~/projects/worksheet_automation/.venv/bin/python
    > at ChildProcess.<anonymous> (/home/~/.cursor-server/extensions/ms-toolsai.jupyter-2024.8.1-linux-x64/dist/extension.node.js:299:44598)
    > stdErr = Traceback (most recent call last):
  File "<frozen runpy>", line 198, in _run_module_as_main
  File "<frozen runpy>", line 88, in _run_code
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/ipykernel_launcher.py", line 16, in <module>
    from ipykernel import kernelapp as app
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/ipykernel/kernelapp.py", line 21, in <module>
    from IPython.core.application import (  # type:ignore[attr-defined]
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/__init__.py", line 53, in <module>
    from .core.application import Application
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/core/application.py", line 37, in <module>
    from IPython.core import crashhandler, release
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/core/crashhandler.py", line 30, in <module>
    from IPython.core import ultratb
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/core/ultratb.py", line 111, in <module>
    from IPython import get_ipython
ImportError: cannot import name 'get_ipython' from partially initialized module 'IPython' (most likely due to a circular import) (/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/__init__.py)
15:21:43.674 [error] Failed to connect raw kernel session: Error: The kernel died. Error: Traceback (most recent call last):
  File "<frozen runpy>", line 198, in _run_module_as_main
  File "<frozen runpy>", line 88, in _run_code
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/ipykernel_launcher.py", line 16, in <module>
    from ipykernel import kernelapp as app
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/ipykernel/kernelapp.py", line 21, in <module>
    from IPython.core.application import (  # type:ignore[attr-defined]
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/__init__.py", line 53, in <module>
    from .core.application import Application
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/core/application.py", line 37, in <module>
    from IPython.core import crashhandler, release
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/core/crashhandler.py", line 30, in <module>
    from IPython.core import ultratb
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/core/ultratb.py", line 111, in <module>
    from IPython import get_ipython
ImportError: cannot import name 'get_ipython' from partially initialized module 'IPython' (most likely due to a circular import) (/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/__init__.py)... View Jupyter [log](command:jupyter.viewOutput) for further details.
15:21:43.675 [error] Failed to connect raw kernel session: Error: The kernel died. Error: Traceback (most recent call last):
  File "<frozen runpy>", line 198, in _run_module_as_main
  File "<frozen runpy>", line 88, in _run_code
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/ipykernel_launcher.py", line 16, in <module>
    from ipykernel import kernelapp as app
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/ipykernel/kernelapp.py", line 21, in <module>
    from IPython.core.application import (  # type:ignore[attr-defined]
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/__init__.py", line 53, in <module>
    from .core.application import Application
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/core/application.py", line 37, in <module>
    from IPython.core import crashhandler, release
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/core/crashhandler.py", line 30, in <module>
    from IPython.core import ultratb
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/core/ultratb.py", line 111, in <module>
    from IPython import get_ipython
ImportError: cannot import name 'get_ipython' from partially initialized module 'IPython' (most likely due to a circular import) (/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/__init__.py)... View Jupyter [log](command:jupyter.viewOutput) for further details.
15:21:43.675 [warn] Failed to shutdown kernel, .jvsc74a57bd04b8b6175ebb045c6837643fdeeb2af16a8fcb612fd1ee157725d0c80dd9e4020./home/~/projects/worksheet_automation/.venv/python./home/~/projects/worksheet_automation/.venv/python.-m#ipykernel_launcher [TypeError: Cannot read properties of undefined (reading 'dispose')
	at L_.shutdown (/home/~/.cursor-server/extensions/ms-toolsai.jupyter-2024.8.1-linux-x64/dist/extension.node.js:304:13629)
	at process.processTicksAndRejections (node:internal/process/task_queues:95:5)
	at async U_.shutdown (/home/~/.cursor-server/extensions/ms-toolsai.jupyter-2024.8.1-linux-x64/dist/extension.node.js:304:22107)]
15:21:43.676 [warn] Error occurred while trying to start the kernel, options.disableUI=false Error: The kernel died. Error: Traceback (most recent call last):
  File "<frozen runpy>", line 198, in _run_module_as_main
  File "<frozen runpy>", line 88, in _run_code
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/ipykernel_launcher.py", line 16, in <module>
    from ipykernel import kernelapp as app
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/ipykernel/kernelapp.py", line 21, in <module>
    from IPython.core.application import (  # type:ignore[attr-defined]
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/__init__.py", line 53, in <module>
    from .core.application import Application
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/core/application.py", line 37, in <module>
    from IPython.core import crashhandler, release
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/core/crashhandler.py", line 30, in <module>
    from IPython.core import ultratb
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/core/ultratb.py", line 111, in <module>
    from IPython import get_ipython
ImportError: cannot import name 'get_ipython' from partially initialized module 'IPython' (most likely due to a circular import) (/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/__init__.py)... View Jupyter [log](command:jupyter.viewOutput) for further details.
    > Kernel Id = .jvsc74a57bd04b8b6175ebb045c6837643fdeeb2af16a8fcb612fd1ee157725d0c80dd9e4020./home/~/projects/worksheet_automation/.venv/python./home/~/projects/worksheet_automation/.venv/python.-m#ipykernel_launcher
    > Interpreter Id = ~/projects/worksheet_automation/.venv/bin/python
    > at ChildProcess.<anonymous> (/home/~/.cursor-server/extensions/ms-toolsai.jupyter-2024.8.1-linux-x64/dist/extension.node.js:299:44598)
    > stdErr = Traceback (most recent call last):
  File "<frozen runpy>", line 198, in _run_module_as_main
  File "<frozen runpy>", line 88, in _run_code
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/ipykernel_launcher.py", line 16, in <module>
    from ipykernel import kernelapp as app
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/ipykernel/kernelapp.py", line 21, in <module>
    from IPython.core.application import (  # type:ignore[attr-defined]
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/__init__.py", line 53, in <module>
    from .core.application import Application
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/core/application.py", line 37, in <module>
    from IPython.core import crashhandler, release
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/core/crashhandler.py", line 30, in <module>
    from IPython.core import ultratb
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/core/ultratb.py", line 111, in <module>
    from IPython import get_ipython
ImportError: cannot import name 'get_ipython' from partially initialized module 'IPython' (most likely due to a circular import) (/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/__init__.py)
15:21:43.677 [warn] Kernel Error, context = start Error: The kernel died. Error: Traceback (most recent call last):
  File "<frozen runpy>", line 198, in _run_module_as_main
  File "<frozen runpy>", line 88, in _run_code
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/ipykernel_launcher.py", line 16, in <module>
    from ipykernel import kernelapp as app
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/ipykernel/kernelapp.py", line 21, in <module>
    from IPython.core.application import (  # type:ignore[attr-defined]
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/__init__.py", line 53, in <module>
    from .core.application import Application
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/core/application.py", line 37, in <module>
    from IPython.core import crashhandler, release
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/core/crashhandler.py", line 30, in <module>
    from IPython.core import ultratb
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/core/ultratb.py", line 111, in <module>
    from IPython import get_ipython
ImportError: cannot import name 'get_ipython' from partially initialized module 'IPython' (most likely due to a circular import) (/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/__init__.py)... View Jupyter [log](command:jupyter.viewOutput) for further details.
    > Kernel Id = .jvsc74a57bd04b8b6175ebb045c6837643fdeeb2af16a8fcb612fd1ee157725d0c80dd9e4020./home/~/projects/worksheet_automation/.venv/python./home/~/projects/worksheet_automation/.venv/python.-m#ipykernel_launcher
    > Interpreter Id = ~/projects/worksheet_automation/.venv/bin/python
    > at ChildProcess.<anonymous> (/home/~/.cursor-server/extensions/ms-toolsai.jupyter-2024.8.1-linux-x64/dist/extension.node.js:299:44598)
    > stdErr = Traceback (most recent call last):
  File "<frozen runpy>", line 198, in _run_module_as_main
  File "<frozen runpy>", line 88, in _run_code
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/ipykernel_launcher.py", line 16, in <module>
    from ipykernel import kernelapp as app
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/ipykernel/kernelapp.py", line 21, in <module>
    from IPython.core.application import (  # type:ignore[attr-defined]
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/__init__.py", line 53, in <module>
    from .core.application import Application
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/core/application.py", line 37, in <module>
    from IPython.core import crashhandler, release
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/core/crashhandler.py", line 30, in <module>
    from IPython.core import ultratb
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/core/ultratb.py", line 111, in <module>
    from IPython import get_ipython
ImportError: cannot import name 'get_ipython' from partially initialized module 'IPython' (most likely due to a circular import) (/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/__init__.py)
15:21:43.690 [info] Process Execution: ~/projects/worksheet_automation/.venv/bin/python -c "import ipykernel;print('6af208d0-cb9c-427f-b937-ff563e17efdf')"
15:21:44.211 [info] Dispose Kernel '~/projects/worksheet_automation/src/scripts/experiments.ipynb' associated with '~/projects/worksheet_automation/src/scripts/experiments.ipynb'
15:21:44.213 [error] Error in notebook cell execution Error: The kernel died. Error: Traceback (most recent call last):
  File "<frozen runpy>", line 198, in _run_module_as_main
  File "<frozen runpy>", line 88, in _run_code
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/ipykernel_launcher.py", line 16, in <module>
    from ipykernel import kernelapp as app
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/ipykernel/kernelapp.py", line 21, in <module>
    from IPython.core.application import (  # type:ignore[attr-defined]
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/__init__.py", line 53, in <module>
    from .core.application import Application
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/core/application.py", line 37, in <module>
    from IPython.core import crashhandler, release
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/core/crashhandler.py", line 30, in <module>
    from IPython.core import ultratb
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/core/ultratb.py", line 111, in <module>
    from IPython import get_ipython
ImportError: cannot import name 'get_ipython' from partially initialized module 'IPython' (most likely due to a circular import) (/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/__init__.py)... View Jupyter [log](command:jupyter.viewOutput) for further details.
    > Kernel Id = .jvsc74a57bd04b8b6175ebb045c6837643fdeeb2af16a8fcb612fd1ee157725d0c80dd9e4020./home/~/projects/worksheet_automation/.venv/python./home/~/projects/worksheet_automation/.venv/python.-m#ipykernel_launcher
    > Interpreter Id = ~/projects/worksheet_automation/.venv/bin/python
    > at ChildProcess.<anonymous> (/home/~/.cursor-server/extensions/ms-toolsai.jupyter-2024.8.1-linux-x64/dist/extension.node.js:299:44598)
    > stdErr = Traceback (most recent call last):
  File "<frozen runpy>", line 198, in _run_module_as_main
  File "<frozen runpy>", line 88, in _run_code
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/ipykernel_launcher.py", line 16, in <module>
    from ipykernel import kernelapp as app
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/ipykernel/kernelapp.py", line 21, in <module>
    from IPython.core.application import (  # type:ignore[attr-defined]
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/__init__.py", line 53, in <module>
    from .core.application import Application
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/core/application.py", line 37, in <module>
    from IPython.core import crashhandler, release
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/core/crashhandler.py", line 30, in <module>
    from IPython.core import ultratb
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/core/ultratb.py", line 111, in <module>
    from IPython import get_ipython
ImportError: cannot import name 'get_ipython' from partially initialized module 'IPython' (most likely due to a circular import) (/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/__init__.py)
15:21:44.215 [error] Error in execution (get message for cell) Error: The kernel died. Error: Traceback (most recent call last):
  File "<frozen runpy>", line 198, in _run_module_as_main
  File "<frozen runpy>", line 88, in _run_code
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/ipykernel_launcher.py", line 16, in <module>
    from ipykernel import kernelapp as app
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/ipykernel/kernelapp.py", line 21, in <module>
    from IPython.core.application import (  # type:ignore[attr-defined]
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/__init__.py", line 53, in <module>
    from .core.application import Application
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/core/application.py", line 37, in <module>
    from IPython.core import crashhandler, release
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/core/crashhandler.py", line 30, in <module>
    from IPython.core import ultratb
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/core/ultratb.py", line 111, in <module>
    from IPython import get_ipython
ImportError: cannot import name 'get_ipython' from partially initialized module 'IPython' (most likely due to a circular import) (/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/__init__.py)... View Jupyter [log](command:jupyter.viewOutput) for further details.
    > Kernel Id = .jvsc74a57bd04b8b6175ebb045c6837643fdeeb2af16a8fcb612fd1ee157725d0c80dd9e4020./home/~/projects/worksheet_automation/.venv/python./home/~/projects/worksheet_automation/.venv/python.-m#ipykernel_launcher
    > Interpreter Id = ~/projects/worksheet_automation/.venv/bin/python
    > at ChildProcess.<anonymous> (/home/~/.cursor-server/extensions/ms-toolsai.jupyter-2024.8.1-linux-x64/dist/extension.node.js:299:44598)
    > stdErr = Traceback (most recent call last):
  File "<frozen runpy>", line 198, in _run_module_as_main
  File "<frozen runpy>", line 88, in _run_code
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/ipykernel_launcher.py", line 16, in <module>
    from ipykernel import kernelapp as app
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/ipykernel/kernelapp.py", line 21, in <module>
    from IPython.core.application import (  # type:ignore[attr-defined]
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/__init__.py", line 53, in <module>
    from .core.application import Application
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/core/application.py", line 37, in <module>
    from IPython.core import crashhandler, release
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/core/crashhandler.py", line 30, in <module>
    from IPython.core import ultratb
  File "/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/core/ultratb.py", line 111, in <module>
    from IPython import get_ipython
ImportError: cannot import name 'get_ipython' from partially initialized module 'IPython' (most likely due to a circular import) (/home/~/projects/worksheet_automation/.venv/lib/python3.11/site-packages/IPython/__init__.py)
15:21:44.225 [info] Process Execution: ~/projects/worksheet_automation/.venv/bin/python -c "import ipykernel;print('6af208d0-cb9c-427f-b937-ff563e17efdf')"
```
</details>


<details>
<summary>I get this error when using `uv run marimo`</summary>

```
Process Process-1:
Traceback (most recent call last):
  File "/home/anuroop/.local/share/uv/python/cpython-3.11.10-linux-x86_64-gnu/lib/python3.11/multiprocessing/process.py", line 314, in _bootstrap
    self.run()
  File "/home/anuroop/.local/share/uv/python/cpython-3.11.10-linux-x86_64-gnu/lib/python3.11/multiprocessing/process.py", line 108, in run
    self._target(*self._args, **self._kwargs)
  File "/home/anuroop/projects/worksheet_automation/.venv/lib/python3.11/site-packages/marimo/_runtime/runtime.py", line 2137, in launch_kernel
    kernel.start_completion_worker(completion_queue)
  File "/home/anuroop/projects/worksheet_automation/.venv/lib/python3.11/site-packages/marimo/_runtime/runtime.py", line 543, in start_completion_worker
    from marimo._runtime.complete import completion_worker
  File "/home/anuroop/projects/worksheet_automation/.venv/lib/python3.11/site-packages/marimo/_runtime/complete.py", line 10, in <module>
    import jedi  # type: ignore # noqa: F401
    ^^^^^^^^^^^
  File "/home/anuroop/projects/worksheet_automation/.venv/lib/python3.11/site-packages/jedi/__init__.py", line 33, in <module>
    from jedi.api import Interpreter, Script, preload_module, set_debug_function
  File "/home/anuroop/projects/worksheet_automation/.venv/lib/python3.11/site-packages/jedi/api/__init__.py", line 17, in <module>
    from jedi.api import classes, helpers, interpreter, refactoring
  File "/home/anuroop/projects/worksheet_automation/.venv/lib/python3.11/site-packages/jedi/api/classes.py", line 24, in <module>
    from jedi.api.helpers import filter_follow_imports
  File "/home/anuroop/projects/worksheet_automation/.venv/lib/python3.11/site-packages/jedi/api/helpers.py", line 15, in <module>
    from jedi.inference.base_value import NO_VALUES
  File "/home/anuroop/projects/worksheet_automation/.venv/lib/python3.11/site-packages/jedi/inference/__init__.py", line 69, in <module>
    from jedi.inference import helpers, imports, recursion
  File "/home/anuroop/projects/worksheet_automation/.venv/lib/python3.11/site-packages/jedi/inference/imports.py", line 23, in <module>
    from jedi.inference.gradual.typeshed import (
  File "/home/anuroop/projects/worksheet_automation/.venv/lib/python3.11/site-packages/jedi/inference/gradual/typeshed.py", line 11, in <module>
    from jedi.inference.gradual.stub_value import StubModuleValue, TypingModuleWrapper
  File "/home/anuroop/projects/worksheet_automation/.venv/lib/python3.11/site-packages/jedi/inference/gradual/stub_value.py", line 4, in <module>
    from jedi.inference.gradual.typing import TypingModuleFilterWrapper
  File "/home/anuroop/projects/worksheet_automation/.venv/lib/python3.11/site-packages/jedi/inference/gradual/typing.py", line 11, in <module>
    from jedi.inference.arguments import repack_with_argument_clinic
  File "/home/anuroop/projects/worksheet_automation/.venv/lib/python3.11/site-packages/jedi/inference/arguments.py", line 18, in <module>
    from jedi.inference.value import iterable
  File "/home/anuroop/projects/worksheet_automation/.venv/lib/python3.11/site-packages/jedi/inference/value/__init__.py", line 5, in <module>
    from jedi.inference.value.instance import (
  File "/home/anuroop/projects/worksheet_automation/.venv/lib/python3.11/site-packages/jedi/inference/value/instance.py", line 7, in <module>
    from jedi.inference.arguments import TreeArgumentsWrapper, ValuesArguments
ImportError: cannot import name 'TreeArgumentsWrapper' from partially initialized module 'jedi.inference.arguments' (most likely due to a circular import) (/home/anuroop/projects/worksheet_automation/.venv/lib/python3.11/site-packages/jedi/inference/arguments.py)
```
</details>


They work fine when installed and using with pip.

---

_Comment by @charliermarsh on 2025-01-03 15:51_

How are you running Jupyter, after installing it?

---

_Label `question` added by @charliermarsh on 2025-01-03 15:51_

---

_Comment by @anuroop18 on 2025-01-03 15:57_

@charliermarsh  Thanks for quick reply.

I did following:
- `uv run jupyter lab` or
- `source .venv/bin/activate` => `jupyter lab` or
- Running in vscode/cursor
Getting same error in all of the above.

- Tried this also with same error: `uv run --with jupyter jupyter lab`


But if the do following:
- `uv venv` 
- `uv pip install pip`
- Select that .venv as kernel in vscode notebook
- Then install ipykernel using the dialog that vscode displays
- Then all works fine, even after `uv sync`

I checked all packages installed in every way, pip/uv. The list is exactly same. 

Additionally, i am running all this using wsl. Everything used to work fine. Getting these errors for only 1 week .Tried different versions still same issue.


---

_Comment by @charliermarsh on 2025-01-03 16:08_

I don't really know what's going on, but you could try (1) `uv cache clean`, then (2) `uv venv` and re-run your workflow. Does that help at all?

---

_Comment by @anuroop18 on 2025-01-03 16:21_

Thankyou so so much. 🙏🙏

This solved my problem instantly.

But what was really causing problem??

---

_Referenced in [Cyfrin/moccasin#179](../../Cyfrin/moccasin/issues/179.md) on 2025-01-03 16:22_

---

_Closed by @anuroop18 on 2025-01-03 16:24_

---

_Comment by @charliermarsh on 2025-01-03 17:41_

I’m not certain. Weird things can happen with Jupyter specifically because a lot of packages tend to write to the same location in the environment. Or perhaps at some point a file within a virtual environment was edited, and that “poisoned” the cache. Hard to say.

---
