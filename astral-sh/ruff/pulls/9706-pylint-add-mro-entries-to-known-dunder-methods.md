```yaml
number: 9706
title: "[`pylint`] Add `__mro_entries__` to known dunder methods (`PLW3201`)"
type: pull_request
state: merged
author: johnslavik
labels:
  - bug
assignees: []
merged: true
base: main
head: add-mro-entries-dunder
created_at: 2024-01-30T15:25:31Z
updated_at: 2024-01-30T23:26:45Z
url: https://github.com/astral-sh/ruff/pull/9706
synced_at: 2026-01-12T15:55:30Z
```

# [`pylint`] Add `__mro_entries__` to known dunder methods (`PLW3201`)

---

_@johnslavik_

Yes, I actually need this dunder in my project.

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
This change adds [`__mro_entries__`](https://docs.python.org/3/reference/datamodel.html#object.__mro_entries__) to the list of known dunder methods.

## Test Plan

<!-- How was it tested? -->
8893a3ca5784034101701bad8024e8bf59c6f8b6.

---

_Renamed from "Add __mro_entries__ to known dunder methods" to "Add `__mro_entries__` to known dunder methods" by @johnslavik on 2024-01-30 15:26_

---

_Renamed from "Add `__mro_entries__` to known dunder methods" to "[`pylint`] Add `__mro_entries__` to known dunder methods" by @johnslavik on 2024-01-30 15:28_

---

_Comment by @github-actions[bot] on 2024-01-30 15:38_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "[`pylint`] Add `__mro_entries__` to known dunder methods" to "[`pylint`] Add `__mro_entries__` to known dunder methods (`PLW3201`)" by @johnslavik on 2024-01-30 15:43_

---

_@charliermarsh approved on 2024-01-30 16:40_

Thanks!

---

_Label `bug` added by @charliermarsh on 2024-01-30 16:41_

---

_Merged by @charliermarsh on 2024-01-30 16:41_

---

_Closed by @charliermarsh on 2024-01-30 16:41_

---

_Comment by @AlexWaygood on 2024-01-30 16:55_

I ran a small script to find all the dunders defined in CPython (not thoroughly tested; it might have bugs):

```py
import ast, re, pathlib, pprint

is_dunder = re.compile(r"__[^_](.+[^_])?__").fullmatch

class DunderCollector(ast.NodeVisitor):
    def visit_FunctionDef(self, node):
        if is_dunder(node.name):
            dunders.add(node.name)
    visit_AsyncFunctionDef = visit_FunctionDef

dunders = set()

for path in pathlib.Path(".").rglob("*.py"):
    try:
        source = path.read_text()
    except Exception as e:
        print(f"Skipping {path} due to {e}")
        continue
    try:
        tree = ast.parse(tree)
    except Exception as e:
        print(f"Skipping {path} due to {e}")
        continue
    DunderCollector().visit(tree)

pprint.pprint(sorted(dunders))
```

Run it from the CPython repo root, and it gives these results:

<details>

```
['__abs__',
 '__add__',
 '__aenter__',
 '__aexit__',
 '__aiter__',
 '__and__',
 '__anext__',
 '__await__',
 '__bool__',
 '__buffer__',
 '__bytes__',
 '__call__',
 '__ceil__',
 '__class__',
 '__class_getitem__',
 '__cmp__',
 '__complex__',
 '__conform__',
 '__contains__',
 '__copy__',
 '__deepcopy__',
 '__del__',
 '__delattr__',
 '__delete__',
 '__delitem__',
 '__dir__',
 '__divmod__',
 '__enter__',
 '__exit__',
 '__float__',
 '__floor__',
 '__floordiv__',
 '__format__',
 '__fspath__',
 '__get__',
 '__getattr__',
 '__getattribute__',
 '__getinitargs__',
 '__getitem__',
 '__getitem_inner__',
 '__getnewargs__',
 '__getnewargs_ex__',
 '__getslice__',
 '__getstate__',
 '__hash__',
 '__iadd__',
 '__iand__',
 '__ilshift__',
 '__import__',
 '__imul__',
 '__index__',
 '__init__',
 '__init_subclass__',
 '__instancecheck__',
 '__int__',
 '__invert__',
 '__ior__',
 '__isabstractmethod__',
 '__isub__',
 '__iter__',
 '__ixor__',
 '__len__',
 '__length_hint__',
 '__lshift__',
 '__members__',
 '__missing__',
 '__mod__',
 '__mro_entries__',
 '__mul__',
 '__neg__',
 '__new__',
 '__newobj__',
 '__newobj_ex__',
 '__next__',
 '__nonzero__',
 '__parameters__',
 '__pos__',
 '__post_init__',
 '__pow__',
 '__prepare__',
 '__radd__',
 '__rand__',
 '__rdivmod__',
 '__reduce__',
 '__reduce_ex__',
 '__repr__',
 '__reversed__',
 '__rfloordiv__',
 '__rich__',
 '__rich_console__',
 '__rich_measure__',
 '__rich_repr__',
 '__rlshift__',
 '__rmod__',
 '__rmul__',
 '__ror__',
 '__round__',
 '__rpow__',
 '__rrshift__',
 '__rshift__',
 '__rsub__',
 '__rtruediv__',
 '__rxor__',
 '__set__',
 '__set_name__',
 '__setattr__',
 '__setitem__',
 '__setslice__',
 '__setstate__',
 '__signature__',
 '__sizeof__',
 '__str__',
 '__sub__',
 '__subclasscheck__',
 '__subclasshook__',
 '__tp_del__',
 '__truediv__',
 '__trunc__',
 '__typing_is_unpacked_typevartuple__',
 '__typing_unpacked_tuple_args__',
 '__unicode__',
 '__version__',
 '__wrapped__',
 '__xor__']
```

</details>

Some of those are internal implementation details that we definitely shouldn't add to the list of known dunder methods, in my opinion (`__typing_is_unpacked_typevartuple__`, etc.), but it might be worth looking through that list to see if there are any we're missing that would be worth adding

---

_Comment by @trag1c on 2024-01-30 17:03_

A [community project I maintain](https://github.com/trag1c/mcoding-all-dunders) has a spreadsheet with a curated list of all* 224 dunders (iirc it includes docs too) :+1:

<sub>*excluding a few false positives like `__name___` or `__dunder__`</sub>

---

_Comment by @AlexWaygood on 2024-01-30 19:38_

> but it might be worth looking through that list to see if there are any we're missing that would be worth adding

having now done this, I think there are 0 true positives (that are not already listed by ruff) in that list my script produced :)

This is the set of dunders that were surfaced in my script and are not currently listed by ruff as excluded from this rule:

```
['__cmp__',
 '__conform__',
 '__getinitargs__',
 '__getitem_inner__',
 '__getslice__',
 '__import__',
 '__isabstractmethod__',
 '__members__',
 '__newobj__',
 '__newobj_ex__',
 '__nonzero__',
 '__parameters__',
 '__rich__',
 '__rich_console__',
 '__rich_measure__',
 '__rich_repr__',
 '__setslice__',
 '__signature__',
 '__tp_del__',
 '__typing_is_unpacked_typevartuple__',
 '__typing_unpacked_tuple_args__',
 '__unicode__',
 '__version__',
 '__wrapped__']
```

Most of them seem to be definitions in tests, internal implementation details that are undocumented, and/or leftovers from Python 2 that still haven't been cleaned up. The `__rich_*__` ones are because I have `rich` installed in a venv inside my cpython local clone, I think?

I like that the script surfaced `__import__` -- that's the literal definition of Python's `import` statement right there 😄

---

_Comment by @johnslavik on 2024-01-30 19:44_

It would be useful to add `__wrapped__` , `__isabstractmethod__` and `__signature__` in case one wants to define those as properties.
`__rich_*__` should definitely be supported, a lot of critical tools & libraries have integrations with rich (including pip or Pydantic).

---

_Comment by @johnslavik on 2024-01-30 19:51_

Let's move the discussion to #9716.

---

_Branch deleted on 2024-01-30 19:56_

---
