```yaml
number: 21916
title: "[ty] Uniformly use \"not supported\" in diagnostics"
type: pull_request
state: merged
author: lucach
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: diagnostics-uniform-not-supported
created_at: 2025-12-11T14:02:33Z
updated_at: 2025-12-11T15:03:56Z
url: https://github.com/astral-sh/ruff/pull/21916
synced_at: 2026-01-12T15:57:36Z
```

# [ty] Uniformly use "not supported" in diagnostics

---

_@lucach_

## Summary

Some (most?) diagnostic messages already use the form "Operator <op> not supported", but others use "Operator <op> unsupported":

https://play.ty.dev/ae13cd3d-531b-4ed3-adea-eba9b0cafb6c
```py
0 + ""
0 < ""
```
> Operator `+` is unsupported between objects of type `Literal[0]` and `Literal[""]` (unsupported-operator) [Ln 1, Col 1]
> Operator `<` is not supported between objects of type `Literal[0]` and `Literal[""]` (unsupported-operator) [Ln 2, Col 1]

This commit uniforms the diagnostic messages, favoring "not supported". While it is two characters longer, this:
- matches pyright/pyrefly's messages,
- is slightly less redundant given that "unsupported" already appears in the diagnostic's type (e.g., unsupported-operator), 
- makes the "not" stand out.

(This commit does not remove all occurrences of "unsupported" in diagnostic messages: there are still some that e.g. start with "Unsupported X". I think those are acceptable as they are.)


## Test Plan

Updated existing tests, and checked the new diagnostics messages with those produced by pyright/pyrefly.

---

_Review requested from @carljm by @lucach on 2025-12-11 14:02_

---

_Review requested from @AlexWaygood by @lucach on 2025-12-11 14:02_

---

_Review requested from @sharkdp by @lucach on 2025-12-11 14:02_

---

_Review requested from @dcreager by @lucach on 2025-12-11 14:02_

---

_Comment by @astral-sh-bot[bot] on 2025-12-11 14:05_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-12-11 14:04:47.563232536 +0000
+++ new-output.txt	2025-12-11 14:04:51.515246565 +0000
@@ -425,7 +425,7 @@
 generics_base_class.py:45:5: error[type-assertion-failure] Type `Iterator[int]` does not match asserted type `Unknown`
 generics_base_class.py:49:38: error[invalid-type-arguments] Too many type arguments to class `LinkedList`: expected 1, got 2
 generics_base_class.py:61:30: error[invalid-type-arguments] Too many type arguments to class `MyDict`: expected 1, got 2
-generics_basic.py:34:12: error[unsupported-operator] Operator `+` is unsupported between objects of type `AnyStr@concat` and `AnyStr@concat`
+generics_basic.py:34:12: error[unsupported-operator] Operator `+` is not supported between objects of type `AnyStr@concat` and `AnyStr@concat`
 generics_basic.py:49:44: error[invalid-legacy-type-variable] A `TypeVar` cannot have exactly one constraint
 generics_basic.py:139:5: error[type-assertion-failure] Type `int` does not match asserted type `Unknown`
 generics_basic.py:140:5: error[type-assertion-failure] Type `int` does not match asserted type `Unknown`

```

</details>




---

_Renamed from "Uniformly use "not supported" in diagnostics" to "[ty] Uniformly use "not supported" in diagnostics" by @AlexWaygood on 2025-12-11 14:06_

---

_Label `ty` added by @AlexWaygood on 2025-12-11 14:06_

---

_Comment by @astral-sh-bot[bot] on 2025-12-11 14:19_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
mypy_primer (https://github.com/hauntsaninja/mypy_primer)
- mypy_primer/utils.py:37:16: error[unsupported-operator] Operator `+` is unsupported between objects of type `Literal["\""]` and `Path`
+ mypy_primer/utils.py:37:16: error[unsupported-operator] Operator `+` is not supported between objects of type `Literal["\""]` and `Path`

attrs (https://github.com/python-attrs/attrs)
- typing-examples/mypy.py:261:48: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'int'>` and `<class 'C'>`
+ typing-examples/mypy.py:261:48: error[unsupported-operator] Operator `|` is not supported between objects of type `<class 'int'>` and `<class 'C'>`

