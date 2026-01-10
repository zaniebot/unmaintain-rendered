---
number: 10422
title: uv add PyMuPDF causes it to build from source and fails
type: issue
state: closed
author: rafaeljcd
labels: []
assignees: []
created_at: 2025-01-09T02:07:57Z
updated_at: 2025-01-09T02:37:38Z
url: https://github.com/astral-sh/uv/issues/10422
synced_at: 2026-01-10T01:24:53Z
---

# uv add PyMuPDF causes it to build from source and fails

---

_Issue opened by @rafaeljcd on 2025-01-09 02:07_

using the command causes it to build from source. 

```shell
uv add PyMuPDF
```

similar to this issue

- https://github.com/pymupdf/PyMuPDF/discussions/2394

```shell
Traceback (most recent call last):
File "setup.py", line 706, in
subprocess.run( command, shell=True, check=True)
File "C:\Users\we\AppData\Local\Programs\Python\Python36\lib\subprocess.py", line 438, in run
output=stdout, stderr=stderr)
subprocess.CalledProcessError: Command 'cd mupdf-1.22.0-source&&"devenv.com" >platform/win32/mupdf.sln /Build "ReleaseTesseract|x64"/Project mupdf' returned non-zero exit status 1.
```

## Workaround

Install it using the `uv pip install` and `uv add`

```shell
uv pip install pymupdf  
Resolved 1 package in 101ms
Prepared 1 package in 3.21s
Installed 1 package 
+ pymupdf==1.25.1
```

```shell
uv add pymupdf==1.25.1
```

will let it install the binary from pypi, then will then resolved any issues when adding it to uv.lock


---

_Comment by @charliermarsh on 2025-01-09 02:20_

What Python version are you using?

---

_Comment by @rafaeljcd on 2025-01-09 02:27_

> What Python version are you using?

3.12

---

_Comment by @charliermarsh on 2025-01-09 02:33_

That package doesn't publish any wheels for Python 3.12. It only publishes wheels for Python 3.9. So it's correct to build from source here: https://pypi.org/project/PyMuPDF/#files.

---

_Comment by @rafaeljcd on 2025-01-09 02:37_

I see, sorry for the trouble and many thanks for the help!

---

_Closed by @rafaeljcd on 2025-01-09 02:37_

---
