---
number: 13482
title: Isolated failures of uv add / uv pip install using private repo
type: issue
state: closed
author: mhscott64
labels:
  - question
assignees: []
created_at: 2025-05-16T03:11:47Z
updated_at: 2025-05-22T17:17:47Z
url: https://github.com/astral-sh/uv/issues/13482
synced_at: 2026-01-10T01:25:34Z
---

# Isolated failures of uv add / uv pip install using private repo

---

_Issue opened by @mhscott64 on 2025-05-16 03:11_

### Summary

We proxy Pypi in Artifactory.  

Our UV config environment variables:
```
  UV_CERT                        C:\Users\***\prog-ca-bundle.crt
  UV_INDEX                       https://***:***@***.jfrog.io/artifactory/api/pypi/pgr-pypi
  UV_TRUSTED_HOST                progressive.jfrog.io
  UV_PYTHON_INSTALL_MIRROR       https://***:***@***.jfrog.io/artifactory/***-github/astral-sh/python-build-standalone/releases/download/
  UV_INSTALLER_GITHUB_BASE_URL   https://***.jfrog.io/artifactory/pgr-github
  UV_GITHUB_TOKEN                ***
  UV_DEFAULT_INDEX               https://***:***@***.jfrog.io/artifactory/api/pypi/pgr-pypi/simple
```

our PIP config environment variables:
```
  PIP_INDEX_URL                  https://://***:***@***.:://***:***@***.@progressive.jfrog.io/artifactory/api/pypi/***-pypi/simple
  PIP_INDEX                      https://://***:***@***.:://***:***@***.@progressive.jfrog.io/artifactory/api/pypi/***-pypi
  PIP_TRUSTED_HOST               ://***:***@***..jfrog.io
  PIP_CERT                       C:\Users\://***:***@***.\***-ca-bundle.crt
```

version:

> PS C:\\Users\\&ast;&ast;&ast;\\python_work\\uv\\pytz_test> **uv version**
> **_uv 0.6.9 (3d9460278 2025-03-20)_**

Attempting to install pytz with uv results in the following error:

