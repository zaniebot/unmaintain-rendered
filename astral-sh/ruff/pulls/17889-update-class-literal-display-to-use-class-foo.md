```yaml
number: 17889
title: "Update class literal display to use `<class 'Foo'>` style"
type: pull_request
state: merged
author: charliermarsh
labels:
  - ty
assignees: []
merged: true
base: main
head: charlie/class-display
created_at: 2025-05-06T13:47:50Z
updated_at: 2025-05-07T00:11:27Z
url: https://github.com/astral-sh/ruff/pull/17889
synced_at: 2026-01-10T18:57:03Z
```

# Update class literal display to use `<class 'Foo'>` style

---

_Pull request opened by @charliermarsh on 2025-05-06 13:47_

## Summary

Closes https://github.com/astral-sh/ruff/issues/17238.


---

_Review requested from @carljm by @charliermarsh on 2025-05-06 13:47_

---

_Review requested from @AlexWaygood by @charliermarsh on 2025-05-06 13:47_

---

_Review requested from @sharkdp by @charliermarsh on 2025-05-06 13:47_

---

_Review requested from @dcreager by @charliermarsh on 2025-05-06 13:47_

---

_Converted to draft by @charliermarsh on 2025-05-06 13:47_

---

_Comment by @charliermarsh on 2025-05-06 13:48_

Want to make sure I'm on the right track before I update the remaining tests.

---

