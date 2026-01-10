```yaml
number: 7036
title: "Can't use tkinter with new venv set up with uv"
type: issue
state: closed
author: dstansby
labels:
  - bug
  - compatibility
assignees: []
created_at: 2024-09-04T17:16:36Z
updated_at: 2025-09-23T17:27:57Z
url: https://github.com/astral-sh/uv/issues/7036
synced_at: 2026-01-10T03:23:52Z
```

# Can't use tkinter with new venv set up with uv

---

_Issue opened by @dstansby on 2024-09-04 17:16_

---

(Maintainer edit)

**NOTE** If you are still affected by this, try reinstalling the managed Python interpreter

```
python install --reinstall ...
```

---

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

```
uv venv
source .venv/bin/activate
python -c "from tkinter import Tk; window = Tk()" 
```

This gives me the error message:
```
Traceback (most recent call last):
  File "<string>", line 1, in <module>
  File "/Users/dstansby/.local/share/uv/python/cpython-3.12.5-macos-aarch64-none/lib/python3.12/tkinter/__init__.py", line 2346, in __init__
    self.tk = _tkinter.create(screenName, baseName, className, interactive, wantobjects, useTk, sync, use)
              ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
_tkinter.TclError: Can't find a usable init.tcl in the following directories: 
    /tools/deps/lib/tcl8.6 /Users/dstansby/.venv/lib/tcl8.6 /Users/dstansby/lib/tcl8.6 /Users/dstansby/.venv/library /Users/dstansby/library /Users/dstansby/tcl8.6.12/library /Users/tcl8.6.12/library



This probably means that Tcl wasn't installed properly.
```

I can successfully run `python -c "from tkinter import Tk; window = Tk()"` in a fresh `conda` environment, but when I install python on conda it also installs `tk`,  so maybe that's why it works? Maybe this isn't an issue with `uv`?, but if anyone knows how to get this working I'd be very gateful!

uv 0.4.4 (Homebrew 2024-09-04)
macOS

---

_Comment by @zanieb on 2024-09-04 17:18_

This is a python-build-standalone quirk: https://gregoryszorc.com/docs/python-build-standalone/main/quirks.html#tcl-tk-support-files

We might need to set the variable as described there. 🤔 

---

_Comment by @bluss on 2024-09-04 19:17_

For the record, rye did this https://github.com/astral-sh/rye/pull/233

---

_Comment by @charliermarsh on 2024-09-04 23:31_

I'm not opposed to doing a similar thing here.

---

_Label `bug` added by @charliermarsh on 2024-09-04 23:31_

---

_Label `compatibility` added by @charliermarsh on 2024-09-04 23:31_

---

_Comment by @mraesener-aubex on 2024-09-09 07:08_

to maybe point others to a workaround, all you'd need to do is to export an env variable called TCL_LIBRARY pointing to the correct tcl directory.
to figure the correct one out, I did the following:

ATTENTION: this assumes that your .venv is based on the default python available to uv and also that tcl is at version 8.6, so keep that in mind

```bash
#!/bin/bash

# Get the Python path
PYTHON_PATH=$(cd ~ && uv run which python)

# Extract the base path
BASE_PATH=$(dirname "$(dirname "$PYTHON_PATH")")

# Construct the TCL_LIBRARY path
TCL_LIBRARY="$BASE_PATH/lib/tcl8.6"

# Set the environment variable
export TCL_LIBRARY

# Print the set variable (for verification)
echo "TCL_LIBRARY has been set to: $TCL_LIBRARY"
```

for comfort I've then just put the var in my .env file which gets automatically loaded by .envrc (direnv) - which btw would totally be an appealing feature for uv to replicate :P 



---

_Comment by @astrojuanlu on 2024-10-16 07:24_

The quirks page says

> Distributions produced from this project contain tcl/tk support files

However,

```
$ uv --version
uv 0.4.22
$ uv venv -p 3.9
Using CPython 3.9.19
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate.fish
$ find .venv -name "*tcl*"
$ # ?
```

What am I doing wrong?

---

_Comment by @emcek on 2024-10-16 08:18_

