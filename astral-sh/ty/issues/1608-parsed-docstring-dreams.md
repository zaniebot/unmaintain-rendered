---
number: 1608
title: Parsed Docstring Dreams
type: issue
state: open
author: Gankra
labels:
  - server
  - wish
assignees: []
created_at: 2025-11-21T13:34:54Z
updated_at: 2026-01-09T03:29:54Z
url: https://github.com/astral-sh/ty/issues/1608
synced_at: 2026-01-10T01:48:23Z
---

# Parsed Docstring Dreams

---

_Issue opened by @Gankra on 2025-11-21 13:34_

I don't want to file a bunch of granular issues about this right now so here's some pie-in-the-sky ideas about docstrings. All of this would require treating them like sub-ASTs (like string annotations):

# LSP Features

* If there are \`singleticks\` around an item, try to resolve the name and make it hoverable/goto-able
* If there are \`singleticks\` around an item, make it a link to the item in renderings of the docstring (hovers)
* Do semantic tokens (syntax highlighting) of doctests (and literal blocks?) 
* Make names and types hoverable/clickable in `:param str myparam:` and `:rtype: str` (any of the various formats)
* Make items in doctests (and literal blocks?) hoverable/goto-able

# Typechecker Features

* Have a diagnostic if a \`singletick\` name doesn't resolve
* Have a diagnostic if we parse a `:param myparam:` but `myparam` isn't a parameter (any of the various formats)
* Have a diagnostic if we parse a `:param int myparam:` but `myparam` isn't an `int`  (any of the various formats)
* Have a diagnostic if we parse an `:rtype: int` but the return type isn't `int` (any of the various formats)
* Typecheck doctests (and literal blocks?)

---

_Label `wish` added by @Gankra on 2025-11-21 13:34_

---

_Comment by @Gankra on 2025-11-21 13:42_

If we shipped this stuff it would simultaneously be extremely cool and aggressively annoying. But maybe JUST MAYBE the annoyingness would encourage people to cleanup their docstrings so that the various checks don't break.

---

_Comment by @Gankra on 2025-11-21 13:46_

Some notes from @AlexWaygood 

For the docs.python.org documentation, there's a bunch of symbols that are implicitly imported before any doctested code block is run in CI: https://github.com/python/cpython/blob/a8733cbc7328dbeeed50e82ef13abb197d511c46/Doc/library/typing.rst?plain=1#L5-L9

We do run some of the `typing.py` docstring codeblocks in CI, the config for that is here: https://github.com/python/cpython/blob/a8733cbc7328dbeeed50e82ef13abb197d511c46/Lib/test/test_typing.py#L11019-L11022

I think any REPL code block (that has the `>>>` chevrons) will, I think, automatically be run in CPython's CI due to that config in `test_typing.py`. So all of those should not be missing any imports. (But some REPL codeblocks explicitly test that exceptions are raised -- having those type-checked would be pretty annoying!)

For the docs.python.org documentation, only codeblocks explicitly introduced with `.. doctest::` and `.. testcode::` are tested in CI (e.g. https://github.com/python/cpython/blob/a8733cbc7328dbeeed50e82ef13abb197d511c46/Doc/library/typing.rst?plain=1#L1480-L1488 or https://github.com/python/cpython/blob/a8733cbc7328dbeeed50e82ef13abb197d511c46/Doc/library/typing.rst?plain=1#L217-L231). `.. doctest::` codeblocks are REPL snippets; `.. testcode::` codeblocks are to be run as scripts.

For projects that autogenerate their online documentation from their docstrings and use Sphinx for their online documentation, they may use similar Sphinx directives in their docstrings to denote that a codeblock in that docstring should be tested in CI.

But the stdlib doctest module pays no heed to such things; it's only if you run doctests via Sphinx that these directives have any effect.

The docs are here if you want to play around with how the stdlib doctest module looks for and executes `>>>` REPL snippets: https://docs.python.org/3/library/doctest.html

---

_Comment by @Viicos on 2025-11-21 16:36_

I think this relates to this issue: https://github.com/astral-sh/ruff/issues/19250. A while ago I started looking into how docutils parses reST, and I'd like to try and write an error tolerant parser that can be configured to some extent (e.g. to allow custom directives). This is far from trivial as it needs to map to [the docutils document tree](https://docutils.sourceforge.io/docs/ref/doctree.html), which is somewhat orthogonal to the reST syntax.

---

_Label `server` added by @carljm on 2026-01-09 03:29_

---
