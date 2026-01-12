```yaml
number: 2453
title: Add __version__ to uv python module
type: issue
state: closed
author: mkleinbort-ic
labels:
  - enhancement
assignees: []
created_at: 2024-03-14T11:31:03Z
updated_at: 2024-03-14T14:07:14Z
url: https://github.com/astral-sh/uv/issues/2453
synced_at: 2026-01-12T15:58:37Z
```

# Add __version__ to uv python module

---

_@mkleinbort-ic_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Just discovered this code fails
```python
import uv
uv.__version__
```
With error

`AttributeError: module 'uv' has no attribute '__version__'`

I know it's rare for users to import `uv` as a module, but it was the "obvious" way to check the version when a jupyter notebook is open.

Note that running `uv --version` on the terminal works fine, so does running `!uv --version` on a notebook


---

_Label `enhancement` added by @AlexWaygood on 2024-03-14 11:48_

---

_Comment by @potiuk on 2024-03-14 12:57_

IMHO (and this is just comment from a side - I've learned about it when I proposed to use `_version_` in another module), we should not continue this practice and where popular packages do not have them already, we should not add them - because that only increases the confusion about status of `__version__` attribute. 

It's not only used inconsistently, but also while it's popular in some packages it's status as "standard" is clearly "NO".

It's been proposed in https://peps.python.org/pep-0396/ and **rejected**. 

Instead the standard (and simple and fast) way of retrieving version of the installed package is via importlib-metadata (https://docs.python.org/3/library/importlib.metadata.html#distribution-versions)  and this is the one I think we - as community should promote.

It's a bit hot topic - see https://mail.python.org/archives/list/python-dev@python.org/thread/EY44EMZ2R4RUMUUIVEFFVOXM2H3L427Z/#R4A266556EN26KO7XOZLPQZFPRPH7QXB where the PEP-396 has been rejected - and of course it's up to maintainers to decide, but my personal opinion is - let's stop using it (or at least let's not add it to packages that don't use it already).


---

_Comment by @mkleinbort-ic on 2024-03-14 13:13_

Ok, I thought it was standard but your argument is very clear. 

I don't fully follow the rationale in the rejections of the PEPs, but so be it.

---

_Closed by @mkleinbort-ic on 2024-03-14 13:13_

---

_Comment by @helderco on 2024-03-14 14:07_

Yes, the way to do this, instead of:

```python
import uv
uv.__version__
```

Do this instead:
```python
>>> from importlib.metadata import version
>>> version("uv")
... 0.1.20
```


---
