---
number: 5572
title: "remove dynamic linking to `liblzma` from pip binary "
type: issue
state: closed
author: universalmind303
labels: []
assignees: []
created_at: 2024-07-29T20:34:52Z
updated_at: 2024-07-30T22:29:12Z
url: https://github.com/astral-sh/uv/issues/5572
synced_at: 2026-01-07T13:12:17-06:00
---

# remove dynamic linking to `liblzma` from pip binary 

---

_Issue opened by @universalmind303 on 2024-07-29 20:34_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

If i install `uv` through pip, it dynamically links to `/usr/local/opt/xz/lib/liblzma.5.dylib`

```sh
# create an empty virtual env
~/Development/uv-dynlink ❯ virtualenv .venv
created virtual environment CPython3.12.4.final.0-64 in 381ms
  creator CPython3macOsBrew(dest=/Users/corygrinstead/Development/uv-dynlink/.venv, clear=False, no_vcs_ignore=False, global=False)
  seeder FromAppData(download=False, pip=bundle, via=copy, app_data_dir=/Users/me/Library/Application Support/virtualenv)
    added seed packages: pip==24.1.2
  activators BashActivator,CShellActivator,FishActivator,NushellActivator,PowerShellActivator,PythonActivator
# activate virtual env (source .venv/bin/activate)
~/Development/uv-dynlink ❯ overlay use .venv/bin/activate.nu
# install uv
(.venv) ~/Development/uv-dynlink ❯ pip install uv
(.venv) ~/Development/uv-dynlink ❯ which uv
╭───┬─────────┬──────────────────────────────────────────────┬──────────╮
│ # │ command │                        path                  │   type   │
├───┼─────────┼──────────────────────────────────────────────┼──────────┤
│ 0 │ uv      │ /Users/me/Development/example/.venv/bin/uv   │ external │
╰───┴─────────┴──────────────────────────────────────────────┴──────────╯
(.venv) ~/Development/uv-dynlink ❯ python3 --version
Python 3.12.4
(.venv) ~/Development/uv-dynlink ❯ python --version
Python 3.12.4
(.venv) ~/Development/uv-dynlink ❯ pip install uv
Collecting uv
  Using cached uv-0.2.31-py3-none-macosx_10_12_x86_64.whl.metadata (35 kB)
Using cached uv-0.2.31-py3-none-macosx_10_12_x86_64.whl (11.6 MB)
Installing collected packages: uv
Successfully installed uv-0.2.31

[notice] A new release of pip is available: 24.1.2 -> 24.2
[notice] To update, run: pip install --upgrade pip
(.venv) ~/Development/uv-dynlink ❯ overlay use .venv/bin/activate.nu
(.venv) ~/Development/uv-dynlink ❯ which uv | get path | otool -L ...$in | grep 'liblzma'
        /usr/local/opt/xz/lib/liblzma.5.dylib (compatibility version 12.0.0, current version 12.2.0)
```

However, if I build it from source, it does not
```sh
> cargo run python install
> otool -L ./target/debug/uv | grep 'liblzma' 
# empty 
```



---

_Renamed from "remove dynamic linking to `liblzma` pip binary " to "remove dynamic linking to `liblzma` from pip binary " by @universalmind303 on 2024-07-29 20:35_

---

_Comment by @rhenzel on 2024-07-29 21:16_

Running into a related issue with `curl -LsSf https://astral.sh/uv/install.sh | sh ` install on Apple Silicon macOS.   
When running `uv pip install ...`, I get: 
```
downloading uv 0.2.31 aarch64-apple-darwin
...
dyld[1134]: Library not loaded: /opt/homebrew/opt/xz/lib/liblzma.5.dylib
  Referenced from: <297928D5-54B8-3A44-B1B4-41CE488775BD> .cargo/bin/uv
  Reason: tried: '/opt/homebrew/opt/xz/lib/liblzma.5.dylib' (no such file), '/System/Cryptexes/OS/opt/homebrew/opt/xz/lib/liblzma.5.dylib' (no such file), '/opt/homebrew/opt/xz/lib/liblzma.5.dylib' (no such file)
```

---

_Comment by @charliermarsh on 2024-07-29 21:30_

Interesting, thanks for reporting. I believe we can statically link it

---

_Referenced in [astral-sh/uv#5577](../../astral-sh/uv/pulls/5577.md) on 2024-07-29 21:31_

---

_Closed by @charliermarsh on 2024-07-29 21:46_

---

_Closed by @charliermarsh on 2024-07-29 21:46_

---

_Referenced in [axodotdev/cargo-dist#1249](../../axodotdev/cargo-dist/issues/1249.md) on 2024-07-30 08:45_

---

_Comment by @manzt on 2024-07-30 22:26_

Just can across this upgrading `uv` today.

---

_Comment by @charliermarsh on 2024-07-30 22:27_

Apologize, going to release shortly.

---

_Comment by @manzt on 2024-07-30 22:29_

No worries! Part of being on the edge :) Thanks for making uv awesome.

---
