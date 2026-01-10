```yaml
number: 17689
title: "Support `typing.Self` in methods"
type: pull_request
state: merged
author: Glyphack
labels:
  - ty
assignees: []
merged: true
base: main
head: typing-self
created_at: 2025-04-28T21:13:45Z
updated_at: 2025-09-18T16:11:46Z
url: https://github.com/astral-sh/ruff/pull/17689
synced_at: 2026-01-10T17:40:28Z
```

# Support `typing.Self` in methods

---

_Pull request opened by @Glyphack on 2025-04-28 21:13_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes: astral-sh/ty#159 

<!-- What's the purpose of the change? What does it do, and why? -->
This PR adds the support for using `Self` in methods.
When the type of an annotation is `TypingSelf` it is converted to a type var based on:
https://typing.python.org/en/latest/spec/generics.html#self

I am naming the type var type that is created on the fly same as the class name so the type revealed shows that it's the class. Pyright for example names it `Self@ClassName` I'm not sure if that's better or not.

I just skipped Protocols because it had more problems and the tests was not useful.
Also I need to create a follow up PR that implicitly assumes `self` argument has type `Self`.

In order to infer the type in the `in_type_expression` method I needed to have scope id and semantic index available. I used the idea from [this PR](https://github.com/astral-sh/ruff/pull/17589/files) to pass additional context to this method.
Also I think in all places that `in_type_expression` is called we need to have this context because `Self` can be there so I didn't split the method into one version with context and one without.

## Test Plan

Added new tests from spec.


Notes:

This test previously did not emit diagnostic but now there is a diagnostic.

```
class Base:
    def __new__(cls, x: int) -> Self: ...
```

I think it’s appropriate to make diagnostic?

mypy does it: [https://mypy-play.net/?mypy=latest&python=3.12&gist=0be580f15a4fe36399738284c5c9688d](https://mypy-play.net/?mypy=latest&python=3.12&gist=0be580f15a4fe36399738284c5c9688d)

pyright does not: [https://pyright-play.net/?code=GYJw9gtgBALgngBwJYDsDmUkQWEMoDKApgDbABQ5AxiQIYDO9UAQg0QFzlTdQAmRwKAH0hKIgHcRAChr0ANFAAe7TChgBKKAFoAfIVLAVAOhPkgA](https://pyright-play.net/?code=GYJw9gtgBALgngBwJYDsDmUkQWEMoDKApgDbABQ5AxiQIYDO9UAQg0QFzlTdQAmRwKAH0hKIgHcRAChr0ANFAAe7TChgBKKAFoAfIVLAVAOhPkgA)

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-04-28 21:16_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pytest-robotframework (https://github.com/detachhead/pytest-robotframework)
+ warning[lint:redundant-cast] pytest_robotframework/_internal/robot/listeners_and_suite_visitors.py:151:40: Value is already of type `Unknown`
- Found 235 diagnostics
+ Found 236 diagnostics

python-chess (https://github.com/niklasf/python-chess)
+ error[lint:no-matching-overload] chess/pgn.py:1850:12: No overload of function `read_game` matches arguments
+ error[lint:no-matching-overload] chess/pgn.py:1857:17: No overload of function `read_game` matches arguments
- Found 60 diagnostics
+ Found 62 diagnostics

attrs (https://github.com/python-attrs/attrs)
+ error[lint:no-matching-overload] tests/test_make.py:803:21: No overload of function `attrib` matches arguments
- Found 600 diagnostics
+ Found 601 diagnostics

kornia (https://github.com/kornia/kornia)
+ warning[lint:redundant-cast] kornia/augmentation/container/augment.py:367:23: Value is already of type `Unknown`
+ warning[lint:redundant-cast] kornia/augmentation/container/augment.py:375:27: Value is already of type `Unknown`
+ warning[lint:redundant-cast] kornia/augmentation/container/augment.py:398:46: Value is already of type `Unknown`
- Found 957 diagnostics
+ Found 960 diagnostics

yarl (https://github.com/aio-libs/yarl)
- error[lint:invalid-assignment] yarl/_url.py:1019:13: Object of type `@Todo(Support for `typing.Self`) | int` is not assignable to `bool`
+ error[lint:invalid-assignment] yarl/_url.py:1019:13: Object of type `int` is not assignable to `bool`

trio (https://github.com/python-trio/trio)
- error[lint:type-assertion-failure] src/trio/_tests/type_tests/path.py:16:5: Actual type `@Todo(Support for `typing.Self`)` is not the same as asserted type `Path`
+ error[lint:type-assertion-failure] src/trio/_tests/type_tests/path.py:16:5: Actual type `Unknown` is not the same as asserted type `Path`
- error[lint:type-assertion-failure] src/trio/_tests/type_tests/path.py:17:5: Actual type `@Todo(Support for `typing.Self`)` is not the same as asserted type `Path`
+ error[lint:type-assertion-failure] src/trio/_tests/type_tests/path.py:17:5: Actual type `Unknown` is not the same as asserted type `Path`
- error[lint:type-assertion-failure] src/trio/_tests/type_tests/path.py:18:5: Actual type `@Todo(Support for `typing.Self`)` is not the same as asserted type `Path`
+ error[lint:type-assertion-failure] src/trio/_tests/type_tests/path.py:18:5: Actual type `Unknown` is not the same as asserted type `Path`
- error[lint:type-assertion-failure] src/trio/_tests/type_tests/path.py:19:5: Actual type `@Todo(Support for `typing.Self`)` is not the same as asserted type `Path`
+ error[lint:type-assertion-failure] src/trio/_tests/type_tests/path.py:19:5: Actual type `Unknown` is not the same as asserted type `Path`
- error[lint:type-assertion-failure] src/trio/_tests/type_tests/path.py:43:5: Actual type `@Todo(Support for `typing.Self`)` is not the same as asserted type `Path`
+ error[lint:type-assertion-failure] src/trio/_tests/type_tests/path.py:43:5: Actual type `Unknown` is not the same as asserted type `Path`
- error[lint:type-assertion-failure] src/trio/_tests/type_tests/path.py:53:5: Actual type `@Todo(Support for `typing.Self`)` is not the same as asserted type `Path`
+ error[lint:type-assertion-failure] src/trio/_tests/type_tests/path.py:53:5: Actual type `Unknown` is not the same as asserted type `Path`
- error[lint:type-assertion-failure] src/trio/_tests/type_tests/path.py:55:5: Actual type `@Todo(Support for `typing.Self`)` is not the same as asserted type `Path`
+ error[lint:type-assertion-failure] src/trio/_tests/type_tests/path.py:55:5: Actual type `Unknown` is not the same as asserted type `Path`
- error[lint:type-assertion-failure] src/trio/_tests/type_tests/path.py:57:9: Actual type `@Todo(Support for `typing.Self`)` is not the same as asserted type `Path`
+ error[lint:type-assertion-failure] src/trio/_tests/type_tests/path.py:57:9: Actual type `Unknown` is not the same as asserted type `Path`
- error[lint:type-assertion-failure] src/trio/_tests/type_tests/path.py:58:5: Actual type `@Todo(Support for `typing.Self`)` is not the same as asserted type `Path`
+ error[lint:type-assertion-failure] src/trio/_tests/type_tests/path.py:58:5: Actual type `Unknown` is not the same as asserted type `Path`
- error[lint:type-assertion-failure] src/trio/_tests/type_tests/path.py:59:5: Actual type `@Todo(Support for `typing.Self`)` is not the same as asserted type `Path`
+ error[lint:type-assertion-failure] src/trio/_tests/type_tests/path.py:59:5: Actual type `Unknown` is not the same as asserted type `Path`
- error[lint:type-assertion-failure] src/trio/_tests/type_tests/path.py:60:5: Actual type `@Todo(Support for `typing.Self`)` is not the same as asserted type `Path`
+ error[lint:type-assertion-failure] src/trio/_tests/type_tests/path.py:60:5: Actual type `Unknown` is not the same as asserted type `Path`

ignite (https://github.com/pytorch/ignite)
+ warning[lint:redundant-cast] ignite/metrics/vision/object_detection_average_precision_recall.py:335:63: Value is already of type `Unknown`
- Found 2257 diagnostics
+ Found 2258 diagnostics

websockets (https://github.com/aaugustin/websockets)
+ error[lint:invalid-assignment] src/websockets/legacy/auth.py:179:9: Object of type `<class 'BasicAuthWebSocketServerProtocol'>` is not assignable to `((...) -> BasicAuthWebSocketServerProtocol) | None`
+ error[lint:invalid-assignment] src/websockets/legacy/client.py:459:13: Object of type `<class 'WebSocketClientProtocol'>` is not assignable to `((...) -> WebSocketClientProtocol) | None`
+ error[lint:invalid-assignment] src/websockets/legacy/server.py:1028:13: Object of type `<class 'WebSocketServerProtocol'>` is not assignable to `((...) -> WebSocketServerProtocol) | None`
- Found 105 diagnostics
+ Found 108 diagnostics

mkosi (https://github.com/systemd/mkosi)
- error[lint:invalid-argument-type] mkosi/__init__.py:4191:13: Argument to this function is incorrect: Expected `Mapping[str, str]`, found `dict[str | _T1, str | _T2] | @Todo(Support for `typing.Self`)`
+ error[lint:invalid-argument-type] mkosi/__init__.py:4191:13: Argument to this function is incorrect: Expected `Mapping[str, str]`, found `dict[str | _T1, str | _T2] | Unknown`
- warning[lint:unused-ignore-comment] mkosi/config.py:1117:39: Unused blanket `type: ignore` directive
- warning[lint:possibly-unbound-attribute] mkosi/qemu.py:514:12: Attribute `exists` on type `@Todo(Support for `typing.Self`) | Path | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] mkosi/qemu.py:514:12: Attribute `exists` on type `Unknown | Path | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] mkosi/qemu.py:517:9: Attribute `mkdir` on type `@Todo(Support for `typing.Self`) | Path | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] mkosi/qemu.py:517:9: Attribute `mkdir` on type `Unknown | Path | None` is possibly unbound
- error[lint:invalid-argument-type] mkosi/qemu.py:519:26: Argument to this function is incorrect: Expected `Path`, found `@Todo(Support for `typing.Self`) | Path | None`
+ error[lint:invalid-argument-type] mkosi/qemu.py:519:26: Argument to this function is incorrect: Expected `Path`, found `Unknown | Path | None`
- error[lint:invalid-argument-type] mkosi/qemu.py:520:29: Argument to this function is incorrect: Expected `Path`, found `@Todo(Support for `typing.Self`) | Path | None`
+ error[lint:invalid-argument-type] mkosi/qemu.py:520:29: Argument to this function is incorrect: Expected `Path`, found `Unknown | Path | None`
- warning[lint:possibly-unbound-attribute] mkosi/qemu.py:541:16: Attribute `stat` on type `@Todo(Support for `typing.Self`) | Path | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] mkosi/qemu.py:541:16: Attribute `stat` on type `Unknown | Path | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] mkosi/qemu.py:542:17: Attribute `stat` on type `@Todo(Support for `typing.Self`) | Path | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] mkosi/qemu.py:542:17: Attribute `stat` on type `Unknown | Path | None` is possibly unbound
- Found 304 diagnostics
+ Found 303 diagnostics

pip (https://github.com/pypa/pip)
- warning[lint:possibly-unbound-attribute] src/pip/_vendor/distlib/util.py:1268:28: Attribute `getmembers` on type `ZipFile | @Todo(Support for `typing.Self`)` is possibly unbound
+ warning[lint:possibly-unbound-attribute] src/pip/_vendor/distlib/util.py:1268:28: Attribute `getmembers` on type `ZipFile | Unknown` is possibly unbound
- error[lint:invalid-assignment] src/pip/_vendor/distlib/util.py:1283:9: Object of type `def extraction_filter(member, path) -> Unknown` is not assignable to attribute `extraction_filter` on type `ZipFile | @Todo(Support for `typing.Self`)`
+ error[lint:invalid-assignment] src/pip/_vendor/distlib/util.py:1283:9: Object of type `def extraction_filter(member, path) -> Unknown` is not assignable to attribute `extraction_filter` on type `ZipFile | Unknown`

werkzeug (https://github.com/pallets/werkzeug)
- error[lint:call-non-callable] src/werkzeug/datastructures/mixins.py:238:13: Object of type `None` is not callable
+ error[lint:invalid-argument-type] src/werkzeug/datastructures/mixins.py:238:28: Argument to this function is incorrect: Expected `Self`, found `UpdateDictMixin[Any, Any]`
- warning[lint:unused-ignore-comment] src/werkzeug/datastructures/mixins.py:260:48: Unused blanket `type: ignore` directive
- warning[lint:unused-ignore-comment] src/werkzeug/datastructures/mixins.py:280:45: Unused blanket `type: ignore` directive
+ error[lint:call-non-callable] src/werkzeug/datastructures/mixins.py:262:13: Object of type `None` is not callable
+ error[lint:invalid-super-argument] src/werkzeug/datastructures/mixins.py:278:18: `Self` is not an instance or subclass of `<class 'UpdateDictMixin'>` in `super(<class 'UpdateDictMixin'>, Self)` call
+ error[lint:call-non-callable] src/werkzeug/datastructures/mixins.py:282:13: Object of type `None` is not callable
+ error[lint:call-non-callable] src/werkzeug/datastructures/structures.py:1104:13: Object of type `None` is not callable
+ error[lint:call-non-callable] src/werkzeug/datastructures/structures.py:1119:13: Object of type `None` is not callable
+ error[lint:call-non-callable] src/werkzeug/datastructures/structures.py:1159:13: Object of type `None` is not callable
+ error[lint:call-non-callable] src/werkzeug/datastructures/structures.py:1185:13: Object of type `None` is not callable
+ error[lint:call-non-callable] src/werkzeug/datastructures/structures.py:1193:13: Object of type `None` is not callable
- warning[lint:possibly-unbound-attribute] tests/test_http.py:159:16: Attribute `type` on type `@Todo(Support for `typing.Self`) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] tests/test_http.py:159:16: Attribute `type` on type `Unknown | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] tests/test_http.py:160:16: Attribute `username` on type `@Todo(Support for `typing.Self`) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] tests/test_http.py:160:16: Attribute `username` on type `Unknown | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] tests/test_http.py:161:16: Attribute `password` on type `@Todo(Support for `typing.Self`) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] tests/test_http.py:161:16: Attribute `password` on type `Unknown | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] tests/test_http.py:164:16: Attribute `type` on type `@Todo(Support for `typing.Self`) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] tests/test_http.py:164:16: Attribute `type` on type `Unknown | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] tests/test_http.py:165:16: Attribute `username` on type `@Todo(Support for `typing.Self`) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] tests/test_http.py:165:16: Attribute `username` on type `Unknown | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] tests/test_http.py:166:16: Attribute `password` on type `@Todo(Support for `typing.Self`) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] tests/test_http.py:166:16: Attribute `password` on type `Unknown | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] tests/test_http.py:169:16: Attribute `type` on type `@Todo(Support for `typing.Self`) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] tests/test_http.py:169:16: Attribute `type` on type `Unknown | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] tests/test_http.py:170:16: Attribute `username` on type `@Todo(Support for `typing.Self`) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] tests/test_http.py:170:16: Attribute `username` on type `Unknown | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] tests/test_http.py:171:16: Attribute `password` on type `@Todo(Support for `typing.Self`) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] tests/test_http.py:171:16: Attribute `password` on type `Unknown | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] tests/test_http.py:183:16: Attribute `type` on type `@Todo(Support for `typing.Self`) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] tests/test_http.py:183:16: Attribute `type` on type `Unknown | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] tests/test_http.py:184:16: Attribute `username` on type `@Todo(Support for `typing.Self`) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] tests/test_http.py:184:16: Attribute `username` on type `Unknown | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] tests/test_http.py:185:16: Attribute `realm` on type `@Todo(Support for `typing.Self`) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] tests/test_http.py:185:16: Attribute `realm` on type `Unknown | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] tests/test_http.py:186:16: Attribute `nonce` on type `@Todo(Support for `typing.Self`) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] tests/test_http.py:186:16: Attribute `nonce` on type `Unknown | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] tests/test_http.py:187:16: Attribute `uri` on type `@Todo(Support for `typing.Self`) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] tests/test_http.py:187:16: Attribute `uri` on type `Unknown | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] tests/test_http.py:188:16: Attribute `qop` on type `@Todo(Support for `typing.Self`) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] tests/test_http.py:188:16: Attribute `qop` on type `Unknown | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] tests/test_http.py:189:16: Attribute `nc` on type `@Todo(Support for `typing.Self`) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] tests/test_http.py:189:16: Attribute `nc` on type `Unknown | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] tests/test_http.py:190:16: Attribute `cnonce` on type `@Todo(Support for `typing.Self`) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] tests/test_http.py:190:16: Attribute `cnonce` on type `Unknown | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] tests/test_http.py:191:16: Attribute `response` on type `@Todo(Support for `typing.Self`) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] tests/test_http.py:191:16: Attribute `response` on type `Unknown | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] tests/test_http.py:192:16: Attribute `opaque` on type `@Todo(Support for `typing.Self`) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] tests/test_http.py:192:16: Attribute `opaque` on type `Unknown | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] tests/test_http.py:202:16: Attribute `type` on type `@Todo(Support for `typing.Self`) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] tests/test_http.py:202:16: Attribute `type` on type `Unknown | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] tests/test_http.py:203:16: Attribute `username` on type `@Todo(Support for `typing.Self`) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] tests/test_http.py:203:16: Attribute `username` on type `Unknown | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] tests/test_http.py:204:16: Attribute `realm` on type `@Todo(Support for `typing.Self`) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] tests/test_http.py:204:16: Attribute `realm` on type `Unknown | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] tests/test_http.py:205:16: Attribute `nonce` on type `@Todo(Support for `typing.Self`) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] tests/test_http.py:205:16: Attribute `nonce` on type `Unknown | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] tests/test_http.py:206:16: Attribute `uri` on type `@Todo(Support for `typing.Self`) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] tests/test_http.py:206:16: Attribute `uri` on type `Unknown | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] tests/test_http.py:207:16: Attribute `response` on type `@Todo(Support for `typing.Self`) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] tests/test_http.py:207:16: Attribute `response` on type `Unknown | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] tests/test_http.py:208:16: Attribute `opaque` on type `@Todo(Support for `typing.Self`) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] tests/test_http.py:208:16: Attribute `opaque` on type `Unknown | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] tests/test_http.py:212:16: Attribute `type` on type `@Todo(Support for `typing.Self`) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] tests/test_http.py:212:16: Attribute `type` on type `Unknown | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] tests/test_http.py:216:16: Attribute `to_header` on type `@Todo(Support for `typing.Self`) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] tests/test_http.py:216:16: Attribute `to_header` on type `Unknown | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] tests/test_http.py:222:16: Attribute `type` on type `@Todo(Support for `typing.Self`) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] tests/test_http.py:222:16: Attribute `type` on type `Unknown | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] tests/test_http.py:223:16: Attribute `token` on type `@Todo(Support for `typing.Self`) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] tests/test_http.py:223:16: Attribute `token` on type `Unknown | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] tests/test_http.py:228:16: Attribute `type` on type `@Todo(Support for `typing.Self`) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] tests/test_http.py:228:16: Attribute `type` on type `Unknown | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] tests/test_http.py:229:16: Attribute `token` on type `@Todo(Support for `typing.Self`) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] tests/test_http.py:229:16: Attribute `token` on type `Unknown | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] tests/test_http.py:253:16: Attribute `type` on type `@Todo(Support for `typing.Self`) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] tests/test_http.py:253:16: Attribute `type` on type `Unknown | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] tests/test_http.py:254:16: Attribute `realm` on type `@Todo(Support for `typing.Self`) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] tests/test_http.py:254:16: Attribute `realm` on type `Unknown | None` is possibly unbound
- error[lint:invalid-assignment] tests/test_http.py:255:9: Object of type `Literal["Foo Bar"]` is not assignable to attribute `realm` on type `@Todo(Support for `typing.Self`) | None`
+ error[lint:invalid-assignment] tests/test_http.py:255:9: Object of type `Literal["Foo Bar"]` is not assignable to attribute `realm` on type `Unknown | None`
- warning[lint:possibly-unbound-attribute] tests/test_http.py:256:16: Attribute `to_header` on type `@Todo(Support for `typing.Self`) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] tests/test_http.py:256:16: Attribute `to_header` on type `Unknown | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] tests/test_http.py:264:16: Attribute `type` on type `@Todo(Support for `typing.Self`) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] tests/test_http.py:264:16: Attribute `type` on type `Unknown | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] tests/test_http.py:265:16: Attribute `realm` on type `@Todo(Support for `typing.Self`) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] tests/test_http.py:265:16: Attribute `realm` on type `Unknown | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] tests/test_http.py:266:16: Attribute `parameters` on type `@Todo(Support for `typing.Self`) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] tests/test_http.py:266:16: Attribute `parameters` on type `Unknown | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] tests/test_http.py:267:16: Attribute `nonce` on type `@Todo(Support for `typing.Self`) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] tests/test_http.py:267:16: Attribute `nonce` on type `Unknown | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] tests/test_http.py:268:16: Attribute `opaque` on type `@Todo(Support for `typing.Self`) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] tests/test_http.py:268:16: Attribute `opaque` on type `Unknown | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] tests/test_http.py:270:16: Attribute `type` on type `@Todo(Support for `typing.Self`) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] tests/test_http.py:270:16: Attribute `type` on type `Unknown | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] tests/test_http.py:277:16: Attribute `type` on type `@Todo(Support for `typing.Self`) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] tests/test_http.py:277:16: Attribute `type` on type `Unknown | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] tests/test_http.py:278:16: Attribute `token` on type `@Todo(Support for `typing.Self`) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] tests/test_http.py:278:16: Attribute `token` on type `Unknown | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] tests/test_http.py:283:16: Attribute `type` on type `@Todo(Support for `typing.Self`) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] tests/test_http.py:283:16: Attribute `type` on type `Unknown | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] tests/test_http.py:284:16: Attribute `token` on type `@Todo(Support for `typing.Self`) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] tests/test_http.py:284:16: Attribute `token` on type `Unknown | None` is possibly unbound
- Found 449 diagnostics
+ Found 455 diagnostics

