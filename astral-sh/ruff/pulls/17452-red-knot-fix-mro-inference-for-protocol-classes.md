```yaml
number: 17452
title: "[red-knot] Fix MRO inference for protocol classes; allow inheritance from subscripted `Generic[]`; forbid subclassing unsubscripted `Generic`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/protocol-mros
created_at: 2025-04-17T18:34:22Z
updated_at: 2025-04-30T13:11:01Z
url: https://github.com/astral-sh/ruff/pull/17452
synced_at: 2026-01-10T19:03:00Z
```

# [red-knot] Fix MRO inference for protocol classes; allow inheritance from subscripted `Generic[]`; forbid subclassing unsubscripted `Generic`

---

_Pull request opened by @AlexWaygood on 2025-04-17 18:34_

## Summary

- Add a new `KnownInstanceType::Generic` variant to represent the type of the symbol `typing(_extensions).Generic`
- Add new `ClassBase::Protocol` and `ClassBase::Generic` variants, to represent the fact that classes can have these symbols in their MRO at runtime, even though these are not represented as classes in typeshed.
- Introduce a separate `SubclassOfInner` variant to represent the inner data that a `type[]` type holds. With the introduction of the new `ClassBase::Protocol` and `ClassBase::Generic` variants, the `ClassBase` enum is no longer suitable for this purpose, since `type[Protocol]` and `type[Generic]` are both meaningless.
- Remove false-positive diagnostics when inheriting from `Generic[]`
- Remove false-positive diagnostics when subscripting an old-style generic class
- Ensure that diagnostics are still emitted if a class inherits from bare `Generic`
- Correctly infer the MROs of protocol classes!

This appears to get rid of a lot of `invalid-base` false-positive errors and greatly improves the accuracy of our MRO inference for classes that inherit from `Generic` and/or `Protocol`... unfortunately it seems to be introducing many new false positives regarding `__new__` in the mypy_primer report, which isn't something I expected. Possibly these were latent bugs that are now exposed due to the fact that we understand a lot of MROs a lot more precisely...?

## Test Plan

Mdtests updated and added.


---

_Label `red-knot` added by @AlexWaygood on 2025-04-17 18:34_

---

