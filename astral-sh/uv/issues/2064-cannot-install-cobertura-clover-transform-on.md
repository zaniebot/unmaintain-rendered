```yaml
number: 2064
title: Cannot install cobertura_clover_transform on first install 
type: issue
state: closed
author: jtanx
labels:
  - bug
assignees: []
created_at: 2024-02-29T00:30:51Z
updated_at: 2024-02-29T17:21:50Z
url: https://github.com/astral-sh/uv/issues/2064
synced_at: 2026-01-12T15:58:34Z
```

# Cannot install cobertura_clover_transform on first install 

---

_@jtanx_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->


This seems to be a regression in uv 0.1.12, this works fine in 0.1.11

```
VIRTUAL_ENV=ve uv pip install cobertura_clover_transform
Resolved 2 packages in 603ms
error: Failed to install: cobertura_clover_transform-1.1.4.post1-py3-none-any.whl (cobertura-clover-transform==1.1.4.post1)
  Caused by: RECORD file is invalid
  Caused by: No such file or directory (os error 2)
```

Rerunning that command, it succeeds 

---

_Comment by @charliermarsh on 2024-02-29 00:33_

Hmm, this ran without error for me repeatedly.

---

_Label `needs-mre` added by @charliermarsh on 2024-02-29 00:33_

---

_Comment by @jtanx on 2024-02-29 01:04_

Odd, this was with artifactory if it matters 

---

_Comment by @mgaitan on 2024-02-29 17:06_

this just bit us in our CI suite with a package from vanilla pypi.  

I could reproduce it in our image (based on a legacy Amazon Linux2) but not in my host on Ubuntu, so maybe is something related to `musl` ?

this is the debug on AL2 

```
$ curl -LsSf https://astral.sh/uv/install.sh | sh
System glibc version (`2.26') is too old; using musl
downloading uv 0.1.12 x86_64-unknown-linux-musl-static
installing to /user/.cargo/bin
  uv
everything's installed!

To add $HOME/.cargo/bin to your PATH, either restart your shell or run:

    source $HOME/.cargo/env

$ /user/.cargo/bin/uv venv /tmp/bug
Using Python 3.8.16 interpreter at /opt/venv/bin/python3
Creating virtualenv at: /tmp/bug
Activate with: source /tmp/bug/bin/activate
$ source /tmp/bug/bin/activate
(bug) $ /user/.cargo/bin/uv pip install --no-cache mpmath
Resolved 1 package in 383ms
Downloaded 1 package in 327ms
error: Failed to install: mpmath-1.3.0-py3-none-any.whl (mpmath==1.3.0)
  Caused by: RECORD file is invalid
  Caused by: No such file or directory (os error 2)  
  ```

---

_Comment by @charliermarsh on 2024-02-29 17:08_

I think I see the issue here.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-29 17:08_

---

_Comment by @charliermarsh on 2024-02-29 17:08_

(If I'm right, it's just a typo.)

---

_Label `needs-mre` removed by @charliermarsh on 2024-02-29 17:18_

---

_Label `bug` added by @charliermarsh on 2024-02-29 17:18_

---

_Closed by @charliermarsh on 2024-02-29 17:21_

---