> $ find .venv -name "*tcl*"
> $ # ?
> ```
> 
> What am I doing wrong?
Those are in **python** installation directory not in **virtualenv**, I'm not next to linux box but under windows:
```console
(venvflon) PS C:\Users\mplichta\Projects\venvflon> uv python list          
cpython-3.13.0-windows-x86_64-none     C:\Users\mplichta\AppData\Roaming\uv\python\cpython-3.13.0-windows-x86_64-none\python.exe
cpython-3.12.7-windows-x86_64-none     <download available>
```
So, path would be: `C:\Users\mplichta\AppData\Roaming\uv\python\cpython-3.13.0-windows-x86_64-none\tcl\tcl8.6`

As workaroud you can use **tkinker** like:
```python
from os import environ
from pathlib import Path
from sys import base_prefix

environ["TCL_LIBRARY"] = str(Path(base_prefix) / "tcl" / "tcl8.6")
environ["TK_LIBRARY"] = str(Path(base_prefix) / "tcl" / "tk8.6")
import tkinter as tk
```

---

_Comment by @astrojuanlu on 2024-10-16 10:12_

Oh I understand now. On macOS they're under `~/.local/share/uv/python/cpython-*-macos-aarch64-none/lib/tcl8.6` 👍🏼 

---

_Comment by @giolucasd on 2024-10-17 19:16_

This is an important issue for me and my employers. I'm currently using the workaround suggeted above.
I'm a CS major with 5 years of SWE xp in python, but no xp in rust yet. Learning rust is a current goal of mine.
Would you consider this a good first issue for me or at least one that I could get along? If so, any special precautions?

---

_Comment by @zanieb on 2024-10-17 19:56_

It'd be a stretch to say it's a good first issue, there's a lot of nuance to the possible solutions and I would have to do quite a bit of research to even understand what the trade-offs are. I think the first best step is to understand Rye's approach and if there are any alternatives. If it's the most compelling approach still, we should be able to port it over without too much difficulty.

---

_Comment by @harrylaulau on 2024-10-18 07:02_

Since most people expect to use Tkinter that comes with the python, I guess including `TCL_LIBRARY` and `TK_LIBRARY` env path pointing to where uv installed python as @emcek described would be a good default solution? The uv can provide some override mechanism, like allowing use to specify custom path to these two environment variable in the `.python_version` file or maybe some other files? This way TCL should always work by default.

I tried @emcek method on Mac and the result is successful
```python
from os import environ
from pathlib import Path
from sys import base_prefix

environ["TCL_LIBRARY"] = str(Path(base_prefix) / "lib" / "tcl8.6")
environ["TK_LIBRARY"] = str(Path(base_prefix) / "lib" / "tk8.6")

print(environ["TCL_LIBRARY"])
print(environ["TK_LIBRARY"])

from tkinter import *
from tkinter import ttk
root = Tk()
frm = ttk.Frame(root, padding=10)
frm.grid()
ttk.Label(frm, text="Hello World!").grid(column=0, row=0)
ttk.Button(frm, text="Quit", command=root.destroy).grid(column=1, row=0)
root.mainloop()
```

However I have changed the path of `TCL_LIBRARY` and `TK_LIBRARY` a bit as for mac its "lib" instead of "tcl", as pointed out by @astrojuanlu 

Maybe uv can populate the two environmental variable by default?


---

_Comment by @sonotley on 2024-10-27 19:26_

So... expanding on the workaround so it works on all platforms, doesn't clobber existing env vars, might work with tk 8.7+, and doesn't bother doing anything if tk is actually working already (given that just because you used uv to write your library there's no guarantee it will be run using the standalone Python builds that uv uses)... we have something like this.

```python
import tkinter
from os import environ
from pathlib import Path
from sys import base_prefix
import platform

if not ("TCL_LIBRARY" in environ and "TK_LIBRARY" in environ):
    try:
        tkinter.Tk()
    except tkinter.TclError:
        tk_dir = "tcl" if platform.system() == "Windows" else "lib"
        tk_path = Path(base_prefix) / tk_dir
        environ["TCL_LIBRARY"] = str(next(tk_path.glob("tcl8.*")))
        environ["TK_LIBRARY"] = str(next(tk_path.glob("tk8.*")))

