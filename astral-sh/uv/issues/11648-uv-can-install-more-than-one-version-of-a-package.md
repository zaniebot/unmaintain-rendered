```yaml
number: 11648
title: uv can install more than one version of a package at a time
type: issue
state: closed
author: jbms
labels:
  - bug
assignees: []
created_at: 2025-02-19T23:48:07Z
updated_at: 2025-02-20T20:17:15Z
url: https://github.com/astral-sh/uv/issues/11648
synced_at: 2026-01-10T03:50:31Z
```

# uv can install more than one version of a package at a time

---

_Issue opened by @jbms on 2025-02-19 23:48_

### Summary

When there are dependency groups with conflicts, uv can incorrectly install more than one version of a package at a time.

pyproject.toml

```toml
[project]
name = "uv-dep-test"
version = "0.1.0"

requires-python = ">=3.13"
dependencies = []

[dependency-groups]
c = [
"sphinx",
]
a = [
"sphinx<5",
]
b = [
"sphinx>5",
]

[tool.uv]
conflicts = [
  [
    { group = "a" },
    { group = "b" },
  ],
]
```

Run:

```shell
uv sync --group a --group c
uv pip list
```

Output:

```
Package                       Version
----------------------------- ---------
alabaster                     0.7.16
alabaster                     1.0.0
babel                         2.17.0
certifi                       2025.1.31
charset-normalizer            3.4.1
docutils                      0.17.1
docutils                      0.21.2
idna                          3.10
imagesize                     1.4.1
jinja2                        3.1.5
markupsafe                    3.0.2
packaging                     24.2
pygments                      2.19.1
requests                      2.32.3
roman-numerals-py             3.0.0
snowballstemmer               2.2.0
sphinx                        4.5.0
sphinx                        8.2.0
sphinxcontrib-applehelp       2.0.0
sphinxcontrib-devhelp         2.0.0
sphinxcontrib-htmlhelp        2.1.0
sphinxcontrib-jsmath          1.0.1
sphinxcontrib-qthelp          2.0.0
sphinxcontrib-serializinghtml 2.0.0
urllib3                       2.3.0
```

Note that group a and group c do NOT conflict.  The expected result is for uv to install sphinx 4.5.0 to satisfy the requirements of both groups.

### Platform

Linux

### Version

uv 0.6.1

### Python version

Python 3.13.0

---

_Label `bug` added by @jbms on 2025-02-19 23:48_

---

_Comment by @charliermarsh on 2025-02-19 23:54_

The lockfile here correctly reflects that, so I'm a little confused at the behavior:

```toml
[[package]]
name = "uv-dep-test"
version = "0.1.0"
source = { virtual = "." }

[package.dev-dependencies]
a = [
    { name = "sphinx", version = "4.5.0", source = { registry = "https://pypi.org/simple" } },
]
b = [
    { name = "sphinx", version = "8.2.0", source = { registry = "https://pypi.org/simple" } },
]
c = [
    { name = "sphinx", version = "4.5.0", source = { registry = "https://pypi.org/simple" }, marker = "extra == 'group-11-uv-dep-test-a'" },
    { name = "sphinx", version = "8.2.0", source = { registry = "https://pypi.org/simple" }, marker = "extra == 'group-11-uv-dep-test-b' or extra != 'group-11-uv-dep-test-a'" },
]
```

---

_Comment by @jbms on 2025-02-19 23:57_

To be clear you are able to reproduce the erroneous behavior, right?

---

_Comment by @zanieb on 2025-02-19 23:57_

Thanks! This looks like a bug.

cc @BurntSushi

```
❯ vi pyproject.toml
❯ uv venv -p 3.13
Using CPython 3.13.2
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
❯ uv sync --group a --group c
Resolved 27 packages in 1ms
Prepared 24 packages in 1.39s
Installed 25 packages in 184ms
 + alabaster==0.7.16
 + alabaster==1.0.0
 + babel==2.17.0
 + certifi==2025.1.31
 + charset-normalizer==3.4.1
 + docutils==0.17.1
 + docutils==0.21.2
 + idna==3.10
 + imagesize==1.4.1
 + jinja2==3.1.5
 + markupsafe==3.0.2
 + packaging==24.2
 + pygments==2.19.1
 + requests==2.32.3
 + roman-numerals-py==3.0.0
 + snowballstemmer==2.2.0
 + sphinx==4.5.0
 + sphinx==8.2.0
 + sphinxcontrib-applehelp==2.0.0
 + sphinxcontrib-devhelp==2.0.0
 + sphinxcontrib-htmlhelp==2.1.0
 + sphinxcontrib-jsmath==1.0.1
 + sphinxcontrib-qthelp==2.0.0
 + sphinxcontrib-serializinghtml==2.0.0
 + urllib3==2.3.0
```

