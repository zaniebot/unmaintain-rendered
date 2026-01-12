```yaml
number: 2638
title: uv fails to pip compile nb-black
type: issue
state: closed
author: owenlamont
labels: []
assignees: []
created_at: 2024-03-24T11:40:58Z
updated_at: 2024-03-24T18:59:08Z
url: https://github.com/astral-sh/uv/issues/2638
synced_at: 2026-01-12T15:58:39Z
```

# uv fails to pip compile nb-black

---

_@owenlamont_

uv pip compile fails with [nb-black](https://github.com/dnanhkhoa/nb_black/blob/master/setup.py) package (while pip compile works with it). I think uv is just being stricter about the version data type in the nb-black's setup.py while pip compile is apparently more lenient. I'm happy to accept this is just something broken in nb-black and I don't plan to keep that dependency... I just noticed the behaviour was different between pip compile and uv pip compile.

<img width="1507" alt="image" src="https://github.com/astral-sh/uv/assets/12672027/0e23393b-527d-4b38-bdb9-bc43b74d2591">

* A minimal code snippet that reproduces the bug (requirements.in)

```ini
nb-black
```

* The command you invoked

```shell
uv pip compile requirements.in -o requirements.txt
```

* The current uv platform.

Running on Windows, pipx installed uv (with a Python 3.12.2 env). Created a Python 3.11.2 venv and attempted the above uv pip compile (I have reproduced the same issue on Ubuntu too also with a Python 3.11.2 venv).

* The current uv version (`uv --version`).
```shell
uv 0.1.24 (a5cae0292 2024-03-22)
```


---

_Comment by @notatallshaw on 2024-03-24 15:59_

I get  the same error with pip:

```
PS C:\Users\damia> py -V
Python 3.12.2
PS C:\Users\damia> py -m pip -V
pip 24.0 from C:\Users\damia\AppData\Local\Programs\Python\Python312\Lib\site-packages\pip (python 3.12)
PS C:\Users\damia> py -m pip install --dry-run nb-black
Collecting nb-black
  Downloading nb_black-1.0.7.tar.gz (4.8 kB)
  Installing build dependencies ... done
  Getting requirements to build wheel ... error
  error: subprocess-exited-with-error

  × Getting requirements to build wheel did not run successfully.
  │ exit code: 1
  ╰─> [3 lines of output]
      error in nb_black setup command: 'install_requires' must be a string or list of strings containing valid project/version requirement specifiers; Expected end or semicolon (after name and no valid version specifier)
          yapf >= '0.28'; python_version < '3.6'
               ^
      [end of output]

  note: This error originates from a subprocess, and is likely not a problem with pip.
error: subprocess-exited-with-error

× Getting requirements to build wheel did not run successfully.
│ exit code: 1
╰─> See above for output.

note: This error originates from a subprocess, and is likely not a problem with pip.
```

---

_Comment by @owenlamont on 2024-03-24 18:43_

Oh, apologies. For me this was running in the context of a larger repo and CI pipeline which didn't give me errors with nb-black but did after I tried migrating to uv.

Not sure if there was some other difference/logic that was handling the error there or whether it was using an older version of pip or setuptools.

I'll investigate the differences further.

---

_Comment by @owenlamont on 2024-03-24 18:57_

I'll close this issue now assuming it's not directly uv related.

---

_Closed by @owenlamont on 2024-03-24 18:57_

---

_Comment by @notatallshaw on 2024-03-24 18:59_

Yeah, I can't help with your larger pipeline without understanding it, but it seems nb_black has been broken for awhile and there has been no response from maintainers, I would suggest removing it from your dependency chain if you can:

* https://github.com/dnanhkhoa/nb_black/pull/41
* https://github.com/dnanhkhoa/nb_black/issues/40

---