> PS C:\\Users\\&ast;&ast;&ast;\python_work\\uv> **uv init pytz_test --python=3.10**
> **_Initialized project \`pytz-test\` at \`C:\\Users\\&ast;&ast;&ast;\\python_work\\uv\\pytz_test\`_**
> PS C:\\Users\\&ast;&ast;&ast;\\python_work\\uv> **cd .\\pytz_test\\**
> PS C:\\Users\\&ast;&ast;&ast;\python_work\uv\pytz_test> **uv add pytz**
> **_Using CPython 3.10.16_**
> **_Creating virtual environment at: .venv_**
> **_⠸ pytz-test==0.1.0_**                                                                                                                                                                                                                    **_error: Unsupported `Content-Type` "application/octet-stream" for https://progressive.jfrog.io/artifactory/api/pypi/&ast;&ast;&ast;-pypi/pytz/. Expected JSON or HTML._**
> 
> PS C:\\Users\\&ast;&ast;&ast;\\python_work\\uv\\pytz_test> **uv pip install pytz**
> **_⠹ Resolving dependencies...          
> error: Unsupported `Content-Type` "application/octet-stream" for https://progressive.jfrog.io/artifactory/api/pypi/***-pypi/pytz/. Expected JSON or HTML._**


But it can be installed with pip:

> PS C:\\Users\\&ast;&ast;&ast;\\python_work\\uv\\pytz_test> **uv add pip**
> **_Resolved 2 packages in 3.26s_**
> **_Prepared 1 package in 2.95s_**
> **_Installed 1 package in 84ms_**
> **_+ pip==25.1.1_**
> PS C:\\Users\\&ast;&ast;&ast;\\python_work\\uv\\pytz_test> **.\\.venv\\Scripts\\activate**
> 
> (pytz-test) PS C:\\Users\\&ast;&ast;&ast;\\python_work\\uv\\pytz_test> **_python -m pip install pytz_**
> **_Looking in indexes: https://&ast;&ast;&ast;:&ast;&ast;&ast;@&ast;&ast;&ast;.jfrog.io/artifactory/api/pypi/&ast;&ast;&ast;-pypi/simple_**
> **_Collecting pytz_**
>   **_Downloading https://&ast;&ast;&ast;.jfrog.io/artifactory/api/pypi/&ast;&ast;&ast;-pypi/packages/packages/81/c4/34e93fe5f5429d7570ec1fa436f1986fb1f00c3e0f43a589fe2bbcd22c3f/pytz-2025.2-py2.py3-none-any.whl (509 kB)_**
> **_Installing collected packages: pytz_**
> **_Successfully installed pytz-2025.2_**

The failure mode is identical on Mac and Windows and has been seen using Python 3.10, 3.11, 3.12


### Platform

Windows 11 x64_86

### Version

uv 0.6.9 (3d9460278 2025-03-20)

### Python version

Python 3.10.16

---

_Label `bug` added by @mhscott64 on 2025-05-16 03:11_

---

_Comment by @charliermarsh on 2025-05-16 03:25_

Unfortunately uv really isn't doing anything special here. We just hit the endpoint with the given credentials. My guess is that Artifactory is doing something strange? But I haven't seen this report before.

---

_Assigned to @jtfmumm by @konstin on 2025-05-16 08:15_

---

_Comment by @mhscott64 on 2025-05-16 11:27_

uv looks like a great tool I'd like to get for us.  If I had the band width, I'd like nothing more than to setup a rust dev env, clone the project, step through the code, identify the failure point, do the same for pip, identify the root cause of the behavior difference, and do a PR; that's just not gonna happen.  Maybe I can find someone else here to do it.
Thanks for looking at it.


---

_Comment by @konstin on 2025-05-16 11:44_

> error: Unsupported Content-Type "application/octet-stream" for [progressive.jfrog.io/artifactory/api/pypi/***-pypi/pytz](https://progressive.jfrog.io/artifactory/api/pypi/***-pypi/pytz/). Expected JSON or HTML.

If you query this URL manually, with curl and your credentials, can you check what response you get?

---

_Comment by @a087861 on 2025-05-16 12:01_

I work with @mhscott64 and just attempted to curl with credentials.  I appear to get back a valid html.

---

_Comment by @konstin on 2025-05-16 12:35_

Just to confirm: Is it the simple HTML format, like a page with lots of `<a href=` to `.whl` files, and in curl, do you also see the response type being `application/octet-stream` instead of `text/html`? Are you running a reverse proxy or something similar that may change the response type?

---

_Comment by @a087861 on 2025-05-16 12:43_

Ahh, I see what you mean.  Running a curl with creds resulted in an HTML that looked significantly more like a web page than something else.  

If I slightly tweak the url that I curl against to "https://progressive.jfrog.io/artifactory/api/pypi/***-pypi/simple/pytz/", then it looks like what you describe.

It almost looks like it's taking our pip_index or it's taking our pip_index_url and removing 'simple' before adding on the package name and thus getting an incorrect response... but that's based off of my very elementary understanding so I could be wrong.

---

_Comment by @konstin on 2025-05-16 13:04_

Can you try replacing

```
UV_INDEX                       https://***:***@***.jfrog.io/artifactory/api/pypi/pgr-pypi
```

with

```
UV_INDEX                       https://***:***@***.jfrog.io/artifactory/api/pypi/pgr-pypi/simple
```


---

_Unassigned @jtfmumm by @konstin on 2025-05-16 13:05_

---

_Label `bug` removed by @konstin on 2025-05-16 13:05_

---

_Label `question` added by @konstin on 2025-05-16 13:05_

---

_Comment by @a087861 on 2025-05-16 13:11_

That did the trick! 

I did a bit more testing to confirm that all other functionality seems to be working properly. Thus far everything seems to be running as expected.

---

_Comment by @mhscott64 on 2025-05-22 12:21_

This resolves my issue.   Feel free to close.
Thank you for your outstanding support!

---

_Closed by @konstin on 2025-05-22 17:17_

---
