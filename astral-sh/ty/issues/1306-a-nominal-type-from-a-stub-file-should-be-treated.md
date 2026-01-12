```yaml
number: 1306
title: "A nominal type from a stub file should be treated as equivalent to a nominal type from a `.py` file with the same module name"
type: issue
state: open
author: AlexWaygood
labels:
  - bug
  - typing semantics
  - stubs
assignees: []
created_at: 2025-10-04T16:54:33Z
updated_at: 2025-10-07T23:45:09Z
url: https://github.com/astral-sh/ty/issues/1306
synced_at: 2026-01-12T15:54:24Z
```

# A nominal type from a stub file should be treated as equivalent to a nominal type from a `.py` file with the same module name

---

_@AlexWaygood_

### Summary

We've been seeing strange diagnostics like [this](https://github.com/astral-sh/ruff/pull/20603#issuecomment-3345317444) in ecosystem reports for bokeh, and they're starting to show up [more](https://github.com/astral-sh/ruff/pull/20368#issuecomment-3368395654) as we add more functionality to ty:

```
src/bokeh/model/model.py:496:16: error[invalid-return-type] Return type does not match returned value: expected `set[src.bokeh.model.model.Model]`, found `set[src.bokeh.model.model.Model]`
```

A [minimal repro](https://play.ty.dev/da6dbbed-748c-44bd-8f35-b2e34145b212) of this issue looks something like the following:

````md

`package/__init__.py`:

```py
from .foo import MyClass

def make_MyClass() -> MyClass:
    return MyClass()
```

`package/foo.pyi`:

```py
class MyClass: ...
```

`package/foo.py`:

```py
class MyClass: ...

def get_MyClass() -> MyClass:
    from . import make_MyClass

    # error: [invalid-return-type] "Return type does not match returned value: expected `package.foo.MyClass`, found `package.foo.MyClass`"
    return make_MyClass()
```
````

The issue is that ty:
1. Views `MyClass` from the `foo.py` file and `MyClass` from `foo.pyi` as being different classes
2. Understands the `get_MyClass` function as being annotated as returning an instance of `MyClass` from `foo.py`
3. Understands the `get_MyClass` function as _actually_ returning an instance of `MyClass` from `foo.pyi`

I propose that we fix this by adding handling to make two nominal-instance types equivalent to each other if the two types come from modules that have the same fully qualified module names and come from the same search path.

### Version

_No response_

---

_Label `bug` added by @AlexWaygood on 2025-10-04 16:54_

---

_Label `typing semantics` added by @AlexWaygood on 2025-10-04 16:54_

---

_Label `stubs` added by @AlexWaygood on 2025-10-04 16:54_

---

_Comment by @carljm on 2025-10-06 15:29_

> I propose that we fix this by adding handling to make two nominal-instance types equivalent to each other if the two types come from modules that have the same fully qualified module names and come from the same search path.

I think this will have unexpected consequences for examples like this:

```py
if FOO:
    class C: ...
else:
    class C: ...
```

I think the case in the OP is quite tricky to handle correctly, as it's essentially another case of type-checking "unreachable" code.

How do other type-checkers handle this case?

---

_Comment by @AlexWaygood on 2025-10-06 20:54_

> How do other type-checkers handle this case?

Hmm, good question. Mypy does not appear to emit this error, but pyrefly and pyright emit the same error we do here (with just as bad a diagnostic). Pyright:

```
Type "Iterable[Model]" is not assignable to return type "Iterable[Model]"
  "typing.Iterable" is not assignable to "typing.Iterable"
    Type parameter "_T_co@Iterable" is covariant, but "Model" is not a subtype of "Model"
      "model.Model" is not assignable to "model.Model"
```

Pyrefly:

```
ERROR Returned type `Iterable[bokeh.model.model.Model@40:7-12]` is not assignable to declared return type `Iterable[bokeh.model.model.Model@79:7-12]` [bad-return]
   --> src/bokeh/model/model.py:510:16
    |
510 |         return find(self.references(), selector)
    |                ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
    |
```

Pyrefly at least gestures at the fact that the two classes have different line numbers, but still doesn't mention the crucial fact that they're defined in different files!

I'm not sure how mypy avoids emitting this error, but given that pyright and pyrefly both have similar behaviour to us, we may want to just try to improve our diagnostic message here. Though I also don't know how we'd expect bokeh to fix this kind of thing? It seems like they're doing everything correctly as per the spec.

---

_Comment by @carljm on 2025-10-07 15:46_

One possible point of view here is to say "shadowing a `.py` file with a `.pyi` is expressing intent to hide the `.py` from the type checker, so we just shouldn't check those files." Is it possible that's what mypy is doing, or does it emit other errors in the `.py` file?

That approach is unsatisfactory as an LSP, though.

I agree there's nothing wrong with what bokeh is doing here.

Better disambiguation of type names in our diagnostics would certainly be a good first step, and would be useful in lots of other cases, too.

---

_Comment by @AlexWaygood on 2025-10-07 21:54_

> One possible point of view here is to say "shadowing a `.py` file with a `.pyi` is expressing intent to hide the `.py` from the type checker, so we just shouldn't check those files." Is it possible that's what mypy is doing, or does it emit other errors in the `.py` file?

That may indeed be what mypy is doing, since it emits no errors on the `.py` file, whereas pyright emits several. (But that could also just be because they're, you know, different type checkers that disagree on many things!)

---

_Comment by @erictraut on 2025-10-07 23:05_

>  I agree there's nothing wrong with what bokeh is doing here.

That's debatable. This is arguably a misuse of a stub file. I'm curious why they don't just add inline type annotations to the ".py" module. If they want to run type checking on their own code, this is the route I'd recommend.

---

_Comment by @AlexWaygood on 2025-10-07 23:15_

> This is arguably a misuse of a stub file.

Why? PEP 484, which introduced stub files, [stated](https://peps.python.org/pep-0484/#storing-and-distributing-stub-files):

> The easiest form of stub file storage and distribution is to put them alongside Python modules in the same directory. This makes them easy to find by both programmers and the tools.

---

_Comment by @erictraut on 2025-10-07 23:37_

Stub files are intended to be used as a proxy for a library when that library is not present, is written in another language, or is untyped. They are not intended to be used for type checking the code they are proxying. Doing so will result in the sort of problems that you discuss above. If a library maintainer intends to type check the code in their library, the type information should be inlined in the library code itself.

Inlined type annotations for libraries were not well considered in the days of PEP 484, but the community has made significant progress since then — first with the introduction of `py.typed` in PEP 561 and with later clarifications in the [typing spec](https://typing.python.org/en/latest/spec/distributing.html).

If you want to type check a piece of code that imports from `bokeh`, the side-by-side stub file should work fine across all type checkers. If the maintainers of `bokeh` want to run a type checker on `bokeh` itself, then I think they should move the annotations inline and delete the stub file.

---

_Comment by @AlexWaygood on 2025-10-07 23:45_

I suspect that there may still be a lot of ecosystem code out there that uses stub files side-by-side with the implementation, especially if mypy permits this practice. We might wish it weren't the case, but this is the first I'm hearing of the idea that this practice should be considered deprecated for projects that want to run type checkers on their own code.

In any event, https://github.com/astral-sh/ruff/pull/20756 improves our diagnostics to make it clearer why we view the two types as being distinct even though they have the same fully qualified name.

---