_Comment by @github-actions[bot] on 2025-05-06 13:51_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
dacite (https://github.com/konradhalas/dacite)
- error[lint:unsupported-operator] dacite/generics.py:64:72: Operator `|` is unsupported between objects of type `Literal[int]` and `Literal[str]`
+ error[lint:unsupported-operator] dacite/generics.py:64:72: Operator `|` is unsupported between objects of type `<class 'int'>` and `<class 'str'>`

packaging (https://github.com/pypa/packaging)
- error[lint:unsupported-operator] src/packaging/_manylinux.py:22:40: Operator `|` is unsupported between objects of type `Literal[ELFFile]` and `None`
+ error[lint:unsupported-operator] src/packaging/_manylinux.py:22:40: Operator `|` is unsupported between objects of type `<class 'ELFFile'>` and `None`

anyio (https://github.com/agronholm/anyio)
- error[lint:unsupported-operator] src/anyio/_core/_contextmanagers.py:38:42: Operator `|` is unsupported between objects of type `Literal[bool]` and `None`
+ error[lint:unsupported-operator] src/anyio/_core/_contextmanagers.py:38:42: Operator `|` is unsupported between objects of type `<class 'bool'>` and `None`
- error[lint:unsupported-operator] src/anyio/_core/_contextmanagers.py:41:48: Operator `|` is unsupported between objects of type `Literal[bool]` and `None`
+ error[lint:unsupported-operator] src/anyio/_core/_contextmanagers.py:41:48: Operator `|` is unsupported between objects of type `<class 'bool'>` and `None`
- error[lint:unsupported-operator] src/anyio/_core/_contextmanagers.py:95:68: Operator `|` is unsupported between objects of type `Literal[bool]` and `None`
+ error[lint:unsupported-operator] src/anyio/_core/_contextmanagers.py:95:68: Operator `|` is unsupported between objects of type `<class 'bool'>` and `None`
- error[lint:unsupported-operator] src/anyio/_core/_contextmanagers.py:124:47: Operator `|` is unsupported between objects of type `Literal[bool]` and `None`
+ error[lint:unsupported-operator] src/anyio/_core/_contextmanagers.py:124:47: Operator `|` is unsupported between objects of type `<class 'bool'>` and `None`
- error[lint:unsupported-operator] src/anyio/_core/_contextmanagers.py:127:60: Operator `|` is unsupported between objects of type `Literal[bool]` and `None`
+ error[lint:unsupported-operator] src/anyio/_core/_contextmanagers.py:127:60: Operator `|` is unsupported between objects of type `<class 'bool'>` and `None`
- error[lint:unsupported-operator] src/anyio/_core/_contextmanagers.py:189:46: Operator `|` is unsupported between objects of type `Literal[bool]` and `None`
+ error[lint:unsupported-operator] src/anyio/_core/_contextmanagers.py:189:46: Operator `|` is unsupported between objects of type `<class 'bool'>` and `None`
- error[lint:invalid-super-argument] src/anyio/_core/_tempfile.py:349:22: `SpooledTemporaryFile[bytes]` is not an instance or subclass of `Literal[SpooledTemporaryFile]` in `super(Literal[SpooledTemporaryFile], SpooledTemporaryFile[bytes])` call
+ error[lint:invalid-super-argument] src/anyio/_core/_tempfile.py:349:22: `SpooledTemporaryFile[bytes]` is not an instance or subclass of `<class 'SpooledTemporaryFile'>` in `super(<class 'SpooledTemporaryFile'>, SpooledTemporaryFile[bytes])` call
- error[lint:invalid-super-argument] src/anyio/_core/_tempfile.py:370:22: `SpooledTemporaryFile[bytes]` is not an instance or subclass of `Literal[SpooledTemporaryFile]` in `super(Literal[SpooledTemporaryFile], SpooledTemporaryFile[bytes])` call
+ error[lint:invalid-super-argument] src/anyio/_core/_tempfile.py:370:22: `SpooledTemporaryFile[bytes]` is not an instance or subclass of `<class 'SpooledTemporaryFile'>` in `super(<class 'SpooledTemporaryFile'>, SpooledTemporaryFile[bytes])` call
- error[lint:invalid-super-argument] src/anyio/_core/_tempfile.py:377:22: `SpooledTemporaryFile[bytes]` is not an instance or subclass of `Literal[SpooledTemporaryFile]` in `super(Literal[SpooledTemporaryFile], SpooledTemporaryFile[bytes])` call
+ error[lint:invalid-super-argument] src/anyio/_core/_tempfile.py:377:22: `SpooledTemporaryFile[bytes]` is not an instance or subclass of `<class 'SpooledTemporaryFile'>` in `super(<class 'SpooledTemporaryFile'>, SpooledTemporaryFile[bytes])` call

aioredis (https://github.com/aio-libs/aioredis)
- error[lint:invalid-parameter-default] aioredis/connection.py:1541:9: Default value of type `Literal[LifoQueue]` is not assignable to annotated parameter type `type[Queue]`
+ error[lint:invalid-parameter-default] aioredis/connection.py:1541:9: Default value of type `<class 'LifoQueue'>` is not assignable to annotated parameter type `type[Queue]`

more-itertools (https://github.com/more-itertools/more-itertools)
- error[lint:unsupported-operator] more_itertools/more.pyi:156:13: Operator `|` is unsupported between objects of type `Literal[BaseException]` and `GenericAlias`
+ error[lint:unsupported-operator] more_itertools/more.pyi:156:13: Operator `|` is unsupported between objects of type `<class 'BaseException'>` and `GenericAlias`
- error[lint:unsupported-operator] more_itertools/more.pyi:162:18: Operator `|` is unsupported between objects of type `Literal[type]` and `GenericAlias`
+ error[lint:unsupported-operator] more_itertools/more.pyi:162:18: Operator `|` is unsupported between objects of type `<class 'type'>` and `GenericAlias`

attrs (https://github.com/python-attrs/attrs)
- error[lint:invalid-argument-type] tests/test_annotations.py:49:35: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[C]`
+ error[lint:invalid-argument-type] tests/test_annotations.py:49:35: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[lint:invalid-argument-type] tests/test_annotations.py:50:35: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[C]`
+ error[lint:invalid-argument-type] tests/test_annotations.py:50:35: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[lint:invalid-argument-type] tests/test_annotations.py:51:36: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[C]`
+ error[lint:invalid-argument-type] tests/test_annotations.py:51:36: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[lint:invalid-argument-type] tests/test_annotations.py:79:48: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[C]`
+ error[lint:invalid-argument-type] tests/test_annotations.py:79:48: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[lint:invalid-argument-type] tests/test_annotations.py:80:52: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[C]`
+ error[lint:invalid-argument-type] tests/test_annotations.py:80:52: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[lint:invalid-argument-type] tests/test_annotations.py:94:37: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[C]`
+ error[lint:invalid-argument-type] tests/test_annotations.py:94:37: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[lint:unresolved-attribute] tests/test_annotations.py:119:39: Type `Literal[C]` has no attribute `__attrs_attrs__`
+ error[lint:unresolved-attribute] tests/test_annotations.py:119:39: Type `<class 'C'>` has no attribute `__attrs_attrs__`
- error[lint:invalid-argument-type] tests/test_annotations.py:123:28: Argument to this function is incorrect: Argument type `Literal[C]` does not satisfy upper bound of type variable `_A`
+ error[lint:invalid-argument-type] tests/test_annotations.py:123:28: Argument to this function is incorrect: Argument type `<class 'C'>` does not satisfy upper bound of type variable `_A`
- error[lint:invalid-argument-type] tests/test_annotations.py:125:35: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[C]`
+ error[lint:invalid-argument-type] tests/test_annotations.py:125:35: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[lint:invalid-argument-type] tests/test_annotations.py:127:50: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[C]`
+ error[lint:invalid-argument-type] tests/test_annotations.py:127:50: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[lint:invalid-argument-type] tests/test_annotations.py:128:48: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[C]`
+ error[lint:invalid-argument-type] tests/test_annotations.py:128:48: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[lint:invalid-argument-type] tests/test_annotations.py:130:35: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[C]`
+ error[lint:invalid-argument-type] tests/test_annotations.py:130:35: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[lint:invalid-argument-type] tests/test_annotations.py:131:33: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[C]`
+ error[lint:invalid-argument-type] tests/test_annotations.py:131:33: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[lint:invalid-argument-type] tests/test_annotations.py:133:35: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[C]`
+ error[lint:invalid-argument-type] tests/test_annotations.py:133:35: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[lint:invalid-argument-type] tests/test_annotations.py:135:42: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[C]`
+ error[lint:invalid-argument-type] tests/test_annotations.py:135:42: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[lint:invalid-argument-type] tests/test_annotations.py:416:28: Argument to this function is incorrect: Argument type `Literal[C]` does not satisfy upper bound of type variable `_A`
+ error[lint:invalid-argument-type] tests/test_annotations.py:416:28: Argument to this function is incorrect: Argument type `<class 'C'>` does not satisfy upper bound of type variable `_A`
- error[lint:invalid-argument-type] tests/test_annotations.py:518:28: Argument to this function is incorrect: Argument type `Literal[C]` does not satisfy upper bound of type variable `_A`
+ error[lint:invalid-argument-type] tests/test_annotations.py:518:28: Argument to this function is incorrect: Argument type `<class 'C'>` does not satisfy upper bound of type variable `_A`
- error[lint:invalid-argument-type] tests/test_annotations.py:520:35: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[C]`
+ error[lint:invalid-argument-type] tests/test_annotations.py:520:35: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[lint:invalid-argument-type] tests/test_annotations.py:521:35: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[C]`
+ error[lint:invalid-argument-type] tests/test_annotations.py:521:35: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[lint:invalid-argument-type] tests/test_annotations.py:522:36: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[C]`
+ error[lint:invalid-argument-type] tests/test_annotations.py:522:36: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[lint:invalid-argument-type] tests/test_annotations.py:540:28: Argument to this function is incorrect: Argument type `Literal[C]` does not satisfy upper bound of type variable `_A`
+ error[lint:invalid-argument-type] tests/test_annotations.py:540:28: Argument to this function is incorrect: Argument type `<class 'C'>` does not satisfy upper bound of type variable `_A`
- error[lint:invalid-argument-type] tests/test_annotations.py:542:56: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[C]`
+ error[lint:invalid-argument-type] tests/test_annotations.py:542:56: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[lint:invalid-argument-type] tests/test_annotations.py:548:28: Argument to this function is incorrect: Argument type `Literal[D]` does not satisfy upper bound of type variable `_A`
+ error[lint:invalid-argument-type] tests/test_annotations.py:548:28: Argument to this function is incorrect: Argument type `<class 'D'>` does not satisfy upper bound of type variable `_A`
- error[lint:invalid-argument-type] tests/test_annotations.py:550:37: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[D]`
+ error[lint:invalid-argument-type] tests/test_annotations.py:550:37: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'D'>`
- error[lint:invalid-argument-type] tests/test_annotations.py:565:28: Argument to this function is incorrect: Argument type `Literal[A]` does not satisfy upper bound of type variable `_A`
+ error[lint:invalid-argument-type] tests/test_annotations.py:565:28: Argument to this function is incorrect: Argument type `<class 'A'>` does not satisfy upper bound of type variable `_A`
- error[lint:invalid-argument-type] tests/test_annotations.py:567:48: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[A]`
+ error[lint:invalid-argument-type] tests/test_annotations.py:567:48: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
- error[lint:invalid-argument-type] tests/test_annotations.py:568:48: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[A]`
+ error[lint:invalid-argument-type] tests/test_annotations.py:568:48: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
- error[lint:invalid-argument-type] tests/test_annotations.py:569:48: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[A]`
+ error[lint:invalid-argument-type] tests/test_annotations.py:569:48: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
- error[lint:invalid-argument-type] tests/test_annotations.py:583:48: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[A]`
+ error[lint:invalid-argument-type] tests/test_annotations.py:583:48: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
- error[lint:invalid-argument-type] tests/test_annotations.py:584:48: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[A]`
+ error[lint:invalid-argument-type] tests/test_annotations.py:584:48: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
- error[lint:invalid-argument-type] tests/test_annotations.py:585:48: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[A]`
+ error[lint:invalid-argument-type] tests/test_annotations.py:585:48: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
- error[lint:invalid-argument-type] tests/test_annotations.py:599:28: Argument to this function is incorrect: Argument type `Literal[A]` does not satisfy upper bound of type variable `_A`
+ error[lint:invalid-argument-type] tests/test_annotations.py:599:28: Argument to this function is incorrect: Argument type `<class 'A'>` does not satisfy upper bound of type variable `_A`
- error[lint:invalid-argument-type] tests/test_annotations.py:601:33: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[A]`
+ error[lint:invalid-argument-type] tests/test_annotations.py:601:33: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
- error[lint:invalid-argument-type] tests/test_annotations.py:602:50: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[A]`
+ error[lint:invalid-argument-type] tests/test_annotations.py:602:50: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
- error[lint:invalid-argument-type] tests/test_annotations.py:619:28: Argument to this function is incorrect: Argument type `Literal[A]` does not satisfy upper bound of type variable `_A`
+ error[lint:invalid-argument-type] tests/test_annotations.py:619:28: Argument to this function is incorrect: Argument type `<class 'A'>` does not satisfy upper bound of type variable `_A`
- error[lint:invalid-argument-type] tests/test_annotations.py:620:28: Argument to this function is incorrect: Argument type `Literal[B]` does not satisfy upper bound of type variable `_A`
+ error[lint:invalid-argument-type] tests/test_annotations.py:620:28: Argument to this function is incorrect: Argument type `<class 'B'>` does not satisfy upper bound of type variable `_A`
- error[lint:invalid-argument-type] tests/test_annotations.py:622:46: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[A]`
+ error[lint:invalid-argument-type] tests/test_annotations.py:622:46: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
- error[lint:invalid-argument-type] tests/test_annotations.py:623:33: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[B]`
+ error[lint:invalid-argument-type] tests/test_annotations.py:623:33: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'B'>`
- error[lint:invalid-argument-type] tests/test_annotations.py:625:46: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[A]`
+ error[lint:invalid-argument-type] tests/test_annotations.py:625:46: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
- error[lint:invalid-argument-type] tests/test_annotations.py:626:33: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[B]`
+ error[lint:invalid-argument-type] tests/test_annotations.py:626:33: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'B'>`
- error[lint:invalid-argument-type] tests/test_annotations.py:667:28: Argument to this function is incorrect: Argument type `Literal[A]` does not satisfy upper bound of type variable `_A`
+ error[lint:invalid-argument-type] tests/test_annotations.py:667:28: Argument to this function is incorrect: Argument type `<class 'A'>` does not satisfy upper bound of type variable `_A`
- error[lint:invalid-argument-type] tests/test_annotations.py:668:28: Argument to this function is incorrect: Argument type `Literal[B]` does not satisfy upper bound of type variable `_A`
+ error[lint:invalid-argument-type] tests/test_annotations.py:668:28: Argument to this function is incorrect: Argument type `<class 'B'>` does not satisfy upper bound of type variable `_A`
- error[lint:invalid-argument-type] tests/test_annotations.py:670:35: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[A]`
+ error[lint:invalid-argument-type] tests/test_annotations.py:670:35: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
- error[lint:invalid-argument-type] tests/test_annotations.py:671:35: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[B]`
+ error[lint:invalid-argument-type] tests/test_annotations.py:671:35: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'B'>`
- error[lint:invalid-argument-type] tests/test_annotations.py:683:28: Argument to this function is incorrect: Argument type `Literal[A]` does not satisfy upper bound of type variable `_A`
+ error[lint:invalid-argument-type] tests/test_annotations.py:683:28: Argument to this function is incorrect: Argument type `<class 'A'>` does not satisfy upper bound of type variable `_A`
- error[lint:invalid-argument-type] tests/test_annotations.py:685:35: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[A]`
+ error[lint:invalid-argument-type] tests/test_annotations.py:685:35: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
- error[lint:invalid-argument-type] tests/test_annotations.py:687:28: Argument to this function is incorrect: Argument type `Literal[A]` does not satisfy upper bound of type variable `_A`
+ error[lint:invalid-argument-type] tests/test_annotations.py:687:28: Argument to this function is incorrect: Argument type `<class 'A'>` does not satisfy upper bound of type variable `_A`
- error[lint:invalid-argument-type] tests/test_annotations.py:689:35: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[A]`
+ error[lint:invalid-argument-type] tests/test_annotations.py:689:35: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
- error[lint:invalid-argument-type] tests/test_converters.py:297:29: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[C]`
+ error[lint:invalid-argument-type] tests/test_converters.py:297:29: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[lint:invalid-argument-type] tests/test_filters.py:33:31: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[C]`
+ error[lint:invalid-argument-type] tests/test_filters.py:33:31: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[lint:invalid-argument-type] tests/test_filters.py:34:46: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[C]`
+ error[lint:invalid-argument-type] tests/test_filters.py:34:46: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[lint:invalid-argument-type] tests/test_filters.py:47:27: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[C]`
+ error[lint:invalid-argument-type] tests/test_filters.py:47:27: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[lint:invalid-argument-type] tests/test_filters.py:48:27: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[C]`
+ error[lint:invalid-argument-type] tests/test_filters.py:48:27: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[lint:invalid-argument-type] tests/test_filters.py:52:27: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[C]`
+ error[lint:invalid-argument-type] tests/test_filters.py:52:27: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[lint:invalid-argument-type] tests/test_filters.py:60:25: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[C]`
+ error[lint:invalid-argument-type] tests/test_filters.py:60:25: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[lint:invalid-argument-type] tests/test_filters.py:67:27: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[C]`
+ error[lint:invalid-argument-type] tests/test_filters.py:67:27: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[lint:invalid-argument-type] tests/test_filters.py:68:27: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[C]`
+ error[lint:invalid-argument-type] tests/test_filters.py:68:27: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[lint:invalid-argument-type] tests/test_filters.py:72:27: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[C]`
+ error[lint:invalid-argument-type] tests/test_filters.py:72:27: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[lint:invalid-argument-type] tests/test_filters.py:80:25: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[C]`
+ error[lint:invalid-argument-type] tests/test_filters.py:80:25: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[lint:invalid-argument-type] tests/test_filters.py:93:27: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[C]`
+ error[lint:invalid-argument-type] tests/test_filters.py:93:27: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[lint:invalid-argument-type] tests/test_filters.py:94:27: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[C]`
+ error[lint:invalid-argument-type] tests/test_filters.py:94:27: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[lint:invalid-argument-type] tests/test_filters.py:98:27: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[C]`
+ error[lint:invalid-argument-type] tests/test_filters.py:98:27: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[lint:invalid-argument-type] tests/test_filters.py:106:25: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[C]`
+ error[lint:invalid-argument-type] tests/test_filters.py:106:25: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[lint:invalid-argument-type] tests/test_filters.py:113:27: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[C]`
+ error[lint:invalid-argument-type] tests/test_filters.py:113:27: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[lint:invalid-argument-type] tests/test_filters.py:114:27: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[C]`
+ error[lint:invalid-argument-type] tests/test_filters.py:114:27: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[lint:invalid-argument-type] tests/test_filters.py:118:27: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[C]`
+ error[lint:invalid-argument-type] tests/test_filters.py:118:27: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[lint:invalid-argument-type] tests/test_filters.py:126:25: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[C]`
+ error[lint:invalid-argument-type] tests/test_filters.py:126:25: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[lint:invalid-argument-type] tests/test_functional.py:167:25: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[C1]`
+ error[lint:invalid-argument-type] tests/test_functional.py:167:25: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C1'>`
- error[lint:invalid-argument-type] tests/test_functional.py:545:42: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[C]`
+ error[lint:invalid-argument-type] tests/test_functional.py:545:42: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[lint:invalid-argument-type] tests/test_functional.py:546:44: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[C]`
+ error[lint:invalid-argument-type] tests/test_functional.py:546:44: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[lint:invalid-argument-type] tests/test_functional.py:547:35: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[C]`
+ error[lint:invalid-argument-type] tests/test_functional.py:547:35: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[lint:invalid-argument-type] tests/test_functional.py:646:35: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[C]`
+ error[lint:invalid-argument-type] tests/test_functional.py:646:35: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[lint:invalid-argument-type] tests/test_functional.py:647:35: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[C]`
+ error[lint:invalid-argument-type] tests/test_functional.py:647:35: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[lint:invalid-argument-type] tests/test_hooks.py:216:40: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[C]`
+ error[lint:invalid-argument-type] tests/test_hooks.py:216:40: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[lint:invalid-argument-type] tests/test_hooks.py:234:54: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[Base]`
+ error[lint:invalid-argument-type] tests/test_hooks.py:234:54: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'Base'>`
- error[lint:unresolved-attribute] tests/test_make.py:256:20: Type `Literal[B]` has no attribute `__attrs_attrs__`
+ error[lint:unresolved-attribute] tests/test_make.py:256:20: Type `<class 'B'>` has no attribute `__attrs_attrs__`
- error[lint:unresolved-attribute] tests/test_make.py:273:20: Type `Literal[B]` has no attribute `__attrs_attrs__`
+ error[lint:unresolved-attribute] tests/test_make.py:273:20: Type `<class 'B'>` has no attribute `__attrs_attrs__`
- error[lint:invalid-argument-type] tests/test_make.py:462:27: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[A]`
+ error[lint:invalid-argument-type] tests/test_make.py:462:27: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
- error[lint:invalid-argument-type] tests/test_make.py:464:26: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[B]`
+ error[lint:invalid-argument-type] tests/test_make.py:464:26: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'B'>`
- error[lint:invalid-argument-type] tests/test_make.py:465:27: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[B]`
+ error[lint:invalid-argument-type] tests/test_make.py:465:27: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'B'>`
- error[lint:invalid-argument-type] tests/test_make.py:467:27: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[C]`
+ error[lint:invalid-argument-type] tests/test_make.py:467:27: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[lint:invalid-argument-type] tests/test_make.py:468:26: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[C]`
+ error[lint:invalid-argument-type] tests/test_make.py:468:26: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[lint:invalid-argument-type] tests/test_make.py:469:27: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[C]`
+ error[lint:invalid-argument-type] tests/test_make.py:469:27: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[lint:unresolved-attribute] tests/test_make.py:486:23: Type `Literal[C]` has no attribute `__attrs_attrs__`
+ error[lint:unresolved-attribute] tests/test_make.py:486:23: Type `<class 'C'>` has no attribute `__attrs_attrs__`
- error[lint:unresolved-attribute] tests/test_make.py:487:54: Type `Literal[C]` has no attribute `__attrs_attrs__`
+ error[lint:unresolved-attribute] tests/test_make.py:487:54: Type `<class 'C'>` has no attribute `__attrs_attrs__`
- error[lint:invalid-argument-type] tests/test_make.py:793:45: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[C]`
+ error[lint:invalid-argument-type] tests/test_make.py:793:45: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[lint:invalid-argument-type] tests/test_make.py:830:26: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[BaseClass]`
+ error[lint:invalid-argument-type] tests/test_make.py:830:26: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'BaseClass'>`
- error[lint:invalid-argument-type] tests/test_make.py:831:26: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[SubClass]`
+ error[lint:invalid-argument-type] tests/test_make.py:831:26: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'SubClass'>`
- error[lint:unresolved-attribute] tests/test_make.py:1079:38: Type `Literal[C2]` has no attribute `__attrs_attrs__`
+ error[lint:unresolved-attribute] tests/test_make.py:1079:38: Type `<class 'C2'>` has no attribute `__attrs_attrs__`
- error[lint:unresolved-attribute] tests/test_make.py:1094:38: Type `Literal[C2]` has no attribute `__attrs_attrs__`
+ error[lint:unresolved-attribute] tests/test_make.py:1094:38: Type `<class 'C2'>` has no attribute `__attrs_attrs__`
- error[lint:invalid-argument-type] tests/test_make.py:2013:34: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[Cases]`
+ error[lint:invalid-argument-type] tests/test_make.py:2013:34: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'Cases'>`
- error[lint:unresolved-attribute] tests/test_make.py:2650:26: Type `Literal[C]` has no attribute `__match_args__`
+ error[lint:unresolved-attribute] tests/test_make.py:2650:26: Type `<class 'C'>` has no attribute `__match_args__`
- error[lint:unresolved-attribute] tests/test_make.py:2692:16: Type `Literal[C]` has no attribute `__match_args__`
+ error[lint:unresolved-attribute] tests/test_make.py:2692:16: Type `<class 'C'>` has no attribute `__match_args__`
- error[lint:unresolved-attribute] tests/test_make.py:2699:16: Type `Literal[C]` has no attribute `__match_args__`
+ error[lint:unresolved-attribute] tests/test_make.py:2699:16: Type `<class 'C'>` has no attribute `__match_args__`
- error[lint:unresolved-attribute] tests/test_make.py:2734:16: Type `Literal[B]` has no attribute `__match_args__`
+ error[lint:unresolved-attribute] tests/test_make.py:2734:16: Type `<class 'B'>` has no attribute `__match_args__`
- error[lint:invalid-argument-type] tests/test_next_gen.py:149:39: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[NewSchool]`
+ error[lint:invalid-argument-type] tests/test_next_gen.py:149:39: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'NewSchool'>`
- error[lint:invalid-argument-type] tests/test_next_gen.py:164:39: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[NewSchool]`
+ error[lint:invalid-argument-type] tests/test_next_gen.py:164:39: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'NewSchool'>`
- error[lint:unresolved-attribute] tests/test_pattern_matching.py:24:26: Type `Literal[C]` has no attribute `__match_args__`
+ error[lint:unresolved-attribute] tests/test_pattern_matching.py:24:26: Type `<class 'C'>` has no attribute `__match_args__`
- error[lint:unresolved-attribute] tests/test_pattern_matching.py:66:26: Type `Literal[C]` has no attribute `__match_args__`
+ error[lint:unresolved-attribute] tests/test_pattern_matching.py:66:26: Type `<class 'C'>` has no attribute `__match_args__`
- error[lint:unresolved-attribute] tests/test_setattr.py:245:16: Type `Literal[Hooked]` has no attribute `__attrs_own_setattr__`
+ error[lint:unresolved-attribute] tests/test_setattr.py:245:16: Type `<class 'Hooked'>` has no attribute `__attrs_own_setattr__`
- error[lint:unresolved-attribute] tests/test_setattr.py:246:20: Type `Literal[NoHook]` has no attribute `__attrs_own_setattr__`
+ error[lint:unresolved-attribute] tests/test_setattr.py:246:20: Type `<class 'NoHook'>` has no attribute `__attrs_own_setattr__`
- error[lint:unresolved-attribute] tests/test_setattr.py:247:16: Type `Literal[WithOnSetAttrHook]` has no attribute `__attrs_own_setattr__`
+ error[lint:unresolved-attribute] tests/test_setattr.py:247:16: Type `<class 'WithOnSetAttrHook'>` has no attribute `__attrs_own_setattr__`
- error[lint:unresolved-attribute] tests/test_setattr.py:348:20: Type `Literal[RemoveNeedForOurSetAttr]` has no attribute `__attrs_own_setattr__`
+ error[lint:unresolved-attribute] tests/test_setattr.py:348:20: Type `<class 'RemoveNeedForOurSetAttr'>` has no attribute `__attrs_own_setattr__`
- error[lint:invalid-argument-type] tests/test_slots.py:103:24: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[C1Slots]`
+ error[lint:invalid-argument-type] tests/test_slots.py:103:24: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C1Slots'>`
- error[lint:invalid-argument-type] tests/test_slots.py:103:48: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[C1]`
+ error[lint:invalid-argument-type] tests/test_slots.py:103:48: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C1'>`
- error[lint:unresolved-attribute] tests/test_slots.py:155:25: Type `Literal[C2Slots]` has no attribute `__slots__`
+ error[lint:unresolved-attribute] tests/test_slots.py:155:25: Type `<class 'C2Slots'>` has no attribute `__slots__`
- error[lint:unresolved-attribute] tests/test_slots.py:246:25: Type `Literal[C2Slots]` has no attribute `__slots__`
+ error[lint:unresolved-attribute] tests/test_slots.py:246:25: Type `<class 'C2Slots'>` has no attribute `__slots__`
- error[lint:unresolved-attribute] tests/test_slots.py:297:25: Type `Literal[C2Slots]` has no attribute `__slots__`
+ error[lint:unresolved-attribute] tests/test_slots.py:297:25: Type `<class 'C2Slots'>` has no attribute `__slots__`
- error[lint:unresolved-attribute] tests/test_slots.py:325:23: Type `Literal[HasXSlot]` has no attribute `x`
+ error[lint:unresolved-attribute] tests/test_slots.py:325:23: Type `<class 'HasXSlot'>` has no attribute `x`
- error[lint:invalid-argument-type] tests/test_slots.py:545:41: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[C]`
+ error[lint:invalid-argument-type] tests/test_slots.py:545:41: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[lint:unresolved-attribute] tests/test_slots.py:754:16: Type `Literal[A]` has no attribute `__slots__`
+ error[lint:unresolved-attribute] tests/test_slots.py:754:16: Type `<class 'A'>` has no attribute `__slots__`
- error[lint:unresolved-attribute] tests/test_slots.py:1021:12: Type `Literal[B]` has no attribute `__slots__`
+ error[lint:unresolved-attribute] tests/test_slots.py:1021:12: Type `<class 'B'>` has no attribute `__slots__`
- error[lint:unresolved-attribute] tests/test_slots.py:1042:12: Type `Literal[B]` has no attribute `__slots__`
+ error[lint:unresolved-attribute] tests/test_slots.py:1042:12: Type `<class 'B'>` has no attribute `__slots__`
- error[lint:unresolved-attribute] tests/test_validators.py:300:16: Type `Literal[C]` has no attribute `__attrs_attrs__`
+ error[lint:unresolved-attribute] tests/test_validators.py:300:16: Type `<class 'C'>` has no attribute `__attrs_attrs__`
- error[lint:unresolved-attribute] tests/test_validators.py:300:50: Type `Literal[C]` has no attribute `__attrs_attrs__`
+ error[lint:unresolved-attribute] tests/test_validators.py:300:50: Type `<class 'C'>` has no attribute `__attrs_attrs__`
- error[lint:invalid-argument-type] tests/test_validators.py:818:23: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[Tester]`
+ error[lint:invalid-argument-type] tests/test_validators.py:818:23: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'Tester'>`
- error[lint:invalid-argument-type] tests/test_validators.py:890:23: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[Tester]`
+ error[lint:invalid-argument-type] tests/test_validators.py:890:23: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'Tester'>`
- error[lint:invalid-argument-type] tests/test_validators.py:963:23: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[Tester]`
+ error[lint:invalid-argument-type] tests/test_validators.py:963:23: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'Tester'>`
- error[lint:invalid-argument-type] tests/typing_example.py:133:13: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[AliasExample]`
+ error[lint:invalid-argument-type] tests/typing_example.py:133:13: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'AliasExample'>`
- error[lint:invalid-argument-type] tests/typing_example.py:134:13: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[AliasExample]`
+ error[lint:invalid-argument-type] tests/typing_example.py:134:13: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'AliasExample'>`
- error[lint:unsupported-operator] tests/typing_example.py:232:48: Operator `|` is unsupported between objects of type `Literal[int]` and `Literal[C]`
+ error[lint:unsupported-operator] tests/typing_example.py:232:48: Operator `|` is unsupported between objects of type `<class 'int'>` and `<class 'C'>`
- error[lint:invalid-argument-type] tests/typing_example.py:396:13: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[NGFrozen]`
+ error[lint:invalid-argument-type] tests/typing_example.py:396:13: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'NGFrozen'>`
- error[lint:invalid-argument-type] tests/typing_example.py:397:17: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[NGFrozen]`
+ error[lint:invalid-argument-type] tests/typing_example.py:397:17: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'NGFrozen'>`
- error[lint:invalid-argument-type] tests/typing_example.py:401:14: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[NGFrozen]`
+ error[lint:invalid-argument-type] tests/typing_example.py:401:14: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'NGFrozen'>`
- error[lint:invalid-argument-type] tests/typing_example.py:402:18: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `Literal[NGFrozen]`
+ error[lint:invalid-argument-type] tests/typing_example.py:402:18: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'NGFrozen'>`
- error[lint:unresolved-attribute] tests/typing_example.py:480:5: Type `Literal[object]` has no attribute `__attrs_attrs__`
+ error[lint:unresolved-attribute] tests/typing_example.py:480:5: Type `<class 'object'>` has no attribute `__attrs_attrs__`

kornia (https://github.com/kornia/kornia)
- error[lint:invalid-argument-type] kornia/feature/dedode/transformer/dinov2.py:336:26: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `Literal[NestedTensorBlock]`
+ error[lint:invalid-argument-type] kornia/feature/dedode/transformer/dinov2.py:336:26: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `<class 'NestedTensorBlock'>`
- error[lint:invalid-argument-type] kornia/feature/dedode/transformer/dinov2.py:350:26: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `Literal[NestedTensorBlock]`
+ error[lint:invalid-argument-type] kornia/feature/dedode/transformer/dinov2.py:350:26: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `<class 'NestedTensorBlock'>`
- error[lint:invalid-argument-type] kornia/feature/dedode/transformer/dinov2.py:364:26: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `Literal[NestedTensorBlock]`
+ error[lint:invalid-argument-type] kornia/feature/dedode/transformer/dinov2.py:364:26: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `<class 'NestedTensorBlock'>`
- error[lint:invalid-argument-type] kornia/feature/dedode/transformer/dinov2.py:378:26: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `Literal[NestedTensorBlock]`
+ error[lint:invalid-argument-type] kornia/feature/dedode/transformer/dinov2.py:378:26: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `<class 'NestedTensorBlock'>`
- error[lint:invalid-parameter-default] kornia/feature/dedode/transformer/layers/block.py:68:9: Default value of type `Literal[Attention]` is not assignable to annotated parameter type `(...) -> Unknown`
+ error[lint:invalid-parameter-default] kornia/feature/dedode/transformer/layers/block.py:68:9: Default value of type `<class 'Attention'>` is not assignable to annotated parameter type `(...) -> Unknown`
- error[lint:invalid-parameter-default] kornia/feature/dedode/transformer/layers/block.py:69:9: Default value of type `Literal[Mlp]` is not assignable to annotated parameter type `(...) -> Unknown`
+ error[lint:invalid-parameter-default] kornia/feature/dedode/transformer/layers/block.py:69:9: Default value of type `<class 'Mlp'>` is not assignable to annotated parameter type `(...) -> Unknown`
- error[lint:invalid-base] kornia/feature/dedode/transformer/layers/swiglu_ffn.py:62:22: Invalid class base with type `Unknown | Literal[SwiGLUFFN]` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[lint:invalid-base] kornia/feature/dedode/transformer/layers/swiglu_ffn.py:62:22: Invalid class base with type `Unknown | <class 'SwiGLUFFN'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)

pyjwt (https://github.com/jpadilla/pyjwt)
- error[lint:unsupported-operator] jwt/algorithms.py:104:37: Operator `|` is unsupported between objects of type `Literal[RSAPrivateKey]` and `Literal[RSAPublicKey]`
+ error[lint:unsupported-operator] jwt/algorithms.py:104:37: Operator `|` is unsupported between objects of type `<class 'RSAPrivateKey'>` and `<class 'RSAPublicKey'>`
- error[lint:unsupported-operator] jwt/algorithms.py:105:36: Operator `|` is unsupported between objects of type `Literal[EllipticCurvePrivateKey]` and `Literal[EllipticCurvePublicKey]`
+ error[lint:unsupported-operator] jwt/algorithms.py:105:36: Operator `|` is unsupported between objects of type `<class 'EllipticCurvePrivateKey'>` and `<class 'EllipticCurvePublicKey'>`
- error[lint:unsupported-operator] jwt/algorithms.py:107:13: Operator `|` is unsupported between objects of type `Literal[Ed25519PrivateKey]` and `Literal[Ed25519PublicKey]`
+ error[lint:unsupported-operator] jwt/algorithms.py:107:13: Operator `|` is unsupported between objects of type `<class 'Ed25519PrivateKey'>` and `<class 'Ed25519PublicKey'>`
- error[lint:unsupported-operator] jwt/algorithms.py:112:13: Operator `|` is unsupported between objects of type `Literal[RSAPrivateKey]` and `Literal[EllipticCurvePrivateKey]`
+ error[lint:unsupported-operator] jwt/algorithms.py:112:13: Operator `|` is unsupported between objects of type `<class 'RSAPrivateKey'>` and `<class 'EllipticCurvePrivateKey'>`
- error[lint:unsupported-operator] jwt/algorithms.py:119:13: Operator `|` is unsupported between objects of type `Literal[RSAPublicKey]` and `Literal[EllipticCurvePublicKey]`
+ error[lint:unsupported-operator] jwt/algorithms.py:119:13: Operator `|` is unsupported between objects of type `<class 'RSAPublicKey'>` and `<class 'EllipticCurvePublicKey'>`

alerta (https://github.com/alerta/alerta)
- error[lint:unresolved-attribute] alerta/models/alarms/alerta.py:90:9: Unresolved attribute `DEFAULT_INFORM_SEVERITY` on type `Literal[StateMachine]`.
+ error[lint:unresolved-attribute] alerta/models/alarms/alerta.py:90:9: Unresolved attribute `DEFAULT_INFORM_SEVERITY` on type `<class 'StateMachine'>`.

starlette (https://github.com/encode/starlette)
- error[lint:invalid-argument-type] tests/conftest.py:20:9: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `Literal[TestClient]`
+ error[lint:invalid-argument-type] tests/conftest.py:20:9: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `<class 'TestClient'>`

mypy-protobuf (https://github.com/dropbox/mypy-protobuf)
- error[lint:unsupported-operator] mypy_protobuf/main.py:166:53: Operator `|` is unsupported between objects of type `Literal[str]` and `None`
+ error[lint:unsupported-operator] mypy_protobuf/main.py:166:53: Operator `|` is unsupported between objects of type `<class 'str'>` and `None`

graphql-core (https://github.com/graphql-python/graphql-core)
- error[lint:unresolved-attribute] src/graphql/language/source.py:44:31: Type `Literal[SourceLocation]` has no attribute `_make`
+ error[lint:unresolved-attribute] src/graphql/language/source.py:44:31: Type `<class 'SourceLocation'>` has no attribute `_make`

strawberry (https://github.com/strawberry-graphql/strawberry)
- error[lint:invalid-assignment] strawberry/codegen/query_codegen.py:648:13: Object of type `Literal[GraphQLOptional] | ((t) -> Unknown)` is not assignable to `((Unknown, /) -> Unknown) | None`
+ error[lint:invalid-assignment] strawberry/codegen/query_codegen.py:648:13: Object of type `<class 'GraphQLOptional'> | ((t) -> Unknown)` is not assignable to `((Unknown, /) -> Unknown) | None`
- error[lint:invalid-assignment] strawberry/codegen/query_codegen.py:656:13: Object of type `Literal[GraphQLList] | ((t) -> Unknown)` is not assignable to `((Unknown, /) -> Unknown) | None`
+ error[lint:invalid-assignment] strawberry/codegen/query_codegen.py:656:13: Object of type `<class 'GraphQLList'> | ((t) -> Unknown)` is not assignable to `((Unknown, /) -> Unknown) | None`
- error[lint:invalid-parameter-default] strawberry/codegen/query_codegen.py:760:9: Default value of type `Literal[GraphQLObjectType]` is not assignable to annotated parameter type `(str, /) -> GraphQLObjectType`
+ error[lint:invalid-parameter-default] strawberry/codegen/query_codegen.py:760:9: Default value of type `<class 'GraphQLObjectType'>` is not assignable to annotated parameter type `(str, /) -> GraphQLObjectType`
- warning[lint:possibly-unbound-attribute] strawberry/experimental/pydantic/conversion.py:63:21: Attribute `_strawberry_type` on type `type[@Todo(Intersection meta-type)] | Literal[NoneType]` is possibly unbound
+ warning[lint:possibly-unbound-attribute] strawberry/experimental/pydantic/conversion.py:63:21: Attribute `_strawberry_type` on type `type[@Todo(Intersection meta-type)] | <class 'NoneType'>` is possibly unbound
- error[lint:unresolved-attribute] strawberry/ext/mypy_plugin.py:340:26: Attribute `VERSION` can only be accessed on instances, not on the class object `Literal[MypyVersion]` itself.
+ error[lint:unresolved-attribute] strawberry/ext/mypy_plugin.py:340:26: Attribute `VERSION` can only be accessed on instances, not on the class object `<class 'MypyVersion'>` itself.
- error[lint:invalid-attribute-access] strawberry/ext/mypy_plugin.py:619:9: Cannot assign to instance attribute `VERSION` from the class object `Literal[MypyVersion]`
+ error[lint:invalid-attribute-access] strawberry/ext/mypy_plugin.py:619:9: Cannot assign to instance attribute `VERSION` from the class object `<class 'MypyVersion'>`
- error[lint:invalid-attribute-access] strawberry/ext/mypy_plugin.py:621:9: Cannot assign to instance attribute `VERSION` from the class object `Literal[MypyVersion]`
+ error[lint:invalid-attribute-access] strawberry/ext/mypy_plugin.py:621:9: Cannot assign to instance attribute `VERSION` from the class object `<class 'MypyVersion'>`

pybind11 (https://github.com/pybind/pybind11)
- error[lint:invalid-base] pybind11/setup_helpers.py:89:25: Invalid class base with type `Unknown | Literal[Extension]` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[lint:invalid-base] pybind11/setup_helpers.py:89:25: Invalid class base with type `Unknown | <class 'Extension'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[lint:invalid-base] pybind11/setup_helpers.py:271:17: Invalid class base with type `Literal[build_ext, build_ext]` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[lint:invalid-base] pybind11/setup_helpers.py:271:17: Invalid class base with type `<class 'build_ext'> | <class 'build_ext'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)

kopf (https://github.com/nolar/kopf)
- error[lint:invalid-base] kopf/_core/actions/loggers.py:109:20: Invalid class base with type `Unknown | Literal[LoggerAdapter[Any]]` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[lint:invalid-base] kopf/_core/actions/loggers.py:109:20: Invalid class base with type `Unknown | <class 'LoggerAdapter[Any]'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[lint:invalid-base] kopf/_core/engines/indexing.py:237:23: Invalid class base with type `Unknown | Literal[Mapping[str, Index[Any, Any]]]` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[lint:invalid-base] kopf/_core/engines/indexing.py:237:23: Invalid class base with type `Unknown | <class 'Mapping[str, Index[Any, Any]]'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)

trio (https://github.com/python-trio/trio)
- error[lint:invalid-return-type] src/trio/_core/_exceptions.py:74:16: Return type does not match returned value: Expected `tuple[() -> Cancelled, tuple[()]]`, found `tuple[bound method Literal[Cancelled]._create(*args: object, **kwargs: object) -> T, tuple[()]]`
+ error[lint:invalid-return-type] src/trio/_core/_exceptions.py:74:16: Return type does not match returned value: Expected `tuple[() -> Cancelled, tuple[()]]`, found `tuple[bound method <class 'Cancelled'>._create(*args: object, **kwargs: object) -> T, tuple[()]]`
- error[lint:invalid-attribute-access] src/trio/_tests/test_threads.py:483:9: Cannot assign to instance attribute `ran` from the class object `Literal[state]`
+ error[lint:invalid-attribute-access] src/trio/_tests/test_threads.py:483:9: Cannot assign to instance attribute `ran` from the class object `<class 'state'>`
- error[lint:invalid-attribute-access] src/trio/_tests/test_threads.py:484:9: Cannot assign to instance attribute `high_water` from the class object `Literal[state]`
+ error[lint:invalid-attribute-access] src/trio/_tests/test_threads.py:484:9: Cannot assign to instance attribute `high_water` from the class object `<class 'state'>`
- error[lint:invalid-attribute-access] src/trio/_tests/test_threads.py:485:9: Cannot assign to instance attribute `running` from the class object `Literal[state]`
+ error[lint:invalid-attribute-access] src/trio/_tests/test_threads.py:485:9: Cannot assign to instance attribute `running` from the class object `<class 'state'>`
- error[lint:invalid-attribute-access] src/trio/_tests/test_threads.py:486:9: Cannot assign to instance attribute `parked` from the class object `Literal[state]`
+ error[lint:invalid-attribute-access] src/trio/_tests/test_threads.py:486:9: Cannot assign to instance attribute `parked` from the class object `<class 'state'>`
- error[lint:unresolved-attribute] src/trio/_tests/test_threads.py:494:17: Attribute `ran` can only be accessed on instances, not on the class object `Literal[state]` itself.
+ error[lint:unresolved-attribute] src/trio/_tests/test_threads.py:494:17: Attribute `ran` can only be accessed on instances, not on the class object `<class 'state'>` itself.
- error[lint:unresolved-attribute] src/trio/_tests/test_threads.py:495:17: Attribute `running` can only be accessed on instances, not on the class object `Literal[state]` itself.
+ error[lint:unresolved-attribute] src/trio/_tests/test_threads.py:495:17: Attribute `running` can only be accessed on instances, not on the class object `<class 'state'>` itself.
- error[lint:invalid-attribute-access] src/trio/_tests/test_threads.py:496:17: Cannot assign to instance attribute `high_water` from the class object `Literal[state]`
+ error[lint:invalid-attribute-access] src/trio/_tests/test_threads.py:496:17: Cannot assign to instance attribute `high_water` from the class object `<class 'state'>`
- error[lint:unresolved-attribute] src/trio/_tests/test_threads.py:496:40: Attribute `high_water` can only be accessed on instances, not on the class object `Literal[state]` itself.
+ error[lint:unresolved-attribute] src/trio/_tests/test_threads.py:496:40: Attribute `high_water` can only be accessed on instances, not on the class object `<class 'state'>` itself.
- error[lint:unresolved-attribute] src/trio/_tests/test_threads.py:496:58: Attribute `running` can only be accessed on instances, not on the class object `Literal[state]` itself.
+ error[lint:unresolved-attribute] src/trio/_tests/test_threads.py:496:58: Attribute `running` can only be accessed on instances, not on the class object `<class 'state'>` itself.
- error[lint:unresolved-attribute] src/trio/_tests/test_threads.py:499:17: Attribute `parked` can only be accessed on instances, not on the class object `Literal[state]` itself.
+ error[lint:unresolved-attribute] src/trio/_tests/test_threads.py:499:17: Attribute `parked` can only be accessed on instances, not on the class object `<class 'state'>` itself.
- error[lint:unresolved-attribute] src/trio/_tests/test_threads.py:502:17: Attribute `parked` can only be accessed on instances, not on the class object `Literal[state]` itself.
+ error[lint:unresolved-attribute] src/trio/_tests/test_threads.py:502:17: Attribute `parked` can only be accessed on instances, not on the class object `<class 'state'>` itself.
- error[lint:unresolved-attribute] src/trio/_tests/test_threads.py:503:17: Attribute `running` can only be accessed on instances, not on the class object `Literal[state]` itself.
+ error[lint:unresolved-attribute] src/trio/_tests/test_threads.py:503:17: Attribute `running` can only be accessed on instances, not on the class object `<class 'state'>` itself.
- error[lint:unresolved-attribute] src/trio/_tests/test_threads.py:539:17: Attribute `parked` can only be accessed on instances, not on the class object `Literal[state]` itself.
+ error[lint:unresolved-attribute] src/trio/_tests/test_threads.py:539:17: Attribute `parked` can only be accessed on instances, not on the class object `<class 'state'>` itself.
- error[lint:unresolved-attribute] src/trio/_tests/test_threads.py:545:16: Attribute `high_water` can only be accessed on instances, not on the class object `Literal[state]` itself.
+ error[lint:unresolved-attribute] src/trio/_tests/test_threads.py:545:16: Attribute `high_water` can only be accessed on instances, not on the class object `<class 'state'>` itself.
- error[lint:unresolved-attribute] src/trio/_tests/test_threads.py:554:16: Attribute `ran` can only be accessed on instances, not on the class object `Literal[state]` itself.
+ error[lint:unresolved-attribute] src/trio/_tests/test_threads.py:554:16: Attribute `ran` can only be accessed on instances, not on the class object `<class 'state'>` itself.
- error[lint:unresolved-attribute] src/trio/_tests/test_threads.py:555:16: Attribute `running` can only be accessed on instances, not on the class object `Literal[state]` itself.
+ error[lint:unresolved-attribute] src/trio/_tests/test_threads.py:555:16: Attribute `running` can only be accessed on instances, not on the class object `<class 'state'>` itself.

pip (https://github.com/pypa/pip)
- error[lint:invalid-base] src/pip/_vendor/distlib/util.py:1675:20: Invalid class base with type `Literal[BaseConfigurator, BaseConfigurator]` (all bases must be a class, `Any`, `Unknown` or `Todo`)
+ error[lint:invalid-base] src/pip/_vendor/distlib/util.py:1675:20: Invalid class base with type `<class 'BaseConfigurator'> | <class 'BaseConfigurator'>` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[lint:invalid-assignment] src/pip/_vendor/distro/distro.py:56:5: Object of type `Literal[dict]` is not assignable to `typing.TypedDict`
+ error[lint:invalid-assignment] src/pip/_vendor/distro/distro.py:56:5: Object of type `<class 'dict'>` is not assignable to `typing.TypedDict`
- error[lint:unsupported-operator] src/pip/_vendor/packaging/_manylinux.py:22:40: Operator `|` is unsupported between objects of type `Literal[ELFFile]` and `None`
+ error[lint:unsupported-operator] src/pip/_vendor/packaging/_manylinux.py:22:40: Operator `|` is unsupported between objects of type `<class 'ELFFile'>` and `None`
- error[lint:unsupported-operator] src/pip/_vendor/truststore/_api.py:28:37: Operator `|` is unsupported between objects of type `Literal[str]` and `Literal[bytes]`
+ error[lint:unsupported-operator] src/pip/_vendor/truststore/_api.py:28:37: Operator `|` is unsupported between objects of type `<class 'str'>` and `<class 'bytes'>`
- error[lint:unsupported-operator] src/pip/_vendor/truststore/_api.py:29:35: Operator `|` is unsupported between objects of type `Literal[str]` and `Literal[bytes]`
+ error[lint:unsupported-operator] src/pip/_vendor/truststore/_api.py:29:35: Operator `|` is unsupported between objects of type `<class 'str'>` and `<class 'bytes'>`
- error[lint:unsupported-operator] src/pip/_vendor/truststore/_api.py:29:69: Operator `|` is unsupported between objects of type `Literal[str]` and `Literal[bytes]`
+ error[lint:unsupported-operator] src/pip/_vendor/truststore/_api.py:29:69: Operator `|` is unsupported between objects of type `<class 'str'>` and `<class 'bytes'>`
- error[lint:unresolved-attribute] src/pip/_vendor/typing_extensions.py:150:52: Type `Literal[ForwardRef]` has no attribute `__slots__`
+ error[lint:unresolved-attribute] src/pip/_vendor/typing_extensions.py:150:52: Type `<class 'ForwardRef'>` has no attribute `__slots__`
- error[lint:unresolved-attribute] src/pip/_vendor/typing_extensions.py:3875:28: Type `Literal[object]` has no attribute `__getattr__`
+ error[lint:unresolved-attribute] src/pip/_vendor/typing_extensions.py:3875:28: Type `<class 'object'>` has no attribute `__getattr__`
- error[lint:unresolved-attribute] src/pip/_vendor/typing_extensions.py:4399:42: Type `Literal[ForwardRef]` has no attribute `__slots__`
+ error[lint:unresolved-attribute] src/pip/_vendor/typing_extensions.py:4399:42: Type `<class 'ForwardRef'>` has no attribute `__slots__`
- error[lint:unresolved-attribute] src/pip/_vendor/urllib3/_collections.py:179:20: Type `Literal[MutableMapping]` has no attribute `iterkeys`
+ error[lint:unresolved-attribute] src/pip/_vendor/urllib3/_collections.py:179:20: Type `<class 'MutableMapping'>` has no attribute `iterkeys`
- error[lint:unresolved-attribute] src/pip/_vendor/urllib3/_collections.py:180:22: Type `Literal[MutableMapping]` has no attribute `itervalues`
+ error[lint:unresolved-attribute] src/pip/_vendor/urllib3/_collections.py:180:22: Type `<class 'MutableMapping'>` has no attribute `itervalues`
- error[lint:unresolved-attribute] src/pip/_vendor/urllib3/connectionpool.py:201:23: Type `Literal[Retry]` has no attribute `DEFAULT`
+ error[lint:unresolved-attribute] src/pip/_vendor/urllib3/connectionpool.py:201:23: Type `<class 'Retry'>` has no attribute `DEFAULT`
- error[lint:unresolved-attribute] src/pip/_vendor/urllib3/contrib/appengine.py:122:35: Type `Literal[Retry]` has no attribute `DEFAULT`
+ error[lint:unresolved-attribute] src/pip/_vendor/urllib3/contrib/appengine.py:122:35: Type `<class 'Retry'>` has no attribute `DEFAULT`
- error[lint:invalid-assignment] src/pip/_vendor/urllib3/contrib/pyopenssl.py:135:5: Object of type `Literal[PyOpenSSLContext]` is not assignable to attribute `SSLContext` of type `Literal[SSLContext, SSLContext]`
+ error[lint:invalid-assignment] src/pip/_vendor/urllib3/contrib/pyopenssl.py:135:5: Object of type `<class 'PyOpenSSLContext'>` is not assignable to attribute `SSLContext` of type `<class 'SSLContext'> | <class 'SSLContext'>`
- error[lint:unresolved-attribute] src/pip/_vendor/urllib3/contrib/pyopenssl.py:420:1: Unresolved attribute `makefile` on type `Literal[WrappedSocket]`.
+ error[lint:unresolved-attribute] src/pip/_vendor/urllib3/contrib/pyopenssl.py:420:1: Unresolved attribute `makefile` ...*[Comment body truncated]*

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/display.rs`:47 on 2025-05-06 14:01_

we probably want to make the same update for `Type::GenericAlias`

---

_@AlexWaygood reviewed on 2025-05-06 14:01_

This is looking great to me so far! Probably best to wait for at least one other person to give the thumbs up before proceeding further, though 😄

---

_Label `ty` added by @AlexWaygood on 2025-05-06 14:02_

---

_Comment by @carljm on 2025-05-06 17:34_

This is looking good to me as well!

---

_Marked ready for review by @charliermarsh on 2025-05-06 22:33_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/display.rs`:606 on 2025-05-06 23:54_

I don't think we are going to add more of these, so I think we could remove this enum entirely and simplify the literal display code to not need to consider multiple "kinds" at all.

But I'm not going to hold up this PR for that, gonna land this quick before there are any conflicts.

---

_@carljm approved on 2025-05-06 23:54_

Thank you!!

---

_Closed by @carljm on 2025-05-06 23:54_

---

_Reopened by @carljm on 2025-05-06 23:54_

---

_@charliermarsh reviewed on 2025-05-06 23:56_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/display.rs`:606 on 2025-05-06 23:56_

Cool I can do it as a follow-up.

---

_Comment by @carljm on 2025-05-06 23:58_

Hmm I noticed after my review that all the CI checks were stuck in some kind of pending state; after closing and reopening to nudge the CI, it seems a few tests are still failing?

---

_Comment by @charliermarsh on 2025-05-07 00:01_

Oh strange, I can fix.

---

_Comment by @charliermarsh on 2025-05-07 00:03_

`precommit` reformatted some lines, which caused expect messages to move around.

---

_Merged by @charliermarsh on 2025-05-07 00:11_

---

_Closed by @charliermarsh on 2025-05-07 00:11_

---

_Branch deleted on 2025-05-07 00:11_

---
