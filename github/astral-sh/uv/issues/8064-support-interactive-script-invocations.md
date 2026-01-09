---
number: 8064
title: Support interactive script invocations
type: issue
state: open
author: zanieb
labels:
  - cli
assignees: []
created_at: 2024-10-10T01:38:11Z
updated_at: 2025-11-02T19:56:44Z
url: https://github.com/astral-sh/uv/issues/8064
synced_at: 2026-01-07T13:12:17-06:00
---

# Support interactive script invocations

---

_Issue opened by @zanieb on 2024-10-10 01:38_

> In: `main.py`
>
> ```python
> # /// script
> # requires-python = ">=3.12"
> # dependencies = [
> #     "numpy",
> #     "pandas",
> # ]
> # ///
>
> import numpy
> import pandas
>
> def main():
>     pass
>
> if __name__ == '__main__':
>     main()
>     print('Done!')
> ```
>
> Then using uv produces no errors:
>
> ```bash
> user@pop-os:~/Documents/GitHub/standalone_scripts$ uv run main.py 
> Reading inline script metadata from: main.py
> Done!
> ```
>
> However when trying to get a REPL after an execution (maybe inspect some variables or have imports ready):
>
> ```bash
> user@pop-os:~/Documents/GitHub/standalone_scripts$ uv run python -i main.py
> Traceback (most recent call last):
>   File "/home/user/Documents/GitHub/standalone_scripts/main.py", line 9, in <module>
>     import numpy
> ModuleNotFoundError: No module named 'numpy'
> >>> 
> ```
>

Originally posted by @Michallote in https://github.com/astral-sh/uv/issues/6548#issuecomment-2403714219_
            

---

_Referenced in [astral-sh/uv#6548](../../astral-sh/uv/issues/6548.md) on 2024-10-10 01:38_

---

_Label `cli` added by @zanieb on 2024-10-10 01:39_

---

_Comment by @Michallote on 2024-10-10 01:40_

wow that was fast!

This was tested with 

```bash
$ uv version
uv 0.4.18
```

---

_Comment by @SnirBroshi on 2024-12-15 01:01_

Also worth mentioning `uv run ipython -i ./main.py` and `uv run bpython -i ./main.py`

---

_Comment by @T-256 on 2024-12-15 10:52_


```shell
$ # run [i/b]python in isolated environment with resolved script dependencies
$ uv run --with-script main.py python -i main.py
$ uv run --with-script main.py ipython -i ./main.py
$ uv run --with-script main.py bpython -i ./main.py
```

---

_Comment by @josiahcoad on 2024-12-17 02:00_

Is it possible to open a python repl with a particular python version and package? ie

`uv run python --python 3.10 --packages boto3,requests`

I often find I want to do this when trying out a new package real quick.

---

_Comment by @charliermarsh on 2024-12-17 02:03_

```
uv run  --python 3.10 --with boto3 --with requests python
```

---

_Referenced in [astral-sh/uv#11472](../../astral-sh/uv/issues/11472.md) on 2025-02-13 10:10_

---

_Comment by @billyzs on 2025-05-22 08:56_

> uv run --with-script main.py python -i main.py

unfortunately as of uv 0.7.6 this option is gone; I couldn't find a replacement after a quick glance at `uv run --help`; is there a way to drop back to REPL after the script finishes? Something like `uv run --script -i script.py`

---

_Comment by @konstin on 2025-05-22 18:44_

Does `uv sync --script script.py` work for your case?

---

_Comment by @billyzs on 2025-05-23 05:14_

> Does `uv sync --script script.py` work for your case?

(Assuming this is directed to me) Somewhat indirectly; My setup is that I have a `script.py` (which declares its deps at the top thanks to `uv`)in a uv managed project directory; the script is for some exploratory works so I'm not ready to introduce the script deps to the project yet. `uv sync --script script.py` actually replaced project deps with script deps in the project venv; if I want to go back to working on the project I'll have to effectively rebuild the venv for the project. The rebuild is fast because uv and uv devs are awesome, but the process still feels a bit clunky. What I propose is something like `uv run --interactive script.py` that drops me back to the REPL so I can keep playing with local variables, like `python -i script.py` would do.

Anyways, I managed to achieve this effect for now by manually adding a `breakpoint()` at the end of the script. Still would be nice to get an official solution. wdyt?    

---

_Comment by @technillogue on 2025-11-02 19:56_

`uv sync --script script.py` does not handle requires-python. it would be great  if `--with-script` was reinstated, or if uv run could just accept the full set of python arguments (-i, -O, -OO, -u, -v, -W, -X)

---