_Comment by @github-actions[bot] on 2025-04-17 18:37_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
dacite (https://github.com/konradhalas/dacite)
- error[lint:invalid-base] /tmp/mypy_primer/projects/dacite/tests/core/test_forward_reference.py:26:12: Invalid class base with type `object` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[lint:invalid-base] /tmp/mypy_primer/projects/dacite/tests/core/test_generics.py:17:9: Invalid class base with type `object` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[lint:unknown-argument] /tmp/mypy_primer/projects/dacite/tests/core/test_generics.py:54:24: Argument `x` does not match any known parameter
- error[lint:unknown-argument] /tmp/mypy_primer/projects/dacite/tests/core/test_generics.py:54:38: Argument `y` does not match any known parameter
- error[lint:invalid-base] /tmp/mypy_primer/projects/dacite/tests/core/test_generics.py:79:13: Invalid class base with type `object` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[lint:non-subscriptable] /tmp/mypy_primer/projects/dacite/tests/test_types.py:235:29: Cannot subscript object of type `Literal[Collection]` with no `__class_getitem__` method
- error[lint:invalid-base] /tmp/mypy_primer/projects/dacite/tests/test_types.py:275:13: Invalid class base with type `object` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- Found 156 diagnostics
+ Found 149 diagnostics

itsdangerous (https://github.com/pallets/itsdangerous)
- error[lint:invalid-base] /tmp/mypy_primer/projects/itsdangerous/src/itsdangerous/serializer.py:42:18: Invalid class base with type `object` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[lint:invalid-assignment] /tmp/mypy_primer/projects/itsdangerous/tests/test_itsdangerous/test_serializer.py:110:9: Object of type `Literal[BadUnsign]` is not assignable to attribute `signer` of type `type[Signer]`
- warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/itsdangerous/tests/test_itsdangerous/test_serializer.py:102:46: Unused blanket `type: ignore` directive
- warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/itsdangerous/tests/test_itsdangerous/test_serializer.py:132:42: Unused blanket `type: ignore` directive
- warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/itsdangerous/tests/test_itsdangerous/test_serializer.py:168:46: Unused blanket `type: ignore` directive
- Found 37 diagnostics
+ Found 34 diagnostics

bidict (https://github.com/jab/bidict)
- error[lint:non-subscriptable] /tmp/mypy_primer/projects/bidict/bidict/_typing.py:23:9: Cannot subscript object of type `Literal[Iterable]` with no `__class_getitem__` method
- error[lint:non-subscriptable] /tmp/mypy_primer/projects/bidict/bidict/_typing.py:34:22: Cannot subscript object of type `Literal[Maplike]` with no `__class_getitem__` method
- error[lint:non-subscriptable] /tmp/mypy_primer/projects/bidict/bidict/_typing.py:34:39: Cannot subscript object of type `Literal[Iterable]` with no `__class_getitem__` method
- error[lint:non-subscriptable] /tmp/mypy_primer/projects/bidict/bidict/_typing.py:35:13: Cannot subscript object of type `Literal[Iterator]` with no `__class_getitem__` method
- error[lint:inconsistent-mro] /tmp/mypy_primer/projects/bidict/bidict/_abc.py:72:1: Cannot create a consistent method resolution order (MRO) for class `MutableBidirectionalMapping` with bases list `[Unknown, Unknown]`
- error[lint:inconsistent-mro] /tmp/mypy_primer/projects/bidict/bidict/_bidict.py:35:1: Cannot create a consistent method resolution order (MRO) for class `MutableBidict` with bases list `[Unknown, Unknown]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/bidict/bidict/__init__.py:96:10: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/bidict/__init__.py:96:10: Argument to this function is incorrect: Expected `str`, found `Literal[suppress]`
- error[lint:invalid-base] /tmp/mypy_primer/projects/bidict/bidict/_orderedbase.py:38:16: Invalid class base with type `object` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[lint:inconsistent-mro] /tmp/mypy_primer/projects/bidict/bidict/_orderedbidict.py:33:1: Cannot create a consistent method resolution order (MRO) for class `OrderedBidict` with bases list `[Unknown, Unknown]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/bidict/bidict/_orderedbidict.py:100:16: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/bidict/_orderedbidict.py:100:16: Argument to this function is incorrect: Expected `str`, found `Literal[_OrderedBidictKeysView]`
- error[lint:non-subscriptable] /tmp/mypy_primer/projects/bidict/bidict/_orderedbidict.py:112:30: Cannot subscript object of type `Literal[BidictKeysView]` with no `__class_getitem__` method
- error[lint:non-subscriptable] /tmp/mypy_primer/projects/bidict/bidict/_orderedbidict.py:131:23: Cannot subscript object of type `Literal[_OrderedBidictKeysView]` with no `__class_getitem__` method
- error[lint:non-subscriptable] /tmp/mypy_primer/projects/bidict/bidict/_base.py:59:22: Cannot subscript object of type `Literal[KeysView]` with no `__class_getitem__` method
- error[lint:non-subscriptable] /tmp/mypy_primer/projects/bidict/bidict/_base.py:59:36: Cannot subscript object of type `Literal[ValuesView]` with no `__class_getitem__` method
- error[lint:missing-argument] /tmp/mypy_primer/projects/bidict/bidict/_base.py:253:53: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/bidict/_base.py:253:53: Argument to this function is incorrect: Expected `str`, found `Literal[BidictKeysView]`
- error[lint:non-subscriptable] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:76:9: Cannot subscript object of type `Literal[Sequence]` with no `__class_getitem__` method
- error[lint:invalid-base] /tmp/mypy_primer/projects/bidict/tests/bidict_test_fixtures.py:40:30: Invalid class base with type `object` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[lint:invalid-base] /tmp/mypy_primer/projects/bidict/tests/bidict_test_fixtures.py:125:14: Invalid class base with type `object` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- Found 88 diagnostics
+ Found 67 diagnostics

porcupine (https://github.com/Akuli/porcupine)
- error[lint:missing-argument] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/porcupine_debug_prompt.py:62:14: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/porcupine_debug_prompt.py:62:14: Argument to this function is incorrect: Expected `str`, found `Literal[redirect_stdout]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/porcupine_debug_prompt.py:62:47: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/porcupine_debug_prompt.py:62:47: Argument to this function is incorrect: Expected `str`, found `Literal[redirect_stderr]`
- Found 414 diagnostics
+ Found 410 diagnostics

packaging (https://github.com/pypa/packaging)
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/src/packaging/requirements.py:43:40: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/src/packaging/requirements.py:43:40: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/src/packaging/specifiers.py:804:21: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/src/packaging/specifiers.py:804:21: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/src/packaging/specifiers.py:808:21: No arguments provided for required parameters `bases`, `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/src/packaging/specifiers.py:808:21: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/src/packaging/specifiers.py:844:21: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/src/packaging/specifiers.py:844:21: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:40:9: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:40:9: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:88:13: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:88:13: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:205:13: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:205:13: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:223:16: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:223:16: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:230:21: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:230:21: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:230:51: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:230:51: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:246:19: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:246:19: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:246:36: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:246:36: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:247:25: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:247:25: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:248:19: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:248:19: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:252:16: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:252:16: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:252:35: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:252:35: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:253:24: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:253:24: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:254:16: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:254:16: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:270:23: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:270:23: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:270:40: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:270:40: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:271:29: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:271:29: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:272:23: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:272:23: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:275:16: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:275:16: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:276:20: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:276:20: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:277:16: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:277:16: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:278:20: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:278:20: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:478:16: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:478:16: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:508:16: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:508:16: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:537:16: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:537:16: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:550:16: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:550:16: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:575:16: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:575:16: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:598:16: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:598:16: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:617:16: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:617:16: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:624:16: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:624:16: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:638:16: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:638:16: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:643:16: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:643:16: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:643:41: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:643:41: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:646:21: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:646:21: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:646:52: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:646:52: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:652:16: No arguments provided for required parameters `bases`, `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:652:16: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:661:18: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:661:18: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:662:16: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:662:16: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:666:16: No arguments provided for required parameters `bases`, `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:666:16: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:675:16: No arguments provided for required parameters `bases`, `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:675:16: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:684:16: No arguments provided for required parameters `bases`, `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:684:16: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:694:16: No arguments provided for required parameters `bases`, `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:694:16: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:699:16: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:699:16: No arguments provided for required parameters `bases`, `namespace` of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:705:16: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:705:16: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:709:16: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:709:16: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:738:20: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:738:20: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:740:20: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:740:20: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:765:16: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:765:16: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:772:21: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:772:21: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:772:54: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:772:54: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:778:18: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:778:18: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:778:39: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:778:39: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:779:26: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:779:26: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:781:18: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:781:18: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:782:26: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:782:26: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:784:18: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:784:18: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:784:57: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:784:57: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:785:26: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:785:26: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:788:18: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:788:18: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:788:58: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:788:58: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:789:26: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:789:26: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:792:18: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:792:18: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:792:39: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:792:39: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:793:26: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:793:26: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:796:18: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:796:18: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:796:39: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:796:39: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:797:26: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:797:26: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:800:18: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:800:18: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:800:57: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:800:57: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:803:26: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:803:26: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:806:18: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:806:18: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:806:58: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:806:58: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:809:26: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:809:26: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:813:22: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:813:22: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:813:61: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:813:61: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:818:22: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:818:22: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:818:62: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:818:62: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:824:13: No arguments provided for required parameters `bases`, `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:824:13: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:840:19: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:840:19: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:840:39: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:840:39: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:841:19: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:841:19: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:841:39: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:841:39: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:842:19: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:842:19: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:842:36: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:842:36: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:843:25: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:843:25: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:844:19: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:844:19: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:860:23: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:860:23: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:860:43: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:860:43: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:861:23: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:861:23: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:861:43: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:861:43: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:862:23: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:862:23: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:862:40: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:862:40: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:863:29: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:863:29: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:864:23: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:864:23: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:868:16: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:868:16: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:868:38: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:868:38: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:869:24: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:869:24: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:870:16: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:870:16: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:873:16: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:873:16: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:874:20: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:874:20: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:888:37: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:888:37: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:891:23: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:891:23: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:891:50: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:891:50: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:non-subscriptable] /tmp/mypy_primer/projects/packaging/src/packaging/_parser.py:46:32: Cannot subscript object of type `Literal[Sequence]` with no `__class_getitem__` method
- error[lint:non-subscriptable] /tmp/mypy_primer/projects/packaging/src/packaging/_parser.py:47:14: Cannot subscript object of type `Literal[Sequence]` with no `__class_getitem__` method
- error[lint:non-subscriptable] /tmp/mypy_primer/projects/packaging/src/packaging/markers.py:28:38: Cannot subscript object of type `Literal[AbstractSet]` with no `__class_getitem__` method
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/src/packaging/markers.py:183:20: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/src/packaging/markers.py:183:20: Argument to this function is incorrect: Expected `str`, found `Literal[Specifier]`
- error[lint:non-subscriptable] /tmp/mypy_primer/projects/packaging/src/packaging/markers.py:217:55: Cannot subscript object of type `Literal[AbstractSet]` with no `__class_getitem__` method
- error[lint:non-subscriptable] /tmp/mypy_primer/projects/packaging/src/packaging/markers.py:353:26: Cannot subscript object of type `Literal[AbstractSet]` with no `__class_getitem__` method
- error[lint:non-subscriptable] /tmp/mypy_primer/projects/packaging/src/packaging/markers.py:354:22: Cannot subscript object of type `Literal[AbstractSet]` with no `__class_getitem__` method
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tasks/licenses.py:51:10: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tasks/licenses.py:51:10: Argument to this function is incorrect: Expected `str`, found `Literal[closing]`
- error[lint:non-subscriptable] /tmp/mypy_primer/projects/packaging/src/packaging/tags.py:27:17: Cannot subscript object of type `Literal[Sequence]` with no `__class_getitem__` method
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/tests/test_metadata.py:530:20: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/tests/test_metadata.py:530:20: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- error[lint:invalid-base] /tmp/mypy_primer/projects/packaging/src/packaging/metadata.py:472:18: Invalid class base with type `object` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[lint:missing-argument] /tmp/mypy_primer/projects/packaging/src/packaging/metadata.py:629:20: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/src/packaging/metadata.py:629:20: Argument to this function is incorrect: Expected `str`, found `Literal[SpecifierSet]`
- Found 333 diagnostics
+ Found 103 diagnostics

pyinstrument (https://github.com/joerick/pyinstrument)
- error[lint:missing-argument] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/renderers/base.py:97:22: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/renderers/base.py:97:22: Argument to this function is incorrect: Expected `str`, found `Literal[suppress]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/renderers/base.py:102:18: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/renderers/base.py:102:18: Argument to this function is incorrect: Expected `str`, found `Literal[suppress]`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/vendor/decorator.py:294:16: No arguments provided for required parameters `args`, `kwds` of function `__init__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/__main__.py:318:61: Argument to this function is incorrect: Expected `TextIO`, found `TextIOWrapper | TextIO | @Todo(Support for `typing.TypeAlias`)`
- Found 197 diagnostics
+ Found 193 diagnostics

paroxython (https://github.com/laowantong/paroxython)
+ error[lint:unresolved-attribute] /tmp/mypy_primer/projects/paroxython/examples/idioms/programs/052.0666-check-if-map-contains-value.py:13:6: Type `Literal[map]` has no attribute `values`
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/paroxython/examples/simple/programs/21_xml_html_parsing.py:19:19: Attribute `getiterator` on type `Unknown | Element` is possibly unbound
+ error[lint:non-subscriptable] /tmp/mypy_primer/projects/paroxython/examples/idioms/programs_with_labels.py:178:5: Cannot subscript object of type `reversed` with no `__getitem__` method
+ error[lint:unresolved-attribute] /tmp/mypy_primer/projects/paroxython/examples/idioms/programs_with_labels.py:183:1: Type `reversed` has no attribute `reverse`
+ error[lint:unresolved-attribute] /tmp/mypy_primer/projects/paroxython/examples/idioms/programs_with_labels.py:447:6: Type `Literal[map]` has no attribute `values`
- Found 459 diagnostics
+ Found 464 diagnostics

python-htmlgen (https://github.com/srittau/python-htmlgen)
- error[lint:missing-argument] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/link.py:10:16: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/link.py:10:16: Argument to this function is incorrect: Expected `str`, found `Literal[Link]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/link.py:14:16: Argument to this function is incorrect: Expected `str`, found `Literal[Link]`
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/link.py:14:39: Too many positional arguments to bound method `__new__`: expected 2, got 3
- error[lint:missing-argument] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/link.py:28:16: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/link.py:28:16: Argument to this function is incorrect: Expected `str`, found `Literal[Link]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/link.py:34:16: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/link.py:34:16: Argument to this function is incorrect: Expected `str`, found `Literal[Link]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/link.py:47:16: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/link.py:47:16: Argument to this function is incorrect: Expected `str`, found `Literal[Link]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/inline.py:18:16: No arguments provided for required parameters `bases`, `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/inline.py:18:16: Argument to this function is incorrect: Expected `str`, found `Literal[Span]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/inline.py:23:16: Argument to this function is incorrect: Expected `str`, found `Literal[Span]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/inline.py:33:21: No arguments provided for required parameters `bases`, `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/inline.py:33:21: Argument to this function is incorrect: Expected `str`, found `Literal[Highlight]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/inline.py:38:21: Argument to this function is incorrect: Expected `str`, found `Literal[Highlight]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/inline.py:48:18: No arguments provided for required parameters `bases`, `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/inline.py:48:18: Argument to this function is incorrect: Expected `str`, found `Literal[Strong]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/inline.py:53:18: Argument to this function is incorrect: Expected `str`, found `Literal[Strong]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/inline.py:63:15: No arguments provided for required parameters `bases`, `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/inline.py:63:15: Argument to this function is incorrect: Expected `str`, found `Literal[Alternate]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/inline.py:68:15: Argument to this function is incorrect: Expected `str`, found `Literal[Alternate]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/inline.py:77:14: No arguments provided for required parameters `bases`, `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/inline.py:77:14: Argument to this function is incorrect: Expected `str`, found `Literal[Emphasis]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/inline.py:82:14: Argument to this function is incorrect: Expected `str`, found `Literal[Emphasis]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/inline.py:92:17: No arguments provided for required parameters `bases`, `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/inline.py:92:17: Argument to this function is incorrect: Expected `str`, found `Literal[Small]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/inline.py:97:17: Argument to this function is incorrect: Expected `str`, found `Literal[Small]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/video.py:10:17: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/video.py:10:17: Argument to this function is incorrect: Expected `str`, found `Literal[Video]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/video.py:16:17: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/video.py:16:17: Argument to this function is incorrect: Expected `str`, found `Literal[Video]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/video.py:26:17: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/video.py:26:17: Argument to this function is incorrect: Expected `str`, found `Literal[Video]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/video.py:36:17: No argument provided for required parameter `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/video.py:36:17: Argument to this function is incorrect: Expected `str`, found `Literal[Video]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/python-htmlgen/htmlgen/document.py:89:21: No arguments provided for required parameters `bases`, `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/python-htmlgen/htmlgen/document.py:89:21: Argument to this function is incorrect: Expected `str`, found `Literal[Head]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/python-htmlgen/htmlgen/document.py:90:21: No arguments provided for required parameters `bases`, `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/python-htmlgen/htmlgen/document.py:90:21: Argument to this function is incorrect: Expected `str`, found `Literal[Body]`
- error[lint:non-subscriptable] /tmp/mypy_primer/projects/python-htmlgen/htmlgen/generator.pyi:56:21: Cannot subscript object of type `Literal[Generator]` with no `__class_getitem__` method
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/table.py:10:17: Argument to this function is incorrect: Expected `str`, found `Literal[Table]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/table.py:10:17: No arguments provided for required parameters `bases`, `namespace` of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/table.py:14:17: No arguments provided for required parameters `bases`, `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/table.py:14:17: Argument to this function is incorrect: Expected `str`, found `Literal[Table]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/table.py:40:17: No arguments provided for required parameters `bases`, `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/table.py:40:17: Argument to this function is incorrect: Expected `str`, found `Literal[Table]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/table.py:41:15: No arguments provided for required parameters `bases`, `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/table.py:41:15: Argument to this function is incorrect: Expected `str`, found `Literal[TableRow]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/table.py:56:17: No arguments provided for required parameters `bases`, `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/table.py:56:17: Argument to this function is incorrect: Expected `str`, found `Literal[Table]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/table.py:71:17: No arguments provided for required parameters `bases`, `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/table.py:71:17: Argument to this function is incorrect: Expected `str`, found `Literal[Table]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/table.py:77:17: No arguments provided for required parameters `bases`, `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/table.py:77:17: Argument to this function is incorrect: Expected `str`, found `Literal[Table]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/table.py:78:15: No arguments provided for required parameters `bases`, `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/table.py:78:15: Argument to this function is incorrect: Expected `str`, found `Literal[TableRow]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/table.py:93:17: Argument to this function is incorrect: Expected `str`, found `Literal[Table]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/table.py:93:17: No arguments provided for required parameters `bases`, `namespace` of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/table.py:108:17: No arguments provided for required parameters `bases`, `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/table.py:108:17: Argument to this function is incorrect: Expected `str`, found `Literal[Table]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/table.py:114:17: No arguments provided for required parameters `bases`, `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/table.py:114:17: Argument to this function is incorrect: Expected `str`, found `Literal[Table]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/table.py:136:17: No arguments provided for required parameters `bases`, `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/table.py:136:17: Argument to this function is incorrect: Expected `str`, found `Literal[Table]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/table.py:145:17: No arguments provided for required parameters `bases`, `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/table.py:145:17: Argument to this function is incorrect: Expected `str`, found `Literal[Table]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/table.py:153:17: No ar...*[Comment body truncated]*

---

_Renamed from "[red-knot] Fix MRO inference for protocol classes; allow inheritance from subscripted `Generic[]`" to "[red-knot] Fix MRO inference for protocol classes; allow inheritance from subscripted `Generic[]`; forbid subclassing unsubscripted `Generic`" by @AlexWaygood on 2025-04-17 21:05_

---

_Comment by @codspeed-hq[bot] on 2025-04-17 21:22_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex%2Fprotocol-mros)

### Merging #17452 will **improve performances by 12.46%**

<sub>Comparing <code>alex/protocol-mros</code> (fe9f655) with <code>main</code> (fd3fc34)</sub>



### Summary

`⚡ 1` improvements  
`✅ 32` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` red_knot_check_file[incremental] `` | 5.5 ms | 4.9 ms | +12.46% |


---

_Comment by @AlexWaygood on 2025-04-17 21:26_

> Merging #17452 will **improve performances by 11.64%**

hah... I think that might be entirely an accidental side effect of a temporary hack I added to reduce false positives when subscripting generic classes? But sure!

---

_Comment by @AlexWaygood on 2025-04-17 21:28_

I'm not sure we can land this with the number of `__new__`-related false positives it currently appears to add. But I'm marking as "ready for review" since I'm off until Tuesday, and I think the overall approach is ready for review. (Plus if somebody has any ideas of how to easily address the `__new__` false positives, I'd love to hear them!)

---

_Marked ready for review by @AlexWaygood on 2025-04-17 21:28_

---

_Review requested from @carljm by @AlexWaygood on 2025-04-17 21:28_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-04-17 21:28_

---

_Review requested from @dcreager by @AlexWaygood on 2025-04-17 21:28_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/subscript/tuple.md`:120 on 2025-04-17 23:01_

I guess this `Todo(Protocol subscript)` should turn into `Generic` in the MRO? At runtime this is 

```
[<class '__main__.C'>, <class 'tuple'>, <class 'typing.Generic'>, <class 'object'>]
```

Generic is there but Protocol is not. I'm not really sure why anything related to `Protocol` should show up here even now?

---

_@carljm reviewed on 2025-04-17 23:17_

This looks reasonable to me. I hope it won't conflict too much with @dcreager 's legacy-generics PR. I do think we need to at least understand the new ecosystem diagnostics in order to make a decision to land this.

---

_@AlexWaygood reviewed on 2025-04-18 12:35_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/subscript/tuple.md`:120 on 2025-04-18 12:35_

The MROs of builtin containers are all very different in typeshed to what they are at runtime. I'm not sure it makes sense for red-knot to distinguish between `tuple` and `Tuple`, and at runtime `tuple` obviously inherits directly from `object`. But in typeshed, `tuple` inherits from `typing.Sequence`, which inherits from `Reversible` and `Collection`. `Collection` and `Reversible` are both protocol classes, which is how `Protocol` ends up in the MRO here.

I can see if I can improve this to include `Generic` in the MRO as well without breaking the special-casing that means we don't emit false positives when users subscript classes that inherit from `Protocol[T]` or `Generic[T]`. (This special-casing is the reason for the `Todo` type in the MRO.)

---

_@AlexWaygood reviewed on 2025-04-18 13:13_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/subscript/tuple.md`:120 on 2025-04-18 13:13_

On the latest version of the PR, this is now

```
revealed: tuple[Literal[C], Literal[tuple], Literal[Sequence], Literal[Reversible], Literal[Collection], Literal[Iterable], Literal[Container], @Todo(`Protocol[]` subscript), @Todo(`Generic[]` subscript), Literal[object]]
```

---

_@carljm reviewed on 2025-04-18 13:13_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/subscript/tuple.md`:120 on 2025-04-18 13:13_

Oh I think the explanation that it's due to typeshed differences is sufficient, no need to try to get Generic in here unless it matters for some other concrete reason. 

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/subscript/tuple.md`:120 on 2025-04-18 13:18_

no worries -- it actually simplified some special-casing elsewhere, so I think it was a worthwhile change!

---

_@AlexWaygood reviewed on 2025-04-18 13:18_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/class_base.rs`:199 on 2025-04-18 13:30_

That's annoying that the different array lengths produce different types and therefore different `Either` wrappers. There's probably a way to simplify this down to a single type/wrapper using slices instead of arrays...but it's also probably not worth the effort! (So no recommended change here, just me blathering)

---

_Comment by @AlexWaygood on 2025-04-18 13:35_

Here's a minimal repro for the `__new__` issue that reproduces on `main`:

```py
import abc

class Foo(metaclass=abc.ABCMeta): ...  

# error: No arguments provided for required parameters `bases`, `namespace` of bound method `__new__` (lint:missing-argument) [Ln 5, Col 1]
# error: Argument to this function is incorrect: Expected `str`, found `Literal[Foo]` (lint:invalid-argument-type) [Ln 5, Col 1]
Foo()  
```

We apparently think that the `__new__` method we should use for inference of `Foo` calls is `abc.ABCMeta.__new__` (which is overridden from `type.__new__`). I assume that this would reproduce with any custom metaclass that overrides `type.__new__`.

Something I don't yet understand though is why we're seeing errors like this on `bidict` _now_ (relating to us thinking that the `__new__` method we should use for `typing.ItemsView` is `abc.ABCMeta.__new__`):

> ```diff
> + error[lint:missing-argument] /tmp/mypy_primer/projects/bidict/bidict/_frozen.py:44:26: No argument provided for required parameter `namespace` of bound method `__new__`
> + error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bidict/bidict/_frozen.py:44:26: Argument to this function is incorrect: Expected `str`, found `Literal[ItemsView]`
> ```

The `bidict` call in question is here: https://github.com/jab/bidict/blob/03ece106f8eb0f77baf983abc88b7ce4735edd4e/bidict/_frozen.py#L44

It appears that we _already_ understand that the metaclass of `ItemsView` is `abc.ABCMeta`, although we have a woefully bad understanding of its MRO right now: https://playknot.ruff.rs/d10b6638-484c-4a3e-9bba-14aa972a8d9f

cc. @sharkdp -- this may be the cause of many of the `__new__`-related false positives in the ecosystem that you mentioned in red-knot planning yesterday

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/infer.rs`:6363 on 2025-04-18 13:35_

nit: Does it work to change this to the following? (If so, I'm surprised clippy didn't warn about that, I'm pretty sure it's suggested that kind of change to me before)

```suggestion
                        class
                            .iter_mro(self.db(), None)
                            .contains(&ClassBase::Dynamic(DynamicType::SubscriptedGeneric))
```

(I was also going to suggest `class.is_subclass_of(...)` at first, but that takes in a `ClassType`, not a `ClassBase`.  Maybe change that method to take in `impl Into<ClassBase>`? That should still support all of the existing call sites, and you let you use it here too.)

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/subclass_of.rs`:123 on 2025-04-18 13:36_

Add a doc comment here, explaining that it's similar to `ClassBase` but only includes the variants that are valid in a subclass-of type

---

_@dcreager reviewed on 2025-04-18 13:38_

> > This looks reasonable to me. I hope it won't conflict too much with @dcreager 's legacy-generics PR. I do think we need to at least understand the new ecosystem diagnostics in order to make a decision to land this.

No worries about this — I think it will actually help that PR quite a bit, since I hadn't done anything yet related to `Generic`!

---

_@AlexWaygood reviewed on 2025-04-18 13:47_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:6363 on 2025-04-18 13:47_

I think `.contains()` requires the third-party `Itertools` trait (which we already have as a dependency, but may not be imported in this module yet). I'm happy to use it here since we already have it as a dependency, just clarifying why Clippy might not be suggesting it 😄

---

_@dcreager reviewed on 2025-04-18 14:06_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/infer.rs`:6363 on 2025-04-18 14:06_

Ah no it's because it was on a `Vec`, not an `Iterator`!

---

_Comment by @carljm on 2025-04-18 15:18_

> Something I don't yet understand though is why we're seeing errors like this on `bidict` _now_

Maybe this is because our current understanding of its MRO has `Unknown` in it? So we find an unknown `__new__` member on `Unknown`, before we get to checking the metaclass. But with this PR, we no longer find that unknown `__new__`. And I think our `__new__` lookup logic uses an "ignore `object`" policy currently, but then we wrongly fall back to a metaclass `__new__`. This is something we should prioritize fixing.

I had wondered on the `__new__` / `__init__` PR whether the "ignore `object`" policy was going to bite us in some way, because it's not really a good reflection of what happens at runtime -- and it appears this is how it bites us.

---

_Comment by @carljm on 2025-04-18 15:33_

Created https://github.com/astral-sh/ruff/issues/17462 for the `__new__` issue. Maybe this is something @sharkdp could look at next week.

---

_@AlexWaygood reviewed on 2025-04-18 17:56_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/class_base.rs`:199 on 2025-04-18 17:56_

I can't see a way of doing it with slices while keeping the borrow checker happy. But I think having to use nested `Either` types is reason enough to bite the bullet and write a custom `Iterator` type 😄 it's not _that_ much boilerplate, and it makes it easier to read at the callsite.

---

_@AlexWaygood reviewed on 2025-04-18 18:21_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/class.rs`:781 on 2025-04-18 18:21_

I added this as a short-term hack to avoid the `__new__` false positives for classes that have `ABCMeta` as their metaclasses. Judging from the mypy_primer report, it looks extremely effective!

---

_Review requested from @dcreager by @AlexWaygood on 2025-04-18 18:24_

---

_Review requested from @carljm by @AlexWaygood on 2025-04-18 18:25_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/class.rs`:781 on 2025-04-18 18:25_

You're robbing @sharkdp of the satisfaction of removing all these ecosystem false positives!

I guess it's also satisfying to remove hacks in the codebase...

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/class_base.rs`:226 on 2025-04-18 18:43_

nit: I think `FromClass` would be a clearer name here (and `fn from_class` below)

---

_@carljm approved on 2025-04-18 18:46_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/class_base.rs`:199 on 2025-04-18 18:57_

I actually prefer the nested `Either` that you had before, tbh. I don't think it will have any performance implications. And I think it _is_ too much boilerplate for what it's achieving, especially since you're still returning `impl Iterator`.

That said, if others disagree I'm happy to go along with the consensus.

---

_@dcreager reviewed on 2025-04-18 19:02_

---

_@carljm reviewed on 2025-04-18 19:22_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/class_base.rs`:199 on 2025-04-18 19:22_

FWIW I don't have strong feelings either way. Both versions seem kind of overwrought for what it's actually doing, but that's just life with the borrow checker sometimes :) The dedicated iterator is definitely more boilerplate overall, but also makes _this_ method (which has the meaningful type logic in it) less painful to read (so it encapsulates the pain a little better). I approved because I'm fine with either.

---

_@AlexWaygood reviewed on 2025-04-18 19:49_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/class_base.rs`:199 on 2025-04-18 19:49_

Yeah, I agree that both versions seem annoyingly verbose/overwrought! I weakly prefer the version I have now, though, basically for the reasons Carl mentions. I also don't have strong feelings any which way, though.

---

_Merged by @AlexWaygood on 2025-04-18 19:55_

---

_Closed by @AlexWaygood on 2025-04-18 19:55_

---

_Branch deleted on 2025-04-18 19:55_

---

_@sharkdp reviewed on 2025-04-30 13:11_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/class.rs`:781 on 2025-04-30 13:11_

==> https://github.com/astral-sh/ruff/pull/17733

---