def main():
    tk = tkinter.Tk()
    tk.mainloop()


if __name__ == "__main__":
    main()
```

---

_Comment by @sonotley on 2024-11-08 20:38_

Hmmm... my workaround above fixes tk, but when I try to use the tkagg backend for matplotlib things get broken again. I'll do some more investigation.

`    from . import _tkagg
ImportError: initialization failed
`

---

_Comment by @harrylaulau on 2024-11-09 16:17_

> Hmmm... my workaround above fixes tk, but when I try to use the tkagg backend for matplotlib things get broken again. I'll do some more investigation.
> 
> ` from . import _tkagg ImportError: initialization failed`

This issue is mentioned in #6893 

Seems like Tkinter is just unusable for now.

---

_Comment by @charliermarsh on 2024-12-15 03:00_

Broadly, we have two options:

1. Add a `.pth` file, like in Rye, to set these environment variables.
2. Patch CPython to set these if not set (in `python-build-standalone`) -- something like:

```diff
--- a/Lib/tkinter/__init__.py
+++ b/Lib/tkinter/__init__.py
@@ -54,6 +54,14 @@
 _magic_re = re.compile(r'([\\{}])')
 _space_re = re.compile(r'([\s])', re.ASCII)

+# Facilitate discovery of the Tcl/Tk libraries.
+import os
+
+if 'TCL_LIBRARY' not in os.environ:
+    os.environ['TCL_LIBRARY'] = sys.base_prefix + '/tcl/tcl' + _tkinter.TCL_VERSION
+if 'TK_LIBRARY' not in os.environ:
+    os.environ['TK_LIBRARY'] = sys.base_prefix + '/tcl/tk' + _tkinter.TK_VERSION
+

 def _join(value):
     """Internal function."""
```

The advantage of (1) is that it's slightly easier, doesn't require us to maintain a patch, and could be disabled in `uv venv` (like, we could have a flag to avoid creating it).

The advantage of (2) is that we'll only set those variables when `tkinter` is imported, and we won't have to add a `.pth` file to the environment, which may confuse users.


---

_Assigned to @charliermarsh by @charliermarsh on 2024-12-15 03:00_

---

_Comment by @mraesener-aubex on 2024-12-15 08:21_

I'd say having a .pth file is the "better" approach of the two in terms of best practices and style etc. so that's what I'd gravitate to...

but, thinking of ease of use especially for beginners and the "uv just works" experience there's probably no way around patching. 

Is it possible to get a list of similar or identical use case? Is this a tkinter speciality because of the UI nature of it or would this affect lots of other builtins, too?

---

_Comment by @zanieb on 2024-12-15 17:05_

I have a minor preference for the upstream patch, I think? There are consumers other than uv.

Perhaps a dumb question, but why do we need to set an environment variable? How does this work in the standard distributions?

---

_Comment by @charliermarsh on 2024-12-15 17:58_

I'm not totally certain why this works in other Pythons (e.g., Framework Python distributions).

---

_Comment by @charliermarsh on 2024-12-15 18:11_

So for the Framework Python at least, the paths appear to get compiled in to the binary:

```
❯ otool -L /Library/Frameworks/Python.framework/Versions/3.12/lib/python3.12/lib-dynload/_tkinter.cpython-312-darwin.so
/Library/Frameworks/Python.framework/Versions/3.12/lib/python3.12/lib-dynload/_tkinter.cpython-312-darwin.so (architecture x86_64):
	/Library/Frameworks/Python.framework/Versions/3.12/lib/libtcl8.6.dylib (compatibility version 8.6.0, current version 8.6.15)
	/Library/Frameworks/Python.framework/Versions/3.12/lib/libtk8.6.dylib (compatibility version 8.6.0, current version 8.6.15)
	/usr/lib/libSystem.B.dylib (compatibility version 1.0.0, current version 1311.0.0)
