```yaml
number: 5697
title: "F401: Improve message for attempted re-export of symbols in `__init__` files "
type: issue
state: closed
author: duckinator
labels:
  - good first issue
  - accepted
assignees: []
created_at: 2023-07-11T23:02:35Z
updated_at: 2024-03-13T17:58:27Z
url: https://github.com/astral-sh/ruff/issues/5697
synced_at: 2026-01-12T15:54:45Z
```

# F401: Improve message for attempted re-export of symbols in `__init__` files 

---

_@duckinator_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

## Problem

A pattern I frequently use is to have `__init__.py` re-export `__version__` from elsewhere, and run the release process whenever that file is modified on the main branch.

If I have `__version__ = '1.2.3'` directly in `__init__.py`, it's fine. If I do `from .version import __version__`, ruff complains about it being unused.

The real-world example where I encountered this is [duckinator/bork](https://github.com/duckinator/bork).

## Code

In `blah/version.py`:

```
__version__ = '1.2.3'
```

In `blah/__init__.py`:

```
from .version import __version__
```

## Command & Output

The testcase consists of nothing but `blah/version.py`, `blah/__init__.py`.

```shellsession
puppy@orthrus.fox:~/ruff-testcase$ tree -a
.
└── blah
    ├── __init__.py
    └── version.py

2 directories, 2 files
puppy@orthrus.fox:~/ruff-testcase$ python3 -m venv ./venv && . venv/bin/activate && pip3 install ruff
Collecting ruff
  Using cached ruff-0.0.277-py3-none-freebsd_13_1_RELEASE_p6_amd64.whl
Installing collected packages: ruff
Successfully installed ruff-0.0.277

[notice] A new release of pip is available: 23.0.1 -> 23.1.2
[notice] To update, run: pip install --upgrade pip
(venv) puppy@orthrus.fox:~/ruff-testcase$ ruff --version
ruff 0.0.277
(venv) puppy@orthrus.fox:~/ruff-testcase$ cat blah/__init__.py
from .version import __version__
(venv) puppy@orthrus.fox:~/ruff-testcase$ cat blah/version.py
__version__ = '1.2.3'
(venv) puppy@orthrus.fox:~/ruff-testcase$ ruff check --isolated --select F401 blah/
blah/__init__.py:1:22: F401 [*] `.version.__version__` imported but unused
Found 1 error.
[*] 1 potentially fixable with the --fix option.
(venv) puppy@orthrus.fox:~/ruff-testcase$ 
```

## Workaround

Just slap `# noqa: F401` at the end of the line:

```
from .version import __version__  # noqa: F401
```

I feel like this shouldn't be necessary, though, which is why I opened this issue.

---

_Comment by @duckinator on 2023-07-11 23:14_

Also, `__version__` becomes replaced with a bold-text "version" if you use `--show-source` lol

![image](https://github.com/astral-sh/ruff/assets/39698/37c539ae-9166-4f0e-b8f2-51b169a5ed08)


---

_Comment by @zanieb on 2023-07-11 23:42_

Hm interesting the bold text doesn't happen in general 

```
example.py:1:1: F821 Undefined name `__test__`
  |
1 | __test__
  | ^^^^^^^^ F821
  |

Found 1 error.
```

but I can reproduce with your example.

```
example.py:1:22: F401 [*] `.version.__version__` imported but unused
  |
1 | from .version import __version__
  |                      ^^^^^^^^^^^ F401
  |
  = help: Remove unused import: `.version.version`

Found 1 error.
[*] 1 potentially fixable with the --fix option.
```

Regarding the original issue, you should "re-export" the name if you want it to be a part of the public interface of your module i.e.

```python
from .version import __version__ as __version__
```

You can also define it in `__all__`

```python
from .version import __version__

__all__ = ['__version__']
```

both of these will result in no violation being raised. See the [Pyright library interface documentation](https://github.com/microsoft/pyright/blob/ada1ea63e8360fe3bd203bff3b0069c34d142817/docs/typed-libraries.md#library-interface) for details on why that is.

---

_Comment by @duckinator on 2023-07-11 23:52_

> Hm interesting the bold text doesn't happen in general [...] but I can reproduce with your example.

I wonder if the `= help:` lines may have more complex formatting enabled that isn't the case for some other things, perhaps?

> Regarding the original issue, you should "re-export" the name if you want it to be a part of the public interface of your module i.e.

Thanks for the info. I didn't realize imports were considered private by default, somehow. Do you think it'd make sense -- possibly for `__init__.py` specifically -- to have a thing explaining `as ...`/`__all__`? If not, I think this issue can probably be closed.

---

_Comment by @zanieb on 2023-07-12 00:07_

> I wonder if the = help: lines may have more complex formatting enabled that isn't the case for some other things, perhaps?

I think you're right I reproduced this with another case. I've opened an issue for that at https://github.com/astral-sh/ruff/issues/5699

> Do you think it'd make sense -- possibly for __init__.py specifically -- to have a thing explaining as .../__all__? 

That does seem nice in `__init__.py` files especially! I updated the title, let me know if you think it makes sense!


---

_Renamed from "F401: Ruff complains about top-level re-export of `__version__`" to "F401: Improve message for attempted re-export of symbols in `__init__` files " by @zanieb on 2023-07-12 00:08_

---

_Label `help wanted` added by @zanieb on 2023-07-12 00:12_

---

_Label `accepted` added by @zanieb on 2023-07-12 00:12_

---

_Label `help wanted` removed by @zanieb on 2023-07-12 00:12_

---

_Label `good first issue` added by @zanieb on 2023-07-12 00:12_

---

_Comment by @charliermarsh on 2023-07-12 03:13_

We actually already support a custom message for these if you enable [`ignore-init-module-imports`](https://beta.ruff.rs/docs/settings/#ignore-init-module-imports), although that setting is a bit strange... Perhaps it should be removed entirely, and we should just _always_ show that custom message.

---

_Comment by @charliermarsh on 2023-07-12 03:17_

My vote would be to remove that setting and make it the default (and make the autofix "suggested" rather than "automatic" for `__init__.py` files, which IIRC is the reason the setting exists in the first place -- to avoid autofixing unused imports in those cases).

---

_Comment by @edgarrmondragon on 2023-07-15 19:36_

I'd like to work on this if it's up for grabs.

EDIT: https://github.com/astral-sh/ruff/pull/5845

---

_Closed by @zanieb on 2024-03-13 17:58_

---
