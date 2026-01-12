```yaml
number: 2763
title: "Panic: Resolution should contain all packages"
type: issue
state: closed
author: BradLucky
labels:
  - bug
assignees: []
created_at: 2024-04-01T21:35:03Z
updated_at: 2024-04-07T00:02:51Z
url: https://github.com/astral-sh/uv/issues/2763
synced_at: 2026-01-12T15:58:40Z
```

# Panic: Resolution should contain all packages

---

_@BradLucky_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

We are using `uv` in Github Actions to install our dependencies. It has been running great for a few weeks now until this afternoon (after 0.1.27 released). Using https://github.com/marketplace/actions/setup-uv, we did not have `uv` locked to a specific version, and it was unable to install dependencies, giving the error.

Running the command `uv pip install --system -r requirements.txt` produces the error:
```shell
Resolved 202 packages in 605ms
thread 'main' panicked at crates/uv/src/commands/pip_install.rs:670:18:
Resolution should contain all packages
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

After a bit of research, I tried locking CI to version 0.1.26 and things worked fine.
```shell
Resolved 202 packages in 6.16s
Downloaded 3 packages in 94ms
Installed 3 packages in 11ms
```

Our CI runner OS is Ubuntu 22.04.4 LTS.

---

_Comment by @zanieb on 2024-04-01 21:58_

This sounds very likely to be a regression related to #2596

Can you share more details on the requirements? Do you have a minimal reproduction?

---

_Label `bug` added by @zanieb on 2024-04-01 21:58_

---

_Comment by @charliermarsh on 2024-04-01 22:01_

Or even a maximal reproduction :)

---

_Comment by @charliermarsh on 2024-04-01 22:21_

One reproduction is if you have multiple versions of a package installed. Then, the resolver will try to use them, but the install plan will reject them all.

```
uv venv --seed
source .venv/bin/activate
pip install anyio==3.7.0 --ignore-installed
pip install anyio==4.0.0 --ignore-installed
uv pip install anyio black --verbose
```

Yields:

```
thread 'main' panicked at crates/uv/src/commands/pip_install.rs:678:18:
Resolution should contain all packages
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

---

_Comment by @charliermarsh on 2024-04-01 22:22_

In general, we probably need to try and reduce the coupling in the assumptions, hmm...

---

_Comment by @bschoenmaeckers on 2024-04-02 13:54_

I got the same error on my local machine when installing the following minimal requirements.txt file:

```
--extra-index-url https://private-package-registry/simple
--extra-index-url https://download.pytorch.org/whl/cpu

torch==2.1.0+cpu
private-package==1.0.0  # Does not depend on torch
```

```console
uv pip install -r requirements.txt
Resolved 47 packages in 330ms
thread 'main' panicked at crates\uv\src\commands\pip_install.rs:670:18:
Resolution should contain all packages
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

---

_Comment by @zanieb on 2024-04-02 14:14_

@bschoenmaeckers are any of those packages already installed locally?

---

_Comment by @zanieb on 2024-04-02 16:03_

@bschoenmaeckers I cannot reproduce this:

```
❯ cat requirements.txt
--extra-index-url https://astral-sh.github.io/packse/0.3.8/simple-html/
--extra-index-url https://download.pytorch.org/whl/cpu

torch==2.1.0+cpu
example-a==1.0.0

❯ uv venv
Using Python 3.11.8 interpreter at: /usr/bin/python3
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate

❯ cargo run -- pip install -r requirements.txt
    Finished dev [unoptimized + debuginfo] target(s) in 0.16s
     Running `target/debug/uv pip install -r requirements.txt`
Resolved 11 packages in 558ms
Installed 11 packages in 860ms
 + example-a==1.0.0
 + example-b==2.0.0
 + filelock==3.9.0
 + fsspec==2023.4.0
 + jinja2==3.1.2
 + markupsafe==2.1.3
 + mpmath==1.3.0
 + networkx==3.2.1
 + sympy==1.12
 + torch==2.1.0+cpu
 + typing-extensions==4.8.0
```

---

_Renamed from "0.1.27 breaks in Github Actions" to "Panic: Resolution should contain all packages" by @zanieb on 2024-04-02 16:04_

---

_Comment by @bschoenmaeckers on 2024-04-02 19:18_

> @bschoenmaeckers are any of those packages already installed locally?

I cannot reproduce this error anymore after creating a new venv. So it was maybe just a fluke. 


---

_Comment by @charliermarsh on 2024-04-07 00:02_

I'm going to close for now since we've fixed a few issues here. If you see it again in the latest release, please LMK and we'll re-open!

---

_Closed by @charliermarsh on 2024-04-07 00:02_

---
