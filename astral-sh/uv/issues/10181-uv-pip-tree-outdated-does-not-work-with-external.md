```yaml
number: 10181
title: uv pip tree --outdated does not work with external venv
type: issue
state: closed
author: logut
labels:
  - enhancement
assignees: []
created_at: 2024-12-26T22:34:48Z
updated_at: 2024-12-27T17:03:07Z
url: https://github.com/astral-sh/uv/issues/10181
synced_at: 2026-01-12T16:00:07Z
```

# uv pip tree --outdated does not work with external venv

---

_@logut_

Hello,

I have not fully switched to uv yet. I'm using `python3.13 -m venv venv` to create the venv and `./venv/bin/python -m pip` to install in it.
I then try to get `uv pip tree --python ./venv/bin/python --outdated` to show me outdated packages but the flag does not change the output, as if no new version can be found. `uv pip list --python ./venv/bin/python --outdated` does lists the new versions.

<details>
 <summary>Commands to reproduce and logs</summary>

```
➜  ~/dev mkdir repro
➜  ~/dev cd repro 
➜  ~/dev/repro python3.13 -m venv venv
➜  ~/dev/repro ./venv/bin/python -m pip --version
pip 24.2 from /home/logut/dev/repro/venv/lib64/python3.13/site-packages/pip (python 3.13)
➜  ~/dev/repro ./venv/bin/python -m pip install Django==3.2.19
Collecting Django==3.2.19
  Using cached Django-3.2.19-py3-none-any.whl.metadata (4.1 kB)
Collecting asgiref<4,>=3.3.2 (from Django==3.2.19)
  Using cached asgiref-3.8.1-py3-none-any.whl.metadata (9.3 kB)
Collecting pytz (from Django==3.2.19)
  Using cached pytz-2024.2-py2.py3-none-any.whl.metadata (22 kB)
Collecting sqlparse>=0.2.2 (from Django==3.2.19)
  Downloading sqlparse-0.5.3-py3-none-any.whl.metadata (3.9 kB)
Using cached Django-3.2.19-py3-none-any.whl (7.9 MB)
Using cached asgiref-3.8.1-py3-none-any.whl (23 kB)
Downloading sqlparse-0.5.3-py3-none-any.whl (44 kB)
Using cached pytz-2024.2-py2.py3-none-any.whl (508 kB)
Installing collected packages: pytz, sqlparse, asgiref, Django
Successfully installed Django-3.2.19 asgiref-3.8.1 pytz-2024.2 sqlparse-0.5.3

[notice] A new release of pip is available: 24.2 -> 24.3.1
[notice] To update, run: python -m pip install --upgrade pip
➜  ~/dev/repro RUST_LOG=uv=trace uv pip tree --python ./venv/bin/python --outdated --verbose
DEBUG uv 0.5.11
DEBUG Checking for Python interpreter at path `./venv/bin/python`
TRACE Querying interpreter executable at ./venv/bin/python
Using Python 3.13.1 environment at: venv
django v3.2.19
├── asgiref v3.8.1
├── pytz v2024.2
└── sqlparse v0.5.3
pip v24.2
➜  ~/dev/repro RUST_LOG=uv=trace uv pip list --python ./venv/bin/python --outdated --verbose
DEBUG uv 0.5.11
DEBUG Checking for Python interpreter at path `./venv/bin/python`
TRACE Cached interpreter info for Python 3.13.1, skipping probing: ./venv/bin/python
Using Python 3.13.1 environment at: venv
DEBUG Using request timeout of 30s
DEBUG Fetching latest version of: `asgiref`
TRACE Fetching metadata for asgiref from https://pypi.org/simple/asgiref/
DEBUG Fetching latest version of: `django`
TRACE Fetching metadata for django from https://pypi.org/simple/django/
DEBUG Fetching latest version of: `pip`
TRACE Fetching metadata for pip from https://pypi.org/simple/pip/
DEBUG Fetching latest version of: `pytz`
TRACE Fetching metadata for pytz from https://pypi.org/simple/pytz/
DEBUG Fetching latest version of: `sqlparse`
TRACE Fetching metadata for sqlparse from https://pypi.org/simple/sqlparse/
TRACE No cache entry exists for /home/logut/.cache/uv/simple-v14/pypi/asgiref.rkyv
DEBUG No cache entry for: https://pypi.org/simple/asgiref/
TRACE Sending fresh GET request for https://pypi.org/simple/asgiref/
TRACE Handling request for https://pypi.org/simple/asgiref/
TRACE Request for https://pypi.org/simple/asgiref/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/asgiref/
TRACE Attempting unauthenticated request for https://pypi.org/simple/asgiref/
TRACE No cache entry exists for /home/logut/.cache/uv/simple-v14/pypi/django.rkyv
DEBUG No cache entry for: https://pypi.org/simple/django/
TRACE Sending fresh GET request for https://pypi.org/simple/django/
TRACE Handling request for https://pypi.org/simple/django/
TRACE Request for https://pypi.org/simple/django/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/django/
TRACE Attempting unauthenticated request for https://pypi.org/simple/django/
TRACE No cache entry exists for /home/logut/.cache/uv/simple-v14/pypi/pytz.rkyv
DEBUG No cache entry for: https://pypi.org/simple/pytz/
TRACE Sending fresh GET request for https://pypi.org/simple/pytz/
TRACE Handling request for https://pypi.org/simple/pytz/
TRACE Request for https://pypi.org/simple/pytz/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/pytz/
TRACE Attempting unauthenticated request for https://pypi.org/simple/pytz/
TRACE No cache entry exists for /home/logut/.cache/uv/simple-v14/pypi/sqlparse.rkyv
DEBUG No cache entry for: https://pypi.org/simple/sqlparse/
TRACE Sending fresh GET request for https://pypi.org/simple/sqlparse/
TRACE Handling request for https://pypi.org/simple/sqlparse/
TRACE Request for https://pypi.org/simple/sqlparse/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/sqlparse/
TRACE Attempting unauthenticated request for https://pypi.org/simple/sqlparse/
TRACE cached request https://pypi.org/simple/pip/ is storable because its response has a 'public' cache-control directive
TRACE freshness lifetime found via cache-control max age setting: 600s
TRACE request https://pypi.org/simple/pip/ does not have a fresh cache because it has a 'no-cache' cache-control directive
DEBUG Found stale response for: https://pypi.org/simple/pip/
DEBUG Sending revalidation request for: https://pypi.org/simple/pip/
TRACE Handling request for https://pypi.org/simple/pip/
TRACE Request for https://pypi.org/simple/pip/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/pip/
TRACE Attempting unauthenticated request for https://pypi.org/simple/pip/
TRACE cached request https://pypi.org/simple/sqlparse/ is storable because its response has a 'public' cache-control directive
TRACE cached request https://pypi.org/simple/django/ is storable because its response has a 'public' cache-control directive
TRACE cached request https://pypi.org/simple/pytz/ is storable because its response has a 'public' cache-control directive
TRACE not modified because old and new etag values ([34, 90, 110, 118, 120, 97, 75, 68, 115, 84, 51, 65, 117, 90, 122, 81, 57, 82, 110, 88, 99, 110, 65, 34]) match
DEBUG Found not-modified response for: https://pypi.org/simple/pip/
WARN Skipping file for pytz: pytz-2004d.tar.gz
WARN Skipping file for pytz: pytz-2005e.tar.gz
WARN Skipping file for pytz: pytz-2005i.tar.gz
WARN Skipping file for pytz: pytz-2005k.tar.bz2
WARN Skipping file for pytz: pytz-2005k.tar.gz
WARN Skipping file for pytz: pytz-2005k.zip
WARN Skipping file for pytz: pytz-2005m.tar.bz2
WARN Skipping file for pytz: pytz-2005m.tar.gz
WARN Skipping file for pytz: pytz-2005m.zip
WARN Skipping file for pytz: pytz-2006g-py2.4.egg
WARN Skipping file for pytz: pytz-2006g.tar.bz2
WARN Skipping file for pytz: pytz-2006g.tar.gz
WARN Skipping file for pytz: pytz-2006g.zip
WARN Skipping file for pytz: pytz-2006j-py2.4.egg
WARN Skipping file for pytz: pytz-2006j.tar.bz2
WARN Skipping file for pytz: pytz-2006j.tar.gz
WARN Skipping file for pytz: pytz-2006j.zip
WARN Skipping file for pytz: pytz-2006p-py2.4.egg
WARN Skipping file for pytz: pytz-2006p-py2.5.egg
WARN Skipping file for pytz: pytz-2006p.tar.bz2
WARN Skipping file for pytz: pytz-2006p.tar.gz
WARN Skipping file for pytz: pytz-2006p.zip
WARN Skipping file for pytz: pytz-2007c-py2.4.egg
WARN Skipping file for pytz: pytz-2007c-py2.5.egg
WARN Skipping file for pytz: pytz-2007d-py2.3.egg
WARN Skipping file for pytz: pytz-2007d-py2.4.egg
WARN Skipping file for pytz: pytz-2007d-py2.5.egg
WARN Skipping file for pytz: pytz-2007d.tar.bz2
WARN Skipping file for pytz: pytz-2007d.tar.gz
WARN Skipping file for pytz: pytz-2007d.zip
WARN Skipping file for pytz: pytz-2007f-py2.3.egg
WARN Skipping file for pytz: pytz-2007f-py2.4.egg
WARN Skipping file for pytz: pytz-2007f-py2.5.egg
WARN Skipping file for pytz: pytz-2007f.tar.bz2
WARN Skipping file for pytz: pytz-2007f.tar.gz
WARN Skipping file for pytz: pytz-2007f.zip
WARN Skipping file for pytz: pytz-2007g-py2.3.egg
WARN Skipping file for pytz: pytz-2007g-py2.4.egg
WARN Skipping file for pytz: pytz-2007g-py2.5.egg
WARN Skipping file for pytz: pytz-2007g.tar.bz2
WARN Skipping file for pytz: pytz-2007g.tar.gz
WARN Skipping file for pytz: pytz-2007g.zip
WARN Skipping file for pytz: pytz-2007i-py2.3.egg
WARN Skipping file for pytz: pytz-2007i-py2.4.egg
WARN Skipping file for pytz: pytz-2007i-py2.5.egg
WARN Skipping file for pytz: pytz-2007i.tar.bz2
WARN Skipping file for pytz: pytz-2007i.tar.gz
WARN Skipping file for pytz: pytz-2007i.zip
WARN Skipping file for pytz: pytz-2007k-py2.3.egg
WARN Skipping file for pytz: pytz-2007k-py2.4.egg
WARN Skipping file for pytz: pytz-2007k-py2.5.egg
WARN Skipping file for pytz: pytz-2007k.tar.bz2
WARN Skipping file for pytz: pytz-2007k.tar.gz
WARN Skipping file for pytz: pytz-2007k.zip
WARN Skipping file for pytz: pytz-2008a-py2.3.egg
WARN Skipping file for pytz: pytz-2008a-py2.4.egg
WARN Skipping file for pytz: pytz-2008a-py2.5.egg
WARN Skipping file for pytz: pytz-2008b-py2.3.egg
WARN Skipping file for pytz: pytz-2008b-py2.4.egg
WARN Skipping file for pytz: pytz-2008b-py2.5.egg
WARN Skipping file for pytz: pytz-2008c-py2.3.egg
WARN Skipping file for pytz: pytz-2008c-py2.4.egg
WARN Skipping file for pytz: pytz-2008c-py2.5.egg
WARN Skipping file for pytz: pytz-2008g-py2.3.egg
WARN Skipping file for pytz: pytz-2008g-py2.4.egg
WARN Skipping file for pytz: pytz-2008g-py2.5.egg
WARN Skipping file for pytz: pytz-2008g-py2.6.egg
WARN Skipping file for pytz: pytz-2008g.tar.bz2
WARN Skipping file for pytz: pytz-2008g.tar.gz
WARN Skipping file for pytz: pytz-2008g.zip
WARN Skipping file for pytz: pytz-2008h-py2.3.egg
WARN Skipping file for pytz: pytz-2008h-py2.4.egg
WARN Skipping file for pytz: pytz-2008h-py2.5.egg
WARN Skipping file for pytz: pytz-2008h-py2.6.egg
WARN Skipping file for pytz: pytz-2008h.tar.bz2
WARN Skipping file for pytz: pytz-2008h.tar.gz
WARN Skipping file for pytz: pytz-2008h.zip
WARN Skipping file for pytz: pytz-2008i-py2.3.egg
WARN Skipping file for pytz: pytz-2008i-py2.4.egg
WARN Skipping file for pytz: pytz-2008i-py2.5.egg
WARN Skipping file for pytz: pytz-2008i-py2.6.egg
WARN Skipping file for pytz: pytz-2008i.tar.bz2
WARN Skipping file for pytz: pytz-2008i.tar.gz
WARN Skipping file for pytz: pytz-2008i.zip
WARN Skipping file for pytz: pytz-2009a-py2.3.egg
WARN Skipping file for pytz: pytz-2009a-py2.4.egg
WARN Skipping file for pytz: pytz-2009a-py2.5.egg
WARN Skipping file for pytz: pytz-2009a-py2.6.egg
WARN Skipping file for pytz: pytz-2009d-py2.3.egg
WARN Skipping file for pytz: pytz-2009d-py2.4.egg
WARN Skipping file for pytz: pytz-2009d-py2.5.egg
WARN Skipping file for pytz: pytz-2009d-py2.6.egg
WARN Skipping file for pytz: pytz-2009d.tar.bz2
WARN Skipping file for pytz: pytz-2009d.tar.gz
WARN Skipping file for pytz: pytz-2009d.zip
WARN Skipping file for pytz: pytz-2009e-py2.3.egg
WARN Skipping file for pytz: pytz-2009e-py2.4.egg
WARN Skipping file for pytz: pytz-2009e-py2.5.egg
WARN Skipping file for pytz: pytz-2009e-py2.6.egg
WARN Skipping file for pytz: pytz-2009e.tar.bz2
WARN Skipping file for pytz: pytz-2009e.tar.gz
WARN Skipping file for pytz: pytz-2009e.zip
WARN Skipping file for pytz: pytz-2009f-py2.3.egg
WARN Skipping file for pytz: pytz-2009f-py2.4.egg
WARN Skipping file for pytz: pytz-2009f-py2.5.egg
WARN Skipping file for pytz: pytz-2009f-py2.6.egg
WARN Skipping file for pytz: pytz-2009f.tar.bz2
WARN Skipping file for pytz: pytz-2009f.tar.gz
WARN Skipping file for pytz: pytz-2009f.zip
WARN Skipping file for pytz: pytz-2009g-py2.3.egg
WARN Skipping file for pytz: pytz-2009g-py2.4.egg
WARN Skipping file for pytz: pytz-2009g-py2.5.egg
WARN Skipping file for pytz: pytz-2009g-py2.6.egg
WARN Skipping file for pytz: pytz-2009g.tar.bz2
WARN Skipping file for pytz: pytz-2009g.tar.gz
WARN Skipping file for pytz: pytz-2009g.zip
WARN Skipping file for pytz: pytz-2009i-py2.3.egg
WARN Skipping file for pytz: pytz-2009i-py2.4.egg
WARN Skipping file for pytz: pytz-2009i-py2.5.egg
WARN Skipping file for pytz: pytz-2009i-py2.6.egg
WARN Skipping file for pytz: pytz-2009i.tar.bz2
WARN Skipping file for pytz: pytz-2009i.tar.gz
WARN Skipping file for pytz: pytz-2009i.zip
WARN Skipping file for pytz: pytz-2009j-py2.3.egg
WARN Skipping file for pytz: pytz-2009j-py2.4.egg
WARN Skipping file for pytz: pytz-2009j-py2.5.egg
WARN Skipping file for pytz: pytz-2009j-py2.6.egg
WARN Skipping file for pytz: pytz-2009j.tar.bz2
WARN Skipping file for pytz: pytz-2009j.tar.gz
WARN Skipping file for pytz: pytz-2009j.zip
WARN Skipping file for pytz: pytz-2009l-py2.3.egg
WARN Skipping file for pytz: pytz-2009l-py2.4.egg
WARN Skipping file for pytz: pytz-2009l-py2.5.egg
WARN Skipping file for pytz: pytz-2009l-py2.6.egg
WARN Skipping file for pytz: pytz-2009l.tar.bz2
WARN Skipping file for pytz: pytz-2009l.tar.gz
WARN Skipping file for pytz: pytz-2009l.zip
WARN Skipping file for pytz: pytz-2009n-py2.3.egg
WARN Skipping file for pytz: pytz-2009n-py2.4.egg
WARN Skipping file for pytz: pytz-2009n-py2.5.egg
WARN Skipping file for pytz: pytz-2009n-py2.6.egg
WARN Skipping file for pytz: pytz-2009n.tar.bz2
WARN Skipping file for pytz: pytz-2009n.tar.gz
WARN Skipping file for pytz: pytz-2009n.zip
WARN Skipping file for pytz: pytz-2009p-py2.3.egg
WARN Skipping file for pytz: pytz-2009p-py2.4.egg
WARN Skipping file for pytz: pytz-2009p-py2.5.egg
WARN Skipping file for pytz: pytz-2009p-py2.6.egg
WARN Skipping file for pytz: pytz-2009p.tar.bz2
WARN Skipping file for pytz: pytz-2009p.tar.gz
WARN Skipping file for pytz: pytz-2009p.zip
WARN Skipping file for pytz: pytz-2009r-py2.3.egg
WARN Skipping file for pytz: pytz-2009r-py2.4.egg
WARN Skipping file for pytz: pytz-2009r-py2.5.egg
WARN Skipping file for pytz: pytz-2009r-py2.6.egg
WARN Skipping file for pytz: pytz-2009u-py2.3.egg
WARN Skipping file for pytz: pytz-2009u-py2.4.egg
WARN Skipping file for pytz: pytz-2009u-py2.5.egg
WARN Skipping file for pytz: pytz-2009u-py2.6.egg
WARN Skipping file for pytz: pytz-2009u.tar.bz2
WARN Skipping file for pytz: pytz-2009u.tar.gz
WARN Skipping file for pytz: pytz-2009u.zip
WARN Skipping file for pytz: pytz-2010b-py2.3.egg
WARN Skipping file for pytz: pytz-2010b-py2.4.egg
WARN Skipping file for pytz: pytz-2010b-py2.5.egg
WARN Skipping file for pytz: pytz-2010b-py2.6.egg
WARN Skipping file for pytz: pytz-2010e-py2.3.egg
WARN Skipping file for pytz: pytz-2010e-py2.4.egg
WARN Skipping file for pytz: pytz-2010e-py2.5.egg
WARN Skipping file for pytz: pytz-2010e-py2.6.egg
WARN Skipping file for pytz: pytz-2010e.tar.bz2
WARN Skipping file for pytz: pytz-2010e.tar.gz
WARN Skipping file for pytz: pytz-2010e.zip
WARN Skipping file for pytz: pytz-2010g-py2.3.egg
WARN Skipping file for pytz: pytz-2010g-py2.4.egg
WARN Skipping file for pytz: pytz-2010g-py2.5.egg
WARN Skipping file for pytz: pytz-2010g-py2.6.egg
WARN Skipping file for pytz: pytz-2010g.tar.bz2
WARN Skipping file for pytz: pytz-2010g.tar.gz
WARN Skipping file for pytz: pytz-2010g.zip
WARN Skipping file for pytz: pytz-2010h-py2.3.egg
WARN Skipping file for pytz: pytz-2010h-py2.4.egg
WARN Skipping file for pytz: pytz-2010h-py2.5.egg
WARN Skipping file for pytz: pytz-2010h-py2.6.egg
WARN Skipping file for pytz: pytz-2010h.tar.bz2
WARN Skipping file for pytz: pytz-2010h.tar.gz
WARN Skipping file for pytz: pytz-2010h.zip
WARN Skipping file for pytz: pytz-2010k-py2.3.egg
WARN Skipping file for pytz: pytz-2010k-py2.4.egg
WARN Skipping file for pytz: pytz-2010k-py2.5.egg
WARN Skipping file for pytz: pytz-2010k-py2.6.egg
WARN Skipping file for pytz: pytz-2010k.tar.bz2
WARN Skipping file for pytz: pytz-2010k.tar.gz
WARN Skipping file for pytz: pytz-2010k.zip
WARN Skipping file for pytz: pytz-2010l-py2.3.egg
WARN Skipping file for pytz: pytz-2010l-py2.4.egg
WARN Skipping file for pytz: pytz-2010l-py2.5.egg
WARN Skipping file for pytz: pytz-2010l-py2.6.egg
WARN Skipping file for pytz: pytz-2010l.tar.bz2
WARN Skipping file for pytz: pytz-2010l.tar.gz
WARN Skipping file for pytz: pytz-2010l.zip
WARN Skipping file for pytz: pytz-2010o-py2.3.egg
WARN Skipping file for pytz: pytz-2010o-py2.4.egg
WARN Skipping file for pytz: pytz-2010o-py2.5.egg
WARN Skipping file for pytz: pytz-2010o-py2.6.egg
WARN Skipping file for pytz: pytz-2010o-py2.7.egg
WARN Skipping file for pytz: pytz-2010o.tar.bz2
WARN Skipping file for pytz: pytz-2010o.tar.gz
WARN Skipping file for pytz: pytz-2010o.zip
WARN Skipping file for pytz: pytz-2011b-py2.4.egg
WARN Skipping file for pytz: pytz-2011b-py2.5.egg
WARN Skipping file for pytz: pytz-2011b-py2.6.egg
WARN Skipping file for pytz: pytz-2011b-py2.7.egg
WARN Skipping file for pytz: pytz-2011b-py3.1.egg
WARN Skipping file for pytz: pytz-2011c-py2.4.egg
WARN Skipping file for pytz: pytz-2011c-py2.5.egg
WARN Skipping file for pytz: pytz-2011c-py2.6.egg
WARN Skipping file for pytz: pytz-2011c-py2.7.egg
WARN Skipping file for pytz: pytz-2011c-py3.1.egg
WARN Skipping file for pytz: pytz-2011c-py3.2.egg
WARN Skipping file for pytz: pytz-2011d-py2.4.egg
WARN Skipping file for pytz: pytz-2011d-py2.5.egg
WARN Skipping file for pytz: pytz-2011d-py2.6.egg
WARN Skipping file for pytz: pytz-2011d-py2.7.egg
WARN Skipping file for pytz: pytz-2011d-py3.1.egg
WARN Skipping file for pytz: pytz-2011d-py3.2.egg
WARN Skipping file for pytz: pytz-2011d.tar.bz2
WARN Skipping file for pytz: pytz-2011d.tar.gz
WARN Skipping file for pytz: pytz-2011d.zip
WARN Skipping file for pytz: pytz-2011e-py2.4.egg
WARN Skipping file for pytz: pytz-2011e-py2.5.egg
WARN Skipping file for pytz: pytz-2011e-py2.6.egg
WARN Skipping file for pytz: pytz-2011e-py2.7.egg
WARN Skipping file for pytz: pytz-2011e-py3.1.egg
WARN Skipping file for pytz: pytz-2011e-py3.2.egg
WARN Skipping file for pytz: pytz-2011e.tar.bz2
WARN Skipping file for pytz: pytz-2011e.tar.gz
WARN Skipping file for pytz: pytz-2011e.zip
WARN Skipping file for pytz: pytz-2011g-py2.4.egg
WARN Skipping file for pytz: pytz-2011g-py2.5.egg
WARN Skipping file for pytz: pytz-2011g-py2.6.egg
WARN Skipping file for pytz: pytz-2011g-py2.7.egg
WARN Skipping file for pytz: pytz-2011g-py3.1.egg
WARN Skipping file for pytz: pytz-2011g-py3.2.egg
WARN Skipping file for pytz: pytz-2011g.tar.bz2
WARN Skipping file for pytz: pytz-2011g.tar.gz
WARN Skipping file for pytz: pytz-2011g.zip
WARN Skipping file for pytz: pytz-2011h-py2.4.egg
WARN Skipping file for pytz: pytz-2011h-py2.5.egg
WARN Skipping file for pytz: pytz-2011h-py2.6.egg
WARN Skipping file for pytz: pytz-2011h-py2.7.egg
WARN Skipping file for pytz: pytz-2011h-py3.1.egg
WARN Skipping file for pytz: pytz-2011h-py3.2.egg
WARN Skipping file for pytz: pytz-2011h.tar.bz2
WARN Skipping file for pytz: pytz-2011h.tar.gz
WARN Skipping file for pytz: pytz-2011h.zip
WARN Skipping file for pytz: pytz-2011j-py2.4.egg
WARN Skipping file for pytz: pytz-2011j-py2.5.egg
WARN Skipping file for pytz: pytz-2011j-py2.6.egg
WARN Skipping file for pytz: pytz-2011j-py2.7.egg
WARN Skipping file for pytz: pytz-2011j-py3.1.egg
WARN Skipping file for pytz: pytz-2011j-py3.2.egg
WARN Skipping file for pytz: pytz-2011j.tar.bz2
WARN Skipping file for pytz: pytz-2011j.tar.gz
WARN Skipping file for pytz: pytz-2011j.zip
WARN Skipping file for pytz: pytz-2011k-py2.4.egg
WARN Skipping file for pytz: pytz-2011k-py2.5.egg
WARN Skipping file for pytz: pytz-2011k-py2.6.egg
WARN Skipping file for pytz: pytz-2011k-py2.7.egg
WARN Skipping file for pytz: pytz-2011k-py3.1.egg
WARN Skipping file for pytz: pytz-2011k-py3.2.egg
WARN Skipping file for pytz: pytz-2011k.tar.bz2
WARN Skipping file for pytz: pytz-2011k.tar.gz
WARN Skipping file for pytz: pytz-2011k.zip
WARN Skipping file for pytz: pytz-2011n-py2.4.egg
WARN Skipping file for pytz: pytz-2011n-py2.5.egg
WARN Skipping file for pytz: pytz-2011n-py2.6.egg
WARN Skipping file for pytz: pytz-2011n-py2.7.egg
WARN Skipping file for pytz: pytz-2011n-py3.1.egg
WARN Skipping file for pytz: pytz-2011n-py3.2.egg
WARN Skipping file for pytz: pytz-2011n.tar.bz2
WARN Skipping file for pytz: pytz-2011n.tar.gz
WARN Skipping file for pytz: pytz-2011n.zip
WARN Skipping file for pytz: pytz-2012b-py2.4.egg
WARN Skipping file for pytz: pytz-2012b-py2.5.egg
WARN Skipping file for pytz: pytz-2012b-py2.6.egg
WARN Skipping file for pytz: pytz-2012b-py2.7.egg
WARN Skipping file for pytz: pytz-2012b-py3.1.egg
WARN Skipping file for pytz: pytz-2012b-py3.2.egg
WARN Skipping file for pytz: pytz-2012c-py2.4.egg
WARN Skipping file for pytz: pytz-2012c-py2.5.egg
WARN Skipping file for pytz: pytz-2012c-py2.6.egg
WARN Skipping file for pytz: pytz-2012c-py2.7.egg
WARN Skipping file for pytz: pytz-2012c-py3.1.egg
WARN Skipping file for pytz: pytz-2012c-py3.2.egg
WARN Skipping file for pytz: pytz-2012d-py2.4.egg
WARN Skipping file for pytz: pytz-2012d-py2.5.egg
WARN Skipping file for pytz: pytz-2012d-py2.6.egg
WARN Skipping file for pytz: pytz-2012d-py2.7.egg
WARN Skipping file for pytz: pytz-2012d-py3.1.egg
WARN Skipping file for pytz: pytz-2012d-py3.2.egg
WARN Skipping file for pytz: pytz-2012d.tar.bz2
WARN Skipping file for pytz: pytz-2012d.tar.gz
WARN Skipping file for pytz: pytz-2012d.zip
WARN Skipping file for pytz: pytz-2012f-py2.4.egg
WARN Skipping file for pytz: pytz-2012f-py2.5.egg
WARN Skipping file for pytz: pytz-2012f-py2.6.egg
WARN Skipping file for pytz: pytz-2012f-py2.7.egg
WARN Skipping file for pytz: pytz-2012f-py3.1.egg
WARN Skipping file for pytz: pytz-2012f-py3.2.egg
WARN Skipping file for pytz: pytz-2012f.tar.bz2
WARN Skipping file for pytz: pytz-2012f.tar.gz
WARN Skipping file for pytz: pytz-2012f.zip
WARN Skipping file for pytz: pytz-2012g-py2.4.egg
WARN Skipping file for pytz: pytz-2012g-py2.5.egg
WARN Skipping file for pytz: pytz-2012g-py2.6.egg
WARN Skipping file for pytz: pytz-2012g-py2.7.egg
WARN Skipping file for pytz: pytz-2012g-py3.1.egg
WARN Skipping file for pytz: pytz-2012g-py3.2.egg
WARN Skipping file for pytz: pytz-2012g.tar.bz2
WARN Skipping file for pytz: pytz-2012g.tar.gz
WARN Skipping file for pytz: pytz-2012g.zip
WARN Skipping file for pytz: pytz-2012h-py2.4.egg
WARN Skipping file for pytz: pytz-2012h-py2.5.egg
WARN Skipping file for pytz: pytz-2012h-py2.6.egg
WARN Skipping file for pytz: pytz-2012h-py2.7.egg
WARN Skipping file for pytz: pytz-2012h-py3.1.egg
WARN Skipping file for pytz: pytz-2012h-py3.2.egg
WARN Skipping file for pytz: pytz-2012h.tar.bz2
WARN Skipping file for pytz: pytz-2012h.tar.gz
WARN Skipping file for pytz: pytz-2012j-py2.4.egg
WARN Skipping file for pytz: pytz-2012j-py2.5.egg
WARN Skipping file for pytz: pytz-2012j-py2.6.egg
WARN Skipping file for pytz: pytz-2012j-py2.7.egg
WARN Skipping file for pytz: pytz-2012j-py3.1.egg
WARN Skipping file for pytz: pytz-2012j-py3.2.egg
WARN Skipping file for pytz: pytz-2012j.tar.bz2
WARN Skipping file for pytz: pytz-2012j.tar.gz
WARN Skipping file for pytz: pytz-2012j.zip
WARN Skipping file for pytz: pytz-2013.6-py2.4.egg
WARN Skipping file for pytz: pytz-2013.6-py2.5.egg
WARN Skipping file for pytz: pytz-2013.6-py2.6.egg
WARN Skipping file for pytz: pytz-2013.6-py2.7.egg
WARN Skipping file for pytz: pytz-2013.6-py3.1.egg
WARN Skipping file for pytz: pytz-2013.6-py3.2.egg
WARN Skipping file for pytz: pytz-2013.6-py3.3.egg
WARN Skipping file for pytz: pytz-2013.7-py2.4.egg
WARN Skipping file for pytz: pytz-2013.7-py2.5.egg
WARN Skipping file for pytz: pytz-2013.7-py2.6.egg
WARN Skipping file for pytz: pytz-2013.7-py2.7.egg
WARN Skipping file for pytz: pytz-2013.7-py3.3.egg
WARN Skipping file for pytz: pytz-2013.8-py2.4.egg
WARN Skipping file for pytz: pytz-2013.8-py2.5.egg
WARN Skipping file for pytz: pytz-2013.8-py2.6.egg
WARN Skipping file for pytz: pytz-2013.8-py2.7.egg
WARN Skipping file for pytz: pytz-2013.8-py3.1.egg
WARN Skipping file for pytz: pytz-2013.8-py3.2.egg
WARN Skipping file for pytz: pytz-2013.8-py3.3.egg
WARN Skipping file for pytz: pytz-2013.9-py2.4.egg
WARN Skipping file for pytz: pytz-2013.9-py2.5.egg
WARN Skipping file for pytz: pytz-2013.9-py2.6.egg
WARN Skipping file for pytz: pytz-2013.9-py2.7.egg
WARN Skipping file for pytz: pytz-2013.9-py3.2.egg
WARN Skipping file for pytz: pytz-2013.9-py3.3.egg
WARN Skipping file for pytz: pytz-2013b-py2.4.egg
WARN Skipping file for pytz: pytz-2013b-py2.5.egg
WARN Skipping file for pytz: pytz-2013b-py2.6.egg
WARN Skipping file for pytz: pytz-2013b-py2.7.egg
WARN Skipping file for pytz: pytz-2013b-py3.1.egg
WARN Skipping file for pytz: pytz-2013b-py3.2.egg
WARN Skipping file for pytz: pytz-2013d-py2.4.egg
WARN Skipping file for pytz: pytz-2013d-py2.5.egg
WARN Skipping file for pytz: pytz-2013d-py2.6.egg
WARN Skipping file for pytz: pytz-2013d-py2.7.egg
WARN Skipping file for pytz: pytz-2013d-py3.1.egg
WARN Skipping file for pytz: pytz-2013d-py3.2.egg
WARN Skipping file for pytz: pytz-2013d-py3.3.egg
WARN Skipping file for pytz: pytz-2013d.tar.bz2
WARN Skipping file for pytz: pytz-2013d.tar.gz
WARN Skipping file for pytz: pytz-2013d.zip
WARN Skipping file for pytz: pytz-2014.1.1-py2.4.egg
WARN Skipping file for pytz: pytz-2014.1.1-py2.5.egg
WARN Skipping file for pytz: pytz-2014.1.1-py2.6.egg
WARN Skipping file for pytz: pytz-2014.1.1-py2.7.egg
WARN Skipping file for pytz: pytz-2014.1.1-py3.3.egg
WARN Skipping file for pytz: pytz-2014.10-py2.4.egg
WARN Skipping file for pytz: pytz-2014.10-py2.5.egg
WARN Skipping file for pytz: pytz-2014.10-py2.6.egg
WARN Skipping file for pytz: pytz-2014.10-py2.7.egg
WARN Skipping file for pytz: pytz-2014.10-py3.1.egg
WARN Skipping file for pytz: pytz-2014.10-py3.2.egg
WARN Skipping file for pytz: pytz-2014.10-py3.3.egg
WARN Skipping file for pytz: pytz-2014.10-py3.4.egg
WARN Skipping file for pytz: pytz-2014.2-py2.4.egg
WARN Skipping file for pytz: pytz-2014.2-py2.5.egg
WARN Skipping file for pytz: pytz-2014.2-py2.6.egg
WARN Skipping file for pytz: pytz-2014.3-py2.4.egg
WARN Skipping file for pytz: pytz-2014.3-py2.5.egg
WARN Skipping file for pytz: pytz-2014.3-py2.6.egg
WARN Skipping file for pytz: pytz-2014.3-py2.7.egg
WARN Skipping file for pytz: pytz-2014.3-py3.3.egg
WARN Skipping file for pytz: pytz-2014.4-py2.4.egg
WARN Skipping file for pytz: pytz-2014.4-py2.5.egg
WARN Skipping file for pytz: pytz-2014.4-py2.6.egg
WARN Skipping file for pytz: pytz-2014.4-py2.7.egg
WARN Skipping file for pytz: pytz-2014.4-py3.3.egg
WARN Skipping file for pytz: pytz-2014.7-py2.4.egg
WARN Skipping file for pytz: pytz-2014.7-py2.5.egg
WARN Skipping file for pytz: pytz-2014.7-py2.6.egg
WARN Skipping file for pytz: pytz-2014.7-py2.7.egg
WARN Skipping file for pytz: pytz-2014.7-py3.3.egg
WARN Skipping file for pytz: pytz-2014.9-py2.4.egg
WARN Skipping file for pytz: pytz-2014.9-py2.5.egg
WARN Skipping file for pytz: pytz-2014.9-py2.6.egg
WARN Skipping file for pytz: pytz-2014.9-py2.7.egg
WARN Skipping file for pytz: pytz-2014.9-py3.1.egg
WARN Skipping file for pytz: pytz-2014.9-py3.2.egg
WARN Skipping file for pytz: pytz-2014.9-py3.3.egg
WARN Skipping file for pytz: pytz-2014.9-py3.4.egg
WARN Skipping file for pytz: pytz-2015.2-py2.4.egg
WARN Skipping file for pytz: pytz-2015.2-py2.5.egg
WARN Skipping file for pytz: pytz-2015.2-py2.6.egg
WARN Skipping file for pytz: pytz-2015.2-py2.7.egg
WARN Skipping file for pytz: pytz-2015.2-py3.1.egg
WARN Skipping file for pytz: pytz-2015.2-py3.2.egg
WARN Skipping file for pytz: pytz-2015.2-py3.3.egg
WARN Skipping file for pytz: pytz-2015.2-py3.4.egg
WARN Skipping file for pytz: pytz-2015.4-py2.4.egg
WARN Skipping file for pytz: pytz-2015.4-py2.5.egg
WARN Skipping file for pytz: pytz-2015.4-py2.6.egg
WARN Skipping file for pytz: pytz-2015.4-py2.7.egg
WARN Skipping file for pytz: pytz-2015.4-py3.1.egg
WARN Skipping file for pytz: pytz-2015.4-py3.2.egg
WARN Skipping file for pytz: pytz-2015.4-py3.3.egg
WARN Skipping file for pytz: pytz-2015.4-py3.4.egg
WARN Skipping file for pytz: pytz-2015.6-py2.4.egg
WARN Skipping file for pytz: pytz-2015.6-py2.5.egg
WARN Skipping file for pytz: pytz-2015.6-py2.6.egg
WARN Skipping file for pytz: pytz-2015.6-py2.7.egg
WARN Skipping file for pytz: pytz-2015.6-py3.1.egg
WARN Skipping file for pytz: pytz-2015.6-py3.2.egg
WARN Skipping file for pytz: pytz-2015.6-py3.3.egg
WARN Skipping file for pytz: pytz-2015.6-py3.4.egg
WARN Skipping file for pytz: pytz-2015.6-py3.5.egg
WARN Skipping file for pytz: pytz-2015.7-py2.4.egg
WARN Skipping file for pytz: pytz-2015.7-py2.5.egg
WARN Skipping file for pytz: pytz-2015.7-py2.6.egg
WARN Skipping file for pytz: pytz-2015.7-py2.7.egg
WARN Skipping file for pytz: pytz-2015.7-py3.1.egg
WARN Skipping file for pytz: pytz-2015.7-py3.2.egg
WARN Skipping file for pytz: pytz-2015.7-py3.3.egg
WARN Skipping file for pytz: pytz-2015.7-py3.4.egg
WARN Skipping file for pytz: pytz-2015.7-py3.5.egg
WARN Skipping file for pytz: pytz-2016.1-py2.4.egg
WARN Skipping file for pytz: pytz-2016.1-py2.5.egg
WARN Skipping file for pytz: pytz-2016.1-py2.6.egg
WARN Skipping file for pytz: pytz-2016.1-py2.7.egg
WARN Skipping file for pytz: pytz-2016.1-py3.1.egg
WARN Skipping file for pytz: pytz-2016.1-py3.2.egg
WARN Skipping file for pytz: pytz-2016.1-py3.3.egg
WARN Skipping file for pytz: pytz-2016.1-py3.4.egg
WARN Skipping file for pytz: pytz-2016.1-py3.5.egg
WARN Skipping file for pytz: pytz-2016.10-py2.4.egg
WARN Skipping file for pytz: pytz-2016.10-py2.5.egg
WARN Skipping file for pytz: pytz-2016.10-py2.6.egg
WARN Skipping file for pytz: pytz-2016.10-py2.7.egg
WARN Skipping file for pytz: pytz-2016.10-py3.1.egg
WARN Skipping file for pytz: pytz-2016.10-py3.2.egg
WARN Skipping file for pytz: pytz-2016.10-py3.3.egg
WARN Skipping file for pytz: pytz-2016.10-py3.4.egg
WARN Skipping file for pytz: pytz-2016.10-py3.5.egg
WARN Skipping file for pytz: pytz-2016.2-py2.4.egg
WARN Skipping file for pytz: pytz-2016.2-py2.5.egg
WARN Skipping file for pytz: pytz-2016.2-py2.6.egg
WARN Skipping file for pytz: pytz-2016.2-py2.7.egg
WARN Skipping file for pytz: pytz-2016.2-py3.1.egg
WARN Skipping file for pytz: pytz-2016.2-py3.2.egg
WARN Skipping file for pytz: pytz-2016.2-py3.3.egg
WARN Skipping file for pytz: pytz-2016.2-py3.4.egg
WARN Skipping file for pytz: pytz-2016.2-py3.5.egg
WARN Skipping file for pytz: pytz-2016.3-py2.4.egg
WARN Skipping file for pytz: pytz-2016.3-py2.5.egg
WARN Skipping file for pytz: pytz-2016.3-py2.6.egg
WARN Skipping file for pytz: pytz-2016.3-py2.7.egg
WARN Skipping file for pytz: pytz-2016.3-py3.1.egg
WARN Skipping file for pytz: pytz-2016.3-py3.2.egg
WARN Skipping file for pytz: pytz-2016.3-py3.3.egg
WARN Skipping file for pytz: pytz-2016.3-py3.4.egg
WARN Skipping file for pytz: pytz-2016.3-py3.5.egg
WARN Skipping file for pytz: pytz-2016.4-py2.4.egg
WARN Skipping file for pytz: pytz-2016.4-py2.5.egg
WARN Skipping file for pytz: pytz-2016.4-py2.6.egg
WARN Skipping file for pytz: pytz-2016.4-py2.7.egg
WARN Skipping file for pytz: pytz-2016.4-py3.1.egg
WARN Skipping file for pytz: pytz-2016.4-py3.2.egg
WARN Skipping file for pytz: pytz-2016.4-py3.3.egg
WARN Skipping file for pytz: pytz-2016.4-py3.4.egg
WARN Skipping file for pytz: pytz-2016.4-py3.5.egg
WARN Skipping file for pytz: pytz-2016.6-py2.4.egg
WARN Skipping file for pytz: pytz-2016.6-py2.5.egg
WARN Skipping file for pytz: pytz-2016.6-py2.6.egg
WARN Skipping file for pytz: pytz-2016.6-py2.7.egg
WARN Skipping file for pytz: pytz-2016.6-py3.1.egg
WARN Skipping file for pytz: pytz-2016.6-py3.2.egg
WARN Skipping file for pytz: pytz-2016.6-py3.3.egg
WARN Skipping file for pytz: pytz-2016.6-py3.4.egg
WARN Skipping file for pytz: pytz-2016.6-py3.5.egg
WARN Skipping file for pytz: pytz-2016.6.1-py2.4.egg
WARN Skipping file for pytz: pytz-2016.6.1-py2.5.egg
WARN Skipping file for pytz: pytz-2016.6.1-py2.6.egg
WARN Skipping file for pytz: pytz-2016.6.1-py2.7.egg
WARN Skipping file for pytz: pytz-2016.6.1-py3.1.egg
WARN Skipping file for pytz: pytz-2016.6.1-py3.2.egg
WARN Skipping file for pytz: pytz-2016.6.1-py3.3.egg
WARN Skipping file for pytz: pytz-2016.6.1-py3.4.egg
WARN Skipping file for pytz: pytz-2016.6.1-py3.5.egg
WARN Skipping file for pytz: pytz-2016.7-py2.4.egg
WARN Skipping file for pytz: pytz-2016.7-py2.5.egg
WARN Skipping file for pytz: pytz-2016.7-py2.6.egg
WARN Skipping file for pytz: pytz-2016.7-py2.7.egg
WARN Skipping file for pytz: pytz-2016.7-py3.1.egg
WARN Skipping file for pytz: pytz-2016.7-py3.2.egg
WARN Skipping file for pytz: pytz-2016.7-py3.3.egg
WARN Skipping file for pytz: pytz-2016.7-py3.4.egg
WARN Skipping file for pytz: pytz-2016.7-py3.5.egg
WARN Skipping file for pytz: pytz-2017.2-py2.4.egg
WARN Skipping file for pytz: pytz-2017.2-py2.5.egg
WARN Skipping file for pytz: pytz-2017.2-py2.6.egg
WARN Skipping file for pytz: pytz-2017.2-py2.7.egg
WARN Skipping file for pytz: pytz-2017.2-py3.3.egg
WARN Skipping file for pytz: pytz-2017.2-py3.4.egg
WARN Skipping file for pytz: pytz-2017.2-py3.5.egg
WARN Skipping file for pytz: pytz-2017.3-py2.4.egg
WARN Skipping file for pytz: pytz-2017.3-py2.5.egg
WARN Skipping file for pytz: pytz-2017.3-py2.6.egg
WARN Skipping file for pytz: pytz-2017.3-py2.7.egg
WARN Skipping file for pytz: pytz-2017.3-py3.3.egg
WARN Skipping file for pytz: pytz-2017.3-py3.4.egg
WARN Skipping file for pytz: pytz-2017.3-py3.5.egg
WARN Skipping file for pytz: pytz-2018.3-py2.4.egg
WARN Skipping file for pytz: pytz-2018.3-py2.5.egg
WARN Skipping file for pytz: pytz-2018.3-py2.6.egg
WARN Skipping file for pytz: pytz-2018.3-py2.7.egg
WARN Skipping file for pytz: pytz-2018.3-py3.3.egg
WARN Skipping file for pytz: pytz-2018.3-py3.4.egg
WARN Skipping file for pytz: pytz-2018.3-py3.5.egg
TRACE cached request https://pypi.org/simple/asgiref/ is storable because its response has a 'public' cache-control directive
Package Version Latest Type
------- ------- ------ -----
django  3.2.19  5.1.4  wheel
pip     24.2    24.3.1 wheel
```

</details>

In the log above `RUST_LOG=uv=trace uv pip tree --python ./venv/bin/python --outdated --verbose` should have displayed the new version for django and pip.

I'm using Fedora 41 with python3.13 installed from default repo.

---

_Comment by @charliermarsh on 2024-12-26 22:43_

I don't think `uv pip tree --outdated` is implemented at all.

---

_Label `enhancement` added by @charliermarsh on 2024-12-26 22:43_

---

_Comment by @charliermarsh on 2024-12-26 22:46_

Thanks for the clear repro steps, I appreciate it.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-12-27 16:35_

---

_Closed by @charliermarsh on 2024-12-27 17:03_

---
