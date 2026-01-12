```yaml
number: 13628
title: mod_wsgi compilation and installation seems to fail silently
type: issue
state: closed
author: xnx
labels:
  - question
assignees: []
created_at: 2025-05-23T22:43:01Z
updated_at: 2025-05-27T08:45:56Z
url: https://github.com/astral-sh/uv/issues/13628
synced_at: 2026-01-12T16:01:33Z
```

# mod_wsgi compilation and installation seems to fail silently

---

_@xnx_

### Summary

I am using Ubuntu 24.04.1 LTS and Apache 2.4.58 and a uv viertual environment with Python 3.13.3 and uv 0.7.7.
I have installed the apache2-dev package but forgot to install python3-dev. Nonetheless,
uv add --group production mod_wsgi
seems to compile mod_wsgi-py313.cpython-313-x86_64-linux-gnu.so into .venv/lib/python3.13/site-packages/mod_wsgi/server/ and install the mod-wsgi 5.0.2 package) but this won't work with Apache, which segfaults with errors I assume are related to a Python version mismatch (Python 3.12 is the system Python).

pip install mod_wsgi in a separate virtualenv under the system Python 3.12 behaves as expected (fails to compile until I install python3-dev and then compiles a compatible module after I do; then Apache is happy).

### Platform

Ubuntu 24.04.1 LTS

### Version

uv 0.7.7

### Python version

Python 3.13.3

---

_Label `bug` added by @xnx on 2025-05-23 22:43_

---

_Label `bug` removed by @konstin on 2025-05-26 22:08_

---

_Label `question` added by @konstin on 2025-05-26 22:08_

---

_Comment by @konstin on 2025-05-26 22:10_

uv does not manage anything related to apache and will install packages on the version you request, i.e., if you install a package on the non-system Python 3.13 it may not work with software expecting the system Python 3.12. Given that uv installs mod_wsgi itself correctly, this does not look like a bug in uv.

---

_Closed by @konstin on 2025-05-26 22:10_

---

_Comment by @xnx on 2025-05-27 07:26_

I think the problem might really be that uv is apparently compiling a broken Apache module without warning about the missing python3-dev library. As long as apxs is installed, the Python version shouldn't matter.
If uv is to be a drop-in replacement for pip, it should surely do the same thing and fail to install mod_wsgi if the OS dependencies aren't present?

---

_Comment by @konstin on 2025-05-27 08:45_

uv is not involved in the `mod_wsgi` build process, that is done by the `mod_wsgi` build backend, which decides whether the build fails or not. Apache is a system package, so Python packaging is not aware of Apache and does not handle the interaction.

---
