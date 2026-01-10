```yaml
number: 12949
title: A005 (builtin module shadowing) should ignore private modules
type: issue
state: closed
author: jab
labels:
  - rule
  - needs-decision
  - preview
assignees: []
created_at: 2024-08-17T13:07:22Z
updated_at: 2024-11-24T02:39:06Z
url: https://github.com/astral-sh/ruff/issues/12949
synced_at: 2026-01-10T11:09:54Z
```

# A005 (builtin module shadowing) should ignore private modules

---

_Issue opened by @jab on 2024-08-17 13:07_

I just updated from ruff 0.5.0 to 0.6.1, and had to add an ignore for A005 so that ruff check would continue to pass:

https://github.com/jab/bidict/commit/57a0d0b592c2db2d8114452bb6615dcb1d64876e

Without that ignore, it would fail as follows:
```
❯ pre-commit run --all-files --verbose --show-diff-on-failure ruff
ruff.....................................................................Failed
- hook id: ruff
- duration: 0.01s
- exit code: 1

bidict/_abc.py:1:1: A005 Module `_abc` is shadowing a Python builtin module
Found 1 error.
```

This check is failing because it treats even private builtin modules as not-to-be-shadowed, and Python provides a private `_abc` builtin:
```
❯ python -c 'import _abc'  # succeeds
```

I propose that private modules be excluded from this check.

---

_Label `rule` added by @charliermarsh on 2024-08-18 22:28_

---

_Label `needs-decision` added by @charliermarsh on 2024-08-18 22:28_

---

_Label `preview` added by @charliermarsh on 2024-08-18 22:29_

---

_Comment by @charliermarsh on 2024-08-18 22:29_

Note that you can suppress this via https://docs.astral.sh/ruff/settings/#lint_flake8-builtins_builtins-allowed-modules.

---

_Comment by @charliermarsh on 2024-08-18 22:30_

I would say it's reasonable to flag these though. What do you think @AlexWaygood?

---

_Comment by @jab on 2024-08-24 19:04_

While we're waiting for others' feedback, I realized I didn't yet provide rationale for why private modules should be excluded from this check, so here is some:

Quoting from ruff's own https://docs.astral.sh/ruff/rules/import-private-name/ rule:

> [PEP 8](https://www.python.org/dev/peps/pep-0008/) states that names starting with an underscore are private. Thus, they are not intended to be used outside of the module in which they are defined. Further, as private imports are not considered part of the public API, they are prone to unexpected changes, especially outside of semantic versioning.

I'm not sure how much ruff tries to harmonize conflicting rules that users have enabled, but this could be an opportunity to improve harmony.

Also, a lot of the builtin private modules are like `_abc` (which is where I hit this), in that:
* they are "claiming" a private name that is general and useful in outside contexts (in the case of `_abc`, for example, any library that provides its own ABCs might want to put implementation details in its own private `_abc.py`)
* (I put "claiming" in quotes because) they do not intend to be taking these names away from use in outside contexts; the fact that the private names are exported is an accident of implementation (plus Python culture: assume we're all ~~"consenting adults"~~ responsible users)

Another interesting observation is that, out of the dozens of private modules in the standard library, https://docs.python.org/3/py-modindex.html documents only two of them, `_thread` and `_tkinter`, further suggesting that the others are not intended to be directly imported, and outside contexts should also have those names available for internal use.

<details><summary>Click for list of builtin private modules in Python 3.9, in case it's helpful</summary>

```
_abc
_aix_support
_ast
_asyncio
_bisect
_blake2
_bootlocale
_bootsubprocess
_bz2
_codecs
_codecs_cn
_codecs_hk
_codecs_iso2022
_codecs_jp
_codecs_kr
_codecs_tw
_collections
_collections_abc
_compat_pickle
_compression
_contextvars
_crypt
_csv
_ctypes
_ctypes_test
_curses
_curses_panel
_datetime
_dbm
_decimal
_elementtree
_functools
_hashlib
_heapq
_imp
_io
_json
_locale
_lsprof
_lzma
_markupbase
_md5
_multibytecodec
_multiprocessing
_opcode
_operator
_osx_support
_peg_parser
_pickle
_posixshmem
_posixsubprocess
_py_abc
_pydecimal
_pyio
_queue
_random
_scproxy
_sha1
_sha256
_sha3
_sha512
_signal
_sitebuiltins
_socket
_sqlite3
_sre
_ssl
_stat
_statistics
_string
_strptime
_struct
_symtable
_sysconfigdata__darwin_darwin
_testbuffer
_testcapi
_testimportmultiple
_testinternalcapi
_testmultiphase
_thread
_threading_local
_tkinter
_tracemalloc
_uuid
_virtualenv
_warnings
_weakref
_weakrefset
_xxsubinterpreters
_xxtestfuzz
_zoneinfo
```
</details>

---

_Comment by @jab on 2024-08-24 19:22_

All that said, I'm fine with just keeping my...
```toml
[tool.ruff.lint.flake8-builtins]
builtins-allowed-modules = ["_abc"]
```
config if it's not worth making any changes here.

---

_Comment by @MichaReiser on 2024-11-19 12:53_

Excluding private modules does make sense to me, considering that there are that many. @AlexWaygood what's your take on this? I would then suggest to make the change so that the rule can be stabilized in the next minor.

---

_Assigned to @AlexWaygood by @AlexWaygood on 2024-11-19 19:17_

---

_Closed by @charliermarsh on 2024-11-24 02:39_

---

_Closed by @charliermarsh on 2024-11-24 02:39_

---