typeshed-stats (https://github.com/AlexWaygood/typeshed-stats)
- error[lint:invalid-argument-type] src/typeshed_stats/_cli.py:321:42: Argument to this function is incorrect: Expected `Path`, found `@Todo(Support for `typing.Self`) | Path | None`
+ error[lint:invalid-argument-type] src/typeshed_stats/_cli.py:321:42: Argument to this function is incorrect: Expected `Path`, found `Unknown | Path | None`
- error[lint:invalid-argument-type] src/typeshed_stats/_cli.py:326:61: Argument to this function is incorrect: Expected `Path | str`, found `@Todo(Support for `typing.Self`) | Path | None`
+ error[lint:invalid-argument-type] src/typeshed_stats/_cli.py:326:61: Argument to this function is incorrect: Expected `Path | str`, found `Unknown | Path | None`

tornado (https://github.com/tornadoweb/tornado)
- error[lint:unresolved-attribute] tornado/locale.py:446:44: Type `timedelta` has no attribute `month`
- error[lint:unresolved-attribute] tornado/locale.py:447:43: Type `timedelta` has no attribute `weekday`
- error[lint:unresolved-attribute] tornado/locale.py:448:28: Type `timedelta` has no attribute `day`
- error[lint:unresolved-attribute] tornado/locale.py:452:44: Type `timedelta` has no attribute `month`
- error[lint:unresolved-attribute] tornado/locale.py:453:28: Type `timedelta` has no attribute `day`
- Found 514 diagnostics
+ Found 509 diagnostics

cki-lib (https://gitlab.com/cki-project/cki-lib)
- error[lint:invalid-argument-type] cki_lib/inttests/remote_responses.py:248:48: Argument to this function is incorrect: Expected `(Any, @Todo(Support for `typing.TypeAlias`), @Todo(Support for `typing.Self`), /) -> BaseRequestHandler`, found `<class '_MockRequestHandler'>`
+ error[lint:invalid-argument-type] cki_lib/inttests/remote_responses.py:248:48: Argument to this function is incorrect: Expected `(Any, @Todo(Support for `typing.TypeAlias`), Unknown, /) -> BaseRequestHandler`, found `<class '_MockRequestHandler'>`

paasta (https://github.com/yelp/paasta)
- error[lint:invalid-argument-type] paasta_tools/firewall_logging.py:133:57: Argument to this function is incorrect: Expected `(Any, @Todo(Support for `typing.TypeAlias`), @Todo(Support for `typing.Self`), /) -> BaseRequestHandler`, found `<class 'SyslogUDPHandler'>`
+ error[lint:invalid-argument-type] paasta_tools/firewall_logging.py:133:57: Argument to this function is incorrect: Expected `(Any, @Todo(Support for `typing.TypeAlias`), Unknown, /) -> BaseRequestHandler`, found `<class 'SyslogUDPHandler'>`

psycopg (https://github.com/psycopg/psycopg)
- error[lint:unresolved-attribute] psycopg/psycopg/types/datetime.py:536:20: Type `timedelta` has no attribute `astimezone`
- Found 1154 diagnostics
+ Found 1153 diagnostics

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
+ error[lint:invalid-return-type] src/hydra_zen/wrapper/_implementations.py:1578:20: Return type does not match returned value: Expected `F | Self`, found `ZenStore`
- error[lint:type-assertion-failure] tests/annotations/declarations.py:1206:5: Actual type `@Todo(Support for `typing.Self`)` is not the same as asserted type `SubStore`
- error[lint:type-assertion-failure] tests/annotations/declarations.py:1208:5: Actual type `@Todo(Support for `typing.Self`)` is not the same as asserted type `dict[str, int]`
+ error[lint:type-assertion-failure] tests/annotations/declarations.py:1208:5: Actual type `dict[Unknown, Unknown]` is not the same as asserted type `dict[str, int]`
- error[lint:type-assertion-failure] tests/annotations/mypy_checks.py:68:5: Actual type `@Todo(Support for `typing.Self`)` is not the same as asserted type `ZenStore`
- error[lint:type-assertion-failure] tests/annotations/mypy_checks.py:76:5: Actual type `@Todo(Support for `typing.Self`)` is not the same as asserted type `bool`
- error[lint:type-assertion-failure] tests/annotations/mypy_checks.py:77:5: Actual type `@Todo(Support for `typing.Self`)` is not the same as asserted type `bool`
- error[lint:type-assertion-failure] tests/annotations/mypy_checks.py:103:5: Actual type `@Todo(Support for `typing.Self`)` is not the same as asserted type `MyStore`
- Found 651 diagnostics
+ Found 647 diagnostics

scrapy (https://github.com/scrapy/scrapy)
+ warning[lint:redundant-cast] scrapy/spiders/crawl.py:133:25: Value is already of type `Unknown`
+ warning[lint:redundant-cast] scrapy/spiders/crawl.py:136:23: Value is already of type `Unknown`
- Found 1388 diagnostics
+ Found 1390 diagnostics

cwltool (https://github.com/common-workflow-language/cwltool)
+ warning[lint:redundant-cast] cwltool/process.py:496:40: Value is already of type `Unknown`
- Found 341 diagnostics
+ Found 342 diagnostics

arviz (https://github.com/arviz-devs/arviz)
- error[lint:call-non-callable] arviz/tests/base_tests/test_stats.py:397:12: Method `__getitem__` of type `(bound method _LocIndexer.__getitem__(key) -> Unknown) | (bound method _LocIndexer[@Todo(Support for `typing.Self`)].__getitem__(key: Mapping[Any, Any]) -> @Todo(Support for `typing.Self`))` is not callable on object of type `_LocIndexer | _LocIndexer[@Todo(Support for `typing.Self`)]`
- error[lint:call-non-callable] arviz/tests/base_tests/test_stats.py:410:15: Method `__getitem__` of type `(bound method _LocIndexer.__getitem__(key) -> Unknown) | (bound method _LocIndexer[@Todo(Support for `typing.Self`)].__getitem__(key: Mapping[Any, Any]) -> @Todo(Support for `typing.Self`))` is not callable on object of type `_LocIndexer | _LocIndexer[@Todo(Support for `typing.Self`)]`
+ error[lint:call-non-callable] arviz/tests/base_tests/test_stats.py:397:12: Method `__getitem__` of type `(bound method _LocIndexer.__getitem__(key) -> Unknown) | (bound method _LocIndexer[Self].__getitem__(key: Mapping[Any, Any]) -> Self)` is not callable on object of type `_LocIndexer | _LocIndexer[Self]`
+ error[lint:call-non-callable] arviz/tests/base_tests/test_stats.py:410:15: Method `__getitem__` of type `(bound method _LocIndexer.__getitem__(key) -> Unknown) | (bound method _LocIndexer[Self].__getitem__(key: Mapping[Any, Any]) -> Self)` is not callable on object of type `_LocIndexer | _LocIndexer[Self]`

setuptools (https://github.com/pypa/setuptools)
+ error[lint:invalid-return-type] setuptools/_scripts.py:58:20: Return type does not match returned value: Expected `Self`, found `Self | str | @Todo(specialized non-generic class) | None`
+ error[lint:no-matching-overload] setuptools/tests/test_core_metadata.py:550:15: No overload of bound method `__init__` matches arguments
- Found 1032 diagnostics
+ Found 1034 diagnostics

mypy (https://github.com/python/mypy)
- error[lint:invalid-assignment] mypy/fixup.py:86:29: Object of type `@Todo(Support for `typing.Self`)` is not assignable to attribute `tvar_tuple_index` on type `TypeAlias | None`
+ error[lint:invalid-assignment] mypy/fixup.py:86:29: Object of type `Unknown` is not assignable to attribute `tvar_tuple_index` on type `TypeAlias | None`
- error[lint:invalid-assignment] mypy/fixup.py:94:29: Object of type `@Todo(Support for `typing.Self`)` is not assignable to attribute `tvar_tuple_index` on type `TypeAlias | None`
+ error[lint:invalid-assignment] mypy/fixup.py:94:29: Object of type `Unknown` is not assignable to attribute `tvar_tuple_index` on type `TypeAlias | None`
- error[lint:invalid-assignment] mypy/semanal.py:2014:17: Object of type `@Todo(Support for `typing.Self`)` is not assignable to attribute `tvar_tuple_index` on type `TypeAlias | None`
+ error[lint:invalid-assignment] mypy/semanal.py:2014:17: Object of type `Unknown` is not assignable to attribute `tvar_tuple_index` on type `TypeAlias | None`

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- error[lint:invalid-argument-type] examples/contrib/mitmproxywrapper.py:164:59: Argument to this function is incorrect: Expected `(Any, @Todo(Support for `typing.TypeAlias`), @Todo(Support for `typing.Self`), /) -> BaseRequestHandler`, found `None`
+ error[lint:invalid-argument-type] examples/contrib/mitmproxywrapper.py:164:59: Argument to this function is incorrect: Expected `(Any, @Todo(Support for `typing.TypeAlias`), Unknown, /) -> BaseRequestHandler`, found `None`
+ error[lint:invalid-return-type] mitmproxy/proxy/mode_servers.py:122:16: Return type does not match returned value: Expected `Self`, found `ServerInstance[Unknown]`
+ error[lint:invalid-return-type] mitmproxy/proxy/mode_specs.py:134:16: Return type does not match returned value: Expected `Self`, found `ProxyMode`
- error[lint:invalid-argument-type] test/helper_tools/passive_close.py:23:40: Argument to this function is incorrect: Expected `(Any, @Todo(Support for `typing.TypeAlias`), @Todo(Support for `typing.Self`), /) -> BaseRequestHandler`, found `<class 'service'>`
+ error[lint:invalid-argument-type] test/helper_tools/passive_close.py:23:40: Argument to this function is incorrect: Expected `(Any, @Todo(Support for `typing.TypeAlias`), Unknown, /) -> BaseRequestHandler`, found `<class 'service'>`
- Found 2152 diagnostics
+ Found 2154 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
+ error[lint:invalid-argument-type] cloudinit/log/loggers.py:222:33: Argument to this function is incorrect: Expected `(...) -> LogRecord`, found `<class 'CloudInitLogRecord'>`
- error[lint:invalid-argument-type] tests/integration_tests/assets/echo_server.py:34:36: Argument to this function is incorrect: Expected `(Any, @Todo(Support for `typing.TypeAlias`), @Todo(Support for `typing.Self`), /) -> BaseRequestHandler`, found `<class 'Server'>`
+ error[lint:invalid-argument-type] tests/integration_tests/assets/echo_server.py:34:36: Argument to this function is incorrect: Expected `(Any, @Todo(Support for `typing.TypeAlias`), Unknown, /) -> BaseRequestHandler`, found `<class 'Server'>`
- Found 706 diagnostics
+ Found 707 diagnostics

pwndbg (https://github.com/pwndbg/pwndbg)
- error[lint:invalid-argument-type] pwndbg/aglib/godbg.py:606:54: Argument to this function is incorrect: Expected `int`, found `list | @Todo(Support for `typing.Self`)`
+ error[lint:invalid-argument-type] pwndbg/aglib/godbg.py:606:54: Argument to this function is incorrect: Expected `int`, found `list | Unknown`
- error[lint:invalid-argument-type] pwndbg/aglib/godbg.py:639:60: Argument to this function is incorrect: Expected `int`, found `list | @Todo(Support for `typing.Self`)`
+ error[lint:invalid-argument-type] pwndbg/aglib/godbg.py:639:60: Argument to this function is incorrect: Expected `int`, found `list | Unknown`

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
- error[lint:invalid-assignment] bson/timestamp.py:58:17: Object of type `timedelta` is not assignable to `datetime | int`
- warning[lint:possibly-unbound-attribute] bson/timestamp.py:59:40: Attribute `timetuple` on type `datetime | int` is possibly unbound
- Found 557 diagnostics
+ Found 555 diagnostics

pyodide (https://github.com/pyodide/pyodide)
- error[lint:invalid-argument-type] pyodide-build/pyodide_build/tests/test_pypi.py:183:35: Argument to this function is incorrect: Expected `(Any, @Todo(Support for `typing.TypeAlias`), @Todo(Support for `typing.Self`), /) -> BaseRequestHandler`, found `<class 'PathRequestHandler'>`
+ error[lint:invalid-argument-type] pyodide-build/pyodide_build/tests/test_pypi.py:183:35: Argument to this function is incorrect: Expected `(Any, @Todo(Support for `typing.TypeAlias`), Unknown, /) -> BaseRequestHandler`, found `<class 'PathRequestHandler'>`

static-frame (https://github.com/static-frame/static-frame)
- warning[lint:unused-ignore-comment] static_frame/core/index_hierarchy.py:2493:57: Unused blanket `type: ignore` directive
- warning[lint:unused-ignore-comment] static_frame/core/index_hierarchy.py:2501:64: Unused blanket `type: ignore` directive
- warning[lint:unused-ignore-comment] static_frame/core/index_hierarchy.py:2509:62: Unused blanket `type: ignore` directive
- warning[lint:unused-ignore-comment] static_frame/core/series.py:1707:40: Unused blanket `type: ignore` directive
- warning[lint:unused-ignore-comment] static_frame/core/series.py:1709:40: Unused blanket `type: ignore` directive
+ error[lint:invalid-argument-type] static_frame/test/unit/test_bus.py:2277:58: Argument to this function is incorrect: Expected `((...) -> IndexBase) | None`, found `<class 'IndexYearMonth'>`
+ error[lint:invalid-argument-type] static_frame/test/unit/test_quilt.py:1222:13: Argument to this function is incorrect: Expected `((...) -> IndexBase) | None`, found `<class 'IndexDate'>`
+ error[lint:invalid-argument-type] static_frame/test/unit/test_series.py:1487:17: Argument to this function is incorrect: Expected `((...) -> IndexBase) | None`, found `<class 'IndexYearMonth'>`
- Found 2625 diagnostics
+ Found 2623 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- error[lint:invalid-argument-type] benchmarks/bm/iast_fixtures/str_methods.py:1099:62: Argument to this function is incorrect: Expected `(Any, @Todo(Support for `typing.TypeAlias`), @Todo(Support for `typing.Self`), /) -> BaseRequestHandler`, found `<class 'WebServerHandler'>`
+ error[lint:invalid-argument-type] benchmarks/bm/iast_fixtures/str_methods.py:1099:62: Argument to this function is incorrect: Expected `(Any, @Todo(Support for `typing.TypeAlias`), Unknown, /) -> BaseRequestHandler`, found `<class 'WebServerHandler'>`
- error[lint:invalid-argument-type] tests/appsec/iast/fixtures/integration/http_config_server.py:51:40: Argument to this function is incorrect: Expected `(Any, @Todo(Support for `typing.TypeAlias`), @Todo(Support for `typing.Self`), /) -> BaseRequestHandler`, found `<class 'SimpleHandler'>`
+ error[lint:invalid-argument-type] tests/appsec/iast/fixtures/integration/http_config_server.py:51:40: Argument to this function is incorrect: Expected `(Any, @Todo(Support for `typing.TypeAlias`), Unknown, /) -> BaseRequestHandler`, found `<class 'SimpleHandler'>`
- error[lint:invalid-argument-type] tests/contrib/selenium/test_selenium_chrome.py:59:34: Argument to this function is incorrect: Expected `(Any, @Todo(Support for `typing.TypeAlias`), @Todo(Support for `typing.Self`), /) -> BaseRequestHandler`, found `<class 'SimpleHTTPRequestHandler'>`
+ error[lint:invalid-argument-type] tests/contrib/selenium/test_selenium_chrome.py:59:34: Argument to this function is incorrect: Expected `(Any, @Todo(Support for `typing.TypeAlias`), Unknown, /) -> BaseRequestHandler`, found `<class 'SimpleHTTPRequestHandler'>`
- error[lint:invalid-argument-type] tests/llmobs/conftest.py:210:43: Argument to this function is incorrect: Expected `(Any, @Todo(Support for `typing.TypeAlias`), @Todo(Support for `typing.Self`), /) -> BaseRequestHandler`, found `<class 'LLMObsServer'>`
+ error[lint:invalid-argument-type] tests/llmobs/conftest.py:210:43: Argument to this function is incorrect: Expected `(Any, @Todo(Support for `typing.TypeAlias`), Unknown, /) -> BaseRequestHandler`, found `<class 'LLMObsServer'>`

zulip (https://github.com/zulip/zulip)
- error[lint:invalid-assignment] analytics/lib/time_utils.py:27:9: Object of type `datetime | timedelta` is not assignable to `datetime`
- error[lint:invalid-argument-type] analytics/management/commands/populate_analytics_db.py:86:54: Argument to this function is incorrect: Expected `datetime | None`, found `timedelta`
- error[lint:invalid-argument-type] analytics/management/commands/populate_analytics_db.py:102:13: Argument to this function is incorrect: Expected `datetime | None`, found `timedelta`
- error[lint:invalid-argument-type] analytics/management/commands/populate_analytics_db.py:113:13: Argument to this function is incorrect: Expected `datetime | None`, found `timedelta`
- error[lint:invalid-argument-type] zerver/actions/user_settings.py:662:17: Argument to this function is incorrect: Expected `datetime`, found `Unknown | datetime | timedelta`
- error[lint:invalid-argument-type] zerver/lib/import_realm.py:1800:54: Argument to this function is incorrect: Expected `datetime`, found `timedelta`
- error[lint:unresolved-attribute] zerver/lib/retention.py:218:28: Type `timedelta` has no attribute `isoformat`
- error[lint:unresolved-attribute] zerver/lib/retention.py:256:28: Type `timedelta` has no attribute `isoformat`
- error[lint:invalid-argument-type] zerver/lib/streams.py:1753:63: Argument to this function is incorrect: Expected `datetime`, found `timedelta`
- error[lint:unresolved-attribute] zerver/management/commands/delete_old_unclaimed_attachments.py:78:71: Type `timedelta` has no attribute `isoformat`
- error[lint:invalid-argument-type] zerver/management/commands/enqueue_digest_emails.py:27:24: Argument to this function is incorrect: Expected `datetime`, found `timedelta`
- error[lint:invalid-assignment] zerver/tests/test_delete_unclaimed_attachments.py:26:13: Object of type `timedelta` is not assignable to `datetime | None`
- error[lint:unresolved-attribute] zerver/tests/test_digest.py:48:30: Type `timedelta` has no attribute `timetuple`
- error[lint:unresolved-attribute] zerver/tests/test_digest.py:185:30: Type `timedelta` has no attribute `timetuple`
- error[lint:unresolved-attribute] zerver/tests/test_digest.py:249:30: Type `timedelta` has no attribute `timetuple`
- error[lint:invalid-argument-type] zerver/tests/test_digest.py:306:51: Argument to this function is incorrect: Expected `datetime`, found `Unknown | timedelta`
- error[lint:invalid-argument-type] zerver/tests/test_digest.py:340:66: Argument to this function is incorrect: Expected `datetime`, found `timedelta`
- error[lint:invalid-argument-type] zerver/tests/test_digest.py:347:66: Argument to this function is incorrect: Expected `datetime`, found `timedelta`
- error[lint:invalid-argument-type] zerver/tests/test_digest.py:373:46: Argument to this function is incorrect: Expected `datetime`, found `timedelta`
- error[lint:invalid-argument-type] zerver/tests/test_digest.py:386:46: Argument to this function is incorrect: Expected `datetime`, found `timedelta`
- error[lint:invalid-argument-type] zerver/tests/test_digest.py:400:32: Argument to this function is incorrect: Expected `datetime`, found `timedelta`
- error[lint:invalid-argument-type] zerver/tests/test_digest.py:422:46: Argument to this function is incorrect: Expected `datetime`, found `timedelta`
- error[lint:invalid-argument-type] zerver/tests/test_digest.py:437:46: Argument to this function is incorrect: Expected `datetime`, found `timedelta`
- error[lint:invalid-argument-type] zerver/tests/test_digest.py:446:46: Argument to this function is incorrect: Expected `datetime`, found `timedelta`
- error[lint:invalid-argument-type] zerver/tests/test_digest.py:479:28: Argument to this function is incorrect: Expected `datetime`, found `timedelta`
- error[lint:invalid-argument-type] zerver/tests/test_digest.py:503:46: Argument to this function is incorrect: Expected `datetime`, found `timedelta`
- error[lint:invalid-argument-type] zerver/tests/test_digest.py:524:72: Argument to this function is incorrect: Expected `datetime`, found `timedelta`
- error[lint:invalid-argument-type] zerver/tests/test_digest.py:543:72: Argument to this function is incorrect: Expected `datetime`, found `timedelta`
- error[lint:invalid-argument-type] zerver/tests/test_digest.py:553:72: Argument to this function is incorrect: Expected `datetime`, found `timedelta`
- error[lint:invalid-argument-type] zerver/tests/test_digest.py:571:45: Argument to this function is incorrect: Expected `datetime`, found `timedelta`
- error[lint:unresolved-attribute] zerver/tests/test_digest.py:584:30: Type `timedelta` has no attribute `timetuple`
- error[lint:invalid-argument-type] zerver/tests/test_events.py:4614:47: Argument to this function is incorrect: Expected `datetime`, found `timedelta`
- error[lint:unresolved-attribute] zerver/tests/test_home.py:1121:43: Type `timedelta` has no attribute `timetuple`
- error[lint:invalid-argument-type] zerver/tests/test_message_move_stream.py:2023:37: Argument to this function is incorrect: Expected `datetime`, found `timedelta`
- error[lint:invalid-argument-type] zerver/tests/test_message_move_stream.py:2077:37: Argument to this function is incorrect: Expected `datetime`, found `timedelta`
- error[lint:invalid-argument-type] zerver/tests/test_message_send.py:1298:48: Argument to this function is incorrect: Expected `datetime`, found `timedelta`
- error[lint:invalid-argument-type] zerver/tests/test_message_send.py:1321:48: Argument to this function is incorrect: Expected `datetime`, found `timedelta`
- error[lint:invalid-argument-type] zerver/tests/test_presence.py:842:52: Argument to this function is incorrect: Expected `datetime`, found `timedelta`
- error[lint:invalid-argument-type] zerver/tests/test_presence.py:869:52: Argument to this function is incorrect: Expected `datetime`, found `timedelta`
- error[lint:invalid-argument-type] zerver/tests/test_presence.py:882:52: Argument to this function is incorrect: Expected `datetime`, found `timedelta`
- error[lint:invalid-argument-type] zerver/tests/test_presence.py:894:52: Argument to this function is incorrect: Expected `datetime`, found `timedelta`
- error[lint:invalid-argument-type] zerver/tests/test_presence.py:916:52: Argument to this function is incorrect: Expected `datetime`, found `timedelta`
- error[lint:invalid-argument-type] zerver/tests/test_presence.py:939:52: Argument to this function is incorrect: Expected `datetime`, found `timedelta`
- error[lint:invalid-argument-type] zerver/tests/test_presence.py:1174:49: Argument to this function is incorrect: Expected `datetime`, found `timedelta`
- error[lint:invalid-argument-type] zerver/tests/test_realm.py:1651:13: Argument to this function is incorrect: Expected `datetime | None`, found `timedelta`
- error[lint:invalid-argument-type] zerver/tests/test_subs.py:9333:51: Argument to this function is incorrect: Expected `datetime`, found `timedelta`
- error[lint:invalid-argument-type] zerver/tests/test_subs.py:9343:21: Argument to this function is incorrect: Expected `datetime`, found `timedelta`
- error[lint:invalid-argument-type] zerver/tests/test_subs.py:9350:21: Argument to this function is incorrect: Expected `datetime`, found `timedelta`
- error[lint:invalid-argument-type] zerver/tests/test_subs.py:9356:51: Argument to this function is incorrect: Expected `datetime`, found `timedelta`
- error[lint:invalid-argument-type] zerver/tests/test_subs.py:9362:51: Argument to this function is incorrect: Expected `datetime`, found `timedelta`
- error[lint:unresolved-attribute] zerver/views/digest.py:17:26: Type `timedelta` has no attribute `timetuple`
- error[lint:invalid-return-type] zilencer/management/commands/populate_db.py:1417:12: Return type does not match returned value: Expected `datetime`, found `timedelta`
- Found 4907 diagnostics
+ Found 4855 diagnostics

rotki (https://github.com/rotki/rotki)
- error[lint:unresolved-attribute] rotkehlchen/api/rest.py:1495:37: Type `timedelta` has no attribute `timestamp`
- Found 2063 diagnostics
+ Found 2062 diagnostics

jax (https://github.com/google/jax)
- error[lint:invalid-argument-type] jax/_src/profiler.py:197:54: Argument to this function is incorrect: Expected `(Any, @Todo(Support for `typing.TypeAlias`), @Todo(Support for `typing.Self`), /) -> BaseRequestHandler`, found `<class '_PerfettoServer'>`
+ error[lint:invalid-argument-type] jax/_src/profiler.py:197:54: Argument to this function is incorrect: Expected `(Any, @Todo(Support for `typing.TypeAlias`), Unknown, /) -> BaseRequestHandler`, found `<class '_PerfettoServer'>`
- error[lint:unsupported-operator] jax/experimental/jet.py:644:19: Operator `in` is not supported for types `@Todo(Support for `typing.Self`)` and `None`
+ error[lint:unsupported-operator] jax/experimental/jet.py:644:19: Operator `in` is not supported for types `Unknown` and `None`
+ warning[lint:redundant-cast] jax/experimental/mosaic/gpu/layout_inference.py:277:27: Value is already of type `Unknown`
- error[lint:unsupported-operator] jax/experimental/mosaic/gpu/utils.py:655:10: Operator `<` is not supported for types `_StartT_co` and `Literal[0]`, in comparing `(@Todo(Support for `typing.Self`) & _StartT_co & ~AlwaysFalsy) | @Todo(Support for `typing.Self`)` with `Literal[0]`
+ error[lint:unsupported-operator] jax/experimental/mosaic/gpu/utils.py:655:10: Operator `<` is not supported for types `_StartT_co` and `Literal[0]`, in comparing `(Unknown & _StartT_co & ~AlwaysFalsy) | Unknown` with `Literal[0]`
- error[lint:unsupported-operator] jax/experimental/mosaic/gpu/utils.py:658:42: Operator `<` is not supported for types `_StartT_co` and `Literal[0]`, in comparing `(@Todo(Support for `typing.Self`) & _StartT_co & ~AlwaysFalsy) | @Todo(Support for `typing.Self`)` with `Literal[0]`
+ error[lint:unsupported-operator] jax/experimental/mosaic/gpu/utils.py:658:42: Operator `<` is not supported for types `_StartT_co` and `Literal[0]`, in comparing `(Unknown & _StartT_co & ~AlwaysFalsy) | Unknown` with `Literal[0]`
- Found 3216 diagnostics
+ Found 3217 diagnostics

sympy (https://github.com/sympy/sympy)
+ error[lint:invalid-return-type] sympy/core/expr.py:74:43: Function can implicitly return `None`, which is not assignable to return type `Self`
- warning[lint:call-possibly-unbound-method] sympy/polys/rings.py:2571:38: Method `__getitem__` of type `None | @Todo(Support for `typing.Self`)` is possibly unbound
+ warning[lint:call-possibly-unbound-method] sympy/polys/rings.py:2571:38: Method `__getitem__` of type `None | Unknown` is possibly unbound
- warning[lint:call-possibly-unbound-method] sympy/polys/rings.py:2571:38: Method `__getitem__` of type `None | @Todo(Support for `typing.Self`)` is possibly unbound
+ warning[lint:call-possibly-unbound-method] sympy/polys/rings.py:2571:38: Method `__getitem__` of type `None | Unknown` is possibly unbound
- warning[lint:call-possibly-unbound-method] sympy/polys/rings.py:2571:38: Method `__getitem__` of type `None | @Todo(Support for `typing.Self`)` is possibly unbound
+ warning[lint:call-possibly-unbound-method] sympy/polys/rings.py:2571:38: Method `__getitem__` of type `None | Unknown` is possibly unbound
- error[lint:unsupported-operator] sympy/utilities/iterables.py:2014:12: Operator `not in` is not supported for types `@Todo(Support for `typing.Self`)` and `None`, in comparing `@Todo(Support for `typing.Self`)` with `Unknown | None | (Unknown & ~AlwaysFalsy) | list`
+ error[lint:unsupported-operator] sympy/utilities/iterables.py:2014:12: Operator `not in` is not supported for types `Unknown` and `None`, in comparing `Unknown` with `Unknown | None | (Unknown & ~AlwaysFalsy) | list`
- Found 17047 diagnostics
+ Found 17048 diagnostics

manticore (https://github.com/trailofbits/manticore)
- error[lint:invalid-argument-type] manticore/core/worker.py:342:64: Argument to this function is incorrect: Expected `(Any, @Todo(Support for `typing.TypeAlias`), @Todo(Support for `typing.Self`), /) -> BaseRequestHandler`, found `<class 'DumpTCPHandler'>`
+ error[lint:invalid-argument-type] manticore/core/worker.py:342:64: Argument to this function is incorrect: Expected `(Any, @Todo(Support for `typing.TypeAlias`), Unknown, /) -> BaseRequestHandler`, found `<class 'DumpTCPHandler'>`
- error[lint:invalid-argument-type] manticore/core/worker.py:405:64: Argument to this function is incorrect: Expected `(Any, @Todo(Support for `typing.TypeAlias`), @Todo(Support for `typing.Self`), /) -> BaseRequestHandler`, found `<class 'DumpTCPHandler'>`
+ error[lint:invalid-argument-type] manticore/core/worker.py:405:64: Argument to this function is incorrect: Expected `(Any, @Todo(Support for `typing.TypeAlias`), Unknown, /) -> BaseRequestHandler`, found `<class 'DumpTCPHandler'>`

```
</details>


---

_Label `red-knot` added by @AlexWaygood on 2025-04-28 21:59_

---

_Comment by @MichaReiser on 2025-05-03 17:59_

@Glyphack I went ahead and rebased your PR ontop the Red Knot renaming. Make sure to pull (force) before making new changes.

---

_Marked ready for review by @Glyphack on 2025-05-06 19:49_

---

_Review requested from @carljm by @Glyphack on 2025-05-06 19:49_

---

_Review requested from @AlexWaygood by @Glyphack on 2025-05-06 19:49_

---

_Review requested from @sharkdp by @Glyphack on 2025-05-06 19:49_

---

_Review requested from @dcreager by @Glyphack on 2025-05-06 19:49_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/annotations/self.md`:19 on 2025-05-07 00:13_

`Shape` looks like the instance type "all instances of class Shape", which is spelled the same way, so we can't spell the self-type that way also. I think we can just display it as `Self` -- this is basically what mypy and pyright both do, although mypy always appends a numeric identifier and pyright always appends the name of the scope in which the typevar is used.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/call/constructor.md`:81 on 2025-05-07 00:34_

```suggestion
    def __new__(cls, x: int) -> Self:
        return cls()
```

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/call/constructor.md`:57 on 2025-05-07 00:34_

```suggestion

```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:1106 on 2025-05-07 00:42_

If you're intending to leave this TODO for a follow-up PR, you'll need to remove a bit more of this code to make clippy happy; it doesn't like the unused `param` variable.

It seems to me that the way this should be handled is to assume the type `type[Self]` for the `cls` argument to `__new__` (if un-annotated), and similarly assume the type `Self` for the `self` argument, if un-annotated. If we do that, it seems like the existing constructor call specialization inference should handle this?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/semantic_index.rs`:437 on 2025-05-07 00:52_

I don't think this method should live on `SemanticIndex` -- it's a violation of our current layering expectations for the semantic index to know anything about types.

It could be a free function that takes the scope and semantic index?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:4646 on 2025-05-07 00:53_

I'm not sure that we need a new type here? We could just take a `ScopeId`, because a `ScopeId` includes a `File`, and from a file we can always cheaply get its semantic index via the `semantic_index` query. 

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:4790 on 2025-05-07 00:59_

I'm not sure we should make these assumptions -- and I don't think we need to? If we use "Self" as the name of the typevar, then we don't need `class.name(db)`, and I think we can just use `class_ty.to_instance(db)` to get the upper bound.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:4752 on 2025-05-07 01:00_

As mentioned above I think we can just use `Self` here -- if we need further disambiguation (like mypy and pyright provide) we would do that for all typevars.

---

_@carljm reviewed on 2025-05-07 01:03_

This is looking really good! A few minor fixups and I think it should be good to go. Would be awesome if we can get the second part (assuming `Self` for un-annotated `self` parameters) in soon as well!

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-05-07 10:49_

---

_@Glyphack reviewed on 2025-05-07 20:16_

---

_Review comment by @Glyphack on `crates/ty_python_semantic/src/types/class.rs`:1106 on 2025-05-07 20:16_

Thanks, I left this code just to show what I tried for adding the generic context. I will remove it before merging. There is a TODO already in the tests for this. I'll add it in my next PR.

So we should not need to add the generic context like I wanted to do in this place and just apply the logic for `cls` and `self` when inferring the parameters of a function?

---

_@Glyphack reviewed on 2025-05-07 20:19_

---

_Review comment by @Glyphack on `crates/ty_python_semantic/src/types.rs`:4790 on 2025-05-07 20:19_

I think I also need the definition of the class. Or maybe I'm doing something bad here? My reasoning for using the class definition as the definition of this type var was that the definition is related to that class.

---

_@Glyphack reviewed on 2025-05-07 20:48_

---

_Review comment by @Glyphack on `crates/ty_python_semantic/src/types.rs`:4796 on 2025-05-07 20:48_

This one was added after I rebased it should be invariant right?

---

_Comment by @Glyphack on 2025-05-07 20:52_

Thank you for the help. I start working on the second PR to recognize `self` parameter without type annotation.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:4796 on 2025-05-07 22:50_

I don't think it matters since `Self` will never be part of the generic context of a generic class; variance only matters when determining subtyping between generic classes. So yeah, invariant is a fine default.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:4788 on 2025-05-07 22:52_

I am not 100% sure that these debug-asserts will always remain true, in the face of e.g. class decorators. But we also haven't fully decided yet how to handle class decorators. I think it's fine to have these here for now; they'll at least force us to consider the impact here if we ever do violate these invariants.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:325 on 2025-05-07 22:54_

Orthogonal to this PR, but this should really be renamed to `enclosing_class_type` -- it returns a `Type`, not a `Symbol`.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:4788 on 2025-05-07 22:55_

I think another option here would be to split the `enclosing_class_symbol` function in half, and turn the first half of it into `enclosing_class_definition` instead, returning a `Definition`, and use a separate function to get the inferred type for that definition. Then you'd have the definition in hand already, and wouldn't have to pull it back out of the type.

---

_@carljm approved on 2025-05-07 22:56_

This looks good to me! I'm going to go ahead and land it; feel free to consider my comments for possible inclusion in a follow-up PR. None of them are critical.

---

_Merged by @carljm on 2025-05-07 22:58_

---

_Closed by @carljm on 2025-05-07 22:58_

---

_Branch deleted on 2025-05-08 06:48_

---

_Comment by @aldanor on 2025-05-09 20:14_

In Python <= 3.10, could using typing_extensions.Self be considered as well? (same as e.g. pyright does it)

---

_Comment by @carljm on 2025-05-09 20:26_

> In Python <= 3.10, could using typing_extensions.Self be considered as well? (same as e.g. pyright does it)

This was already supported in this PR. I add a test for it in https://github.com/astral-sh/ruff/pull/17995

---
