```yaml
number: 16618
title: Broken cache?
type: issue
state: open
author: hainkind
labels:
  - needs-mre
assignees: []
created_at: 2025-11-06T18:34:04Z
updated_at: 2025-12-16T14:19:33Z
url: https://github.com/astral-sh/uv/issues/16618
synced_at: 2026-01-12T16:02:35Z
```

# Broken cache?

---

_@hainkind_

### Summary

(.venv) ~/recipeArchivist git:[main]
uvx --python 3.13 --with requests python -c "import requests"
Traceback (most recent call last):
  File "<string>", line 1, in <module>
    import requests
  File "/home/hk/.cache/uv/archive-v0/TYwbnz_cNUdWuxOBkEKEa/lib/python3.13/site-packages/requests/__init__.py", line 45, in <module>
    from .exceptions import RequestsDependencyWarning
  File "/home/hk/.cache/uv/archive-v0/TYwbnz_cNUdWuxOBkEKEa/lib/python3.13/site-packages/requests/exceptions.py", line 9, in <module>
    from .compat import JSONDecodeError as CompatJSONDecodeError
ImportError: cannot import name 'JSONDecodeError' from 'requests.compat' (/home/hk/.cache/uv/archive-v0/TYwbnz_cNUdWuxOBkEKEa/lib/python3.13/site-packages/requests/compat.py)

similar issue with python 3.14

somehow my cache ended up broken.

cleaning fixed it...

### Platform

Linux 6.17.7-arch1-1 x86_64 GNU/Linux

### Version

uv 0.9.7 (0adb44480 2025-10-30)

### Python version

3.14, 3.13

---

_Label `bug` added by @hainkind on 2025-11-06 18:34_

---

_Comment by @hainkind on 2025-11-06 18:37_

```
(.venv) ~/recipeArchivist git:[main]
uv cache clean
Clearing cache at: /home/hk/.cache/uv
Removed 2915 files (79.0MiB)
(.venv) ~/recipeArchivist git:[main]
uvx --python 3.14 --with requests python -c "import requests"
Installed 5 packages in 6ms
Traceback (most recent call last):
  File "<string>", line 1, in <module>
    import requests
  File "/home/hk/.cache/uv/archive-v0/bOmimMNhQktjqT22BmUGt/lib/python3.14/site-packages/requests/__init__.py", line 43, in <module>
    import urllib3
  File "/home/hk/.cache/uv/archive-v0/bOmimMNhQktjqT22BmUGt/lib/python3.14/site-packages/urllib3/__init__.py", line 34, in <module>
    if not ssl.OPENSSL_VERSION.startswith("OpenSSL "):  # Defensive:
           ^^^^^^^^^^^^^^^^^^^
AttributeError: module 'ssl' has no attribute 'OPENSSL_VERSION'
```

actually the issue persists??

---

_Comment by @RafaelJohn9 on 2025-11-09 12:27_

Perhaps the issue lies with the packages themselves rather than a corrupted cache.

According to the logs although they vary they all appear to read from the cache successfully, and the errors seem to originate from within the packages’ own code.

---

_Comment by @konstin on 2025-12-16 14:19_

Does it work when using pip?

---

_Label `bug` removed by @konstin on 2025-12-16 14:19_

---

_Label `needs-mre` added by @konstin on 2025-12-16 14:19_

---