/Library/Frameworks/Python.framework/Versions/3.12/lib/python3.12/lib-dynload/_tkinter.cpython-312-darwin.so (architecture arm64):
	/Library/Frameworks/Python.framework/Versions/3.12/lib/libtcl8.6.dylib (compatibility version 8.6.0, current version 8.6.15)
	/Library/Frameworks/Python.framework/Versions/3.12/lib/libtk8.6.dylib (compatibility version 8.6.0, current version 8.6.15)
	/usr/lib/libSystem.B.dylib (compatibility version 1.0.0, current version 1311.0.0)
```

---

_Comment by @charliermarsh on 2024-12-15 18:29_

(I'll see if we can do something similar.)

---

_Comment by @charliermarsh on 2024-12-15 18:51_

I think another thing we could do is symlink those files into `.venv/lib`, since tkl does look there.

---

_Comment by @mraesener-aubex on 2024-12-15 19:06_

So if a fresh venv gets created the tcl dependencies are "copied" (symlinked) from somewhere else to the `.venv/lib` dir, or did I get this wrong?

---

_Comment by @charliermarsh on 2024-12-15 23:03_

Ok, I have a patch that is working well here: https://github.com/indygreg/python-build-standalone/pull/421. It avoids all the disadvantages, at the cost of requiring us to maintain a CPython C patch.

---

_Comment by @domfellner on 2024-12-17 11:57_

Thank you for looking into this an resolving it! As a user of uv I wonder when this patch will be available? Will I need to wait for new Python patch releases like 3.10.17? or can force uv to use an archive from a specific standalone release, marked by its date?

---

_Comment by @zanieb on 2024-12-17 20:44_

It'll be available following a `python-build-standalone` release, then a uv release — probably relatively soon.

We don't support using arbitrary archives yet.

---

_Comment by @zanieb on 2024-12-20 00:28_

I just released this in https://github.com/astral-sh/uv/releases/tag/0.5.11

---

_Closed by @charliermarsh on 2024-12-20 01:05_

---

_Comment by @astrojuanlu on 2024-12-20 15:00_

For the record, this won't work out of the box with Python versions that were already downloaded.

```
❯ uv self update
...
❯ uv --version
uv 0.5.11 (c4d0caaee 2024-12-19)
❯ uv venv -p 3.10
Using CPython 3.10.14
...
❯ uv pip install foxdot
...
❯ uv run python -m FoxDot
_tkinter.TclError: Can't find a usable init.tcl in the following directories: 
    /tools/deps/lib/tcl8.6 /Users/juan_cano/Projects/Personal/Music/.venv/lib/tcl8.6 /Users/juan_cano/Projects/Personal/Music/lib/tcl8.6 /Users/juan_cano/Projects/Personal/Music/.venv/library /Users/juan_cano/Projects/Personal/Music/library /Users/juan_cano/Projects/Personal/Music/tcl8.6.12/library /Users/juan_cano/Projects/Personal/tcl8.6.12/library

This probably means that Tcl wasn't installed properly.
```

But it does work with newly installed versions!

```
❯ uv python install 3.10.16
Installed Python 3.10.16 in 13.09s
 + cpython-3.10.16-macos-aarch64-none
