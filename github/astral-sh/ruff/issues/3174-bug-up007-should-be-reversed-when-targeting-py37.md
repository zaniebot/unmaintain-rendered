---
number: 3174
title: "bug: UP007 should be reversed when targeting py37"
type: issue
state: closed
author: upstartjohnvandivier
labels: []
assignees: []
created_at: 2023-02-23T16:29:29Z
updated_at: 2023-03-29T15:03:55Z
url: https://github.com/astral-sh/ruff/issues/3174
synced_at: 2026-01-07T13:12:14-06:00
---

# bug: UP007 should be reversed when targeting py37

---

_Issue opened by @upstartjohnvandivier on 2023-02-23 16:29_

thanks for the library! we love it. found another issue tho, consider the class:
```
class Foo(TypedDict):
    bar: NotRequired[Union[bool, None]]
```

ruff lint throws on the property and will autofix to use a pipe operator when `"UP"` ruleset is selected:
```
    bar: NotRequired[bool | None]
```

this syntax is enabled only in [python 3.10+](https://peps.python.org/pep-0604/) and breaks applications on an earlier version. in my case, I have `pyproject.toml` with the following subsnippet:
```
[tool.ruff]
target-version = 'py37'
```

two interesting asides:
1. I am running ruff cli inside a docker container that has python 3.10 installed. hopefully ruff is smart enough to use the pyproject.toml for reference and does try to detect via the current process version
2. i couldn't easily see documentation on how to dump or log the ruff config from inside the process, so I simply have to trust that pyproject.toml is respected and I can't really troubleshoot. I'll file a feature request to enhance `ruff -V` or some other mechanism

---

_Renamed from "UP007 should be reversed when targeting py37" to "bug: UP007 should be reversed when targeting py37" by @upstartjohnvandivier on 2023-02-23 16:29_

---

_Referenced in [astral-sh/ruff#3177](../../astral-sh/ruff/issues/3177.md) on 2023-02-23 16:38_

---

_Comment by @upstartjohnvandivier on 2023-02-23 16:43_

note: just to be a bit more clear about `reversed` instead of `ignored`

if possible, i'm hoping `NotRequired[bool | None]` will actually be linted as an error under py37 target and with autofix the expression be refactored using the `Union` keyword

---

_Comment by @charliermarsh on 2023-02-23 17:15_

That rule should only be triggered on Python 3.10+, unless you have future annotations enabled (i.e., the file contains `from __future__ import annotations`). Could it be that the configuration is off somehow? (Ruff only uses the `pyproject.toml` to determine the appropriate version, it doesn't look at your Python runtime or any other versions encoded in other files.)

---

_Comment by @charliermarsh on 2023-02-23 17:15_

(We _don't_ flag the reverse.)

---

_Comment by @henryiii on 2023-02-23 17:28_

pyupgrade has a flag,`--keep-runtime-typing`, for this. Given you can deactivate individual rules, including on a per-file basis, and you can ignore rules on a line and in a file (none of which pyupgrade can do), and you probably shouldn't have `from __future__ import annotations` on a file where runtime annotations being used, I'm not sure this extra flag is all the important to support, though. In general, `from __future__ import annotations` means you aren't planning on using the type annotations at runtime, and trying to do so will break.

---

_Comment by @upstartjohnvandivier on 2023-02-23 17:30_

this totally could be a config issue in fact I suspect it I was simply not able to debug as I was unaware of `--show-settings` previously.

I now believe `__future__ import annotations` is applied to the file in question, so there is no bug here

feel free to close this as a bug or rename it to be a feature enhancement for reversal rather than a bug, issue solved from my POV!

---

_Comment by @charliermarsh on 2023-02-23 17:34_

I'm gonna close for now, thanks for filing :)

---

_Closed by @charliermarsh on 2023-02-23 17:34_

---

_Comment by @cclauss on 2023-02-24 16:00_

Does ruff have some kind of _immediate mode_ for testing stuff like this on the command line?

In Python, we can use the REPL or do
```
python3 -c "print(2 + 2)"
```

Could we pipe sample Python code into ruff like:
```
echo "from typing import Optional ; s: Optional[str] = None" | ruff --select=UP --target-version=py37
```

---

_Comment by @charliermarsh on 2023-02-24 16:04_

That should work today if you use a dash suffix, like:

```console
❯ echo "from typing import Optional; s: Optional[str] = a" | ruff --target-version=py311 --select UP -
-:1:33: UP007 [*] Use `X | Y` for type annotations
Found 1 error.
[*] 1 potentially fixable with the --fix option.
```

---

_Comment by @cclauss on 2023-02-24 16:26_

% `echo "from typing import Optional ; s: Optional[str] = 'a'" | ruff --select=F401,Q000,UP --target-version=py311 --fix -`
```python
s: str | None = "a"
```
### Impressive!

---

_Comment by @pmhahn on 2023-03-25 13:11_

`UP007` should not be tied to `from __future__ import annotations`:
- The `X|Y` syntax requires the PEG-Parser from Python 3.9+, which is  [PEP-617](https://peps.python.org/pep-0617/)
- `annotations` is [PEP-563](https://peps.python.org/pep-0563/) and about "Postponed Evaluation of Annotations", which was even back-ported to Python 3.7

So it makes send to do the `import` for Python 3.7 and 3.8, which still use the old parser and thus do not support the new syntax.

---

_Comment by @cclauss on 2023-03-25 13:18_

### The `X | Y` syntax requires Python 3.10+ (or the future import)

Q: Is the conversion made even when `--target-version=py37` or `--target-version=py38`?

A: The conversion happens on __Py10 and above__:

% `echo "from typing import Optional ; s: Optional[str] = 'a'" | ruff --select=F401,Q000,UP --target-version=py39 --fix -`
```
from typing import Optional ; s: Optional[str] = "a"
```
% `echo "from typing import Optional ; s: Optional[str] = 'a'" | ruff --select=F401,Q000,UP --target-version=py310 --fix -`
```
s: str | None = "a"
```
---
This GitHub Action passes on Python >= v3.10 and fails on Python <= v3.9.
```yaml
name: type_hint_test
on: [push]
env:
  CODE: "def func(x: str | None) -> None: pass"
jobs:
  type_hint_test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/setup-python@v4
        with:
          python-version: |
            3.7
            3.8
            3.9
            3.10
            3.11
      - run: python3.11 -c "$CODE"  # Pass
      - run: python3.10 -c "$CODE"  # Pass
      - run: python3.9  -c "$CODE"  # Fail
      - run: python3.8  -c "$CODE"  # Fail
      - run: python3.7  -c "$CODE"  # Fail
```

---

_Comment by @henryiii on 2023-03-25 14:22_

That all looks fine - the conversion should happen if the python version (which defaults to the current version but is settable) is 3.10+, or when `from __future__ import annotations` is true. If you have a runtime use of annotations (pydantic, typer, cattrs, etc), then don't use `from __future__ import annotations`. For most code (and even most code inside a codebase with a little runtime usage), the future import allows much simpler, cleaner annotations - and typing annotations are something that really need as much cleanup as possible.

PyUpgrade itself does have a "keep runtime annotations" flag (one of _very_ few configuration options!) that will ignore `from __future__ import annotations`, but you can't set it per-file and you really shouldn't be using that import if you are using your annotations at runtime anyway. So I've not really found it useful.

I would like it if there was more granularity in error codes, though - this one could possibly be split, and personally, I'd really like `I001` split up - my problem is I like the automatic injection of the future import, but sometimes I do need it disabled on a few files that use runtime annotations.


FYI, all parsers support `A | B` **syntax**. That's a very old bit of syntax. They don't support or'ing together types at _runtime_ but that has nothing to do with the parser, only the evaluation. That's why `from __future__ import annotation` works. If they were to introduce new _syntax_ around an annotation, one that doesn't parse today, it would not work with the import either.

---

_Comment by @antdking on 2023-03-29 14:37_

currently setting up a repo, and encountered this behaviour.
I truly thought `target-version` wasn't being detected correctly.

Henryiii absolutely nailed it: There are more and more libraries relying on runtime annotations.
Gone are the times where we can rely on `__future__.annotations` for anything more than working around cyclic/hoisted references.

My 2-cents is `__future__` is ignored, **without** a flag to prevent unexpected broken code.

---

_Comment by @henryiii on 2023-03-29 15:03_

If you specifically are using a library that depends on runtime annotations, don't use `from __future__ import annotations`. That's basically a statement saying "make all annotations strings, because I'm _not_ going to use them at runtime, so I'll never need to resolve them". You absolutely can get broken runtime code without the simplification if you are using this import - that's why this "future" is currently not being added to Python in the future anymore.

Using it anyway (especially now as that looks like it's not the "future" of Python) and then asking a _very_ useful simplification to be removed from all code that is using it correctly isn't a good idea.

Static typing code is hard. Anything simplifying it is highly appreciated.

---

_Referenced in [astral-sh/ruff#4427](../../astral-sh/ruff/pulls/4427.md) on 2023-05-20 02:39_

---
