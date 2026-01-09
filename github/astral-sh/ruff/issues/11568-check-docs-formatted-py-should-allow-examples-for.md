---
number: 11568
title: "`check_docs_formatted.py` should allow examples for PYI rules to be formatted like stub files"
type: issue
state: closed
author: AlexWaygood
labels:
  - documentation
  - help wanted
  - ci
assignees: []
created_at: 2024-05-27T14:54:31Z
updated_at: 2024-09-03T22:47:20Z
url: https://github.com/astral-sh/ruff/issues/11568
synced_at: 2026-01-07T13:12:15-06:00
---

# `check_docs_formatted.py` should allow examples for PYI rules to be formatted like stub files

---

_Issue opened by @AlexWaygood on 2024-05-27 14:54_

Currently the script complains if you try to apply `.pyi`-style formatting to any examples. But many of the PYI rules are specific to stubs, so it makes sense to format the examples for these rules as though they are stubs.

---

_Label `documentation` added by @AlexWaygood on 2024-05-27 14:54_

---

_Label `ci` added by @AlexWaygood on 2024-05-27 14:54_

---

_Referenced in [astral-sh/ruff#11541](../../astral-sh/ruff/pulls/11541.md) on 2024-05-27 14:54_

---

_Comment by @MichaReiser on 2024-05-27 14:59_

Could we extend our parsing to support markdown language specifier 

````
```pyi
stub_file
```
````

---

_Renamed from "`check_docs_formatted.py` should allow examples for PYI files to be formatted like stub files" to "`check_docs_formatted.py` should allow examples for PYI rules to be formatted like stub files" by @AlexWaygood on 2024-05-27 15:01_

---

_Comment by @AlexWaygood on 2024-05-27 15:05_

> Could we extend our parsing to support markdown language specifier

That's a really interesting idea. I've never seen anybody use `pyi` as a markdown language specifier anywhere else, as in terms of syntax highlighting `.pyi` files obey the exact same rules as `.py` files. But for us it makes total sense, as we want to differentiate between the two in terms of formatting style.

---

_Label `help wanted` added by @MichaReiser on 2024-05-27 15:39_

---

_Comment by @charliermarsh on 2024-05-29 00:29_

What does MkDocs / GitHub do if you use `pyi` as the specifier? I assume it's fine?

---

_Comment by @Avasam on 2024-05-29 01:51_

> What does GitHub do if you use pyi as the specifier?

Best way to know is to try!

```py
# py
class A:
  def foo(bar: None = None): pass
  ...
```
```pyi
# pyi
class A:
  def foo(bar: None = None): pass
  ...
```

---

_Comment by @charliermarsh on 2024-05-29 02:05_

Interesting. Discord at least doesn't get it:

![Screenshot 2024-05-28 at 10 04 27 PM](https://github.com/astral-sh/ruff/assets/1309177/bb79e3db-1f2b-4e8a-969f-d54deac42212)


---

_Comment by @Avasam on 2024-05-29 04:34_

Discord uses https://github.com/highlightjs/highlight.js can always make a request to add `pyi` to the Python aliases
![image](https://github.com/astral-sh/ruff/assets/1350584/09e8f7e1-b630-4768-835a-b76efb9b2d0c)

It seems that VSCode uses highlight.js too
![image](https://github.com/astral-sh/ruff/assets/1350584/7b361527-952b-4b29-81cf-6b1e2fd5099a)
https://github.com/microsoft/vscode/issues/138855#issuecomment-991301667

GitHub uses https://github.com/github-linguist/linguist
Source: https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/creating-and-highlighting-code-blocks#syntax-highlighting

MKDocs uses https://github.com/Python-Markdown/markdown/blob/master/docs/extensions/code_hilite.md which uses pygment, which looks like it supports `pyi` as a file extension, but not as a shortname (untested but here's the doc: https://pygments.org/docs/lexers/#pygments.lexers.python.PythonLexer ). 

---

_Referenced in [astral-sh/ruff#13087](../../astral-sh/ruff/pulls/13087.md) on 2024-08-24 16:24_

---

_Referenced in [astral-sh/ruff#13116](../../astral-sh/ruff/pulls/13116.md) on 2024-08-26 21:57_

---

_Comment by @calumy on 2024-09-03 22:39_

Closed by #13116 

---

_Comment by @zanieb on 2024-09-03 22:47_

Thanks!

---

_Closed by @zanieb on 2024-09-03 22:47_

---
