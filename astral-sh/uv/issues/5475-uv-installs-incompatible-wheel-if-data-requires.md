---
number: 5475
title: "uv installs incompatible wheel if `data-requires-python` isn't set "
type: issue
state: open
author: konstin
labels:
  - bug
assignees: []
created_at: 2024-07-26T11:52:23Z
updated_at: 2024-08-10T01:02:02Z
url: https://github.com/astral-sh/uv/issues/5475
synced_at: 2026-01-10T01:23:49Z
---

# uv installs incompatible wheel if `data-requires-python` isn't set 

---

_Issue opened by @konstin on 2024-07-26 11:52_

This bug was accidentally surfaced by packse. My local index would generate a page with missing  `data-requires-python` entry: 

```html
    <!DOCTYPE html>
    <html>
        <head>
            <title>Links for python-version-does-not-exist-a</title>
        </head>
        <body>
            <h1>Links for python-version-does-not-exist-a</h1>
                 <a href="/packages/python-version-does-not-exist/python_version_does_not_exist_a-1.0.0-py3-none-any.whl#sha256=bc034d134e4ecbd282692aaaa508b235cfeb0c49b4eb1d77ef86863546091a76">python_version_does_not_exist_a-1.0.0-py3-none-any.whl</a><br>
                 <a href="/packages/python-version-does-not-exist/python_version_does_not_exist_a-1.0.0.tar.gz#sha256=6a2ce117ad33c3e44a5e22eec82b81737cb43e5ac042ac103f076749d74a65da">python_version_does_not_exist_a-1.0.0.tar.gz</a><br>
        </body>
    </html>
```

vs. the published entry:

```html
<!DOCTYPE html>
<html>
  <head>
    <meta name="pypi:repository-version" content="1.1" />
  </head>
  <body>
    <h1>Links for python-version-does-not-exist-a</h1>
    <a
      href="../../files/python_version_does_not_exist_a-1.0.0-py3-none-any.whl#sha256=bc034d134e4ecbd282692aaaa508b235cfeb0c49b4eb1d77ef86863546091a76"
      data-requires-python=">=3.30"
    >
      python_version_does_not_exist_a-1.0.0-py3-none-any.whl
    </a>
    <br />
    <a
      href="../../files/python_version_does_not_exist_a-1.0.0.tar.gz#sha256=6a2ce117ad33c3e44a5e22eec82b81737cb43e5ac042ac103f076749d74a65da"
      data-requires-python=">=3.30"
    >
      python_version_does_not_exist_a-1.0.0.tar.gz
    </a>
    <br />
  </body>
</html>
```

The wheel itself says `Requires-Python: >=3.30` in both cases, but in the first case, we would install the incompatible wheel. Specifically, this passes incorrectly:

```
uv pip install python-version-does-not-exist-a==1.0.0 --index-url http://localhost:3141/simple/ --no-cache-dir --reinstall -v
```

while this fails correctly:

```
uv pip install python-version-does-not-exist-a==1.0.0 --index-url https://astral-sh.github.io/packse/0.3.31/simple-html/ --no-cache-dir --reinstall -v
```

I'm not sure how to properly MRE this, but the solution is to check the METADATA for requires-python if there is no annotation from the index.

---

_Label `bug` added by @konstin on 2024-07-26 11:52_

---

_Comment by @charliermarsh on 2024-07-26 12:10_

This arguably seems like a bug in the index rather than in uv, hmm.

---

_Comment by @konstin on 2024-07-26 12:11_

Yeah, i'm working on fixing this in packse, but i argue we should still check METADATA, at least in the same way that we check that the name of a built wheel matches the name that the user requested.

---

_Comment by @konstin on 2024-07-26 12:33_

pypiserver never writes `data-requires-python`: https://github.com/pypiserver/pypiserver/blob/a70b77d608cb1a86e136f7a06ab2e2b2cc871c50/pypiserver/_app.py#L314. Bug report: https://github.com/pypiserver/pypiserver/issues/296. Stale PR: https://github.com/pypiserver/pypiserver/pull/320

---

_Comment by @charliermarsh on 2024-07-26 12:33_

Makes sense. I don't know that I'd want to check this in the resolver, but we should reject in the installer at minimum?

---

_Referenced in [astral-sh/packse#205](../../astral-sh/packse/pulls/205.md) on 2024-07-26 12:56_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-27 02:10_

---

_Unassigned @charliermarsh by @charliermarsh on 2024-08-10 01:02_

---