❯ uv venv -p 3.10.16
Using CPython 3.10.16
...
❯ # Now everything works!
```

---

_Comment by @zanieb on 2024-12-20 15:59_

You can `uv python install --reinstall <version>`

---

_Comment by @Satge96 on 2025-02-17 07:15_

I use uv 0.5.30 and updated all the python installs. Still i get the following error:
    from tkinter import Variable, StringVar, IntVar, DoubleVar, BooleanVar
ImportError: cannot import name 'Variable' from 'tkinter' (unknown location)

Is this related?

---

_Comment by @sanjeev-bhandari on 2025-05-12 05:28_

Has this issue been resolved, or do we still need to rely on the https://github.com/astral-sh/uv/issues/7036#issuecomment-2440145724?

It's quite frustrating, especially since I rely on multiple Python versions — which is one of the main reasons I chose to use uv — and this error keeps appearing when I try to use matplotlib.

---

_Comment by @astrojuanlu on 2025-05-12 06:05_

I think this is still an issue, but not in uv https://github.com/astral-sh/python-build-standalone/issues/146

---

_Comment by @spierepf on 2025-05-17 11:53_

This issue is closed. Is there a fix? I just tried this on Ubuntu, and python cannot find tkinter.

---

_Comment by @Liquidmasl on 2025-05-19 14:18_

I am sorry guys i read this up and down x times now and scoured google, and i still dont know what i have todo to get tkinter to work with uv...
can someone help me out here?

---

_Comment by @konstin on 2025-05-19 14:19_

Did you try reinstalling the managed python versions?

---

_Comment by @Liquidmasl on 2025-05-19 14:19_

> Did you try reinstalling the managed python versions?

wow, that was fast!
I reinstalled the version i am currently using and then resynched my venv so 
`python install --reinstall 3.12 `

---

_Comment by @DeflateAwning on 2025-06-11 17:01_

To anyone stumbling on this in the future, the process that worked for my team is:

1. Delete your `.venv` folder
2. Run `uv python install --reinstall <version>` (e.g., `uv python install --reinstall 3.10`)
3. Re-create the virtual environment: `.venv` (with `uv venv`)

To test:

1. Activate the virtual environment: `source .venv/bin/activate`
2. Run `python -m tkinter`
3. You should see a basic Tk popup window.

---

_Comment by @myarcana on 2025-09-23 09:21_

> To anyone stumbling on this in the future, the process that worked for my team is:
> 
> 1. Delete your `.venv` folder
> 2. Run `uv python install --reinstall <version>` (e.g., `uv python install --reinstall 3.10`)
> 3. Re-create the virtual environment: `.venv`
> 
> To test:
> 
> 1. Activate the virtual environment: `source .venv/bin/activate`
> 2. Run `python -m tkinter`
> 3. You should see a basic Tk popup window.

I did exactly this but it didn't work for me:


```
% uv --version
uv 0.8.20 (Homebrew 2025-09-22)
% uv run python --version
Python 3.11.6
% uv python install --reinstall 3.11.6 
Installed Python 3.11.6 in 7.47s
 ~ cpython-3.11.6-macos-aarch64-none (python3.11)
% uv venv
Using CPython 3.11.6
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
% source .venv/bin/activate
(tkinter-test) % python -m tkinter
Traceback (most recent call last):
  File "<frozen runpy>", line 198, in _run_module_as_main
  File "<frozen runpy>", line 88, in _run_code
  File "/Users/me/.local/share/uv/python/cpython-3.11.6-macos-aarch64-none/lib/python3.11/tkinter/__main__.py", line 7, in <module>
    main()
  File "/Users/me/.local/share/uv/python/cpython-3.11.6-macos-aarch64-none/lib/python3.11/tkinter/__init__.py", line 4618, in _test
    root = Tk()
           ^^^^
  File "/Users/me/.local/share/uv/python/cpython-3.11.6-macos-aarch64-none/lib/python3.11/tkinter/__init__.py", line 2326, in __init__
    self.tk = _tkinter.create(screenName, baseName, className, interactive, wantobjects, useTk, sync, use)
              ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
_tkinter.TclError: Can't find a usable init.tcl in the following directories: 
    /tools/deps/lib/tcl8.6 {/Users/me/tkinter-test/.venv/lib/tcl8.6} {/Users/me/tkinter-test/lib/tcl8.6} {/Users/me/tkinter-test/.venv/library} {/Users/me/tkinter-test/library} {/Users/me/tkinter-test/tcl8.6.12/library} {/Users/me/tkinter-test/tcl8.6.12/library}



This probably means that Tcl wasn't installed properly.
```


I could "solve" it by running

```
export TCL_LIBRARY="$HOME"/.local/share/uv/python/cpython-3.11.6-macos-aarch64-none/lib/tcl8.6
export TK_LIBRARY="$HOME"/.local/share/uv/python/cpython-3.11.6-macos-aarch64-none/lib/tk8.6
```

But I don't want to manually pollute my shell with environment variables to make Python work, it would be better if the uv venv made tkinter work out of the box.

---
