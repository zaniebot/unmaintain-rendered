```yaml
number: 21112
title: "[ty] Make auto import completions include global imports from other modules in suggestions"
type: pull_request
state: closed
author: BurntSushi
labels:
  - server
  - ty
assignees: []
draft: true
base: main
head: ag/auto-import-include-global-imports
created_at: 2025-10-28T18:10:44Z
updated_at: 2025-12-04T18:21:29Z
url: https://github.com/astral-sh/ruff/pull/21112
synced_at: 2026-01-12T15:57:16Z
```

# [ty] Make auto import completions include global imports from other modules in suggestions

---

_@BurntSushi_

Previously, our special auto-import code for discovering symbols in
other files quickly (without running ty on them) didn't take imports
into account. This PR makes a small change to do exactly that.

This in particular helps with libraries that build their public API
from submodules. In particular, numpy. This consequently improves our
numpy evaluation tasks (which includes sub-optimal ranking because of
precisely this bug). The ranking still isn't perfect, but at least the
correct result appears in the suggestions. It previously did not.

Unfortunately, this does regress some other tasks. For example, invoking
auto-import on `TypeVa<CURSOR>` now brings up `TypeVar` from a whole
bunch of modules. Presumably because it's imported in those modules.

So I guess that means this heuristic is probably wrong. How does one
differentiate imports that are meant to build out an API and just a
regular old import meant for internal use?

One idea is to perhaps down-rank symbols derived from imports, but above
symbols from private modules (beginning with `_`). This would work for
the numpy case I believe without (hopefully) regressing other tasks.


---

_Review requested from @carljm by @BurntSushi on 2025-10-28 18:10_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-10-28 18:10_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-10-28 18:10_

---

_Review requested from @sharkdp by @BurntSushi on 2025-10-28 18:10_

---

_Review requested from @dcreager by @BurntSushi on 2025-10-28 18:10_

---

_Review request for @dcreager removed by @BurntSushi on 2025-10-28 18:10_

---

_Review request for @carljm removed by @BurntSushi on 2025-10-28 18:10_

---

_Review request for @MichaReiser removed by @BurntSushi on 2025-10-28 18:10_

---

_Comment by @BurntSushi on 2025-10-28 18:11_

Demo:

https://github.com/user-attachments/assets/f1f8ac07-a1fd-4698-8fbe-cffd74631fdc



---

_Comment by @github-actions[bot] on 2025-10-28 18:12_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-28 18:14_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Label `server` added by @MichaReiser on 2025-10-28 18:28_

---

_Label `ty` added by @MichaReiser on 2025-10-28 18:28_

---

_Comment by @sharkdp on 2025-10-29 12:04_

> So I guess that means this heuristic is probably wrong. How does one
> differentiate imports that are meant to build out an API and just a
> regular old import meant for internal use?

Not a complete answer, but this is typically what `__all__` is for. In numpy's case: https://github.com/numpy/numpy/blob/5566cc4375badc1a1f218c4a1bb8924abbf34618/numpy/__init__.pyi#L647. So maybe we should use our `__all__` and star-import functionality to look for publicly available modules in a case like this?

---

_Comment by @BurntSushi on 2025-10-29 13:12_

@sharkdp Yeah I was thinking about `__all__`, but numpy doesn't use a wildcard import here.

I'm going to attack this by coming up with some minimal examples and testing out what other LSPs do.

I'll put this in draft for now.

---

_Converted to draft by @BurntSushi on 2025-10-29 13:13_

---

_Comment by @MichaReiser on 2025-10-29 13:29_

@AlexWaygood or @amyreese: Are there any other conventions we could leverge? 

---

_Comment by @AlexWaygood on 2025-10-29 13:58_

> @AlexWaygood or @amyreese: Are there any other conventions we could leverge?

Yes:
- As David says, any symbol in `__all__` is always considered publicly re-exported. In terms of the impact that this has on semantics at _runtime_, @BurntSushi is correct that this only has an impact on `*` imports. But the convention applies even outside of `*` imports: if `__all__` exists in a module, anything not listed in `__all__` is conventionally considered "private to that module".
- If `__all__` _doesn't_ exist:
	- anything defined via an import that uses a "redundant alias" is considered explicitly re-exported from a module. Similar to `__all__`, this is only _specified_ to have an impact in some specific situations, but has been adopted by the community in many other situations. I.e., it is only [_specified_](https://typing.python.org/en/latest/spec/distributing.html#import-conventions) that type checkers should view the semantics differently for redundant aliases if the import is in a stub file, but it's these days generally used by the community more broadly (in `.py` files as well as `.pyi` files) to indicate "this is being re-exported". A "redundant alias" is something like `import foo as foo`, `from . import bar as bar` or `from baz import eggs as eggs` -- the alias immediately after the `as` keyword must be identical to the symbol immediately after the `import` keyword
	- An exception to the above rule is that in `__init__.py(i)` files, imports of submodules are conventionally considered explicitly re-exported even if they do not use a redundant alias. This is what @Gankra is implementing in https://github.com/astral-sh/ruff/pull/20855
	- Anything that's defined in the module (but not defined via an import) is generally considered re-exported unless it has a name that starts with an underscore and does not end with an underscore

---

_Comment by @BurntSushi on 2025-10-29 14:46_

Thanks @AlexWaygood!

One thing that occurs to me is how and whether auto-import should differ from completions on modules already in scope. Consider this example.

Here's `bar.py`:

```python
ZQZQ = 1
```

And `foo.py`:

```python
from bar import ZQZQ
```

And now two different versions of `main.py`. First, one that already has an explicit import:

```python
import foo
foo.ZQ<CURSOR>
```

And now one that relies on auto-import:

```python
ZQ<CURSOR>
```

In the former case, `ZQZQ` is indeed a defined symbol on `foo`, but it's not considered "exported" per the typing spec as far as I can tell. We still offer completions for unexported symbols (like those that begin with an underscore), so us offering completions for them seems consistent there. Indeed, we do that today.

In the latter case, it kind of seems like we shouldn't offer `foo.ZQZQ` (but yes offer `bar.ZQZQ`) _for the purposes of auto-import_. The former case is fundamentally more constrained, where as if we allow the latter case, completions fundamentally become way more noisy.

Indeed, it seems like this is the same setup used by pyright. It offers `foo.ZQZQ` in the former case, but only `bar.ZQZQ` in the latter case:

https://github.com/user-attachments/assets/af948f01-3406-4ddb-bcc8-aa0c1301a73a



---

_Comment by @AlexWaygood on 2025-10-29 14:54_

@BurntSushi yep, that all sounds correct to me!

---

_Closed by @BurntSushi on 2025-12-04 18:21_

---
