```yaml
number: 10435
title: uv pip install fails with 403 whereas pip install works
type: issue
state: closed
author: kressi
labels:
  - question
  - needs-mre
assignees: []
created_at: 2025-01-09T16:23:20Z
updated_at: 2025-01-10T12:31:40Z
url: https://github.com/astral-sh/uv/issues/10435
synced_at: 2026-01-12T16:00:13Z
```

# uv pip install fails with 403 whereas pip install works

---

_@kressi_

I'm trying to `uv pip install` a project in GitBash and keep getting 403 from our internal PyPI. However, `python -m pip install` is working perfectly fine. I have configured `pip.conf` and `uv.toml` with equivalent settings. Is there anything I'm missing?

```bash
$ python -m uv pip install pip-install-test
Using Python 3.12.4 environment at: C:\Program Files\python
  × No solution found when resolving dependencies:
  ╰─▶ Because pip-install-test was not found in the package registry and you require pip-install-test, we can conclude that your requirements are unsatisfiable.

      hint: An index URL (https://internal.company.domain/repository/public-pypi/simple) could not be queried due to a lack of valid authentication credentials (403 Forbidden).
```

```bash
$ python -m pip install pip-install-test
Collecting pip-install-test
  Downloading pip_install_test-0.5-py3-none-any.whl.metadata (979 bytes)
Downloading pip_install_test-0.5-py3-none-any.whl (1.7 kB)
Installing collected packages: pip-install-test
Successfully installed pip-install-test-0.5

[notice] A new release of pip is available: 24.1.2 -> 24.3.1
[notice] To update, run: python.exe -m pip install --upgrade pip
```

## Environment

```bash
$ git --version
git version 2.47.0.windows.2
```

```bash
$ uv --version
uv 0.5.16 (333f03f11 2025-01-08)
```

```bash
$ cat $UV_CONFIG_FILE
link-mode = "copy"
allow-insecure-host = ["internal.company.domain"]

[[index]]
name = "pypi"
url = "https://internal.company.domain/repository/public-pypi/simple"
default = true
```

```bash
$ cat ~/pip.conf
[global]
index-url = https://internal.company.domain/repository/public-pypi/simple
trusted-host = internal.company.domain
```

---

_Comment by @zanieb on 2025-01-09 17:44_

Where are the credentials coming from with `pip`? Are you using a `pypirc` file or keyring?

---

_Label `question` added by @zanieb on 2025-01-09 17:44_

---

_Comment by @kressi on 2025-01-09 18:36_

There are no credentials for the index

---

_Comment by @zanieb on 2025-01-09 19:10_

Can't say why your server would return a 403 then, I think you'll need to get more details. You can use `--verbose` or `RUST_LOG=uv=trace` to get more logs from uv and details about the failing request (see #9452).

---

_Label `needs-mre` added by @zanieb on 2025-01-09 19:10_

---

_Comment by @kressi on 2025-01-10 12:31_

Thank you, turned out pip and uv have not been properly configured for Windows.  `RUST_LOG=uv=trace` has helped recognizing this.

Btw. thanks for your work on uv. It's improving user experience with python significantly. ❤ 

---

_Closed by @kressi on 2025-01-10 12:31_

---