aioredis (https://github.com/aio-libs/aioredis)
- aioredis/lock.py:299:23: error[unsupported-operator] Operator `*` is unsupported between objects of type `Unknown | int | float | None` and `Literal[1000]`
+ aioredis/lock.py:299:23: error[unsupported-operator] Operator `*` is not supported between objects of type `Unknown | int | float | None` and `Literal[1000]`

parso (https://github.com/davidhalter/parso)
- parso/python/pep8.py:120:13: error[unsupported-operator] Operator `+=` is unsupported between objects of type `None` and `Literal[" "]`
+ parso/python/pep8.py:120:13: error[unsupported-operator] Operator `+=` is not supported between objects of type `None` and `Literal[" "]`

spack (https://github.com/spack/spack)
- lib/spack/spack/cmd/blame.py:266:23: error[unsupported-operator] Operator `/` is unsupported between objects of type `None | Path` and `Literal[".git-blame-ignore-revs"]`
+ lib/spack/spack/cmd/blame.py:266:23: error[unsupported-operator] Operator `/` is not supported between objects of type `None | Path` and `Literal[".git-blame-ignore-revs"]`
- lib/spack/spack/graph.py:143:58: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | None` and `Literal["-1.5 "]`
+ lib/spack/spack/graph.py:143:58: error[unsupported-operator] Operator `+` is not supported between objects of type `Unknown | None` and `Literal["-1.5 "]`
- lib/spack/spack/graph.py:153:58: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | None` and `Literal["-1 "]`
+ lib/spack/spack/graph.py:153:58: error[unsupported-operator] Operator `+` is not supported between objects of type `Unknown | None` and `Literal["-1 "]`
- lib/spack/spack/graph.py:156:57: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | None` and `Literal["-2 "]`
+ lib/spack/spack/graph.py:156:57: error[unsupported-operator] Operator `+` is not supported between objects of type `Unknown | None` and `Literal["-2 "]`
- lib/spack/spack/graph.py:222:17: error[unsupported-operator] Operator `+=` is unsupported between objects of type `None` and `Literal[1]`
+ lib/spack/spack/graph.py:222:17: error[unsupported-operator] Operator `+=` is not supported between objects of type `None` and `Literal[1]`
- lib/spack/spack/vendor/jinja2/filters.py:1300:9: error[unsupported-operator] Operator `+=` is unsupported between objects of type `V@do_sum` and `Any | V@do_sum`
+ lib/spack/spack/vendor/jinja2/filters.py:1300:9: error[unsupported-operator] Operator `+=` is not supported between objects of type `V@do_sum` and `Any | V@do_sum`
- lib/spack/spack/vendor/macholib/MachO.py:411:13: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | None` and `Unknown | None | Literal[0]`
+ lib/spack/spack/vendor/macholib/MachO.py:411:13: error[unsupported-operator] Operator `+` is not supported between objects of type `Unknown | None` and `Unknown | None | Literal[0]`
- lib/spack/spack/vendor/macholib/MachO.py:419:21: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | None` and `Unknown | None | Literal[0]`
+ lib/spack/spack/vendor/macholib/MachO.py:419:21: error[unsupported-operator] Operator `+` is not supported between objects of type `Unknown | None` and `Unknown | None | Literal[0]`
- lib/spack/spack/vendor/typing_extensions.py:111:49: error[unsupported-operator] Operator `-` is unsupported between objects of type `~AlwaysFalsy | int` and `int`
+ lib/spack/spack/vendor/typing_extensions.py:111:49: error[unsupported-operator] Operator `-` is not supported between objects of type `~AlwaysFalsy | int` and `int`
- lib/spack/spack/version/version_types.py:1244:35: error[unsupported-operator] Operator `+` is unsupported between objects of type `int | VersionStrComponent` and `Literal[1]`
+ lib/spack/spack/version/version_types.py:1244:35: error[unsupported-operator] Operator `+` is not supported between objects of type `int | VersionStrComponent` and `Literal[1]`
- lib/spack/spack/version/version_types.py:1266:35: error[unsupported-operator] Operator `-` is unsupported between objects of type `int | VersionStrComponent` and `Literal[1]`
+ lib/spack/spack/version/version_types.py:1266:35: error[unsupported-operator] Operator `-` is not supported between objects of type `int | VersionStrComponent` and `Literal[1]`

pip (https://github.com/pypa/pip)
- src/pip/_vendor/pygments/token.py:44:16: error[unsupported-operator] Operator `+` is unsupported between objects of type `Literal["Token"]` and `(Self@__repr__ & ~AlwaysTruthy & ~AlwaysFalsy) | Literal[".", ""]`
+ src/pip/_vendor/pygments/token.py:44:16: error[unsupported-operator] Operator `+` is not supported between objects of type `Literal["Token"]` and `(Self@__repr__ & ~AlwaysTruthy & ~AlwaysFalsy) | Literal[".", ""]`
- src/pip/_vendor/truststore/_api.py:30:37: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'str'>` and `<class 'bytes'>`
+ src/pip/_vendor/truststore/_api.py:30:37: error[unsupported-operator] Operator `|` is not supported between objects of type `<class 'str'>` and `<class 'bytes'>`
- src/pip/_vendor/truststore/_api.py:31:35: error[unsupported-operator] Operator `|` is unsupported between objects of type `<class 'str'>` and `<class 'bytes'>`
+ src/pip/_vendor/truststore/_api.py:31:35: error[unsupported-operator] Operator `|` is not supported between objects of type `<class 'str'>` and `<class 'bytes'>`

asynq (https://github.com/quora/asynq)
- asynq/tools.py:467:28: error[unsupported-operator] Operator `-` is unsupported between objects of type `Utime` and `Unknown | None | Utime`
+ asynq/tools.py:467:28: error[unsupported-operator] Operator `-` is not supported between objects of type `Utime` and `Unknown | None | Utime`

stone (https://github.com/dropbox/stone)
- stone/frontend/lexer.py:78:46: error[unsupported-operator] Operator `*` is unsupported between objects of type `list[Unknown]` and `Unknown | None | Literal[0]`
+ stone/frontend/lexer.py:78:46: error[unsupported-operator] Operator `*` is not supported between objects of type `list[Unknown]` and `Unknown | None | Literal[0]`

jinja (https://github.com/pallets/jinja)
- src/jinja2/filters.py:1352:9: error[unsupported-operator] Operator `+=` is unsupported between objects of type `V@do_sum` and `Any | V@do_sum`
+ src/jinja2/filters.py:1352:9: error[unsupported-operator] Operator `+=` is not supported between objects of type `V@do_sum` and `Any | V@do_sum`

aiortc (https://github.com/aiortc/aiortc)
- src/aiortc/jitterbuffer.py:71:20: error[unsupported-operator] Operator `+` is unsupported between objects of type `int | None` and `int`
+ src/aiortc/jitterbuffer.py:71:20: error[unsupported-operator] Operator `+` is not supported between objects of type `int | None` and `int`
- src/aiortc/jitterbuffer.py:103:19: error[unsupported-operator] Operator `%` is unsupported between objects of type `int | None` and `Unknown | int`
+ src/aiortc/jitterbuffer.py:103:19: error[unsupported-operator] Operator `%` is not supported between objects of type `int | None` and `Unknown | int`
- src/aiortc/jitterbuffer.py:114:19: error[unsupported-operator] Operator `%` is unsupported between objects of type `int | None` and `Unknown | int`
+ src/aiortc/jitterbuffer.py:114:19: error[unsupported-operator] Operator `%` is not supported between objects of type `int | None` and `Unknown | int`
- src/aiortc/rate.py:226:25: error[unsupported-operator] Unary operator `-` is unsupported for type `Unknown | int | None`
+ src/aiortc/rate.py:226:25: error[unsupported-operator] Unary operator `-` is not supported for type `Unknown | int | None`
- src/aiortc/rate.py:229:25: error[unsupported-operator] Operator `-` is unsupported between objects of type `int | None` and `int | None`
+ src/aiortc/rate.py:229:25: error[unsupported-operator] Operator `-` is not supported between objects of type `int | None` and `int | None`
- src/aiortc/rate.py:247:49: error[unsupported-operator] Unary operator `-` is unsupported for type `Unknown | int | None`
+ src/aiortc/rate.py:247:49: error[unsupported-operator] Unary operator `-` is not supported for type `Unknown | int | None`
- src/aiortc/rate.py:249:30: error[unsupported-operator] Operator `-` is unsupported between objects of type `int` and `int | None`
+ src/aiortc/rate.py:249:30: error[unsupported-operator] Operator `-` is not supported between objects of type `int` and `int | None`
- src/aiortc/rate.py:259:53: error[unsupported-operator] Unary operator `-` is unsupported for type `Unknown | int | None`
+ src/aiortc/rate.py:259:53: error[unsupported-operator] Unary operator `-` is not supported for type `Unknown | int | None`
- src/aiortc/rate.py:263:49: error[unsupported-operator] Unary operator `-` is unsupported for type `Unknown | int | None`
+ src/aiortc/rate.py:263:49: error[unsupported-operator] Unary operator `-` is not supported for type `Unknown | int | None`
- src/aiortc/rate.py:506:13: error[unsupported-operator] Operator `+=` is unsupported between objects of type `None` and `Literal[1]`
+ src/aiortc/rate.py:506:13: error[unsupported-operator] Operator `+=` is not supported between objects of type `None` and `Literal[1]`
- src/aiortc/rtcrtpreceiver.py:158:22: error[unsupported-operator] Operator `-` is unsupported between objects of type `int` and `int | None`
+ src/aiortc/rtcrtpreceiver.py:158:22: error[unsupported-operator] Operator `-` is not supported between objects of type `int` and `int | None`
- src/aiortc/rtcrtpreceiver.py:159:24: error[unsupported-operator] Operator `-` is unsupported between objects of type `Unknown | int` and `int | None`
+ src/aiortc/rtcrtpreceiver.py:159:24: error[unsupported-operator] Operator `-` is not supported between objects of type `Unknown | int` and `int | None`
- src/aiortc/rtcrtpreceiver.py:184:16: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | Literal[0]` and `int | None`
+ src/aiortc/rtcrtpreceiver.py:184:16: error[unsupported-operator] Operator `+` is not supported between objects of type `Unknown | Literal[0]` and `int | None`
- src/aiortc/rtcrtpsender.py:249:27: error[unsupported-operator] Operator `-` is unsupported between objects of type `int | float` and `int | float | None`
+ src/aiortc/rtcrtpsender.py:249:27: error[unsupported-operator] Operator `-` is not supported between objects of type `int | float` and `int | float | None`

scrapy (https://github.com/scrapy/scrapy)
- tests/test_pipeline_files.py:660:69: error[unsupported-operator] Operator `+` is unsupported between objects of type `str | bytes` and `Literal["full/filename"]`
+ tests/test_pipeline_files.py:660:69: error[unsupported-operator] Operator `+` is not supported between objects of type `str | bytes` and `Literal["full/filename"]`

graphql-core (https://github.com/graphql-python/graphql-core)
- tests/execution/test_customize.py:70:17: error[unsupported-operator] Operator `*=` is unsupported between objects of type `object` and `Literal[2]`
+ tests/execution/test_customize.py:70:17: error[unsupported-operator] Operator `*=` is not supported between objects of type `object` and `Literal[2]`

pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/capture.py:948:13: error[unsupported-operator] Operator `+=` is unsupported between objects of type `AnyStr@CaptureFixture` and `AnyStr@CaptureFixture`
+ src/_pytest/capture.py:948:13: error[unsupported-operator] Operator `+=` is not supported between objects of type `AnyStr@CaptureFixture` and `AnyStr@CaptureFixture`
- src/_pytest/capture.py:949:13: error[unsupported-operator] Operator `+=` is unsupported between objects of type `AnyStr@CaptureFixture` and `AnyStr@CaptureFixture`
+ src/_pytest/capture.py:949:13: error[unsupported-operator] Operator `+=` is not supported between objects of type `AnyStr@CaptureFixture` and `AnyStr@CaptureFixture`
- src/_pytest/capture.py:964:13: error[unsupported-operator] Operator `+=` is unsupported between objects of type `AnyStr@CaptureFixture` and `AnyStr@CaptureFixture`
+ src/_pytest/capture.py:964:13: error[unsupported-operator] Operator `+=` is not supported between objects of type `AnyStr@CaptureFixture` and `AnyStr@CaptureFixture`
- src/_pytest/capture.py:965:13: error[unsupported-operator] Operator `+=` is unsupported between objects of type `AnyStr@CaptureFixture` and `AnyStr@CaptureFixture`
+ src/_pytest/capture.py:965:13: error[unsupported-operator] Operator `+=` is not supported between objects of type `AnyStr@CaptureFixture` and `AnyStr@CaptureFixture`
- src/_pytest/config/__init__.py:1807:21: error[unsupported-operator] Operator `/` is unsupported between objects of type `Path | Unknown` and `object`
+ src/_pytest/config/__init__.py:1807:21: error[unsupported-operator] Operator `/` is not supported between objects of type `Path | Unknown` and `object`

paasta (https://github.com/yelp/paasta)
- paasta_tools/cli/cmds/get_image_version.py:98:21: error[unsupported-operator] Operator `-` is unsupported between objects of type `datetime | None` and `datetime | None`
+ paasta_tools/cli/cmds/get_image_version.py:98:21: error[unsupported-operator] Operator `-` is not supported between objects of type `datetime | None` and `datetime | None`

starlette (https://github.com/encode/starlette)
- starlette/middleware/wsgi.py:69:21: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | str | tuple[int, int] | ... omitted 3 union elements` and `Literal[","]`
+ starlette/middleware/wsgi.py:69:21: error[unsupported-operator] Operator `+` is not supported between objects of type `Unknown | str | tuple[int, int] | ... omitted 3 union elements` and `Literal[","]`

alerta (https://github.com/alerta/alerta)
- alerta/database/backends/mongodb/base.py:533:24: error[unsupported-operator] Operator `-` is unsupported between objects of type `Unknown | None` and `Literal[1]`
+ alerta/database/backends/mongodb/base.py:533:24: error[unsupported-operator] Operator `-` is not supported between objects of type `Unknown | None` and `Literal[1]`
- alerta/database/backends/mongodb/base.py:571:24: error[unsupported-operator] Operator `-` is unsupported between objects of type `Unknown | None` and `Literal[1]`
+ alerta/database/backends/mongodb/base.py:571:24: error[unsupported-operator] Operator `-` is not supported between objects of type `Unknown | None` and `Literal[1]`
- alerta/database/backends/mongodb/base.py:626:24: error[unsupported-operator] Operator `-` is unsupported between objects of type `Unknown | None` and `Literal[1]`
+ alerta/database/backends/mongodb/base.py:626:24: error[unsupported-operator] Operator `-` is not supported between objects of type `Unknown | None` and `Literal[1]`
- alerta/database/backends/mongodb/base.py:968:81: error[unsupported-operator] Operator `-` is unsupported between objects of type `Unknown | None` and `Literal[1]`
+ alerta/database/backends/mongodb/base.py:968:81: error[unsupported-operator] Operator `-` is not supported between objects of type `Unknown | None` and `Literal[1]`
- alerta/database/backends/mongodb/base.py:1104:82: error[unsupported-operator] Operator `-` is unsupported between objects of type `Unknown | None` and `Literal[1]`
+ alerta/database/backends/mongodb/base.py:1104:82: error[unsupported-operator] Operator `-` is not supported between objects of type `Unknown | None` and `Literal[1]`
- alerta/database/backends/mongodb/base.py:1130:24: error[unsupported-operator] Operator `-` is unsupported between objects of type `Unknown | None` and `Literal[1]`
+ alerta/database/backends/mongodb/base.py:1130:24: error[unsupported-operator] Operator `-` is not supported between objects of type `Unknown | None` and `Literal[1]`
- alerta/database/backends/mongodb/base.py:1173:76: error[unsupported-operator] Operator `-` is unsupported between objects of type `Unknown | None` and `Literal[1]`
+ alerta/database/backends/mongodb/base.py:1173:76: error[unsupported-operator] Operator `-` is not supported between objects of type `Unknown | None` and `Literal[1]`
- alerta/database/backends/mongodb/base.py:1234:77: error[unsupported-operator] Operator `-` is unsupported between objects of type `Unknown | None` and `Literal[1]`
+ alerta/database/backends/mongodb/base.py:1234:77: error[unsupported-operator] Operator `-` is not supported between objects of type `Unknown | None` and `Literal[1]`
- alerta/database/backends/mongodb/base.py:1321:78: error[unsupported-operator] Operator `-` is unsupported between objects of type `Unknown | None` and `Literal[1]`
+ alerta/database/backends/mongodb/base.py:1321:78: error[unsupported-operator] Operator `-` is not supported between objects of type `Unknown | None` and `Literal[1]`
- alerta/database/backends/mongodb/base.py:1394:77: error[unsupported-operator] Operator `-` is unsupported between objects of type `Unknown | None` and `Literal[1]`
+ alerta/database/backends/mongodb/base.py:1394:77: error[unsupported-operator] Operator `-` is not supported between objects of type `Unknown | None` and `Literal[1]`
- alerta/database/backends/mongodb/base.py:1445:81: error[unsupported-operator] Operator `-` is unsupported between objects of type `Unknown | None` and `Literal[1]`
+ alerta/database/backends/mongodb/base.py:1445:81: error[unsupported-operator] Operator `-` is not supported between objects of type `Unknown | None` and `Literal[1]`
- alerta/database/backends/mongodb/base.py:1504:77: error[unsupported-operator] Operator `-` is unsupported between objects of type `Unknown | None` and `Literal[1]`
+ alerta/database/backends/mongodb/base.py:1504:77: error[unsupported-operator] Operator `-` is not supported between objects of type `Unknown | None` and `Literal[1]`
- alerta/database/backends/mongodb/base.py:1511:54: error[unsupported-operator] Operator `-` is unsupported between objects of type `Unknown | None` and `Literal[1]`
+ alerta/database/backends/mongodb/base.py:1511:54: error[unsupported-operator] Operator `-` is not supported between objects of type `Unknown | None` and `Literal[1]`
- alerta/database/backends/mongodb/base.py:1514:71: error[unsupported-operator] Operator `-` is unsupported between objects of type `Unknown | None` and `Literal[1]`
+ alerta/database/backends/mongodb/base.py:1514:71: error[unsupported-operator] Operator `-` is not supported between objects of type `Unknown | None` and `Literal[1]`
- alerta/database/backends/postgres/base.py:438:76: error[unsupported-operator] Operator `-` is unsupported between objects of type `Unknown | None` and `Literal[1]`
+ alerta/database/backends/postgres/base.py:438:76: error[unsupported-operator] Operator `-` is not supported between objects of type `Unknown | None` and `Literal[1]`
- alerta/database/backends/postgres/base.py:472:85: error[unsupported-operator] Operator `-` is unsupported between objects of type `Unknown | None` and `Literal[1]`
+ alerta/database/backends/postgres/base.py:472:85: error[unsupported-operator] Operator `-` is not supported between objects of type `Unknown | None` and `Literal[1]`
- alerta/database/backends/postgres/base.py:512:84: error[unsupported-operator] Operator `-` is unsupported between objects of type `Unknown | None` and `Literal[1]`
+ alerta/database/backends/postgres/base.py:512:84: error[unsupported-operator] Operator `-` is not supported between objects of type `Unknown | None` and `Literal[1]`
- alerta/database/backends/postgres/base.py:758:76: error[unsupported-operator] Operator `-` is unsupported between objects of type `Unknown | None` and `Literal[1]`
+ alerta/database/backends/postgres/base.py:758:76: error[unsupported-operator] Operator `-` is not supported between objects of type `Unknown | None` and `Literal[1]`
- alerta/database/backends/postgres/base.py:928:76: error[unsupported-operator] Operator `-` is unsupported between objects of type `Unknown | None` and `Literal[1]`
+ alerta/database/backends/postgres/base.py:928:76: error[unsupported-operator] Operator `-` is not supported between objects of type `Unknown | None` and `Literal[1]`
- alerta/database/backends/postgres/base.py:963:76: error[unsupported-operator] Operator `-` is unsupported between objects of type `Unknown | None` and `Literal[1]`
+ alerta/database/backends/postgres/base.py:963:76: error[unsupported-operator] Operator `-` is not supported between objects of type `Unknown | None` and `Literal[1]`
- alerta/database/backends/postgres/base.py:1006:76: error[unsupported-operator] Operator `-` is unsupported between objects of type `Unknown | None` and `Literal[1]`
+ alerta/database/backends/postgres/base.py:1006:76: error[unsupported-operator] Operator `-` is not supported between objects of type `Unknown | None` and `Literal[1]`
- alerta/database/backends/postgres/base.py:1085:76: error[unsupported-operator] Operator `-` is unsupported between objects of type `Unknown | None` and `Literal[1]`
+ alerta/database/backends/postgres/base.py:1085:76: error[unsupported-operator] Operator `-` is not supported between objects of type `Unknown | None` and `Literal[1]`
- alerta/database/backends/postgres/base.py:1195:76: error[unsupported-operator] Operator `-` is unsupported between objects of type `Unknown | None` and `Literal[1]`
+ alerta/database/backends/postgres/base.py:1195:76: error[unsupported-operator] Operator `-` is not supported between objects of type `Unknown | None` and `Literal[1]`
- alerta/database/backends/postgres/base.py:1286:76: error[unsupported-operator] Operator `-` is unsupported between objects of type `Unknown | None` and `Literal[1]`
+ alerta/database/backends/postgres/base.py:1286:76: error[unsupported-operator] Operator `-` is not supported between objects of type `Unknown | None` and `Literal[1]`
- alerta/database/backends/postgres/base.py:1360:76: error[unsupported-operator] Operator `-` is unsupported between objects of type `Unknown | None` and `Literal[1]`
+ alerta/database/backends/postgres/base.py:1360:76: error[unsupported-operator] Operator `-` is not supported between objects of type `Unknown | None` and `Literal[1]`
- alerta/database/backends/postgres/base.py:1439:76: error[unsupported-operator] Operator `-` is unsupported between objects of type `Unknown | None` and `Literal[1]`
+ alerta/database/backends/postgres/base.py:1439:76: error[unsupported-operator] Operator `-` is not supported between objects of type `Unknown | None` and `Literal[1]`
- alerta/database/backends/postgres/base.py:1446:71: error[unsupported-operator] Operator `-` is unsupported between objects of type `Unknown | None` and `Literal[1]`
+ alerta/database/backends/postgres/base.py:1446:71: error[unsupported-operator] Operator `-` is not supported between objects of type `Unknown | None` and `Literal[1]`
- alerta/database/backends/postgres/base.py:1453:77: error[unsupported-operator] Operator `-` is unsupported between objects of type `Unknown | None` and `Literal[1]`
+ alerta/database/backends/postgres/base.py:1453:77: error[unsupported-operator] Operator `-` is not supported between objects of type `Unknown | None` and `Literal[1]`
- alerta/models/history.py:105:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Literal["/alert/"]` and `Unknown | None`
+ alerta/models/history.py:105:34: error[unsupported-operator] Operator `+` is not supported between objects of type `Literal["/alert/"]` and `Unknown | None`
- alerta/utils/logging.py:309:22: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int` and `Unknown | str | int`
+ alerta/utils/logging.py:309:22: error[unsupported-operator] Operator `+` is not supported between objects of type `Unknown | int` and `Unknown | str | int`

sockeye (https://github.com/awslabs/sockeye)
- sockeye/data_io.py:176:74: error[unsupported-operator] Operator `/` is unsupported between objects of type `int` and `int | float | None`
+ sockeye/data_io.py:176:74: error[unsupported-operator] Operator `/` is not supported between objects of type `int` and `int | float | None`
- sockeye/data_io.py:193:27: error[unsupported-operator] Operator `*` is unsupported between objects of type `int | Unknown` and `int | float | None`
+ sockeye/data_io.py:193:27: error[unsupported-operator] Operator `*` is not supported between objects of type `int | Unknown` and `int | float | None`

dulwich (https://github.com/dulwich/dulwich)
- dulwich/ignore.py:150:17: error[unsupported-operator] Operator `+=` is unsupported between objects of type `Literal[b""]` and `str`
+ dulwich/ignore.py:150:17: error[unsupported-operator] Operator `+=` is not supported between objects of type `Literal[b""]` and `str`
- dulwich/ignore.py:153:17: error[unsupported-operator] Operator `+=` is unsupported between objects of type `Literal[b""]` and `str`
+ dulwich/ignore.py:153:17: error[unsupported-operator] Operator `+=` is not supported between objects of type `Literal[b""]` and `str`
- dulwich/ignore.py:173:13: error[unsupported-operator] Operator `+=` is unsupported between objects of type `Literal[b""]` and `str`
+ dulwich/ignore.py:173:13: error[unsupported-operator] Operator `+=` is not supported between objects of type `Literal[b""]` and `str`
- dulwich/repo.py:729:31: error[unsupported-operator] Operator `-` is unsupported between objects of type `object` and `set[Unknown]`
+ dulwich/repo.py:729:31: error[unsupported-operator] Operator `-` is not supported between objects of type `object` and `set[Unknown]`
- dulwich/worktree.py:643:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Literal[b"commit: "]` and `bytes | (@Todo & ~None & ~str) | (((Any, Commit, /) -> bytes) & ~(() -> object) & ~str)`
+ dulwich/worktree.py:643:29: error[unsupported-operator] Operator `+` is not supported between objects of type `Literal[b"commit: "]` and `bytes | (@Todo & ~None & ~str) | (((Any, Commit, /) -> bytes) & ~(() -> object) & ~str)`
- dulwich/worktree.py:661:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Literal[b"commit: "]` and `bytes | (@Todo & ~None & ~str) | (((Any, Commit, /) -> bytes) & ~(() -> object) & ~str)`
+ dulwich/worktree.py:661:29: error[unsupported-operator] Operator `+` is not supported between objects of type `Literal[b"commit: "]` and `bytes | (@Todo & ~None & ~str) | (((Any, Commit, /) -> bytes) & ~(() -> object) & ~str)`

ignite (https://github.com/pytorch/ignite)
- examples/references/classification/imagenet/main.py:181:57: error[unsupported-operator] Operator `%` is unsupported between objects of type `Unknown | Literal[0]` and `Unknown | int | None`
+ examples/references/classification/imagenet/main.py:181:57: error[unsupported-operator] Operator `%` is not supported between objects of type `Unknown | Literal[0]` and `Unknown | int | None`
- examples/references/classification/imagenet/main.py:186:23: error[unsupported-operator] Operator `%` is unsupported between objects of type `Unknown | Literal[0]` and `Unknown | int | None`
+ examples/references/classification/imagenet/main.py:186:23: error[unsupported-operator] Operator `%` is not supported between objects of type `Unknown | Literal[0]` and `Unknown | int | None`
- ignite/engine/engine.py:1123:63: error[unsupported-operator] Operator `/` is unsupported between objects of type `(Unknown & ~None) | int` and `Unknown | int | None`
+ ignite/engine/engine.py:1123:63: error[unsupported-operator] Operator `/` is not supported between objects of type `(Unknown & ~None) | int` and `Unknown | int | None`
- ignite/engine/engine.py:1311:63: error[unsupported-operator] Operator `/` is unsupported between objects of type `(Unknown & ~None) | int` and `Unknown | int | None`
+ ignite/engine/engine.py:1311:63: error[unsupported-operator] Operator `/` is not supported between objects of type `(Unknown & ~None) | int` and `Unknown | int | None`
- tests/ignite/metrics/test_mean_absolute_error.py:90:36: error[unsupported-operator] Operator `-` is unsupported between objects of type `Unknown | int | float | ... omitted 3 union elements` and `Unknown | int | float | ... omitted 3 union elements`
+ tests/ignite/metrics/test_mean_absolute_error.py:90:36: error[unsupported-operator] Operator `-` is not supported between objects of type `Unknown | int | float | ... omitted 3 union elements` and `Unknown | int | float | ... omitted 3 union elements`
- tests/ignite/metrics/test_mean_squared_error.py:89:38: error[unsupported-operator] Operator `-` is unsupported between objects of type `Unknown | int | float | ... omitted 3 union elements` and `Unknown | int | float | ... omitted 3 union elements`
+ tests/ignite/metrics/test_mean_squared_error.py:89:38: error[unsupported-operator] Operator `-` is not supported between objects of type `Unknown | int | float | ... omitted 3 union elements` and `Unknown | int | float | ... omitted 3 union elements`
- tests/ignite/metrics/test_root_mean_squared_error.py:87:47: error[unsupported-operator] Operator `-` is unsupported between objects of type `Unknown | int | float | ... omitted 3 union elements` and `Unknown | int | float | ... omitted 3 union elements`
+ tests/ignite/metrics/test_root_mean_squared_error.py:87:47: error[unsupported-operator] Operator `-` is not supported between objects of type `Unknown | int | float | ... omitted 3 union elements` and `Unknown | int | float | ... omitted 3 union elements`

mkosi (https://github.com/systemd/mkosi)
- mkosi/__init__.py:4482:15: error[unsupported-operator] Operator `/` is unsupported between objects of type `Path | None` and `Literal["mkosi.key"]`
+ mkosi/__init__.py:4482:15: error[unsupported-operator] Operator `/` is not supported between objects of type `Path | None` and `Literal["mkosi.key"]`
- mkosi/__init__.py:4482:40: error[unsupported-operator] Operator `/` is unsupported between objects of type `Path | None` and `Literal["mkosi.crt"]`
+ mkosi/__init__.py:4482:40: error[unsupported-operator] Operator `/` is not supported between objects of type `Path | None` and `Literal["mkosi.crt"]`
- mkosi/__init__.py:4506:24: error[unsupported-operator] Operator `/` is unsupported between objects of type `Path | None` and `Literal["mkosi.key"]`
+ mkosi/__init__.py:4506:24: error[unsupported-operator] Operator `/` is not supported between objects of type `Path | None` and `Literal["mkosi.key"]`
- mkosi/__init__.py:4507:21: error[unsupported-operator] Operator `/` is unsupported between objects of type `Path | None` and `Literal["mkosi.crt"]`
+ mkosi/__init__.py:4507:21: error[unsupported-operator] Operator `/` is not supported between objects of type `Path | None` and `Literal["mkosi.crt"]`
- mkosi/__init__.py:4520:9: error[unsupported-operator] Operator `/` is unsupported between objects of type `Path | None` and `Literal["mkosi.version"]`
+ mkosi/__init__.py:4520:9: error[unsupported-operator] Operator `/` is not supported between objects of type `Path | None` and `Literal["mkosi.version"]`

tornado (https://github.com/tornadoweb/tornado)
- tornado/options.py:580:50: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | datetime | timedelta | bool | str` and `Literal[1]`
+ tornado/options.py:580:50: error[unsupported-operator] Operator `+` is not supported between objects of type `Unknown | datetime | timedelta | bool | str` and `Literal[1]`
- tornado/web.py:2058:23: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | None | str` and `Literal["/"]`
+ tornado/web.py:2058:23: error[unsupported-operator] Operator `+` is not supported between objects of type `Unknown | None | str` and `Literal["/"]`
- tornado/web.py:2985:31: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | None | str` and `Literal["/"]`
+ tornado/web.py:2985:31: error[unsupported-operator] Operator `+` is not supported between objects of type `Unknown | None | str` and `Literal["/"]`

cki-lib (https://gitlab.com/cki-project/cki-lib)
- cki_lib/metrics/__init__.py:28:21: error[unsupported-operator] Operator `-` is unsupported between objects of type `int | float` and `Unknown | None | int | float`
+ cki_lib/metrics/__init__.py:28:21: error[unsupported-operator] Operator `-` is not supported between objects of type `int | float` and `Unknown | None | int | float`
- cki_lib/metrics/__init__.py:28:49: error[unsupported-operator] Operator `-` is unsupported between objects of type `int | float` and `Unknown | None | int | float`
+ cki_lib/metrics/__init__.py:28:49: error[unsupported-operator] Operator `-` is not supported between objects of type `int | float` and `Unknown | None | int | float`

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- docs/scripts/clirecording/clidirector.py:137:33: error[unsupported-operator] Operator `*` is unsupported between objects of type `int | None` and `Literal[1000]`
+ docs/scripts/clirecording/clidirector.py:137:33: error[unsupported-operator] Operator `*` is not supported between objects of type `int | None` and `Literal[1000]`
- docs/scripts/clirecording/clidirector.py:145:20: error[unsupported-operator] Operator `+` is unsupported between objects of type `int | None` and `float`
+ docs/scripts/clirecording/clidirector.py:145:20: error[unsupported-operator] Operator `+` is not supported between objects of type `int | None` and `float`
- docs/scripts/clirecording/clidirector.py:186:22: error[unsupported-operator] Operator `-` is unsupported between objects of type `int | float` and `Unknown | None | int | float`
+ docs/scripts/clirecording/clidirector.py:186:22: error[unsupported-operator] Operator `-` is not supported between objects of type `int | float` and `Unknown | None | int | float`

vision (https://github.com/pytorch/vision)
- release/apply-release-changes.py:89:20: error[unsupported-operator] Operator `/` is unsupported between objects of type `Path | None` and `Literal[".github"]`
+ release/apply-release-changes.py:89:20: error[unsupported-operator] Operator `/` is not supported between objects of type `Path | None` and `Literal[".github"]`
- setup.py:416:32: error[unsupported-operator] Operator `+` is unsupported between objects of type `list[Unknown | str]` and `str | list[str] | list[Unknown]`
+ setup.py:416:32: error[unsupported-operator] Operator `+` is not supported between objects of type `list[Unknown | str]` and `str | list[str] | list[Unknown]`
- setup.py:506:30: error[unsupported-operator] Operator `+` is unsupported between objects of type `list[Unknown | Path]` and `str | list[str] | list[Unknown]`
+ setup.py:506:30: error[unsupported-operator] Operator `+` is not supported between objects of type `list[Unknown | Path]` and `str | list[str] | list[Unknown]`
- test/common_extended_utils.py:135:9: error[unsupported-operator] Operator `+=` is unsupported between objects of type `Literal[0]` and `Number`
+ test/common_extended_utils.py:135:9: error[unsupported-operator] Operator `+=` is not supported between objects of type `Literal[0]` and `Number`
- test/common_extended_utils.py:138:9: error[unsupported-operator] Operator `+=` is unsupported between objects of type `Literal[0]` and `Number`
+ test/common_extended_utils.py:138:9: error[unsupported-operator] Operator `+=` is not supported between objects of type `Literal[0]` and `Number`
- test/test_datasets.py:457:18: error[unsupported-operator] Operator `/` is unsupported between objects of type `Path` and `Unknown | str | tuple[str, ...] | int`
+ test/test_datasets.py:457:18: error[unsupported-operator] Operator `/` is not supported between objects of type `Path` and `Unknown | str | tuple[str, ...] | int`
- test/test_datasets.py:480:30: error[unsupported-operator] Operator `-` is unsupported between objects of type `Unknown | str | tuple[str, ...] | int` and `Literal[1]`
+ test/test_datasets.py:480:30: error[unsupported-operator] Operator `-` is not supported between objects of type `Unknown | str | tuple[str, ...] | int` and `Literal[1]`
- test/test_datasets.py:3128:19: error[unsupported-operator] Operator `/` is unsupported between objects of type `str` and `Literal["_camera_settings.json"]`
+ test/test_datasets.py:3128:19: error[unsupported-operator] Operator `/` is not supported between objects of type `str` and `Literal["_camera_settings.json"]`
- test/test_datasets.py:3194:65: error[unsupported-operator] Operator `/` is unsupported between objects of type `str` and `str`
+ test/test_datasets.py:3194:65: error[unsupported-operator] Operator `/` is not supported between objects of type `str` and `str`
- test/test_datasets.py:3396:25: error[unsupported-operator] Operator `/` is unsupported between objects of type `str` and `str`
+ test/test_datasets.py:3396:25: error[unsupported-operator] Operator `/` is not supported between objects of type `str` and `str`
- test/test_datasets.py:3410:25: error[unsupported-operator] Operator `/` is unsupported between objects of type `str` and `str`
+ test/test_datasets.py:3410:25: error[unsupported-operator] Operator `/` is not supported between objects of type `str` and `str`
- test/test_datasets.py:3466:25: error[unsupported-operator] Operator `/` is unsupported between objects of type `str` and `str`
+ test/test_datasets.py:3466:25: error[unsupported-operator] Operator `/` is not supported between objects of type `str` and `str`
- test/test_models.py:120:54: error[unsupported-operator] Operator `+` is unsupported between objects of type `Literal["ModelTester.test_"]` and `Unknown | None`
+ test/test_models.py:120:54: error[unsupported-operator] Operator `+` is not supported between objects of type `Literal["ModelTester.test_"]` and `Unknown | None`
- test/test_models.py:1015:38: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | list[Unknown | int]` and `Literal[1]`
+ test/test_models.py:1015:38: error[unsupported-operator] Operator `+` is not supported between objects of type `Unknown | int | list[Unknown | int]` and `Literal[1]`
- torchvision/ops/poolers.py:190:9: error[unsupported-operator] Operator `+` is unsupported between objects of type `tuple[int, Unknown]` and `list[int]`
+ torchvision/ops/poolers.py:190:9: error[unsupported-operator] Operator `+` is not supported between objects of type `tuple[int, Unknown]` and `list[int]`

comtypes (https://github.com/enthought/comtypes)
- comtypes/server/automation.py:38:17: error[unsupported-operator] Operator `+=` is unsupported between objects of type `None` and `Literal[1]`
+ comtypes/server/automation.py:38:17: error[unsupported-operator] Operator `+=` is not supported between objects of type `None` and `Literal[1]`

koda-validate (https://github.com/keithasaurus/koda-validate)
- koda_validate/generic.py:107:16: error[unsupported-operator] Operator `%` is unsupported between objects of type `Num@MultipleOf` and `Unknown | Num@MultipleOf`
+ koda_validate/generic.py:107:16: error[unsupported-operator] Operator `%` is not supported between objects of type `Num@MultipleOf` and `Unknown | Num@MultipleOf`

mypy (https://github.com/python/mypy)
- mypy/test/testfinegrained.py:318:55: error[unsupported-operator] Operator `+` is unsupported between objects of type `object` and `object`
+ mypy/test/testfinegrained.py:318:55: error[unsupported-operator] Operator `+` is not supported between objects of type `object` and `object`
- mypy/test/testfinegrained.py:356:55: error[unsupported-operator] Operator `+` is unsupported between objects of type `object` and `object`
+ mypy/test/testfinegrained.py:356:55: error[unsupported-operator] Operator `+` is not supported between objects of type `object` and `object`
- mypy/typeshed/stdlib/typing.pyi:399:28: error[unsupported-operator] Operator `|` is unsupported between objects of type `object` and `<class 'type'>`
+ mypy/typeshed/stdlib/typing.pyi:399:28: error[unsupported-operator] Operator `|` is not supported between objects of type `object` and `<class 'type'>`

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/conversion.py:194:47: error[unsupported-operator] Unary operator `~` is unsupported for type `(Cache & ~AlwaysTruthy & ~AlwaysFalsy) | (CacheComponents & ~AlwaysFalsy) | Literal[CacheComponents.NONE]`
+ tanjun/conversion.py:194:47: error[unsupported-operator] Unary operator `~` is not supported for type `(Cache & ~AlwaysTruthy & ~AlwaysFalsy) | (CacheComponents & ~AlwaysFalsy) | Literal[CacheComponents.NONE]`
- tanjun/dependencies/reloaders.py:364:75: error[unsupported-operator] Operator `-` is unsupported between objects of type `set[Path]` and `set[str] | set[Path]`
+ tanjun/dependencies/reloaders.py:364:75: error[unsupported-operator] Operator `-` is not supported between objects of type `set[Path]` and `set[str] | set[Path]`

freqtrade (https://github.com/freqtrade/freqtrade)
- freqtrade/data/converter/orderflow.py:175:21: error[unsupported-operator] Operator `-` is unsupported between objects of type `str | bytes | date | ... omitted 9 union elements` and `str | bytes | date | ... omitted 9 union elements`
+ freqtrade/data/converter/orderflow.py:175:21: error[unsupported-operator] Operator `-` is not supported between objects of type `str | bytes | date | ... omitted 9 union elements` and `str | bytes | date | ... omitted 9 union elements`
- freqtrade/data/metrics.py:241:39: error[unsupported-operator] Operator `+` is unsupported between objects of type `int | str` and `Literal[1]`
+ freqtrade/data/metrics.py:241:39: error[unsupported-operator] Operator `+` is not supported between objects of type `int | str` and `Literal[1]`
- freqtrade/data/metrics.py:242:39: error[unsupported-operator] Operator `-` is unsupported between objects of type `int | str` and `Literal[1]`
+ freqtrade/data/metrics.py:242:39: error[unsupported-operator] Operator `-` is not supported between objects of type `int | str` and `Literal[1]`
- freqtrade/data/metrics.py:243:38: error[unsupported-operator] Operator `-` is unsupported between objects of type `int | str` and `Literal[1]`
+ freqtrade/data/metrics.py:243:38: error[unsupported-operator] Operator `-` is not supported between objects of type `int | str` and `Literal[1]`
- freqtrade/data/metrics.py:250:47: error[unsupported-operator] Operator `-` is unsupported between objects of type `int | str` and `Literal[1]`
+ freqtrade/data/metrics.py:250:47: error[unsupported-operator] Operator `-` is not supported between objects of type `int | str` and `Literal[1]`
- freqtrade/optimize/analysis/lookahead.py:85:51: error[unsupported-operator] Operator `+` is unsupported between objects of type `int | slice[Any, Any, Any] | ndarray[tuple[int], dtype[bool[bool]]]` and `Literal[1]`
+ freqtrade/optimize/analysis/lookahead.py:85:51: error[unsupported-operator] Operator `+` is not supported between objects of type `int | slice[Any, Any, Any] | ndarray[tuple[int], dtype[bool[bool]]]` and `Literal[1]`
- freqtrade/strategy/strategy_helper.py:107:37: error[unsupported-operator] Operator `-` is unsupported between objects of type `(str & ~AlwaysFalsy) | (bytes & ~AlwaysFalsy) | (date & ~AlwaysFalsy) | ... omitted 9 union elements` and `Literal[1]`
+ freqtrade/strategy/strategy_helper.py:107:37: error[unsupported-operator] Operator `-` is not supported between objects of type `(str & ~AlwaysFalsy) | (bytes & ~AlwaysFalsy) | (date & ~AlwaysFalsy) | ... omitted 9 union elements` and `Literal[1]`
- freqtrade/strategy/strategy_helper.py:108:27: error[unsupported-operator] Operator `-` is unsupported between objects of type `(str & ~AlwaysFalsy) | (bytes & ~AlwaysFalsy) | (date & ~AlwaysFalsy) | ... omitted 9 union elements` and `Literal[1]`
+ freqtrade/strategy/strategy_helper.py:108:27: error[unsupported-operator] Operator `-` is not supported between objects of type `(str & ~AlwaysFalsy) | (bytes & ~AlwaysFalsy) | (date & ~AlwaysFalsy) | ... omitted 9 union elements` and `Literal[1]`

meson (https://github.com/mesonbuild/meson)
- mesonbuild/ast/printer.py:277:9: error[unsupported-operator] Operator `+=` is unsupported between objects of type `Literal[""]` and `object`
+ mesonbuild/ast/printer.py:277:9: error[unsupported-operator] Operator `+=` is not supported between objects of type `Literal[""]` and `object`
- mesonbuild/backend/ninjabackend.py:3578:17: error[unsupported-operator] Operator `+=` is unsupported between objects of type `list[ExtractedObjects]` and `list[str]`
+ mesonbuild/backend/ninjabackend.py:3578:17: error[unsupported-operator] Operator `+=` is not supported between objects of type `list[ExtractedObjects]` and `list[str]`
- mesonbuild/backend/ninjabackend.py:3909:39: error[unsupported-operator] Operator `+` is unsupported between objects of type `ImmutableListProtocol[str] | None` and `list[Unknown | str]`
+ mesonbuild/backend/ninjabackend.py:3909:39: error[unsupported-operator] Operator `+` is not supported between objects of type `ImmutableListProtocol[str] | None` and `list[Unknown | str]`
- mesonbuild/backend/ninjabackend.py:3914:39: error[unsupported-operator] Operator `+` is unsupported between objects of type `ImmutableListProtocol[str] | None` and `list[Unknown | str]`
+ mesonbuild/backend/ninjabackend.py:3914:39: error[unsupported-operator] Operator `+` is not supported between objects of type `ImmutableListProtocol[str] | None` and `list[Unknown | str]`
- mesonbuild/backend/vs2010backend.py:645:49: error[unsupported-operator] Operator `+` is unsupported between objects of type `str | Unknown | int | list[str]` and `Literal["|"]`
+ mesonbuild/backend/vs2010backend.py:645:49: error[unsupported-operator] Operator `+` is not supported between objects of type `str | Unknown | int | list[str]` and `Literal["|"]`
- mesonbuild/backend/vs2010backend.py:1051:13: error[unsupported-operator] Operator `+=` is unsupported between objects of type `CompilerArgs` and `str | int | list[str]`
+ mesonbuild/backend/vs2010backend.py:1051:13: error[unsupported-operator] Operator `+=` is not supported between objects of type `CompilerArgs` and `str | int | list[str]`
- mesonbuild/backend/xcodebackend.py:1376:15: error[unsupported-operator] Operator `+` is unsupported between objects of type `ImmutableListProtocol[str] | None` and `list[Unknown | str]`
+ mesonbuild/backend/xcodebackend.py:1376:15: error[unsupported-operator] Operator `+` is not supported between objects of type `ImmutableListProtocol[str] | None` and `list[Unknown | str]`
- mesonbuild/backend/xcodebackend.py:1391:15: error[unsupported-operator] Operator `+` is unsupported between objects of type `ImmutableListProtocol[str] | None` and `list[Unknown | str]`
+ mesonbuild/backend/xcodebackend.py:1391:15: error[unsupported-operator] Operator `+` is not supported between objects of type `ImmutableListProtocol[str] | None` and `list[Unknown | str]`
- mesonbuild/build.py:2810:17: error[unsupported-operator] Operator `+=` is unsupported between objects of type `list[str | File | BuildTarget | CustomTarget]` and `list[str | File | BuildTarget | CustomTarget | ExternalProgram]`
+ mesonbuild/build.py:2810:17: error[unsupported-operator] Operator `+=` is not supported between objects of type `list[str | File | BuildTarget | CustomTarget]` and `list[str | File | BuildTarget | CustomTarget | ExternalProgram]`
- mesonbuild/build.py:2812:17: error[unsupported-operator] Operator `+=` is unsupported between objects of type `list[str | File | BuildTarget | CustomTarget]` and `list[str | File | BuildTarget | CustomTarget | ExternalProgram]`
+ mesonbuild/build.py:2812:17: error[unsupported-operator] Operator `+=` is not supported between objects of type `list[str | File | BuildTarget | CustomTarget]` and `list[str | File | BuildTarget | CustomTarget | ExternalProgram]`
- mesonbuild/cmake/fileapi.py:195:21: error[unsupported-operator] Operator `+=` is unsupported between objects of type `Path` and `list[Unknown | dict[Unknown, Unknown]]`
+ mesonbuild/cmake/fileapi.py:195:21: error[unsupported-operator] Operator `+=` is not supported between objects of type `Path` and `list[Unknown | dict[Unknown, Unknown]]`
- mesonbuild/cmake/fileapi.py:195:21: error[unsupported-operator] Operator `+=` is unsupported between objects of type `bool` and `list[Unknown | dict[Unknown, Unknown]]`
+ mesonbuild/cmake/fileapi.py:195:21: error[unsupported-operator] Operator `+=` is not supported between objects of type `bool` and `list[Unknown | dict[Unknown, Unknown]]`
- mesonbuild/cmake/fileapi.py:200:21: error[unsupported-operator] Operator `+=` is unsupported between objects of type `Path` and `list[Unknown | dict[Unknown, Unknown]]`
+ mesonbuild/cmake/fileapi.py:200:21: error[unsupported-operator] Operator `+=` is not supported between objects of type `Path` and `list[Unknown | dict[Unknown, Unknown]]`
- mesonbuild/cmake/fileapi.py:200:21: error[unsupported-operator] Operator `+=` is unsupported between objects of type `bool` and `list[Unknown | dict[Unknown, Unknown]]`
+ mesonbuild/cmake/fileapi.py:200:21: error[unsupported-operator] Operator `+=` is not supported between objects of type `bool` and `list[Unknown | dict[Unknown, Unknown]]`
- mesonbuild/cmake/fileapi.py:218:17: error[unsupported-operator] Operator `+=` is unsupported between objects of type `Path` and `list[U

... (truncated 4069 lines) ...
```

</details>


No memory usage changes detected ✅



---

_Comment by @lucach on 2025-12-11 14:33_

Some checks failed because GitHub's reliability is quite poor recently...

```
    error: Failed to install ruff 0.12.5
      Caused by: Failed to download from: https://github.com/astral-sh/ruff/releases/download/0.12.5/ruff-x86_64-unknown-linux-gnu.tar.gz
      Caused by: HTTP status server error (503 Service Unavailable) for url (https://github.com/astral-sh/ruff/releases/download/0.12.5/ruff-x86_64-unknown-linux-gnu.tar.gz)
```

---

_Label `diagnostics` added by @AlexWaygood on 2025-12-11 14:36_

---

_@AlexWaygood approved on 2025-12-11 15:02_

Thanks!

---

_Merged by @AlexWaygood on 2025-12-11 15:03_

---

_Closed by @AlexWaygood on 2025-12-11 15:03_

---