<details>
<summary>Logs</summary>
<pre>
DEBUG uv 0.6.2 (6d3614eec 2025-02-19)
DEBUG Found project root: `/Users/zb/workspace/uv/example`
TRACE Found `pyproject.toml` at: `/Users/zb/workspace/uv/pyproject.toml`
DEBUG Project is contained in non-workspace project: `/Users/zb/workspace/uv`
DEBUG No workspace root found, using project root
TRACE Checking lock for `/Users/zb/workspace/uv/example` at `/var/folders/6p/k5sd5z7j31b31pq4lhn0l8d80000gn/T/uv-cff78d0b34cfcb51.lock`
DEBUG Acquired lock for `/Users/zb/workspace/uv/example`
DEBUG Using Python request `>=3.13` from `requires-python` metadata
DEBUG Checking for Python environment at `.venv`
TRACE Cached interpreter info for Python 3.13.2, skipping probing: .venv/bin/python3
DEBUG The virtual environment's Python version satisfies `>=3.13`
DEBUG Released lock at `/var/folders/6p/k5sd5z7j31b31pq4lhn0l8d80000gn/T/uv-cff78d0b34cfcb51.lock`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: uv-dep-test @ file:///Users/zb/workspace/uv/example
TRACE Found `pyproject.toml` at: `/Users/zb/workspace/uv/pyproject.toml`
DEBUG Project is contained in non-workspace project: `/Users/zb/workspace/uv`
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 27 packages in 2ms
DEBUG Using request timeout of 30s
DEBUG Registry requirement already cached: sphinx==4.5.0
DEBUG Registry requirement already cached: sphinx==8.2.0
DEBUG Registry requirement already cached: alabaster==0.7.16
DEBUG Registry requirement already cached: babel==2.17.0
DEBUG Registry requirement already cached: docutils==0.17.1
DEBUG Registry requirement already cached: imagesize==1.4.1
DEBUG Registry requirement already cached: jinja2==3.1.5
DEBUG Registry requirement already cached: packaging==24.2
DEBUG Registry requirement already cached: pygments==2.19.1
DEBUG Registry requirement already cached: requests==2.32.3
DEBUG Registry requirement already cached: snowballstemmer==2.2.0
DEBUG Registry requirement already cached: sphinxcontrib-applehelp==2.0.0
DEBUG Registry requirement already cached: sphinxcontrib-devhelp==2.0.0
DEBUG Registry requirement already cached: sphinxcontrib-htmlhelp==2.1.0
DEBUG Registry requirement already cached: sphinxcontrib-jsmath==1.0.1
DEBUG Registry requirement already cached: sphinxcontrib-qthelp==2.0.0
DEBUG Registry requirement already cached: sphinxcontrib-serializinghtml==2.0.0
DEBUG Registry requirement already cached: alabaster==1.0.0
DEBUG Registry requirement already cached: docutils==0.21.2
DEBUG Registry requirement already cached: roman-numerals-py==3.0.0
DEBUG Registry requirement already cached: markupsafe==3.0.2
DEBUG Registry requirement already cached: certifi==2025.1.31
DEBUG Registry requirement already cached: charset-normalizer==3.4.1
DEBUG Registry requirement already cached: idna==3.10
DEBUG Registry requirement already cached: urllib3==2.3.0
TRACE Extracting file name=PackageName("sphinxcontrib-qthelp")
TRACE Extracting file name=PackageName("sphinx")
TRACE Extracting file name=PackageName("babel")
TRACE Cloning /Users/zb/.cache/uv/archive-v0/kVwYSVIQatZKUg8itVVZi/sphinxcontrib_qthelp-2.0.0.dist-info to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinxcontrib_qthelp-2.0.0.dist-info
TRACE Extracting file name=PackageName("imagesize")
TRACE Extracting file name=PackageName("alabaster")
TRACE Extracting file name=PackageName("jinja2")
TRACE Extracting file name=PackageName("docutils")
TRACE Extracting file name=PackageName("sphinxcontrib-devhelp")
TRACE Extracting file name=PackageName("alabaster")
TRACE Extracting file name=PackageName("certifi")
TRACE Extracting file name=PackageName("sphinx")
TRACE Cloning /Users/zb/.cache/uv/archive-v0/ykxHGvDoutzD2jm8TYRZv/alabaster to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/alabaster
TRACE Extracting file name=PackageName("roman-numerals-py")
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx-8.2.0.dist-info to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx-8.2.0.dist-info
TRACE Cloning /Users/zb/.cache/uv/archive-v0/Q75RcYIhEARbwk6dmVZkK/Sphinx-4.5.0.dist-info to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/Sphinx-4.5.0.dist-info
TRACE Cloning /Users/zb/.cache/uv/archive-v0/_A6gPp_arFPgA7F2sVILX/jinja2 to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/jinja2
TRACE Cloning /Users/zb/.cache/uv/archive-v0/iGzolqCtU9k_BPPDppTyN/docutils to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils
TRACE Cloning /Users/zb/.cache/uv/archive-v0/JUhabKb_517syspCLJwsB/imagesize to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/imagesize
TRACE Extracting file name=PackageName("sphinxcontrib-htmlhelp")
TRACE Extracting file name=PackageName("sphinxcontrib-serializinghtml")
TRACE Cloning /Users/zb/.cache/uv/archive-v0/ZHtCZZ4MKEUnwHWNTYj1T/sphinxcontrib_devhelp-2.0.0.dist-info to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinxcontrib_devhelp-2.0.0.dist-info
TRACE Cloning /Users/zb/.cache/uv/archive-v0/pxzXev9twgRhhC2t_xjXB/babel-2.17.0.dist-info to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/babel-2.17.0.dist-info
TRACE Cloning /Users/zb/.cache/uv/archive-v0/eTQVVYQeIw3WpY7FGZcJm/alabaster to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/alabaster
TRACE Cloning /Users/zb/.cache/uv/archive-v0/ktTAdMSxwcPEA3_Nq0PtY/sphinxcontrib_serializinghtml-2.0.0.dist-info to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinxcontrib_serializinghtml-2.0.0.dist-info
TRACE Cloning /Users/zb/.cache/uv/archive-v0/FMr8S_4JIh5qX6jFFCe4y/certifi to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/certifi
TRACE Extracting file name=PackageName("sphinxcontrib-jsmath")
TRACE Cloning /Users/zb/.cache/uv/archive-v0/tSr8nZjGLm1wTHcikue_q/roman_numerals_py-3.0.0.dist-info to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/roman_numerals_py-3.0.0.dist-info
TRACE Cloning /Users/zb/.cache/uv/archive-v0/pdPiMDfoOU9W9Hu7dyaI9/sphinxcontrib_htmlhelp-2.1.0.dist-info to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinxcontrib_htmlhelp-2.1.0.dist-info
TRACE Cloning /Users/zb/.cache/uv/archive-v0/TuFspcIl1dhULEBdeCKPE/sphinxcontrib_jsmath-1.0.1.dist-info to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinxcontrib_jsmath-1.0.1.dist-info
TRACE Extracting file name=PackageName("docutils")
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils-0.17.1.data to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils-0.17.1.data
TRACE Cloning /Users/zb/.cache/uv/archive-v0/kVwYSVIQatZKUg8itVVZi/sphinxcontrib to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinxcontrib
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx
TRACE Cloning /Users/zb/.cache/uv/archive-v0/JUhabKb_517syspCLJwsB/imagesize-1.4.1.dist-info to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/imagesize-1.4.1.dist-info
TRACE Cloning /Users/zb/.cache/uv/archive-v0/ktTAdMSxwcPEA3_Nq0PtY/sphinxcontrib to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinxcontrib
TRACE Cloning /Users/zb/.cache/uv/archive-v0/ykxHGvDoutzD2jm8TYRZv/alabaster-0.7.16.dist-info to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/alabaster-0.7.16.dist-info
TRACE Cloning /Users/zb/.cache/uv/archive-v0/_A6gPp_arFPgA7F2sVILX/jinja2-3.1.5.dist-info to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/jinja2-3.1.5.dist-info
TRACE Cloning /Users/zb/.cache/uv/archive-v0/Q75RcYIhEARbwk6dmVZkK/sphinx to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx
TRACE Cloning /Users/zb/.cache/uv/archive-v0/pxzXev9twgRhhC2t_xjXB/babel to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/babel
TRACE Cloning /Users/zb/.cache/uv/archive-v0/ZHtCZZ4MKEUnwHWNTYj1T/sphinxcontrib to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinxcontrib
TRACE Extracted 2 files name=PackageName("sphinxcontrib-qthelp")
TRACE No entrypoints name=PackageName("sphinxcontrib-qthelp")
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils
TRACE No data name=PackageName("sphinxcontrib-qthelp")
TRACE Writing installer metadata name=PackageName("sphinxcontrib-qthelp")
TRACE Cloning /Users/zb/.cache/uv/archive-v0/eTQVVYQeIw3WpY7FGZcJm/alabaster/support.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/alabaster/support.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/iGzolqCtU9k_BPPDppTyN/docutils-0.21.2.dist-info to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils-0.21.2.dist-info
TRACE Cloning /Users/zb/.cache/uv/archive-v0/ZHtCZZ4MKEUnwHWNTYj1T/sphinxcontrib/devhelp to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinxcontrib/devhelp
TRACE Extracted 2 files name=PackageName("babel")
TRACE Writing entrypoints name=PackageName("babel")
TRACE Extracted 2 files name=PackageName("sphinx")
TRACE Cloning /Users/zb/.cache/uv/archive-v0/eTQVVYQeIw3WpY7FGZcJm/alabaster/about.html to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/alabaster/about.html
TRACE No data name=PackageName("babel")
TRACE Writing installer metadata name=PackageName("babel")
TRACE Extracted 2 files name=PackageName("jinja2")
TRACE No entrypoints name=PackageName("jinja2")
TRACE No data name=PackageName("jinja2")
TRACE Writing installer metadata name=PackageName("jinja2")
TRACE Extracted 2 files name=PackageName("alabaster")
TRACE Writing entrypoints name=PackageName("sphinx")
TRACE No entrypoints name=PackageName("alabaster")
TRACE No data name=PackageName("alabaster")
TRACE Writing installer metadata name=PackageName("alabaster")
TRACE Cloning /Users/zb/.cache/uv/archive-v0/eTQVVYQeIw3WpY7FGZcJm/alabaster/layout.html to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/alabaster/layout.html
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers
TRACE Extracted 2 files name=PackageName("sphinxcontrib-devhelp")
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/recommonmark_wrapper.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/recommonmark_wrapper.py
TRACE No entrypoints name=PackageName("sphinxcontrib-devhelp")
TRACE No data name=PackageName("sphinxcontrib-devhelp")
TRACE Writing installer metadata name=PackageName("sphinxcontrib-devhelp")
TRACE Extracted 2 files name=PackageName("docutils")
TRACE Writing record name=PackageName("sphinxcontrib-qthelp")
TRACE Writing entrypoints name=PackageName("docutils")
TRACE Writing record name=PackageName("jinja2")
TRACE Cloning /Users/zb/.cache/uv/archive-v0/eTQVVYQeIw3WpY7FGZcJm/alabaster/theme.conf to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/alabaster/theme.conf
TRACE Writing record name=PackageName("babel")
TRACE Writing record name=PackageName("alabaster")
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/null.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/null.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/ktTAdMSxwcPEA3_Nq0PtY/sphinxcontrib/serializinghtml to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinxcontrib/serializinghtml
TRACE Extracting file name=PackageName("markupsafe")
TRACE Cloning /Users/zb/.cache/uv/archive-v0/DdMaRnfzAbgkJd8D5VOAA/markupsafe to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/markupsafe
TRACE Cloning /Users/zb/.cache/uv/archive-v0/tSr8nZjGLm1wTHcikue_q/roman_numerals to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/roman_numerals
TRACE Extracting file name=PackageName("packaging")
TRACE Cloning /Users/zb/.cache/uv/archive-v0/cE0NvoIKRIPcItaL8udZ3/packaging to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/packaging
TRACE Extracting file name=PackageName("idna")
TRACE Cloning /Users/zb/.cache/uv/archive-v0/CcWIYGsWOwQ1CEqoB3zdO/idna-3.10.dist-info to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/idna-3.10.dist-info
TRACE Cloning /Users/zb/.cache/uv/archive-v0/FMr8S_4JIh5qX6jFFCe4y/certifi-2025.1.31.dist-info to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/certifi-2025.1.31.dist-info
TRACE Writing record name=PackageName("sphinxcontrib-devhelp")
TRACE Cloning /Users/zb/.cache/uv/archive-v0/JUhabKb_517syspCLJwsB/imagesize.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/imagesize.py
TRACE Extracting file name=PackageName("charset-normalizer")
TRACE Cloning /Users/zb/.cache/uv/archive-v0/SgPdc0qEaje8NOi4zGu7C/charset_normalizer to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/charset_normalizer
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/directives to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/directives
TRACE Extracting file name=PackageName("urllib3")
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/directives/code.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/directives/code.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/LFEBe5w-1NDbHQeGPLSxo/urllib3-2.3.0.dist-info to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/urllib3-2.3.0.dist-info
TRACE Extracted 3 files name=PackageName("imagesize")
TRACE No entrypoints name=PackageName("imagesize")
TRACE No data name=PackageName("imagesize")
TRACE Writing installer metadata name=PackageName("imagesize")
TRACE Cloning /Users/zb/.cache/uv/archive-v0/cE0NvoIKRIPcItaL8udZ3/packaging-24.2.dist-info to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/packaging-24.2.dist-info
TRACE Extracted 2 files name=PackageName("roman-numerals-py")
TRACE No entrypoints name=PackageName("roman-numerals-py")
TRACE No data name=PackageName("roman-numerals-py")
TRACE Writing installer metadata name=PackageName("roman-numerals-py")
TRACE Extracted 2 files name=PackageName("packaging")
TRACE No entrypoints name=PackageName("packaging")
TRACE No data name=PackageName("packaging")
TRACE Writing installer metadata name=PackageName("packaging")
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/directives to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/directives
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/directives/misc.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/directives/misc.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/SgPdc0qEaje8NOi4zGu7C/charset_normalizer-3.4.1.dist-info to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/charset_normalizer-3.4.1.dist-info
TRACE Cloning /Users/zb/.cache/uv/archive-v0/CcWIYGsWOwQ1CEqoB3zdO/idna to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/idna
TRACE Extracted 2 files name=PackageName("certifi")
TRACE No entrypoints name=PackageName("certifi")
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/directives/__init__.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/directives/__init__.py
TRACE No data name=PackageName("certifi")
TRACE Writing installer metadata name=PackageName("certifi")
TRACE Cloning /Users/zb/.cache/uv/archive-v0/DdMaRnfzAbgkJd8D5VOAA/MarkupSafe-3.0.2.dist-info to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/MarkupSafe-3.0.2.dist-info
TRACE Extracted 2 files name=PackageName("idna")
TRACE No entrypoints name=PackageName("idna")
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/directives/body.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/directives/body.py
TRACE No data name=PackageName("idna")
TRACE Writing installer metadata name=PackageName("idna")
TRACE Extracted 2 files name=PackageName("markupsafe")
TRACE Writing record name=PackageName("imagesize")
TRACE No entrypoints name=PackageName("markupsafe")
TRACE No data name=PackageName("markupsafe")
TRACE Writing installer metadata name=PackageName("markupsafe")
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/directives/other.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/directives/other.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/LFEBe5w-1NDbHQeGPLSxo/urllib3 to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/urllib3
TRACE Extracting file name=PackageName("requests")
TRACE Cloning /Users/zb/.cache/uv/archive-v0/gdifYIBuvJptCMPoixvI3/requests-2.32.3.dist-info to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/requests-2.32.3.dist-info
TRACE Extracted 2 files name=PackageName("charset-normalizer")
TRACE Writing record name=PackageName("certifi")
TRACE Writing record name=PackageName("idna")
TRACE Writing entrypoints name=PackageName("charset-normalizer")
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/directives/admonitions.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/directives/admonitions.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/gdifYIBuvJptCMPoixvI3/requests to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/requests
TRACE Extracting file name=PackageName("snowballstemmer")
TRACE Cloning /Users/zb/.cache/uv/archive-v0/zVbLkASR0bsj3wvrGOStD/snowballstemmer to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/snowballstemmer
TRACE Extracting file name=PackageName("pygments")
TRACE Cloning /Users/zb/.cache/uv/archive-v0/8JX1-Tg2FRgeZYClL2FKB/pygments-2.19.1.dist-info to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/pygments-2.19.1.dist-info
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/directives/patches.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/directives/patches.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/directives/references.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/directives/references.py
TRACE Extracted 2 files name=PackageName("urllib3")
TRACE No entrypoints name=PackageName("urllib3")
TRACE No data name=PackageName("urllib3")
TRACE Writing installer metadata name=PackageName("urllib3")
TRACE Extracted 2 files name=PackageName("sphinxcontrib-serializinghtml")
TRACE No entrypoints name=PackageName("sphinxcontrib-serializinghtml")
TRACE No data name=PackageName("sphinxcontrib-serializinghtml")
TRACE Writing installer metadata name=PackageName("sphinxcontrib-serializinghtml")
TRACE Cloning /Users/zb/.cache/uv/archive-v0/eTQVVYQeIw3WpY7FGZcJm/alabaster/__init__.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/alabaster/__init__.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/pdPiMDfoOU9W9Hu7dyaI9/sphinxcontrib to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinxcontrib
TRACE Cloning /Users/zb/.cache/uv/archive-v0/pdPiMDfoOU9W9Hu7dyaI9/sphinxcontrib/htmlhelp to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinxcontrib/htmlhelp
TRACE Writing record name=PackageName("markupsafe")
TRACE Cloning /Users/zb/.cache/uv/archive-v0/8JX1-Tg2FRgeZYClL2FKB/pygments to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/pygments
TRACE Writing record name=PackageName("urllib3")
TRACE Extracting file name=PackageName("sphinxcontrib-applehelp")
TRACE Cloning /Users/zb/.cache/uv/archive-v0/-PfxiH-a_k0zhpyBeDWbW/sphinxcontrib_applehelp-2.0.0.dist-info to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinxcontrib_applehelp-2.0.0.dist-info
TRACE Extracted 2 files name=PackageName("requests")
TRACE No data name=PackageName("charset-normalizer")
TRACE Writing installer metadata name=PackageName("charset-normalizer")
TRACE No entrypoints name=PackageName("requests")
TRACE No data name=PackageName("requests")
TRACE Writing installer metadata name=PackageName("requests")
TRACE Cloning /Users/zb/.cache/uv/archive-v0/TuFspcIl1dhULEBdeCKPE/sphinxcontrib_jsmath-1.0.1-py3.7-nspkg.pth to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinxcontrib_jsmath-1.0.1-py3.7-nspkg.pth
TRACE Cloning /Users/zb/.cache/uv/archive-v0/TuFspcIl1dhULEBdeCKPE/sphinxcontrib to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinxcontrib
TRACE Cloning /Users/zb/.cache/uv/archive-v0/TuFspcIl1dhULEBdeCKPE/sphinxcontrib/jsmath to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinxcontrib/jsmath
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/directives/html.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/directives/html.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/-PfxiH-a_k0zhpyBeDWbW/sphinxcontrib to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinxcontrib
TRACE Cloning /Users/zb/.cache/uv/archive-v0/-PfxiH-a_k0zhpyBeDWbW/sphinxcontrib/applehelp to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinxcontrib/applehelp
TRACE Cloning /Users/zb/.cache/uv/archive-v0/eTQVVYQeIw3WpY7FGZcJm/alabaster/static to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/alabaster/static
TRACE Cloning /Users/zb/.cache/uv/archive-v0/eTQVVYQeIw3WpY7FGZcJm/alabaster/static/alabaster.css_t to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/alabaster/static/alabaster.css_t
TRACE Extracted 2 files name=PackageName("sphinxcontrib-htmlhelp")
TRACE Extracted 3 files name=PackageName("sphinxcontrib-jsmath")
TRACE No entrypoints name=PackageName("sphinxcontrib-htmlhelp")
TRACE No data name=PackageName("sphinxcontrib-htmlhelp")
TRACE Writing installer metadata name=PackageName("sphinxcontrib-htmlhelp")
TRACE No entrypoints name=PackageName("sphinxcontrib-jsmath")
TRACE No data name=PackageName("sphinxcontrib-jsmath")
TRACE Writing installer metadata name=PackageName("sphinxcontrib-jsmath")
TRACE Extracted 2 files name=PackageName("pygments")
TRACE Writing record name=PackageName("roman-numerals-py")
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/jinja2glue.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/jinja2glue.py
TRACE Writing entrypoints name=PackageName("pygments")
TRACE Cloning /Users/zb/.cache/uv/archive-v0/zVbLkASR0bsj3wvrGOStD/snowballstemmer-2.2.0.dist-info to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/snowballstemmer-2.2.0.dist-info
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/cmd to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/cmd
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/cmd/build.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/cmd/build.py
TRACE Extracted 2 files name=PackageName("snowballstemmer")
TRACE Writing record name=PackageName("packaging")
TRACE No entrypoints name=PackageName("snowballstemmer")
TRACE No data name=PackageName("pygments")
TRACE Writing installer metadata name=PackageName("pygments")
TRACE No data name=PackageName("snowballstemmer")
TRACE Writing installer metadata name=PackageName("snowballstemmer")
TRACE Writing record name=PackageName("sphinxcontrib-htmlhelp")
TRACE Cloning /Users/zb/.cache/uv/archive-v0/eTQVVYQeIw3WpY7FGZcJm/alabaster/static/github-banner.svg to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/alabaster/static/github-banner.svg
TRACE Cloning /Users/zb/.cache/uv/archive-v0/eTQVVYQeIw3WpY7FGZcJm/alabaster/static/custom.css to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/alabaster/static/custom.css
TRACE Extracted 2 files name=PackageName("sphinxcontrib-applehelp")
TRACE Writing record name=PackageName("sphinxcontrib-jsmath")
TRACE No entrypoints name=PackageName("sphinxcontrib-applehelp")
TRACE No data name=PackageName("sphinxcontrib-applehelp")
TRACE Writing installer metadata name=PackageName("sphinxcontrib-applehelp")
TRACE Writing record name=PackageName("requests")
TRACE Writing record name=PackageName("charset-normalizer")
TRACE Cloning /Users/zb/.cache/uv/archive-v0/eTQVVYQeIw3WpY7FGZcJm/alabaster/relations.html to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/alabaster/relations.html
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/directives/__init__.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/directives/__init__.py
TRACE Writing record name=PackageName("snowballstemmer")
TRACE Writing record name=PackageName("pygments")
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/cmd/__init__.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/cmd/__init__.py
TRACE Writing record name=PackageName("sphinxcontrib-serializinghtml")
TRACE Writing record name=PackageName("sphinxcontrib-applehelp")
TRACE No data name=PackageName("sphinx")
TRACE Writing installer metadata name=PackageName("sphinx")
TRACE Cloning /Users/zb/.cache/uv/archive-v0/eTQVVYQeIw3WpY7FGZcJm/alabaster/donate.html to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/alabaster/donate.html
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/directives/admonitions.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/directives/admonitions.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/cmd/quickstart.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/cmd/quickstart.py
TRACE Writing record name=PackageName("sphinx")
TRACE Cloning /Users/zb/.cache/uv/archive-v0/eTQVVYQeIw3WpY7FGZcJm/alabaster/navigation.html to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/alabaster/navigation.html
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/directives/images.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/directives/images.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/cmd/make_mode.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/cmd/make_mode.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/eTQVVYQeIw3WpY7FGZcJm/alabaster-1.0.0.dist-info to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/alabaster-1.0.0.dist-info
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/directives/parts.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/directives/parts.py
TRACE Extracted 2 files name=PackageName("alabaster")
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/texinputs to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/texinputs
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/texinputs/sphinxlatexstylepage.sty to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/texinputs/sphinxlatexstylepage.sty
TRACE No entrypoints name=PackageName("alabaster")
TRACE No data name=PackageName("alabaster")
TRACE Writing installer metadata name=PackageName("alabaster")
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/directives/tables.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/directives/tables.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/texinputs/sphinxlatexliterals.sty to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/texinputs/sphinxlatexliterals.sty
TRACE No data name=PackageName("docutils")
TRACE Writing installer metadata name=PackageName("docutils")
TRACE Writing record name=PackageName("alabaster")
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/roles.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/roles.py
TRACE Writing record name=PackageName("docutils")
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/texinputs/LatinRules.xdy to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/texinputs/LatinRules.xdy
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/include to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/include
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/include/isonum.txt to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/include/isonum.txt
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/texinputs/LICRcyr2utf8.xdy to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/texinputs/LICRcyr2utf8.xdy
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/include/isolat1.txt to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/include/isolat1.txt
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/texinputs/sphinxlatexstyletext.sty to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/texinputs/sphinxlatexstyletext.sty
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/include/isobox.txt to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/include/isobox.txt
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/texinputs/LICRlatin2utf8.xdy to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/texinputs/LICRlatin2utf8.xdy
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/include/isolat2.txt to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/include/isolat2.txt
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/texinputs/sphinxlatexgraphics.sty to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/texinputs/sphinxlatexgraphics.sty
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/include/isoamsr.txt to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/include/isoamsr.txt
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/texinputs/sphinxmanual.cls to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/texinputs/sphinxmanual.cls
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/include/mmlalias.txt to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/include/mmlalias.txt
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/texinputs/sphinxpackagecyrillic.sty to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/texinputs/sphinxpackagecyrillic.sty
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/include/isotech.txt to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/include/isotech.txt
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/texinputs/python.ist to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/texinputs/python.ist
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/include/isoamsa.txt to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/include/isoamsa.txt
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/texinputs/sphinxlatexobjects.sty to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/texinputs/sphinxlatexobjects.sty
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/include/isoamsc.txt to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/include/isoamsc.txt
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/texinputs/make.bat.jinja to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/texinputs/make.bat.jinja
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/texinputs/sphinxhowto.cls to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/texinputs/sphinxhowto.cls
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/include/isoamsb.txt to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/include/isoamsb.txt
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/texinputs/sphinxlatextables.sty to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/texinputs/sphinxlatextables.sty
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/include/isomfrk.txt to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/include/isomfrk.txt
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/texinputs/sphinxlatexshadowbox.sty to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/texinputs/sphinxlatexshadowbox.sty
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/include/isogrk1.txt to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/include/isogrk1.txt
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/texinputs/sphinxlatexindbibtoc.sty to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/texinputs/sphinxlatexindbibtoc.sty
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/include/isogrk3.txt to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/include/isogrk3.txt
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/texinputs/sphinx.sty to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/texinputs/sphinx.sty
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/include/xhtml1-special.txt to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/include/xhtml1-special.txt
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/texinputs/sphinxlatexstyleheadings.sty to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/texinputs/sphinxlatexstyleheadings.sty
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/include/isogrk2.txt to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/include/isogrk2.txt
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/texinputs/sphinxoptionsgeometry.sty to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/texinputs/sphinxoptionsgeometry.sty
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/include/s5defs.txt to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/include/s5defs.txt
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/texinputs/sphinxpackagesubstitutefont.sty to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/texinputs/sphinxpackagesubstitutefont.sty
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/texinputs/sphinx.xdy to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/texinputs/sphinx.xdy
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/include/isogrk4.txt to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/include/isogrk4.txt
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/texinputs/sphinxlatexlists.sty to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/texinputs/sphinxlatexlists.sty
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/include/xhtml1-lat1.txt to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/include/xhtml1-lat1.txt
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/texinputs/sphinxlatexadmonitions.sty to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/texinputs/sphinxlatexadmonitions.sty
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/include/isopub.txt to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/include/isopub.txt
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/include/isogrk4-wide.txt to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/include/isogrk4-wide.txt
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/texinputs/Makefile.jinja to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/texinputs/Makefile.jinja
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/texinputs/sphinxlatexnumfig.sty to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/texinputs/sphinxlatexnumfig.sty
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/include/isodia.txt to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/include/isodia.txt
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/texinputs/latexmkrc.jinja to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/texinputs/latexmkrc.jinja
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/texinputs/sphinxpackagefootnote.sty to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/texinputs/sphinxpackagefootnote.sty
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/include/mmlextra.txt to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/include/mmlextra.txt
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/texinputs/sphinxoptionshyperref.sty to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/texinputs/sphinxoptionshyperref.sty
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/include/isocyr2.txt to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/include/isocyr2.txt
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/texinputs/sphinxlatexcontainers.sty to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/texinputs/sphinxlatexcontainers.sty
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/include/isomopf-wide.txt to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/include/isomopf-wide.txt
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/include/isocyr1.txt to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/include/isocyr1.txt
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/texinputs/sphinxpackageboxes.sty to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/texinputs/sphinxpackageboxes.sty
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/texinputs/latexmkjarc.jinja to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/texinputs/latexmkjarc.jinja
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/domains to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/domains
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/domains/citation.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/domains/citation.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/include/README.txt to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/include/README.txt
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/domains/index.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/domains/index.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/include/isoamso.txt to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/include/isoamso.txt
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/domains/python to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/domains/python
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/include/isoamsn.txt to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/include/isoamsn.txt
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/domains/_index.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/domains/_index.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/domains/__init__.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/domains/__init__.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/include/isomscr.txt to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/include/isomscr.txt
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/domains/std to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/domains/std
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/include/isomfrk-wide.txt to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/include/isomfrk-wide.txt
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/domains/changeset.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/domains/changeset.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/include/mmlextra-wide.txt to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/include/mmlextra-wide.txt
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/domains/cpp to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/domains/cpp
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/domains/math.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/domains/math.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/include/isomopf.txt to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/include/isomopf.txt
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/domains/_domains_container.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/domains/_domains_container.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/include/isomscr-wide.txt to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/include/isomscr-wide.txt
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/domains/javascript.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/domains/javascript.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/include/xhtml1-symbol.txt to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/include/xhtml1-symbol.txt
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/domains/c to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/domains/c
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/domains/rst.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/domains/rst.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/tableparser.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/tableparser.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/theming.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/theming.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/__init__.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/__init__.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/sl to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/sl
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/languages to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/languages
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/sl/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/sl/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/languages/he.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/languages/he.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/sl/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/sl/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/languages/ja.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/languages/ja.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/sl/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/sl/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/languages/af.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/languages/af.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/sl/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/sl/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/languages/ko.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/languages/ko.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/sk to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/sk
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/sk/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/sk/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/sk/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/sk/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/languages/pl.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/languages/pl.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/sk/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/sk/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/languages/gl.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/languages/gl.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/sk/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/sk/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/languages/lv.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/languages/lv.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/ur to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/ur
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/languages/__init__.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/languages/__init__.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/ur/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/ur/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/ur/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/ur/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/languages/ru.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/languages/ru.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/ur/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/ur/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/languages/fi.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/languages/fi.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/ur/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/ur/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/languages/lt.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/languages/lt.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/.tx to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/.tx
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/en_HK to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/en_HK
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/en_HK/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/en_HK/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/en_HK/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/en_HK/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/languages/fr.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/languages/fr.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/en_HK/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/en_HK/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/languages/nl.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/languages/nl.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/en_HK/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/en_HK/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/languages/cs.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/languages/cs.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/pl to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/pl
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/languages/sv.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/languages/sv.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/pl/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/pl/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/pl/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/pl/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/languages/en.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/languages/en.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/pl/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/pl/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/languages/zh_cn.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/languages/zh_cn.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/pl/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/pl/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/languages/pt_br.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/languages/pt_br.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/vi to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/vi
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/vi/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/vi/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/languages/zh_tw.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/languages/zh_tw.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/vi/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/vi/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/languages/eo.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/languages/eo.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/vi/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/vi/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/languages/es.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/languages/es.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/vi/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/vi/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/languages/sk.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/languages/sk.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/sq to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/sq
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/sq/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/sq/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/sq/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/sq/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/languages/it.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/languages/it.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/sq/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/sq/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/languages/ar.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/languages/ar.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/sq/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/sq/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/languages/ca.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/languages/ca.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/sv to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/sv
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/sv/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/sv/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/sv/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/sv/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/languages/de.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/languages/de.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/sv/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/sv/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/languages/fa.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/languages/fa.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/sv/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/sv/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/languages/da.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/languages/da.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/zh_HK to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/zh_HK
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/zh_HK/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/zh_HK/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/zh_HK/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/zh_HK/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/rst/states.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/rst/states.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/zh_HK/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/zh_HK/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/parsers/__init__.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/parsers/__init__.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/zh_HK/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/zh_HK/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/manpage.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/manpage.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/uk_UA to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/uk_UA
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/uk_UA/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/uk_UA/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/html5_polyglot to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/html5_polyglot
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/uk_UA/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/uk_UA/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/html5_polyglot/__init__.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/html5_polyglot/__init__.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/uk_UA/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/uk_UA/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/html5_polyglot/responsive.css to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/html5_polyglot/responsive.css
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/uk_UA/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/uk_UA/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/html5_polyglot/template.txt to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/html5_polyglot/template.txt
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/he to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/he
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/he/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/he/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/html5_polyglot/minimal.css to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/html5_polyglot/minimal.css
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/he/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/he/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/html5_polyglot/plain.css to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/html5_polyglot/plain.css
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/he/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/he/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/he/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/he/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/html5_polyglot/math.css to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/html5_polyglot/math.css
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/da to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/da
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/html5_polyglot/tuftig.css to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/html5_polyglot/tuftig.css
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/da/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/da/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/da/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/da/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/pseudoxml.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/pseudoxml.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/da/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/da/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/xetex to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/xetex
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/xetex/__init__.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/xetex/__init__.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/da/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/da/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/s5_html to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/s5_html
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/s5_html/__init__.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/s5_html/__init__.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/pt_BR to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/pt_BR
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/pt_BR/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/pt_BR/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/pt_BR/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/pt_BR/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/s5_html/themes to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/s5_html/themes
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/s5_html/themes/small-black to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/s5_html/themes/small-black
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/s5_html/themes/small-black/pretty.css to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/s5_html/themes/small-black/pretty.css
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/pt_BR/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/pt_BR/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/s5_html/themes/small-black/__base__ to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/s5_html/themes/small-black/__base__
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/pt_BR/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/pt_BR/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/s5_html/themes/small-white to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/s5_html/themes/small-white
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/es_CO to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/es_CO
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/s5_html/themes/small-white/pretty.css to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/s5_html/themes/small-white/pretty.css
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/ja to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/ja
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/ja/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/ja/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/ja/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/ja/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/s5_html/themes/small-white/framing.css to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/s5_html/themes/small-white/framing.css
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/ja/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/ja/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/s5_html/themes/medium-black to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/s5_html/themes/medium-black
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/s5_html/themes/medium-black/pretty.css to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/s5_html/themes/medium-black/pretty.css
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/ja/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/ja/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/s5_html/themes/medium-black/__base__ to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/s5_html/themes/medium-black/__base__
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/el to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/el
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/el/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/el/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/s5_html/themes/big-black to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/s5_html/themes/big-black
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/el/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/el/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/s5_html/themes/big-black/pretty.css to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/s5_html/themes/big-black/pretty.css
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/s5_html/themes/big-black/framing.css to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/s5_html/themes/big-black/framing.css
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/el/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/el/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/s5_html/themes/big-black/__base__ to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/s5_html/themes/big-black/__base__
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/el/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/el/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/s5_html/themes/default to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/s5_html/themes/default
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/s5_html/themes/default/pretty.css to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/s5_html/themes/default/pretty.css
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/lv to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/lv
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/lv/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/lv/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/lv/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/lv/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/s5_html/themes/default/s5-core.css to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/s5_html/themes/default/s5-core.css
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/lv/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/lv/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/s5_html/themes/default/slides.css to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/s5_html/themes/default/slides.css
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/lv/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/lv/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/s5_html/themes/default/iepngfix.htc to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/s5_html/themes/default/iepngfix.htc
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/s5_html/themes/default/framing.css to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/s5_html/themes/default/framing.css
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/it to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/it
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/it/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/it/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/it/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/it/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/s5_html/themes/default/blank.gif to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/s5_html/themes/default/blank.gif
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/s5_html/themes/default/print.css to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/s5_html/themes/default/print.css
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/it/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/it/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/s5_html/themes/default/slides.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/s5_html/themes/default/slides.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/it/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/it/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/s5_html/themes/default/opera.css to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/s5_html/themes/default/opera.css
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/ca to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/ca
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/s5_html/themes/default/outline.css to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/s5_html/themes/default/outline.css
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/ca/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/ca/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/ca/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/ca/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/s5_html/themes/big-white to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/s5_html/themes/big-white
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/s5_html/themes/big-white/pretty.css to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/s5_html/themes/big-white/pretty.css
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/ca/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/ca/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/s5_html/themes/big-white/framing.css to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/s5_html/themes/big-white/framing.css
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/ca/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/ca/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/s5_html/themes/medium-white to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/s5_html/themes/medium-white
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/s5_html/themes/medium-white/pretty.css to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/s5_html/themes/medium-white/pretty.css
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/zh_TW to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/zh_TW
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/zh_TW/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/zh_TW/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/zh_TW/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/zh_TW/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/s5_html/themes/medium-white/framing.css to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/s5_html/themes/medium-white/framing.css
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/zh_TW/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/zh_TW/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/s5_html/themes/README.txt to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/s5_html/themes/README.txt
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/zh_TW/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/zh_TW/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/null.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/null.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/sr_RS to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/sr_RS
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/sr_RS/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/sr_RS/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/sr_RS/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/sr_RS/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/__init__.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/__init__.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/sr_RS/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/sr_RS/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/latex2e to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/latex2e
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/latex2e/__init__.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/latex2e/__init__.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/sr_RS/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/sr_RS/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/latex2e/xelatex.tex to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/latex2e/xelatex.tex
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/is to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/is
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/latex2e/default.tex to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/latex2e/default.tex
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/is/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/is/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/is/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/is/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/latex2e/docutils.sty to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/latex2e/docutils.sty
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/is/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/is/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/latex2e/titlepage.tex to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/latex2e/titlepage.tex
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/is/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/is/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/latex2e/titlingpage.tex to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/latex2e/titlingpage.tex
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/nb_NO to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/nb_NO
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/nb_NO/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/nb_NO/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/nb_NO/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/nb_NO/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/odf_odt to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/odf_odt
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/odf_odt/styles.odt to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/odf_odt/styles.odt
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/nb_NO/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/nb_NO/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/odf_odt/__init__.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/odf_odt/__init__.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/nb_NO/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/nb_NO/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/odf_odt/pygmentsformatter.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/odf_odt/pygmentsformatter.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/cs to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/cs
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/cs/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/cs/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/cs/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/cs/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/_html_base.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/_html_base.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/cs/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/cs/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/docutils_xml.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/docutils_xml.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/cs/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/cs/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/pep_html to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/pep_html
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/pep_html/__init__.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/pep_html/__init__.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/sphinx.pot to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/sphinx.pot
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/pep_html/template.txt to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/pep_html/template.txt
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/te to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/te
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/te/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/te/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/te/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/te/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/pep_html/pep.css to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/pep_html/pep.css
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/te/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/te/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/html4css1 to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/html4css1
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/html4css1/html4css1.css to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/html4css1/html4css1.css
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/te/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/te/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/html4css1/__init__.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/html4css1/__init__.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/pt_PT to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/pt_PT
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/pt_PT/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/pt_PT/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/pt_PT/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/pt_PT/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/writers/html4css1/template.txt to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/writers/html4css1/template.txt
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/pt_PT/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/pt_PT/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/io.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/io.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/pt_PT/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/pt_PT/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/frontend.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/frontend.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/ru to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/ru
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/__init__.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/__init__.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/ru/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/ru/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/ru/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/ru/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/core.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/core.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/ru/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/ru/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/utils to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/utils
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/utils/roman.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/utils/roman.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/ru/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/ru/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/utils/punctuation_chars.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/utils/punctuation_chars.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/__init__.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/__init__.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/utils/smartquotes.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/utils/smartquotes.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/hi_IN to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/hi_IN
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/hi_IN/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/hi_IN/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/hi_IN/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/hi_IN/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/utils/urischemes.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/utils/urischemes.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/hi_IN/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/hi_IN/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/utils/error_reporting.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/utils/error_reporting.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/hi_IN/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/hi_IN/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/utils/__init__.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/utils/__init__.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/ro to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/ro
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/ro/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/ro/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/ro/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/ro/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/utils/math to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/utils/math
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/utils/math/latex2mathml.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/utils/math/latex2mathml.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/ro/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/ro/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/utils/math/unichar2tex.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/utils/math/unichar2tex.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/ro/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/ro/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/utils/math/tex2unichar.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/utils/math/tex2unichar.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/zh_CN to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/zh_CN
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/zh_CN/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/zh_CN/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/zh_CN/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/zh_CN/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/utils/math/math2html.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/utils/math/math2html.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/zh_CN/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/zh_CN/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/utils/math/__init__.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/utils/math/__init__.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/zh_CN/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/zh_CN/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/utils/math/tex2mathml_extern.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/utils/math/tex2mathml_extern.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/cak to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/cak
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/cak/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/cak/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/utils/code_analyzer.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/utils/code_analyzer.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/cak/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/cak/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/languages to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/languages
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/languages/he.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/languages/he.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/cak/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/cak/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/languages/ja.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/languages/ja.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/cak/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/cak/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/languages/af.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/languages/af.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/yue to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/yue
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/yue/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/yue/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/yue/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/yue/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/languages/ko.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/languages/ko.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/yue/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/yue/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/languages/pl.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/languages/pl.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/yue/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/yue/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/languages/gl.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/languages/gl.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/en_DE to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/en_DE
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/pt to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/pt
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/pt/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/pt/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/pt/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/pt/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/languages/lv.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/languages/lv.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/pt/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/pt/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/languages/__init__.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/languages/__init__.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/pt/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/pt/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/languages/ru.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/languages/ru.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/zh_TW.Big5 to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/zh_TW.Big5
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/zh_TW.Big5/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/zh_TW.Big5/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/languages/fi.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/languages/fi.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/zh_TW.Big5/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/zh_TW.Big5/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/languages/lt.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/languages/lt.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/zh_TW.Big5/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/zh_TW.Big5/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/languages/fr.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/languages/fr.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/zh_TW.Big5/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/zh_TW.Big5/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/languages/nl.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/languages/nl.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/sr to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/sr
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/sr/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/sr/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/sr/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/sr/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/languages/cs.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/languages/cs.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/sr/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/sr/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/languages/sv.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/languages/sv.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/sr/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/sr/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/languages/en.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/languages/en.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/sr@latin to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/sr@latin
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/sr@latin/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/sr@latin/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/sr@latin/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/sr@latin/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/languages/zh_cn.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/languages/zh_cn.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/sr@latin/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/sr@latin/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/languages/pt_br.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/languages/pt_br.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/sr@latin/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/sr@latin/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/languages/zh_tw.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/languages/zh_tw.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/en_GB to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/en_GB
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/languages/eo.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/languages/eo.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/en_GB/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/en_GB/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/en_GB/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/en_GB/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/languages/es.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/languages/es.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/en_GB/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/en_GB/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/languages/sk.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/languages/sk.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/en_GB/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/en_GB/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/languages/it.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/languages/it.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/si to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/si
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/si/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/si/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/si/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/si/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/languages/ar.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/languages/ar.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/si/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/si/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/languages/ca.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/languages/ca.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/si/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/si/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/languages/de.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/languages/de.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/mk to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/mk
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/languages/fa.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/languages/fa.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/mk/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/mk/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/mk/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/mk/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/languages/da.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/languages/da.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/mk/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/mk/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/transforms to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/transforms
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/mk/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/mk/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/transforms/misc.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/transforms/misc.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/ar to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/ar
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/transforms/frontmatter.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/transforms/frontmatter.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/ar/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/ar/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/ar/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/ar/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/transforms/references.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/transforms/references.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/ar/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/ar/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/transforms/__init__.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/transforms/__init__.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/ar/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/ar/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/transforms/writer_aux.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/transforms/writer_aux.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/gl to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/gl
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/hr to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/hr
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/hr/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/hr/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/transforms/peps.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/transforms/peps.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/hr/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/hr/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/transforms/parts.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/transforms/parts.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/hr/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/hr/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/transforms/components.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/transforms/components.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/hr/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/hr/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/hu to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/hu
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/transforms/universal.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/transforms/universal.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/hu/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/hu/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/hu/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/hu/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/examples.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/examples.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/hu/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/hu/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/readers to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/readers
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/readers/standalone.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/readers/standalone.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/hu/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/hu/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/readers/pep.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/readers/pep.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/nl to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/nl
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/nl/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/nl/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/nl/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/nl/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/readers/__init__.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/readers/__init__.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/nl/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/nl/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/readers/doctree.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/readers/doctree.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/nl/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/nl/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/nodes.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/nodes.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/bg to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/bg
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/bg/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/bg/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/bg/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/bg/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils/statemachine.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils/statemachine.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/bg/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/bg/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/lyWvcoc9LHBVRz6-DNbml/docutils-0.17.1.dist-info to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/docutils-0.17.1.dist-info
TRACE Extracted 3 files name=PackageName("docutils")
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/bg/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/bg/LC_MESSAGES/sphinx.po
TRACE No entrypoints name=PackageName("docutils")
TRACE Installing data/scripts dist_name=PackageName("docutils")
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/bn to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/bn
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/bn/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/bn/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/bn/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/bn/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/bn/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/bn/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/bn/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/bn/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/ne to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/ne
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/ne/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/ne/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/ne/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/ne/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/ne/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/ne/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/ne/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/ne/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/hi to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/hi
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/hi/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/hi/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/hi/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/hi/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/hi/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/hi/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/hi/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/hi/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/ka to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/ka
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/de to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/de
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/de/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/de/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/de/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/de/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/de/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/de/LC_MESSAGES/sphinx.js
TRACE Writing installer metadata name=PackageName("docutils")
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/de/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/de/LC_MESSAGES/sphinx.po
TRACE Writing record name=PackageName("docutils")
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/ko to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/ko
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/ko/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/ko/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/ko/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/ko/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/ko/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/ko/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/ko/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/ko/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/ca@valencia to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/ca@valencia
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/fi to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/fi
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/fi/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/fi/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/fi/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/fi/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/fi/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/fi/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/fi/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/fi/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/eo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/eo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/eo/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/eo/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/eo/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/eo/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/eo/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/eo/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/eo/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/eo/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/id to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/id
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/id/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/id/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/id/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/id/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/id/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/id/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/id/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/id/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/fr to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/fr
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/fr/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/fr/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/fr/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/fr/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/fr/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/fr/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/fr/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/fr/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/es to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/es
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/es/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/es/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/es/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/es/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/es/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/es/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/es/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/es/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/et to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/et
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/et/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/et/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/et/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/et/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/et/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/et/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/et/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/et/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/fa to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/fa
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/fa/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/fa/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/fa/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/fa/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/fa/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/fa/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/fa/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/fa/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/lt to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/lt
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/lt/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/lt/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/lt/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/lt/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/lt/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/lt/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/lt/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/lt/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/cy to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/cy
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/cy/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/cy/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/cy/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/cy/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/cy/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/cy/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/cy/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/cy/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/eu to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/eu
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/eu/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/eu/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/eu/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/eu/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/eu/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/eu/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/eu/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/eu/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/ta to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/ta
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/ta/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/ta/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/ta/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/ta/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/ta/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/ta/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/ta/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/ta/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/fr_FR to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/fr_FR
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/fr_FR/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/fr_FR/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/fr_FR/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/fr_FR/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/fr_FR/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/fr_FR/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/fr_FR/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/fr_FR/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/tr to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/tr
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/tr/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/tr/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/tr/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/tr/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/tr/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/tr/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/tr/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/tr/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/de_DE to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/de_DE
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/en_FR to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/en_FR
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/en_FR/LC_MESSAGES to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/en_FR/LC_MESSAGES
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/en_FR/LC_MESSAGES/sphinx.mo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/en_FR/LC_MESSAGES/sphinx.mo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/en_FR/LC_MESSAGES/sphinx.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/en_FR/LC_MESSAGES/sphinx.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/locale/en_FR/LC_MESSAGES/sphinx.po to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/locale/en_FR/LC_MESSAGES/sphinx.po
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/config.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/config.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/writers to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/writers
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/writers/texinfo.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/writers/texinfo.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/writers/manpage.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/writers/manpage.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/writers/html5.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/writers/html5.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/writers/html.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/writers/html.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/writers/xml.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/writers/xml.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/writers/__init__.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/writers/__init__.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/writers/text.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/writers/text.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/writers/latex.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/writers/latex.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/util to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/util
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/util/tags.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/util/tags.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/util/logging.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/util/logging.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/util/console.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/util/console.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/util/_importer.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/util/_importer.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/util/_timestamps.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/util/_timestamps.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/util/parsing.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/util/parsing.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/util/build_phase.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/util/build_phase.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/util/texescape.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/util/texescape.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/util/_io.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/util/_io.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/util/docutils.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/util/docutils.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/util/fileutil.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/util/fileutil.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/util/__init__.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/util/__init__.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/util/_uri.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/util/_uri.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/util/docfields.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/util/docfields.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/util/_files.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/util/_files.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/util/display.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/util/display.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/util/docstrings.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/util/docstrings.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/util/matching.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/util/matching.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/util/png.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/util/png.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/util/inspect.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/util/inspect.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/util/cfamily.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/util/cfamily.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/util/http_date.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/util/http_date.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/util/_serialise.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/util/_serialise.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/util/index_entries.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/util/index_entries.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/util/images.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/util/images.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/util/osutil.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/util/osutil.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/util/template.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/util/template.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/util/i18n.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/util/i18n.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/util/math.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/util/math.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/util/_pathlib.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/util/_pathlib.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/util/nodes.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/util/nodes.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/util/requests.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/util/requests.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/util/_inventory_file_reader.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/util/_inventory_file_reader.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/util/typing.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/util/typing.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/util/parallel.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/util/parallel.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/util/_lines.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/util/_lines.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/util/rst.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/util/rst.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/util/inventory.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/util/inventory.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/builders to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/builders
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/builders/texinfo.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/builders/texinfo.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/builders/changes.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/builders/changes.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/builders/manpage.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/builders/manpage.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/builders/gettext.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/builders/gettext.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/builders/latex to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/builders/latex
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/builders/latex/transforms.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/builders/latex/transforms.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/builders/latex/theming.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/builders/latex/theming.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/builders/latex/util.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/builders/latex/util.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/builders/latex/constants.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/builders/latex/constants.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/builders/latex/__init__.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/builders/latex/__init__.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/builders/latex/nodes.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/builders/latex/nodes.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/builders/xml.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/builders/xml.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/builders/__init__.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/builders/__init__.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/builders/html to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/builders/html
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/builders/html/_assets.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/builders/html/_assets.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/builders/html/__init__.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/builders/html/__init__.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/builders/html/_build_info.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/builders/html/_build_info.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/builders/text.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/builders/text.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/builders/linkcheck.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/builders/linkcheck.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/builders/dirhtml.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/builders/dirhtml.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/builders/singlehtml.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/builders/singlehtml.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/builders/dummy.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/builders/dummy.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/builders/_epub_base.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/builders/_epub_base.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/builders/epub3.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/builders/epub3.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/roles.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/roles.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/deprecation.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/deprecation.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/registry.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/registry.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/events.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/events.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/ext to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/ext
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/ext/imgconverter.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/ext/imgconverter.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/ext/extlinks.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/ext/extlinks.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/ext/mathjax.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/ext/mathjax.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/ext/autodoc to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/ext/autodoc
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/ext/autodoc/importer.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/ext/autodoc/importer.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/ext/autodoc/preserve_defaults.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/ext/autodoc/preserve_defaults.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/ext/autodoc/__init__.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/ext/autodoc/__init__.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/ext/autodoc/directive.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/ext/autodoc/directive.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/ext/autodoc/typehints.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/ext/autodoc/typehints.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/ext/autodoc/type_comment.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/ext/autodoc/type_comment.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/ext/autodoc/mock.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/ext/autodoc/mock.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/ext/graphviz.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/ext/graphviz.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/ext/githubpages.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/ext/githubpages.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/ext/duration.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/ext/duration.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/ext/apidoc to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/ext/apidoc
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/ext/__init__.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/ext/__init__.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/ext/linkcode.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/ext/linkcode.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/ext/coverage.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/ext/coverage.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/ext/imgmath.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/ext/imgmath.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/ext/inheritance_diagram.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/ext/inheritance_diagram.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/ext/napoleon to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/ext/napoleon
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/ext/napoleon/__init__.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/ext/napoleon/__init__.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/ext/napoleon/docstring.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/ext/napoleon/docstring.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/ext/autosummary to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/ext/autosummary
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/ext/autosummary/generate.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/ext/autosummary/generate.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/ext/autosummary/__init__.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/ext/autosummary/__init__.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/ext/autosummary/templates to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/ext/autosummary/templates
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/ext/autosummary/templates/autosummary to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/ext/autosummary/templates/autosummary
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/ext/autosummary/templates/autosummary/class.rst to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/ext/autosummary/templates/autosummary/class.rst
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/ext/autosummary/templates/autosummary/base.rst to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/ext/autosummary/templates/autosummary/base.rst
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/ext/autosummary/templates/autosummary/module.rst to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/ext/autosummary/templates/autosummary/module.rst
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/ext/doctest.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/ext/doctest.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/ext/ifconfig.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/ext/ifconfig.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/ext/todo.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/ext/todo.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/ext/intersphinx to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/ext/intersphinx
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/ext/viewcode.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/ext/viewcode.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/ext/autosectionlabel.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/ext/autosectionlabel.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/io.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/io.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/pycode to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/pycode
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/pycode/__init__.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/pycode/__init__.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/pycode/parser.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/pycode/parser.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/pycode/ast.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/pycode/ast.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/__init__.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/__init__.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/addnodes.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/addnodes.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/parsers.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/parsers.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/search to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/search
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/search/minified-js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/search/minified-js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/search/minified-js/russian-stemmer.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/search/minified-js/russian-stemmer.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/search/minified-js/portuguese-stemmer.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/search/minified-js/portuguese-stemmer.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/search/minified-js/french-stemmer.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/search/minified-js/french-stemmer.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/search/minified-js/romanian-stemmer.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/search/minified-js/romanian-stemmer.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/search/minified-js/hungarian-stemmer.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/search/minified-js/hungarian-stemmer.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/search/minified-js/finnish-stemmer.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/search/minified-js/finnish-stemmer.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/search/minified-js/norwegian-stemmer.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/search/minified-js/norwegian-stemmer.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/search/minified-js/base-stemmer.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/search/minified-js/base-stemmer.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/search/minified-js/dutch-stemmer.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/search/minified-js/dutch-stemmer.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/search/minified-js/turkish-stemmer.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/search/minified-js/turkish-stemmer.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/search/minified-js/swedish-stemmer.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/search/minified-js/swedish-stemmer.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/search/minified-js/spanish-stemmer.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/search/minified-js/spanish-stemmer.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/search/minified-js/porter-stemmer.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/search/minified-js/porter-stemmer.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/search/minified-js/italian-stemmer.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/search/minified-js/italian-stemmer.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/search/minified-js/danish-stemmer.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/search/minified-js/danish-stemmer.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/search/minified-js/german-stemmer.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/search/minified-js/german-stemmer.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/search/ja.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/search/ja.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/search/pt.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/search/pt.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/search/__init__.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/search/__init__.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/search/no.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/search/no.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/search/ru.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/search/ru.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/search/non-minified-js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/search/non-minified-js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/search/non-minified-js/russian-stemmer.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/search/non-minified-js/russian-stemmer.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/search/non-minified-js/portuguese-stemmer.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/search/non-minified-js/portuguese-stemmer.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/search/non-minified-js/french-stemmer.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/search/non-minified-js/french-stemmer.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/search/non-minified-js/romanian-stemmer.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/search/non-minified-js/romanian-stemmer.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/search/non-minified-js/hungarian-stemmer.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/search/non-minified-js/hungarian-stemmer.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/search/non-minified-js/finnish-stemmer.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/search/non-minified-js/finnish-stemmer.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/search/non-minified-js/norwegian-stemmer.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/search/non-minified-js/norwegian-stemmer.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/search/non-minified-js/base-stemmer.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/search/non-minified-js/base-stemmer.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/search/non-minified-js/dutch-stemmer.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/search/non-minified-js/dutch-stemmer.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/search/non-minified-js/turkish-stemmer.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/search/non-minified-js/turkish-stemmer.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/search/non-minified-js/swedish-stemmer.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/search/non-minified-js/swedish-stemmer.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/search/non-minified-js/spanish-stemmer.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/search/non-minified-js/spanish-stemmer.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/search/non-minified-js/porter-stemmer.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/search/non-minified-js/porter-stemmer.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/search/non-minified-js/italian-stemmer.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/search/non-minified-js/italian-stemmer.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/search/non-minified-js/danish-stemmer.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/search/non-minified-js/danish-stemmer.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/search/non-minified-js/german-stemmer.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/search/non-minified-js/german-stemmer.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/search/fi.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/search/fi.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/search/hu.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/search/hu.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/search/fr.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/search/fr.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/search/nl.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/search/nl.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/search/zh.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/search/zh.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/search/sv.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/search/sv.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/search/en.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/search/en.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/search/tr.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/search/tr.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/search/ro.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/search/ro.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/search/es.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/search/es.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/search/it.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/search/it.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/search/de.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/search/de.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/search/da.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/search/da.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/application.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/application.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/testing to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/testing
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/testing/util.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/testing/util.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/testing/__init__.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/testing/__init__.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/testing/restructuredtext.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/testing/restructuredtext.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/testing/path.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/testing/path.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/testing/fixtures.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/testing/fixtures.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/transforms to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/transforms
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/transforms/references.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/transforms/references.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/transforms/__init__.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/transforms/__init__.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/transforms/compact_bullet_list.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/transforms/compact_bullet_list.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/transforms/i18n.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/transforms/i18n.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/transforms/post_transforms to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/transforms/post_transforms
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/transforms/post_transforms/code.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/transforms/post_transforms/code.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/transforms/post_transforms/__init__.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/transforms/post_transforms/__init__.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/transforms/post_transforms/images.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/transforms/post_transforms/images.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/extension.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/extension.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/versioning.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/versioning.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/_cli to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/_cli
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/py.typed to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/py.typed
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/environment to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/environment
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/environment/collectors to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/environment/collectors
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/environment/collectors/toctree.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/environment/collectors/toctree.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/environment/collectors/metadata.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/environment/collectors/metadata.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/environment/collectors/asset.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/environment/collectors/asset.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/environment/collectors/__init__.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/environment/collectors/__init__.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/environment/collectors/dependencies.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/environment/collectors/dependencies.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/environment/collectors/title.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/environment/collectors/title.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/environment/__init__.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/environment/__init__.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/environment/adapters to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/environment/adapters
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/environment/adapters/toctree.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/environment/adapters/toctree.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/environment/adapters/asset.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/environment/adapters/asset.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/environment/adapters/__init__.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/environment/adapters/__init__.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/environment/adapters/indexentries.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/environment/adapters/indexentries.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/templates to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/templates
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/templates/htmlhelp to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/templates/htmlhelp
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/templates/htmlhelp/project.stp to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/templates/htmlhelp/project.stp
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/templates/htmlhelp/project.hhp to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/templates/htmlhelp/project.hhp
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/templates/htmlhelp/project.hhc to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/templates/htmlhelp/project.hhc
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/templates/imgmath to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/templates/imgmath
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/templates/imgmath/template.tex.jinja to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/templates/imgmath/template.tex.jinja
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/templates/imgmath/preview.tex.jinja to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/templates/imgmath/preview.tex.jinja
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/templates/latex to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/templates/latex
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/templates/latex/tabulary.tex.jinja to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/templates/latex/tabulary.tex.jinja
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/templates/latex/sphinxmessages.sty.jinja to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/templates/latex/sphinxmessages.sty.jinja
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/templates/latex/longtable.tex.jinja to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/templates/latex/longtable.tex.jinja
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/templates/latex/latex.tex.jinja to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/templates/latex/latex.tex.jinja
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/templates/latex/tabular.tex.jinja to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/templates/latex/tabular.tex.jinja
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/templates/apidoc to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/templates/apidoc
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/templates/apidoc/package.rst.jinja to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/templates/apidoc/package.rst.jinja
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/templates/apidoc/toc.rst.jinja to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/templates/apidoc/toc.rst.jinja
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/templates/apidoc/module.rst.jinja to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/templates/apidoc/module.rst.jinja
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/templates/quickstart to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/templates/quickstart
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/templates/quickstart/make.bat.new.jinja to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/templates/quickstart/make.bat.new.jinja
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/templates/quickstart/Makefile.new.jinja to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/templates/quickstart/Makefile.new.jinja
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/templates/quickstart/conf.py.jinja to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/templates/quickstart/conf.py.jinja
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/templates/quickstart/root_doc.rst.jinja to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/templates/quickstart/root_doc.rst.jinja
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/templates/epub3 to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/templates/epub3
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/templates/epub3/container.xml to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/templates/epub3/container.xml
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/templates/epub3/toc.ncx.jinja to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/templates/epub3/toc.ncx.jinja
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/templates/epub3/mimetype to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/templates/epub3/mimetype
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/templates/epub3/content.opf.jinja to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/templates/epub3/content.opf.jinja
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/templates/epub3/nav.xhtml.jinja to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/templates/epub3/nav.xhtml.jinja
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/templates/gettext to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/templates/gettext
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/templates/gettext/message.pot.jinja to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/templates/gettext/message.pot.jinja
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/templates/graphviz to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/templates/graphviz
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/templates/graphviz/graphviz.css to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/templates/graphviz/graphviz.css
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/templates/texinfo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/templates/texinfo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/templates/texinfo/Makefile to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/templates/texinfo/Makefile
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/errors.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/errors.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/highlighting.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/highlighting.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/pygments_styles.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/pygments_styles.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/classic to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/classic
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/classic/layout.html to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/classic/layout.html
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/classic/static to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/classic/static
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/classic/static/classic.css.jinja to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/classic/static/classic.css.jinja
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/classic/static/sidebar.js.jinja to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/classic/static/sidebar.js.jinja
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/classic/theme.toml to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/classic/theme.toml
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/pyramid to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/pyramid
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/pyramid/layout.html to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/pyramid/layout.html
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/pyramid/static to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/pyramid/static
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/pyramid/static/headerbg.png to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/pyramid/static/headerbg.png
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/pyramid/static/dialog-warning.png to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/pyramid/static/dialog-warning.png
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/pyramid/static/transparent.gif to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/pyramid/static/transparent.gif
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/pyramid/static/epub.css.jinja to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/pyramid/static/epub.css.jinja
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/pyramid/static/ie6.css to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/pyramid/static/ie6.css
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/pyramid/static/dialog-todo.png to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/pyramid/static/dialog-todo.png
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/pyramid/static/dialog-seealso.png to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/pyramid/static/dialog-seealso.png
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/pyramid/static/footerbg.png to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/pyramid/static/footerbg.png
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/pyramid/static/dialog-topic.png to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/pyramid/static/dialog-topic.png
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/pyramid/static/middlebg.png to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/pyramid/static/middlebg.png
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/pyramid/static/dialog-note.png to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/pyramid/static/dialog-note.png
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/pyramid/static/pyramid.css.jinja to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/pyramid/static/pyramid.css.jinja
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/pyramid/theme.toml to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/pyramid/theme.toml
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/bizstyle to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/bizstyle
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/bizstyle/layout.html to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/bizstyle/layout.html
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/bizstyle/static to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/bizstyle/static
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/bizstyle/static/bizstyle.css.jinja to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/bizstyle/static/bizstyle.css.jinja
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/bizstyle/static/css3-mediaqueries.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/bizstyle/static/css3-mediaqueries.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/bizstyle/static/bizstyle.js.jinja to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/bizstyle/static/bizstyle.js.jinja
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/bizstyle/static/css3-mediaqueries_src.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/bizstyle/static/css3-mediaqueries_src.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/bizstyle/static/background_b01.png to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/bizstyle/static/background_b01.png
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/bizstyle/theme.toml to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/bizstyle/theme.toml
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/agogo to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/agogo
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/agogo/layout.html to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/agogo/layout.html
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/agogo/static to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/agogo/static
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/agogo/static/agogo.css.jinja to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/agogo/static/agogo.css.jinja
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/agogo/static/bgtop.png to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/agogo/static/bgtop.png
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/agogo/static/bgfooter.png to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/agogo/static/bgfooter.png
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/agogo/theme.toml to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/agogo/theme.toml
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/basic to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/basic
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/basic/opensearch.xml to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/basic/opensearch.xml
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/basic/page.html to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/basic/page.html
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/basic/globaltoc.html to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/basic/globaltoc.html
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/basic/defindex.html to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/basic/defindex.html
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/basic/changes to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/basic/changes
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/basic/changes/frameset.html to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/basic/changes/frameset.html
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/basic/changes/rstsource.html to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/basic/changes/rstsource.html
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/basic/changes/versionchanges.html to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/basic/changes/versionchanges.html
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/basic/layout.html to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/basic/layout.html
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/basic/genindex-split.html to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/basic/genindex-split.html
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/basic/genindex-single.html to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/basic/genindex-single.html
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/basic/static to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/basic/static
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/basic/static/plus.png to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/basic/static/plus.png
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/basic/static/sphinx_highlight.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/basic/static/sphinx_highlight.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/basic/static/searchtools.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/basic/static/searchtools.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/basic/static/file.png to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/basic/static/file.png
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/basic/static/minus.png to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/basic/static/minus.png
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/basic/static/basic.css.jinja to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/basic/static/basic.css.jinja
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/basic/static/language_data.js.jinja to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/basic/static/language_data.js.jinja
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/basic/static/doctools.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/basic/static/doctools.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/basic/static/documentation_options.js.jinja to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/basic/static/documentation_options.js.jinja
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/basic/theme.toml to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/basic/theme.toml
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/basic/genindex.html to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/basic/genindex.html
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/basic/localtoc.html to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/basic/localtoc.html
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/basic/search.html to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/basic/search.html
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/basic/searchfield.html to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/basic/searchfield.html
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/basic/sourcelink.html to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/basic/sourcelink.html
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/basic/relations.html to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/basic/relations.html
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/basic/domainindex.html to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/basic/domainindex.html
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/basic/searchbox.html to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/basic/searchbox.html
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/default to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/default
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/default/static to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/default/static
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/default/static/default.css to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/default/static/default.css
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/default/theme.toml to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/default/theme.toml
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/scrolls to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/scrolls
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/scrolls/artwork to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/scrolls/artwork
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/scrolls/artwork/logo.svg to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/scrolls/artwork/logo.svg
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/scrolls/layout.html to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/scrolls/layout.html
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/scrolls/static to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/scrolls/static
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/scrolls/static/headerbg.png to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/scrolls/static/headerbg.png
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/scrolls/static/watermark.png to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/scrolls/static/watermark.png
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/scrolls/static/metal.png to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/scrolls/static/metal.png
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/scrolls/static/scrolls.css.jinja to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/scrolls/static/scrolls.css.jinja
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/scrolls/static/watermark_blur.png to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/scrolls/static/watermark_blur.png
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/scrolls/static/darkmetal.png to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/scrolls/static/darkmetal.png
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/scrolls/static/logo.png to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/scrolls/static/logo.png
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/scrolls/static/print.css to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/scrolls/static/print.css
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/scrolls/static/theme_extras.js to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/scrolls/static/theme_extras.js
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/scrolls/static/navigation.png to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/scrolls/static/navigation.png
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/scrolls/theme.toml to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/scrolls/theme.toml
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/haiku to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/haiku
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/haiku/layout.html to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/haiku/layout.html
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/haiku/static to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/haiku/static
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/haiku/static/haiku.css.jinja to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/haiku/static/haiku.css.jinja
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/haiku/static/alert_info_32.png to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/haiku/static/alert_info_32.png
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/haiku/static/bullet_orange.png to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/haiku/static/bullet_orange.png
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/haiku/static/alert_warning_32.png to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/haiku/static/alert_warning_32.png
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/haiku/static/bg-page.png to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/haiku/static/bg-page.png
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/haiku/theme.toml to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/haiku/theme.toml
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/nature to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/nature
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/nature/static to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/nature/static
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/nature/static/nature.css.jinja to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/nature/static/nature.css.jinja
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/nature/theme.toml to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/nature/theme.toml
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/traditional to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/traditional
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/traditional/static to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/traditional/static
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/traditional/static/traditional.css.jinja to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/traditional/static/traditional.css.jinja
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/traditional/theme.toml to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/traditional/theme.toml
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/nonav to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/nonav
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/nonav/layout.html to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/nonav/layout.html
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/nonav/static to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/nonav/static
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/nonav/static/nonav.css.jinja to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/nonav/static/nonav.css.jinja
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/nonav/theme.toml to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/nonav/theme.toml
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/sphinxdoc to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/sphinxdoc
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/sphinxdoc/static to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/sphinxdoc/static
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/sphinxdoc/static/contents.png to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/sphinxdoc/static/contents.png
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/sphinxdoc/static/sphinxdoc.css.jinja to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/sphinxdoc/static/sphinxdoc.css.jinja
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/sphinxdoc/static/navigation.png to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/sphinxdoc/static/navigation.png
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/sphinxdoc/theme.toml to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/sphinxdoc/theme.toml
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/epub to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/epub
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/epub/epub-cover.html to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/epub/epub-cover.html
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/epub/layout.html to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/epub/layout.html
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/epub/static to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/epub/static
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/epub/static/epub.css.jinja to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/epub/static/epub.css.jinja
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/themes/epub/theme.toml to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/themes/epub/theme.toml
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/texinputs_win to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/texinputs_win
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/texinputs_win/Makefile.jinja to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/texinputs_win/Makefile.jinja
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/__main__.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/__main__.py
TRACE Cloning /Users/zb/.cache/uv/archive-v0/evVrlRUVIsvtH8TIfvz5g/sphinx/project.py to /Users/zb/workspace/uv/example/.venv/lib/python3.13/site-packages/sphinx/project.py
TRACE Extracted 2 files name=PackageName("sphinx")
TRACE Writing entrypoints name=PackageName("sphinx")
TRACE No data name=PackageName("sphinx")
TRACE Writing installer metadata name=PackageName("sphinx")
TRACE Writing record name=PackageName("sphinx")
Installed 25 packages in 190ms
 + alabaster==0.7.16
 + alabaster==1.0.0
 + babel==2.17.0
 + certifi==2025.1.31
 + charset-normalizer==3.4.1
 + docutils==0.17.1
 + docutils==0.21.2
 + idna==3.10
 + imagesize==1.4.1
 + jinja2==3.1.5
 + markupsafe==3.0.2
 + packaging==24.2
 + pygments==2.19.1
 + requests==2.32.3
 + roman-numerals-py==3.0.0
 + snowballstemmer==2.2.0
 + sphinx==4.5.0
 + sphinx==8.2.0
 + sphinxcontrib-applehelp==2.0.0
 + sphinxcontrib-devhelp==2.0.0
 + sphinxcontrib-htmlhelp==2.1.0
 + sphinxcontrib-jsmath==1.0.1
 + sphinxcontrib-qthelp==2.0.0
 + sphinxcontrib-serializinghtml==2.0.0
 + urllib3==2.3.0
</pre>
</details>

---

_Comment by @charliermarsh on 2025-02-20 00:03_

Yeah.

---

_Comment by @charliermarsh on 2025-02-20 00:07_

I think it's just a bug in the installer phase though, not the resolver, which is good. It should be pretty easy to fix. I'm surprised this hasn't come up before.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-02-20 05:01_

---

_Closed by @charliermarsh on 2025-02-20 20:17_

---
