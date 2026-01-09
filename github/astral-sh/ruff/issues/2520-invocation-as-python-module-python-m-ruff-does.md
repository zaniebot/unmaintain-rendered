---
number: 2520
title: "Invocation as python module `python -m ruff` does not work properly"
type: issue
state: closed
author: trottomv
labels:
  - bug
assignees: []
created_at: 2023-02-03T09:19:10Z
updated_at: 2023-02-04T13:23:25Z
url: https://github.com/astral-sh/ruff/issues/2520
synced_at: 2026-01-07T13:12:14-06:00
---

# Invocation as python module `python -m ruff` does not work properly

---

_Issue opened by @trottomv on 2023-02-03 09:19_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

When ruff is installed on system without virtualenv or in a different path than the default, the invocation as a python module does not work, and a sys exit occurs without errors printed on the console:

`$ pip install ruff`
or
`$ pip install --user --no-cache ruff`

```
$ ruff -V
ruff 0.0.238
```

```
$ python -m ruff check -v .
$ ruff check -v .
...
[2023-02-03][10:15:35][ruff::rules::isort::categorize][DEBUG] Categorized 'json' as StandardLibrary (KnownStandardL
[2023-02-03][10:15:35][ruff::rules::isort::categorize][DEBUG] Categorized 'django' as ThirdParty (KnownThirdParty)
[2023-02-03][10:15:35][ruff::rules::isort::categorize][DEBUG] Categorized 'django_drf_filepond' as ThirdParty (NoMa
[2023-02-03][10:15:35][ruff::rules::isort::categorize][DEBUG] Categorized 'django' as ThirdParty (KnownThirdParty)
[2023-02-03][10:15:35][ruff::rules::isort::categorize][DEBUG] Categorized 'django' as ThirdParty (KnownThirdParty)
...
```

```
$ find / -name "ruff"
/home/trotto/.local/lib/python3.11/site-packages/ruff
/home/trotto/.local/bin/ruff
```

---

_Label `bug` added by @charliermarsh on 2023-02-03 12:40_

---

_Comment by @charliermarsh on 2023-02-03 16:25_

Thanks for filing :) I'll take a look today (hopefully).

---

_Comment by @charliermarsh on 2023-02-03 16:25_

Although where possible I recommend `ruff` over `python -m ruff`.

---

_Comment by @trottomv on 2023-02-03 18:09_

Thank to you @charliermarsh 

in fact it is absolutely not a blocking problem, but it seems internal to the `__main__.py`

`sysconfig.get_path("scripts")` returns the correct path in case of a virtualenv is active, but if `pip install` is run to a custom PATH at that point `python -m` would no longer work because the result of `sysconfig.get_path("scripts ")` is always `/usr/local/bin`

---

_Comment by @charliermarsh on 2023-02-03 18:10_

Yeah I'm trying to figure out what the "right" fix is here. We could just call `subprocess.check_call(["ruff", ...])` or similar. But, is that guaranteed to give you the right version of Ruff, if you have multiple installed in your path...?

---

_Comment by @charliermarsh on 2023-02-03 18:14_

I guess we could call `subprocess.check_call` and add the scripts directory to the front of the path?

---

_Comment by @trottomv on 2023-02-03 20:20_

I consider quite important to avoid the sys exit without having an explicit error in console, it is not easy to predict the various edge cases and where the ruff binary can be installed, i would suggest something like this:

```
if __name__ == "__main__":                                                   
    ruff = Path(sysconfig.get_path("scripts")) / "ruff"                      
    if not ruff.is_file():                                                   
        ruff = Path(sysconfig.get_config_var("userbase")) / "bin" / "ruff"   
        if not ruff.is_file():                                               
            raise FileNotFoundError(ruff)                                    
    sys.exit(os.spawnv(os.P_WAIT, ruff, [ruff, *sys.argv[1:]]))              
                                                                             
```

This should cover most cases, 
if you like it, I can gladly open a PR.

---

_Comment by @charliermarsh on 2023-02-03 21:02_

Yeah that seems to work for me. Feel free to send a PR! Would be welcome!

---

_Referenced in [astral-sh/ruff#2563](../../astral-sh/ruff/pulls/2563.md) on 2023-02-04 08:36_

---

_Comment by @charliermarsh on 2023-02-04 13:23_

Closed by #2563.

---

_Closed by @charliermarsh on 2023-02-04 13:23_

---

_Referenced in [Project-MONAI/MONAI#6822](../../Project-MONAI/MONAI/pulls/6822.md) on 2023-08-04 20:46_

---

_Referenced in [astral-sh/ruff#15630](../../astral-sh/ruff/issues/15630.md) on 2025-01-21 08:57_

---
