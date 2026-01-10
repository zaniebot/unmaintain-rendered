```yaml
number: 8172
title: "`Missing href attribute on anchor link` when using private pypi"
type: issue
state: closed
author: isaacimholt
labels:
  - bug
assignees: []
created_at: 2024-10-14T11:07:25Z
updated_at: 2025-01-14T11:01:54Z
url: https://github.com/astral-sh/uv/issues/8172
synced_at: 2026-01-10T04:27:58Z
```

# `Missing href attribute on anchor link` when using private pypi

---

_Issue opened by @isaacimholt on 2024-10-14 11:07_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
I am trying to use `uv` on a company project but it is not able to parse the package page. I suspect it is attempting to parse some javascript.

```zsh
❯ uv pip install --index-url https://devpi.my_company.net/root/namespace/+simple my_package --reinstall --verbose
DEBUG uv 0.4.20 (Homebrew 2024-10-08)
DEBUG Searching for default Python interpreter in system path
DEBUG Found `cpython-3.10.15-macos-aarch64-none` at `/Volumes/sourcecode/my_project/.venv/bin/python3` (active virtual environment)
DEBUG Using Python 3.10.15 environment at .venv
DEBUG Acquired lock for `.venv`
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.10.15
DEBUG Solving with target Python version: >=3.10.15
DEBUG Adding direct dependency: my_package*
DEBUG No cache entry for: https://devpi.my_company.net/root/namespace/+simple/my_package/
DEBUG Released lock at `/Volumes/sourcecode/my_project/.venv/.lock`
error: Received some unexpected HTML from https://devpi.my_company.net/root/namespace/+simple/my_package/
  Caused by: Missing href attribute on anchor link
```

I think I cannot paste the source of the page in question, but I searched for `<a` and I found 1 extra occurrence in some javascript which looks like this (this is under a `<head><script type="text/javascript">` html section, if that is relevant):

```javascript
for(t&&t(r);u<a.length;u++)
```

It works fine with `pip`, please let me know if you need any other information to resolve this issue.

---

_Comment by @konstin on 2024-10-14 11:11_

can you try `https://devpi.my_company.net/root/namespace/+simple/`, without the package name in the url?

---

_Comment by @isaacimholt on 2024-10-14 12:04_

> can you try `https://devpi.my_company.net/root/namespace/+simple/`, without the package name in the url?

Sorry about that it was a formatting error, the index url does not include the package name, I've edited the original post

---

_Comment by @charliermarsh on 2024-10-14 13:43_

Can you include some redacted content from https://devpi.my_company.net/root/namespace/+simple/my_package/? Are there a `<a/>` elements without `href` values?

---

_Label `needs-mre` added by @charliermarsh on 2024-10-14 13:43_

---

_Comment by @isaacimholt on 2024-10-21 13:41_

Sorry for the late reply, here is the body of the page:

```html
<body><h1>root/my_namespace: links for my_package</h1>
root/my_namespace <a href="../../+f/086/8962b8e5583b4/my_package-0.2.tar.gz#sha256=0868962b8e5583b4fb05e059025b9ed9b48b9e9499d75c9a800c987823579e78" data-requires-python=">=2.7">my_package-0.2.tar.gz</a><br>
root/my_namespace <a href="../../+f/abe/2ed2f4444082c/my_package-0.2-py2.py3-none-any.whl#sha256=abe2ed2f4444082cadbbd3debc9a1a14500a99d4f8771a2bd9622d9ac15de36e" data-requires-python=">=2.7">my_package-0.2-py2.py3-none-any.whl</a><br>
root/my_namespace <a href="../../+f/9b5/e4768d3dfc9a0/my_package-0.1.2.tar.gz#sha256=9b5e4768d3dfc9a014cad4ed92b320e4d30b5af6d676b780a5341946b6fc29cd" data-requires-python=">=2.7">my_package-0.1.2.tar.gz</a><br>
root/my_namespace <a href="../../+f/97f/3f532c81bc708/my_package-0.1.2-py2.py3-none-any.whl#sha256=97f3f532c81bc708cf4f1a8d29510d483dc6fedc1d3b889783310c8e9446e2d6" data-requires-python=">=2.7">my_package-0.1.2-py2.py3-none-any.whl</a><br>
root/my_namespace <a href="../../+f/159/86c1b2e850a56/my_package-0.1.1.tar.gz#sha256=15986c1b2e850a56eb8fd26e26956b6c0a5c186fcd01cc476dd23b9824511b7b" data-requires-python=">=2.7">my_package-0.1.1.tar.gz</a><br>
root/my_namespace <a href="../../+f/d58/c0ad2b9e84b4f/my_package-0.1.1-py2.py3-none-any.whl#sha256=d58c0ad2b9e84b4f5c3cfccab4d1dcc035cc71dc2f1ac46e9b9d4226d45930e8" data-requires-python=">=2.7">my_package-0.1.1-py2.py3-none-any.whl</a><br>
</body>
```

---

_Comment by @charliermarsh on 2024-10-21 17:45_

Unfortunately I can't reproduce this. Pasting that HTML into our HTML parser gives me the expected output.

---

_Comment by @musab-k on 2024-11-13 18:17_

I don't know if it helps since it sounds like you can see the package via pip, but I just got the same error and it was resolved by ensuring I was on the company VPN.

---

_Comment by @charliermarsh on 2025-01-06 22:07_

Do you mind trying again? We no longer error on this, we just filter them out.

---

_Comment by @isaacimholt on 2025-01-14 10:17_

It does indeed work now, thank you so much!

---

_Closed by @isaacimholt on 2025-01-14 10:17_

---

_Label `needs-mre` removed by @konstin on 2025-01-14 11:01_

---

_Label `bug` added by @konstin on 2025-01-14 11:01_

---
