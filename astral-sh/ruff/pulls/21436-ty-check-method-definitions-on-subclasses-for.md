```yaml
number: 21436
title: "[ty] Check method definitions on subclasses for Liskov violations"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: alex/basic-liskov
created_at: 2025-11-13T20:03:16Z
updated_at: 2025-11-23T18:08:17Z
url: https://github.com/astral-sh/ruff/pull/21436
synced_at: 2026-01-10T16:48:01Z
```

# [ty] Check method definitions on subclasses for Liskov violations

---

_Pull request opened by @AlexWaygood on 2025-11-13 20:03_

## Summary

A basic implementation of Liskov assignability.

Currently we only check methods on subclasses: not attributes or properties. These should also obviously be checked as well, but they're deferred for now. Similarly, even for methods, there is much that is currently deferred: no checks are done for staticmethods or classmethods for now, and we only check definitions inferred as `Type::FunctionLiteral`s.

The main thing missing from the checks implemented here is that the diagnostics don't do a good job of explaining _why_ a particular override violates the Liskov Substitution Principle. Other type checkers do a great job at this: they'll tell you that the return annotation is invalid, or that you've changed a parameter name, etc. All of that is very difficult for us to implement until something like https://github.com/astral-sh/ruff/pull/19580 is implemented to propagate up information about _why_ an assignability or subtyping check failed. I think we'll definitely need something like that eventually, but I'm loath to do that until the new constraint solver has landed, since it'll touch the same area of code and cause a lot of merge conflicts!

Another thing to note (which looks like it comes up a lot in the ecosystem) is that other type checkers are less strict than us in some respects. For example, pyright ignores Liskov issues stemming from a parameter on a method override having the wrong name _if_ the method is a dunder method. We could consider pushing "parameter renaming issues" into a separate (disabled-by-default) error code, but once again this is very difficult to do without something like https://github.com/astral-sh/ruff/pull/19580.

## Test Plan

Mdtests and snapshots


---

_Label `ty` added by @AlexWaygood on 2025-11-13 20:03_

---

_Comment by @astral-sh-bot[bot] on 2025-11-13 20:05_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-11-23 17:50:26.942738321 +0000
+++ new-output.txt	2025-11-23 17:50:30.674772221 +0000
@@ -1,4 +1,4 @@
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/17bc55d/src/function/execute.rs:469:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(19ea3)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/17bc55d/src/function/execute.rs:469:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(1a6a3)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
@@ -868,6 +868,7 @@
 qualifiers_final_annotation.py:166:1: error[invalid-assignment] Reassignment of `Final` symbol `TEN` is not allowed: Reassignment of `Final` symbol
 qualifiers_final_decorator.py:21:16: error[subclass-of-final-class] Class `Derived1` cannot inherit from final class `Base1`
 qualifiers_final_decorator.py:89:9: error[invalid-overload] `@final` decorator should be applied only to the overload implementation
+qualifiers_final_decorator.py:118:9: error[invalid-method-override] Invalid override of method `method`: Definition is incompatible with `Base5_2.method`
 specialtypes_any.py:86:1: error[type-assertion-failure] Type `int` does not match asserted type `int | @Todo(instance attribute on class with dynamic base)`
 specialtypes_never.py:19:22: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Never`
 specialtypes_never.py:32:12: error[invalid-return-type] Return type does not match returned value: expected `int`, found `Literal["whatever works"]`
@@ -1001,5 +1002,5 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Unknown key "title" for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 1003 diagnostics
+Found 1004 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.

```

</details>




---

_Comment by @codspeed-hq[bot] on 2025-11-13 20:23_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex%2Fbasic-liskov?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21436 will **degrade performances by 6.73%**

<sub>Comparing <code>alex/basic-liskov</code> (1d6c816) with <code>main</code> (aec225d)</sub>



### Summary

`❌ 3 (👁 3)` regressions  
`✅ 19` untouched  
`⏩ 30` skipped[^skipped]  



### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| 👁 | Simulation | [`` anyio ``](https://codspeed.io/astral-sh/ruff/branches/alex%2Fbasic-liskov?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Aproject%3A%3Aanyio%3A%3Aproject%3A%3Aanyio&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 1.3 s | 1.4 s | -4.07% |
| 👁 | WallTime | [`` medium[static-frame] ``](https://codspeed.io/astral-sh/ruff/branches/alex%2Fbasic-liskov?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Amedium%5Bstatic-frame%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 16 s | 17.1 s | -6.73% |
| 👁 | WallTime | [`` small[tanjun] ``](https://codspeed.io/astral-sh/ruff/branches/alex%2Fbasic-liskov?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Asmall%5Btanjun%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 2.3 s | 2.5 s | -4.46% |
[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/alex%2Fbasic-liskov?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Comment by @astral-sh-bot[bot] on 2025-11-13 21:14_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
bidict (https://github.com/jab/bidict)
+ bidict/_bidict.py:126:9: error[invalid-method-override] Invalid override of method `pop`: Definition is incompatible with `MutableMapping.pop`
+ bidict/_bidict.py:151:9: error[invalid-method-override] Invalid override of method `update`: Definition is incompatible with `MutableMapping.update`
- Found 14 diagnostics
+ Found 16 diagnostics

more-itertools (https://github.com/more-itertools/more-itertools)
+ more_itertools/more.py:2278:9: error[invalid-method-override] Invalid override of method `__contains__`: Definition is incompatible with `Sequence.__contains__`
+ more_itertools/more.py:2303:9: error[invalid-method-override] Invalid override of method `__getitem__`: Definition is incompatible with `Sequence.__getitem__`
+ more_itertools/more.py:2383:9: error[invalid-method-override] Invalid override of method `index`: Definition is incompatible with `Sequence.index`
- more_itertools/more.pyi:516:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ more_itertools/more.pyi:501:9: error[invalid-method-override] Invalid override of method `__contains__`: Definition is incompatible with `Sequence.__contains__`
+ more_itertools/more.pyi:506:9: error[invalid-method-override] Invalid override of method `__getitem__`: Definition is incompatible with `Sequence.__getitem__`
- Found 29 diagnostics
+ Found 33 diagnostics

attrs (https://github.com/python-attrs/attrs)
+ tests/test_setattr.py:374:21: error[invalid-method-override] Invalid override of method `__setattr__`: Definition is incompatible with `Frozen.__setattr__`
- Found 595 diagnostics
+ Found 596 diagnostics

aioredis (https://github.com/aio-libs/aioredis)
+ aioredis/client.py:146:9: error[invalid-method-override] Invalid override of method `update`: Definition is incompatible with `MutableMapping.update`
- Found 28 diagnostics
+ Found 29 diagnostics

com2ann (https://github.com/ilevkivskyi/com2ann)
+ src/com2ann.py:155:9: error[invalid-method-override] Invalid override of method `visit_Assign`: Definition is incompatible with `NodeVisitor.visit_Assign`
+ src/com2ann.py:191:9: error[invalid-method-override] Invalid override of method `visit_For`: Definition is incompatible with `NodeVisitor.visit_For`
+ src/com2ann.py:194:9: error[invalid-method-override] Invalid override of method `visit_AsyncFor`: Definition is incompatible with `NodeVisitor.visit_AsyncFor`
+ src/com2ann.py:197:9: error[invalid-method-override] Invalid override of method `visit_With`: Definition is incompatible with `NodeVisitor.visit_With`
+ src/com2ann.py:200:9: error[invalid-method-override] Invalid override of method `visit_AsyncWith`: Definition is incompatible with `NodeVisitor.visit_AsyncWith`
+ src/com2ann.py:208:9: error[invalid-method-override] Invalid override of method `visit_FunctionDef`: Definition is incompatible with `NodeVisitor.visit_FunctionDef`
+ src/com2ann.py:211:9: error[invalid-method-override] Invalid override of method `visit_AsyncFunctionDef`: Definition is incompatible with `NodeVisitor.visit_AsyncFunctionDef`
- Found 3 diagnostics
+ Found 10 diagnostics

parso (https://github.com/davidhalter/parso)
+ parso/python/errors.py:557:9: error[invalid-method-override] Invalid override of method `is_issue`: Definition is incompatible with `Rule.is_issue`
+ parso/python/errors.py:569:9: error[invalid-method-override] Invalid override of method `is_issue`: Definition is incompatible with `Rule.is_issue`
+ parso/python/errors.py:582:9: error[invalid-method-override] Invalid override of method `is_issue`: Definition is incompatible with `Rule.is_issue`
+ parso/python/errors.py:604:9: error[invalid-method-override] Invalid override of method `get_node`: Definition is incompatible with `Rule.get_node`
+ parso/python/errors.py:607:9: error[invalid-method-override] Invalid override of method `is_issue`: Definition is incompatible with `Rule.is_issue`
+ parso/python/errors.py:617:9: error[invalid-method-override] Invalid override of method `is_issue`: Definition is incompatible with `Rule.is_issue`
+ parso/python/errors.py:631:9: error[invalid-method-override] Invalid override of method `is_issue`: Definition is incompatible with `Rule.is_issue`
+ parso/python/errors.py:664:9: error[invalid-method-override] Invalid override of method `is_issue`: Definition is incompatible with `Rule.is_issue`
+ parso/python/errors.py:680:9: error[invalid-method-override] Invalid override of method `is_issue`: Definition is incompatible with `Rule.is_issue`
+ parso/python/errors.py:692:9: error[invalid-method-override] Invalid override of method `get_node`: Definition is incompatible with `Rule.get_node`
+ parso/python/errors.py:695:9: error[invalid-method-override] Invalid override of method `is_issue`: Definition is incompatible with `Rule.is_issue`
+ parso/python/errors.py:995:9: error[invalid-method-override] Invalid override of method `is_issue`: Definition is incompatible with `Rule.is_issue`
+ parso/python/errors.py:1038:9: error[invalid-method-override] Invalid override of method `is_issue`: Definition is incompatible with `Rule.is_issue`
+ parso/python/errors.py:1232:9: error[invalid-method-override] Invalid override of method `is_issue`: Definition is incompatible with `Rule.is_issue`
+ parso/python/errors.py:1238:9: error[invalid-method-override] Invalid override of method `is_issue`: Definition is incompatible with `Rule.is_issue`
+ parso/python/errors.py:1247:9: error[invalid-method-override] Invalid override of method `is_issue`: Definition is incompatible with `Rule.is_issue`
+ parso/python/errors.py:1254:9: error[invalid-method-override] Invalid override of method `is_issue`: Definition is incompatible with `Rule.is_issue`
+ parso/python/errors.py:1265:9: error[invalid-method-override] Invalid override of method `is_issue`: Definition is incompatible with `Rule.is_issue`
+ parso/python/parser.py:101:9: error[invalid-method-override] Invalid override of method `convert_leaf`: Definition is incompatible with `BaseParser.convert_leaf`
+ parso/python/pep8.py:766:9: error[invalid-method-override] Invalid override of method `is_issue`: Definition is incompatible with `Rule.is_issue`
- Found 183 diagnostics
+ Found 203 diagnostics

nionutils (https://github.com/nion-software/nionutils)
+ nion/utils/Converter.py:112:9: error[invalid-method-override] Invalid override of method `convert_back`: Definition is incompatible with `ConverterLike.convert_back`
+ nion/utils/Converter.py:161:9: error[invalid-method-override] Invalid override of method `convert_back`: Definition is incompatible with `ConverterLike.convert_back`
+ nion/utils/Converter.py:172:9: error[invalid-method-override] Invalid override of method `convert_back`: Definition is incompatible with `ConverterLike.convert_back`
+ nion/utils/Converter.py:181:9: error[invalid-method-override] Invalid override of method `convert_back`: Definition is incompatible with `ConverterLike.convert_back`
+ nion/utils/Converter.py:192:9: error[invalid-method-override] Invalid override of method `convert_back`: Definition is incompatible with `ConverterLike.convert_back`
+ nion/utils/Converter.py:220:9: error[invalid-method-override] Invalid override of method `convert_back`: Definition is incompatible with `ConverterLike.convert_back`
+ nion/utils/Converter.py:254:9: error[invalid-method-override] Invalid override of method `convert_back`: Definition is incompatible with `ConverterLike.convert_back`
+ nion/utils/StructuredModel.py:256:9: error[invalid-method-override] Invalid override of method `__getitem__`: Definition is incompatible with `MutableSequence.__getitem__`
+ nion/utils/StructuredModel.py:259:9: error[invalid-method-override] Invalid override of method `__setitem__`: Definition is incompatible with `MutableSequence.__setitem__`
+ nion/utils/StructuredModel.py:262:9: error[invalid-method-override] Invalid override of method `__delitem__`: Definition is incompatible with `MutableSequence.__delitem__`
+ nion/utils/StructuredModel.py:265:9: error[invalid-method-override] Invalid override of method `__contains__`: Definition is incompatible with `Sequence.__contains__`
- Found 12 diagnostics
+ Found 23 diagnostics

kornia (https://github.com/kornia/kornia)
- kornia/constants.py:38:59: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/geometry/boxes.py:761:92: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/geometry/keypoints.py:214:37: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ kornia/geometry/vector.py:42:9: error[invalid-method-override] Invalid override of method `__getitem__`: Definition is incompatible with `TensorWrapper.__getitem__`
+ kornia/geometry/vector.py:120:9: error[invalid-method-override] Invalid override of method `__getitem__`: Definition is incompatible with `TensorWrapper.__getitem__`
- kornia/models/depth_estimation/base.py:33:94: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/models/detection/base.py:178:19: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/models/edge_detection/base.py:102:19: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/models/super_resolution/base.py:117:19: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 767 diagnostics
+ Found 762 diagnostics

spack (https://github.com/spack/spack)
+ lib/spack/spack/audit.py:125:9: error[invalid-method-override] Invalid override of method `__getitem__`: Definition is incompatible with `Sequence.__getitem__`
+ lib/spack/spack/builder.py:555:9: error[invalid-method-override] Invalid override of method `__getitem__`: Definition is incompatible with `Sequence.__getitem__`
+ lib/spack/spack/cmd/common/arguments.py:104:9: error[invalid-method-override] Invalid override of method `__call__`: Definition is incompatible with `Action.__call__`
+ lib/spack/spack/cmd/common/arguments.py:123:9: error[invalid-method-override] Invalid override of method `__call__`: Definition is incompatible with `Action.__call__`
+ lib/spack/spack/cmd/common/arguments.py:158:9: error[invalid-method-override] Invalid override of method `__call__`: Definition is incompatible with `Action.__call__`
+ lib/spack/spack/cmd/common/arguments.py:598:9: error[invalid-method-override] Invalid override of method `__call__`: Definition is incompatible with `Action.__call__`
+ lib/spack/spack/cmd/info.py:160:9: error[invalid-method-override] Invalid override of method `format_name`: Definition is incompatible with `Formatter.format_name`
+ lib/spack/spack/cmd/info.py:163:9: error[invalid-method-override] Invalid override of method `format_values`: Definition is incompatible with `Formatter.format_values`
+ lib/spack/spack/cmd/info.py:539:9: error[invalid-method-override] Invalid override of method `format_name`: Definition is incompatible with `Formatter.format_name`
+ lib/spack/spack/cmd/info.py:544:9: error[invalid-method-override] Invalid override of method `format_values`: Definition is incompatible with `Formatter.format_values`
+ lib/spack/spack/cmd/info.py:556:9: error[invalid-method-override] Invalid override of method `format_description`: Definition is incompatible with `Formatter.format_description`
+ lib/spack/spack/llnl/util/filesystem.py:1961:9: error[invalid-method-override] Invalid override of method `__getitem__`: Definition is incompatible with `Sequence.__getitem__`
+ lib/spack/spack/llnl/util/lang.py:967:9: error[invalid-method-override] Invalid override of method `__getitem__`: Definition is incompatible with `MutableSequence.__getitem__`
+ lib/spack/spack/llnl/util/lang.py:970:9: error[invalid-method-override] Invalid override of method `__setitem__`: Definition is incompatible with `MutableSequence.__setitem__`
+ lib/spack/spack/llnl/util/lang.py:973:9: error[invalid-method-override] Invalid override of method `__delitem__`: Definition is incompatible with `MutableSequence.__delitem__`
+ lib/spack/spack/llnl/util/lang.py:979:9: error[invalid-method-override] Invalid override of method `insert`: Definition is incompatible with `MutableSequence.insert`
+ lib/spack/spack/repo.py:524:9: error[invalid-method-override] Invalid override of method `needs_update`: Definition is incompatible with `Indexer.needs_update`
+ lib/spack/spack/report.py:167:9: error[invalid-method-override] Invalid override of method `succeed`: Definition is incompatible with `SpecRecord.succeed`
+ lib/spack/spack/reporters/cdash.py:273:9: error[invalid-method-override] Invalid override of method `build_report`: Definition is incompatible with `Reporter.build_report`
+ lib/spack/spack/reporters/cdash.py:362:9: error[invalid-method-override] Invalid override of method `test_report`: Definition is incompatible with `Reporter.test_report`
+ lib/spack/spack/reporters/cdash.py:396:9: error[invalid-method-override] Invalid override of method `concretization_report`: Definition is incompatible with `Reporter.concretization_report`
- lib/spack/spack/util/prefix.py:52:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ lib/spack/spack/variant.py:581:9: error[invalid-method-override] Invalid override of method `__getitem__`: Definition is incompatible with `Sequence.__getitem__`
+ lib/spack/spack/vendor/altgraph/GraphAlgo.py:165:9: error[invalid-method-override] Invalid override of method `setdefault`: Definition is incompatible with `MutableMapping.setdefault`
+ lib/spack/spack/vendor/jinja2/runtime.py:1033:9: error[invalid-method-override] Invalid override of method `__getattr__`: Definition is incompatible with `Undefined.__getattr__`
+ lib/spack/spack/vendor/markupsafe/__init__.py:83:9: error[invalid-method-override] Invalid override of method `__add__`: Definition is incompatible with `str.__add__`
+ lib/spack/spack/vendor/markupsafe/__init__.py:95:9: error[invalid-method-override] Invalid override of method `__mul__`: Definition is incompatible with `str.__mul__`
- lib/spack/spack/vendor/markupsafe/__init__.py:119:17: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/markupsafe/__init__.py:126:18: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/markupsafe/__init__.py:133:72: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ lib/spack/spack/vendor/markupsafe/__init__.py:101:5: error[invalid-method-override] Invalid override of method `__rmul__`: Definition is incompatible with `str.__rmul__`
+ lib/spack/spack/vendor/markupsafe/__init__.py:103:9: error[invalid-method-override] Invalid override of method `__mod__`: Definition is incompatible with `str.__mod__`
+ lib/spack/spack/vendor/markupsafe/__init__.py:114:9: error[invalid-method-override] Invalid override of method `join`: Definition is incompatible with `str.join`
+ lib/spack/spack/vendor/markupsafe/__init__.py:193:9: error[invalid-method-override] Invalid override of method `partition`: Definition is incompatible with `str.partition`
+ lib/spack/spack/vendor/markupsafe/__init__.py:198:9: error[invalid-method-override] Invalid override of method `rpartition`: Definition is incompatible with `str.rpartition`
+ lib/spack/spack/vendor/markupsafe/__init__.py:203:9: error[invalid-method-override] Invalid override of method `format`: Definition is incompatible with `str.format`
+ lib/spack/spack/vendor/pyrsistent/typing.pyi:100:9: error[invalid-method-override] Invalid override of method `__contains__`: Definition is incompatible with `AbstractSet.__contains__`
+ lib/spack/spack/vendor/ruamel/yaml/comments.py:554:9: error[invalid-method-override] Invalid override of method `insert`: Definition is incompatible with `MutableSequence.insert`
+ lib/spack/spack/vendor/ruamel/yaml/comments.py:563:9: error[invalid-method-override] Invalid override of method `extend`: Definition is incompatible with `MutableSequence.extend`
+ lib/spack/spack/vendor/ruamel/yaml/comments.py:739:9: error[invalid-method-override] Invalid override of method `__contains__`: Definition is incompatible with `AbstractSet.__contains__`
+ lib/spack/spack/vendor/ruamel/yaml/comments.py:758:9: error[invalid-method-override] Invalid override of method `__contains__`: Definition is incompatible with `AbstractSet.__contains__`
- lib/spack/spack/vendor/ruamel/yaml/constructor.py:1396:61: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ lib/spack/spack/vendor/ruamel/yaml/representer.py:967:9: error[invalid-method-override] Invalid override of method `represent_set`: Definition is incompatible with `SafeRepresenter.represent_set`
- Found 7924 diagnostics
+ Found 7957 diagnostics

pip (https://github.com/pypa/pip)
+ src/pip/_internal/network/auth.py:440:9: error[invalid-method-override] Invalid override of method `__call__`: Definition is incompatible with `AuthBase.__call__`
- src/pip/_vendor/cachecontrol/adapter.py:81:26: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/pip/_vendor/cachecontrol/heuristics.py:124:9: error[invalid-method-override] Invalid override of method `update_headers`: Definition is incompatible with `BaseHeuristic.update_headers`
+ src/pip/_vendor/cachecontrol/heuristics.py:156:9: error[invalid-method-override] Invalid override of method `warning`: Definition is incompatible with `BaseHeuristic.warning`
+ src/pip/_vendor/distlib/compat.py:970:13: error[invalid-method-override] Invalid override of method `__getitem__`: Definition is incompatible with `MutableSequence.__getitem__`
+ src/pip/_vendor/distlib/compat.py:982:13: error[invalid-method-override] Invalid override of method `pop`: Definition is incompatible with `MutableSequence.pop`
+ src/pip/_vendor/distlib/compat.py:994:13: error[invalid-method-override] Invalid override of method `__getitem__`: Definition is incompatible with `Sequence.__getitem__`
+ src/pip/_vendor/idna/codec.py:11:9: error[invalid-method-override] Invalid override of method `encode`: Definition is incompatible with `codecs.Codec.encode`
+ src/pip/_vendor/idna/codec.py:20:9: error[invalid-method-override] Invalid override of method `decode`: Definition is incompatible with `codecs.Codec.decode`
+ src/pip/_vendor/idna/codec.py:31:9: error[invalid-method-override] Invalid override of method `_buffer_encode`: Definition is incompatible with `BufferedIncrementalEncoder._buffer_encode`
+ src/pip/_vendor/idna/codec.py:65:9: error[invalid-method-override] Invalid override of method `_buffer_decode`: Definition is incompatible with `BufferedIncrementalDecoder._buffer_decode`
+ src/pip/_vendor/pkg_resources/__init__.py:2128:9: error[invalid-method-override] Invalid override of method `_has`: Definition is incompatible with `NullProvider._has`
+ src/pip/_vendor/pkg_resources/__init__.py:2132:9: error[invalid-method-override] Invalid override of method `_isdir`: Definition is incompatible with `NullProvider._isdir`
+ src/pip/_vendor/pkg_resources/__init__.py:2135:9: error[invalid-method-override] Invalid override of method `_listdir`: Definition is incompatible with `NullProvider._listdir`
+ src/pip/_vendor/pygments/lexer.py:784:9: error[invalid-method-override] Invalid override of method `get_tokens_unprocessed`: Definition is incompatible with `RegexLexer.get_tokens_unprocessed`
+ src/pip/_vendor/pygments/lexers/python.py:1189:9: error[invalid-method-override] Invalid override of method `get_tokens_unprocessed`: Definition is incompatible with `RegexLexer.get_tokens_unprocessed`
+ src/pip/_vendor/pygments/token.py:28:9: error[invalid-method-override] Invalid override of method `__contains__`: Definition is incompatible with `Sequence.__contains__`
+ src/pip/_vendor/requests/cookies.py:358:9: error[invalid-method-override] Invalid override of method `update`: Definition is incompatible with `MutableMapping.update`
- src/pip/_vendor/resolvelib/resolvers/resolution.py:568:19: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/pip/_vendor/rich/_null_file.py:39:9: error[invalid-method-override] Invalid override of method `writelines`: Definition is incompatible with `IO.writelines`
+ src/pip/_vendor/rich/_null_file.py:59:9: error[invalid-method-override] Invalid override of method `write`: Definition is incompatible with `IO.write`
- src/pip/_vendor/rich/progress.py:250:65: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/pip/_vendor/rich/progress.py:255:51: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/pip/_vendor/urllib3/connectionpool.py:535:9: error[invalid-method-override] Invalid override of method `urlopen`: Definition is incompatible with `RequestMethods.urlopen`
+ src/pip/_vendor/urllib3/contrib/appengine.py:131:9: error[invalid-method-override] Invalid override of method `urlopen`: Definition is incompatible with `RequestMethods.urlopen`
+ src/pip/_vendor/urllib3/contrib/ntlmpool.py:115:9: error[invalid-method-override] Invalid override of method `urlopen`: Definition is incompatible with `HTTPConnectionPool.urlopen`
+ src/pip/_vendor/urllib3/poolmanager.py:353:9: error[invalid-method-override] Invalid override of method `urlopen`: Definition is incompatible with `RequestMethods.urlopen`
+ src/pip/_vendor/urllib3/poolmanager.py:526:9: error[invalid-method-override] Invalid override of method `urlopen`: Definition is incompatible with `RequestMethods.urlopen`
- Found 583 diagnostics
+ Found 603 diagnostics

itsdangerous (https://github.com/pallets/itsdangerous)
- src/itsdangerous/timed.py:185:17: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/itsdangerous/timed.py:222:24: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 4 diagnostics
+ Found 2 diagnostics

asynq (https://github.com/quora/asynq)
+ asynq/debug.py:147:9: error[invalid-method-override] Invalid override of method `formatException`: Definition is incompatible with `Formatter.formatException`
+ asynq/debug.pyi:30:9: error[invalid-method-override] Invalid override of method `formatException`: Definition is incompatible with `Formatter.formatException`
- asynq/decorators.pyi:103:133: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ asynq/tests/test_contexts.py:38:9: error[invalid-method-override] Invalid override of method `__exit__`: Definition is incompatible with `AsyncContext.__exit__`
+ asynq/tools.py:352:9: error[invalid-method-override] Invalid override of method `asyncio`: Definition is incompatible with `PureAsyncDecorator.asyncio`
- Found 173 diagnostics
+ Found 176 diagnostics

isort (https://github.com/pycqa/isort)
- isort/io.py:69:58: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 34 diagnostics
+ Found 33 diagnostics

bandersnatch (https://github.com/pypa/bandersnatch)
+ src/bandersnatch_storage_plugins/s3.py:417:9: error[invalid-method-override] Invalid override of method `symlink`: Definition is incompatible with `Storage.symlink`
- src/bandersnatch_storage_plugins/swift.py:344:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/bandersnatch_storage_plugins/swift.py:402:22: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/bandersnatch_storage_plugins/swift.py:422:23: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/bandersnatch_storage_plugins/swift.py:947:9: error[invalid-method-override] Invalid override of method `symlink`: Definition is incompatible with `Storage.symlink`
- Found 104 diagnostics
+ Found 103 diagnostics

websockets (https://github.com/aaugustin/websockets)
+ src/websockets/datastructures.py:132:9: error[invalid-method-override] Invalid override of method `update`: Definition is incompatible with `MutableMapping.update`
- Found 40 diagnostics
+ Found 41 diagnostics

beartype (https://github.com/beartype/beartype)
- beartype/_check/metadata/hint/hintsmeta.py:311:58: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- beartype/_util/cache/pool/utilcachepoollistfixed.py:147:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ beartype/_util/cache/pool/utilcachepoollistfixed.py:251:9: error[invalid-method-override] Invalid override of method `append`: Definition is incompatible with `MutableSequence.append`
+ beartype/_util/cache/pool/utilcachepoollistfixed.py:261:9: error[invalid-method-override] Invalid override of method `extend`: Definition is incompatible with `MutableSequence.extend`
- beartype/claw/_importlib/_clawimpload.py:395:26: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ beartype/typing/_typingpep544.py:253:9: error[invalid-method-override] Invalid override of method `__instancecheck__`: Definition is incompatible with `ABCMeta.__instancecheck__`

werkzeug (https://github.com/pallets/werkzeug)
+ src/werkzeug/datastructures/accept.py:77:9: error[invalid-method-override] Invalid override of method `__getitem__`: Definition is incompatible with `MutableSequence.__getitem__`
- src/werkzeug/datastructures/accept.py:100:50: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/werkzeug/datastructures/accept.py:110:60: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/werkzeug/datastructures/etag.py:102:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/werkzeug/datastructures/headers.py:627:46: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/werkzeug/datastructures/mixins.py:302:18: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/werkzeug/datastructures/mixins.py:314:19: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/werkzeug/datastructures/structures.py:75:15: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/werkzeug/datastructures/structures.py:314:9: error[invalid-method-override] Invalid override of method `setdefault`: Definition is incompatible with `MutableMapping.setdefault`
- src/werkzeug/datastructures/structures.py:351:74: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/werkzeug/datastructures/structures.py:375:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/werkzeug/datastructures/structures.py:418:18: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/werkzeug/datastructures/structures.py:456:19: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/werkzeug/datastructures/structures.py:656:9: error[invalid-method-override] Invalid override of method `__setstate__`: Definition is incompatible with `MultiDict.__setstate__`
- src/werkzeug/datastructures/structures.py:674:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/werkzeug/datastructures/structures.py:680:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/werkzeug/datastructures/structures.py:683:74: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/werkzeug/datastructures/structures.py:744:18: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/werkzeug/datastructures/structures.py:866:15: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/werkzeug/datastructures/structures.py:900:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/werkzeug/datastructures/structures.py:922:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/werkzeug/datastructures/structures.py:910:9: error[invalid-method-override] Invalid override of method `items`: Definition is incompatible with `MultiDict.items`
+ src/werkzeug/datastructures/structures.py:1081:9: error[invalid-method-override] Invalid override of method `add`: Definition is incompatible with `MutableSet.add`
+ src/werkzeug/datastructures/structures.py:1085:9: error[invalid-method-override] Invalid override of method `remove`: Definition is incompatible with `MutableSet.remove`
+ src/werkzeug/datastructures/structures.py:1121:9: error[invalid-method-override] Invalid override of method `discard`: Definition is incompatible with `MutableSet.discard`
- src/werkzeug/datastructures/structures.py:1195:51: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/werkzeug/debug/console.py:159:64: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/werkzeug/serving.py:117:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/werkzeug/utils.py:153:60: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/werkzeug/utils.py:146:9: error[invalid-method-override] Invalid override of method `lookup`: Definition is incompatible with `_DictAccessorProperty.lookup`
- src/werkzeug/wsgi.py:520:54: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tests/test_routing.py:770:13: error[invalid-method-override] Invalid override of method `to_python`: Definition is incompatible with `BaseConverter.to_python`
+ tests/test_routing.py:774:13: error[invalid-method-override] Invalid override of method `to_url`: Definition is incompatible with `BaseConverter.to_url`
- Found 411 diagnostics
+ Found 398 diagnostics

twine (https://github.com/pypa/twine)
+ twine/auth.py:73:9: error[invalid-method-override] Invalid override of method `__call__`: Definition is incompatible with `AuthBase.__call__`
+ twine/commands/check.py:53:9: error[invalid-method-override] Invalid override of method `write`: Definition is incompatible with `IO.write`
- Found 8 diagnostics
+ Found 10 diagnostics

python-htmlgen (https://github.com/srittau/python-htmlgen)
+ test_htmlgen/document.py:33:9: error[invalid-method-override] Invalid override of method `add_stylesheet`: Definition is incompatible with `Head.add_stylesheet`
- Found 23 diagnostics
+ Found 24 diagnostics

paasta (https://github.com/yelp/paasta)
+ paasta_tools/cassandracluster_tools.py:127:9: error[invalid-method-override] Invalid override of method `validate`: Definition is incompatible with `LongRunningServiceConfig.validate`
+ paasta_tools/flink_tools.py:92:9: error[invalid-method-override] Invalid override of method `validate`: Definition is incompatible with `LongRunningServiceConfig.validate`
+ paasta_tools/flink_tools.py:115:9: error[invalid-method-override] Invalid override of method `get_pool`: Definition is incompatible with `InstanceConfig.get_pool`
+ paasta_tools/kafkacluster_tools.py:60:9: error[invalid-method-override] Invalid override of method `validate`: Definition is incompatible with `LongRunningServiceConfig.validate`
+ paasta_tools/kubernetes_tools.py:1971:9: error[invalid-method-override] Invalid override of method `get_autoscaled_instances`: Definition is incompatible with `LongRunningServiceConfig.get_autoscaled_instances`
+ paasta_tools/monkrelaycluster_tools.py:60:9: error[invalid-method-override] Invalid override of method `validate`: Definition is incompatible with `LongRunningServiceConfig.validate`
+ paasta_tools/nrtsearchservice_tools.py:60:9: error[invalid-method-override] Invalid override of method `validate`: Definition is incompatible with `LongRunningServiceConfig.validate`
+ paasta_tools/tron_tools.py:189:9: error[invalid-method-override] Invalid override of method `get_value`: Definition is incompatible with `Formatter.get_value`
+ paasta_tools/tron_tools.py:472:9: error[invalid-method-override] Invalid override of method `get_deploy_group`: Definition is incompatible with `InstanceConfig.get_deploy_group`
+ paasta_tools/tron_tools.py:671:9: error[invalid-method-override] Invalid override of method `validate`: Definition is incompatible with `InstanceConfig.validate`
+ paasta_tools/tron_tools.py:722:9: error[invalid-method-override] Invalid override of method `get_projected_sa_volumes`: Definition is incompatible with `InstanceConfig.get_projected_sa_volumes`
- Found 1101 diagnostics
+ Found 1112 diagnostics

kopf (https://github.com/nolar/kopf)
+ kopf/_cogs/structs/diffs.py:95:9: error[invalid-method-override] Invalid override of method `__getitem__`: Definition is incompatible with `Sequence.__getitem__`
- Found 260 diagnostics
+ Found 261 diagnostics

starlette (https://github.com/encode/starlette)
- starlette/datastructures.py:530:35: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- starlette/datastructures.py:533:37: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- starlette/datastructures.py:536:48: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tests/test_authentication.py:27:15: error[invalid-method-override] Invalid override of method `authenticate`: Definition is incompatible with `AuthenticationBackend.authenticate`
- Found 224 diagnostics
+ Found 222 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
+ src/graphql/pyutils/ref_set.py:38:9: error[invalid-method-override] Invalid override of method `__contains__`: Definition is incompatible with `AbstractSet.__contains__`
- src/graphql/pyutils/ref_map.py:63:37: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/graphql/pyutils/ref_map.py:67:39: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/graphql/pyutils/ref_map.py:71:48: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/graphql/pyutils/ref_map.py:75:76: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 531 diagnostics
+ Found 528 diagnostics

pytest (https://github.com/pytest-dev/pytest)
+ src/_pytest/_code/code.py:417:9: error[invalid-method-override] Invalid override of method `__getitem__`: Definition is incompatible with `MutableSequence.__getitem__`
+ src/_pytest/assertion/rewrite.py:854:9: error[invalid-method-override] Invalid override of method `visit_Assert`: Definition is incompatible with `NodeVisitor.visit_Assert`
+ src/_pytest/assertion/rewrite.py:975:9: error[invalid-method-override] Invalid override of method `visit_NamedExpr`: Definition is incompatible with `NodeVisitor.visit_NamedExpr`
+ src/_pytest/assertion/rewrite.py:987:9: error[invalid-method-override] Invalid override of method `visit_Name`: Definition is incompatible with `NodeVisitor.visit_Name`
+ src/_pytest/assertion/rewrite.py:997:9: error[invalid-method-override] Invalid override of method `visit_BoolOp`: Definition is incompatible with `NodeVisitor.visit_BoolOp`
+ src/_pytest/assertion/rewrite.py:1043:9: error[invalid-method-override] Invalid override of method `visit_UnaryOp`: Definition is incompatible with `NodeVisitor.visit_UnaryOp`
+ src/_pytest/assertion/rewrite.py:1049:9: error[invalid-method-override] Invalid override of method `visit_BinOp`: Definition is incompatible with `NodeVisitor.visit_BinOp`
+ src/_pytest/assertion/rewrite.py:1059:9: error[invalid-method-override] Invalid override of method `visit_Call`: Definition is incompatible with `NodeVisitor.visit_Call`
+ src/_pytest/assertion/rewrite.py:1092:9: error[invalid-method-override] Invalid override of method `visit_Starred`: Definition is incompatible with `NodeVisitor.visit_Starred`
+ src/_pytest/assertion/rewrite.py:1098:9: error[invalid-method-override] Invalid override of method `visit_Attribute`: Definition is incompatible with `NodeVisitor.visit_Attribute`
+ src/_pytest/assertion/rewrite.py:1110:9: error[invalid-method-override] Invalid override of method `visit_Compare`: Definition is incompatible with `NodeVisitor.visit_Compare`
+ src/_pytest/capture.py:217:9: error[invalid-method-override] Invalid override of method `write`: Definition is incompatible with `IO.write`
+ src/_pytest/capture.py:273:9: error[invalid-method-override] Invalid override of method `write`: Definition is incompatible with `IO.write`
+ src/_pytest/capture.py:276:9: error[invalid-method-override] Invalid override of method `writelines`: Definition is incompatible with `IO.writelines`
- src/_pytest/doctest.py:317:24: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/_pytest/mark/structures.py:495:13: error[invalid-method-override] Invalid override of method `__call__`: Definition is incompatible with `MarkDecorator.__call__`
- src/_pytest/mark/structures.py:498:24: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/_pytest/mark/structures.py:510:13: error[invalid-method-override] Invalid override of method `__call__`: Definition is incompatible with `MarkDecorator.__call__`
+ src/_pytest/mark/structures.py:542:13: error[invalid-method-override] Invalid override of method `__call__`: Definition is incompatible with `MarkDecorator.__call__`
- src/_pytest/mark/structures.py:555:63: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/_pytest/mark/structures.py:559:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/_pytest/mark/structures.py:670:18: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/_pytest/nodes.py:515:24: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/_pytest/python.py:1720:24: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/_pytest/recwarn.py:231:35: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/_pytest/reports.py:512:9: error[invalid-method-override] Invalid override of method `toterminal`: Definition is incompatible with `TerminalRepr.toterminal`
+ testing/test_assertion.py:854:17: error[invalid-method-override] Invalid override of method `__getitem__`: Definition is incompatible with `MutableSequence.__getitem__`
+ testing/test_assertion.py:860:17: error[invalid-method-override] Invalid override of method `__setitem__`: Definition is incompatible with `MutableSequence.__setitem__`
+ testing/test_assertion.py:863:17: error[invalid-method-override] Invalid override of method `__delitem__`: Definition is incompatible with `MutableSequence.__delitem__`
- Found 446 diagnostics
+ Found 459 diagnostics

alerta (https://github.com/alerta/alerta)
+ alerta/database/backends/mongodb/base.py:1513:9: error[invalid-method-override] Invalid override of method `get_customer_notes`: Definition is incompatible with `Database.get_customer_notes`
+ alerta/database/backends/postgres/base.py:554:9: error[invalid-method-override] Invalid override of method `get_topn_count`: Definition is incompatible with `Database.get_topn_count`
+ alerta/database/backends/postgres/base.py:580:9: error[invalid-method-override] Invalid override of method `get_topn_flapping`: Definition is incompatible with `Database.get_topn_flapping`
+ alerta/database/backends/postgres/base.py:606:9: error[invalid-method-override] Invalid override of method `get_topn_standing`: Definition is incompatible with `Database.get_topn_standing`
+ alerta/database/backends/postgres/base.py:1448:9: error[invalid-method-override] Invalid override of method `get_customer_notes`: Definition is incompatible with `Database.get_customer_notes`
+ tests/test_plugins.py:472:9: error[invalid-method-override] Invalid override of method `post_receive`: Definition is incompatible with `PluginBase.post_receive`
+ tests/test_plugins.py:476:9: error[invalid-method-override] Invalid override of method `status_change`: Definition is incompatible with `PluginBase.status_change`
- Found 548 diagnostics
+ Found 555 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- scrapy/exporters.py:373:66: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- scrapy/http/headers.py:34:18: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ scrapy/http/headers.py:76:9: error[invalid-method-override] Invalid override of method `get`: Definition is incompatible with `Mapping.get`
- scrapy/http/headers.py:103:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- scrapy/http/headers.py:106:46: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ scrapy/pipelines/__init__.py:40:9: error[invalid-method-override] Invalid override of method `_add_middleware`: Definition is incompatible with `MiddlewareManager._add_middleware`
- scrapy/settings/__init__.py:506:89: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ scrapy/utils/datatypes.py:82:9: error[invalid-method-override] Invalid override of method `get`: Definition is incompatible with `Mapping.get`
- scrapy/utils/datatypes.py:89:90: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ scrapy/utils/datatypes.py:115:9: error[invalid-method-override] Invalid override of method `__setitem__`: Definition is incompatible with `UserDict.__setitem__`
+ tests/test_downloader_handler_twisted_ftp.py:188:9: error[invalid-method-override] Invalid override of method `_get_factory`: Definition is incompatible with `TestFTPBase._get_factory`
+ tests/test_extension_statsmailer.py:21:13: error[invalid-method-override] Invalid override of method `get_stats`: Definition is incompatible with `StatsCollector.get_stats`
+ tests/test_feedexport.py:846:9: error[invalid-method-override] Invalid override of method `export_item`: Definition is incompatible with `JsonItemExporter.export_item`
+ tests/test_spidermiddleware_referer.py:690:9: error[invalid-method-override] Invalid override of method `referrer`: Definition is incompatible with `ReferrerPolicy.referrer`
- tests/test_spidermiddleware_referer.py:1025:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 1726 diagnostics
+ Found 1727 diagnostics

pylint (https://github.com/pycqa/pylint)
+ pylint/checkers/exceptions.py:274:9: error[invalid-method-override] Invalid override of method `visit_default`: Definition is incompatible with `BaseVisitor.visit_default`
- Found 214 diagnostics
+ Found 215 diagnostics

sockeye (https://github.com/awslabs/sockeye)
+ sockeye/arguments.py:91:9: error[invalid-method-override] Invalid override of method `__call__`: Definition is incompatible with `Action.__call__`
+ sockeye/lexicon.py:260:9: error[invalid-method-override] Invalid override of method `get_blocked_trg_ids`: Definition is incompatible with `RestrictLexicon.get_blocked_trg_ids`
+ sockeye/lexicon.py:307:9: error[invalid-method-override] Invalid override of method `get_allowed_trg_ids`: Definition is incompatible with `RestrictLexicon.get_allowed_trg_ids`
+ sockeye/loss.py:326:9: error[invalid-method-override] Invalid override of method `update`: Definition is incompatible with `LossMetric.update`
- Found 409 diagnostics
+ Found 413 diagnostics

httpx-caching (https://github.com/johtso/httpx-caching)
+ tests/_async/test_expires_heuristics.py:30:17: error[invalid-method-override] Invalid override of method `update_headers`: Definition is incompatible with `BaseHeuristic.update_headers`
+ tests/_async/test_expires_heuristics.py:46:17: error[invalid-method-override] Invalid override of method `update_headers`: Definition is incompatible with `BaseHeuristic.update_headers`
- Found 25 diagnostics
+ Found 27 diagnostics

rich (https://github.com/Textualize/rich)
+ rich/_null_file.py:39:9: error[invalid-method-override] Invalid override of method `writelines`: Definition is incompatible with `IO.writelines`
+ rich/_null_file.py:59:9: error[invalid-method-override] Invalid override of method `write`: Definition is incompatible with `IO.write`
- rich/progress.py:250:65: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rich/progress.py:255:51: warning[unused-ignore-comment] Unused blanket `type: ignore` directive

ignite (https://github.com/pytorch/ignite)
- ignite/handlers/checkpoint.py:1044:60: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- ignite/handlers/param_scheduler.py:1677:78: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- ignite/handlers/tqdm_logger.py:174:18: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- ignite/metrics/frequency.py:92:18: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- ignite/metrics/gpu_info.py:103:18: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- ignite/metrics/precision_recall_curve.py:96:76: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- ignite/metrics/roc_auc.py:185:76: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tests/ignite/engine/test_event_handlers.py:16:9: error[invalid-method-override] Invalid override of method `run`: Definition is incompatible with `Engine.run`
+ tests/ignite/metrics/test_metric.py:586:13: error[invalid-method-override] Invalid override of method `update`: Definition is incompatible with `Metric.update`
- Found 2129 diagnostics
+ Found 2124 diagnostics

dedupe (https://github.com/dedupeio/dedupe)
- dedupe/levenshtein.py:16:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- dedupe/levenshtein.py:21:43: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- dedupe/levenshtein.py:29:67: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- dedupe/predicates.py:370:64: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ dedupe/serializer.py:28:9: error[invalid-method-override] Invalid override of method `encode`: Definition is incompatible with `JSONEncoder.encode`
+ dedupe/serializer.py:31:9: error[invalid-method-override] Invalid override of method `iterencode`: Definition is incompatible with `JSONEncoder.iterencode`
+ dedupe/serializer.py:34:9: error[invalid-method-override] Invalid override of method `default`: Definition is incompatible with `JSONEncoder.default`
- Found 53 diagnostics
+ Found 52 diagnostics

dulwich (https://github.com/dulwich/dulwich)
- dulwich/cli.py:628:68: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- dulwich/cli.py:638:85: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ dulwich/cli.py:776:9: error[invalid-method-override] Invalid override of method `write`: Definition is incompatible with `IO.write`
+ dulwich/cli.py:842:9: error[invalid-method-override] Invalid override of method `writelines`: Definition is incompatible with `IO.writelines`
+ dulwich/cli.py:1122:9: error[invalid-method-override] Invalid override of method `run`: Definition is incompatible with `Command.run`
+ dulwich/cli.py:1143:9: error[invalid-method-override] Invalid override of method `run`: Definition is incompatible with `Command.run`
+ dulwich/cli.py:1167:9: error[invalid-method-override] Invalid override of method `run`: Definition is incompatible with `Command.run`
+ dulwich/cli.py:1180:9: error[invalid-method-override] Invalid override of method `run`: Definition is incompatible with `Command.run`
+ dulwich/cli.py:1199:9: error[invalid-method-override] Invalid override of method `run`: Definition is incompatible with `Command.run`
+ dulwich/cli.py:1222:9: error[invalid-method-override] Invalid override of method `run`: Definition is incompatible with `Command.run`
+ dulwich/cli.py:2308:9: error[invalid-method-override] Invalid override of method `run`: Definition is incompatible with `Command.run`
+ dulwich/cli.py:2327:9: error[invalid-method-override] Invalid override of method `run`: Definition is incompatible with `Command.run`
+ dulwich/cli.py:2372:9: error[invalid-method-override] Invalid override of method `run`: Definition is incompatible with `Command.run`
+ dulwich/cli.py:3487:9: error[invalid-method-override] Invalid override of method `run`: Definition is incompatible with `Command.run`
+ dulwich/cli.py:3565:9: error[invalid-method-override] Invalid override of method `run`: Definition is incompatible with `Command.run`
+ dulwich/cli.py:3580:9: error[invalid-method-override] Invalid override of method `run`: Definition is incompatible with `Command.run`
+ dulwich/cli.py:3594:9: error[invalid-method-override] Invalid override of method `run`: Definition is incompatible with `Command.run`
+ dulwich/cli.py:3611:9: error[invalid-method-override] Invalid override of method `run`: Definition is incompatible with `Command.run`
+ dulwich/cli.py:5525:9: error[invalid-method-override] Invalid override of method `run`: Definition is incompatible with `Command.run`
+ dulwich/client.py:2110:9: error[invalid-method-override] Invalid override of method `_connect`: Definition is incompatible with `TraditionalGitClient._connect`
+ dulwich/config.py:251:17: error[invalid-method-override] Invalid override of method `__contains__`: Definition is incompatible with `AbstractSet.__contains__`
+ dulwich/config.py:278:17: error[invalid-method-override] Invalid override of method `__contains__`: Definition is incompatible with `AbstractSet.__contains__`
+ dulwich/contrib/swift.py:1098:9: error[invalid-method-override] Invalid override of method `_put_named_file`: Definition is incompatible with `BaseRepo._put_named_file`
- dulwich/diff.py:570:52: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- dulwich/diff.py:591:69: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ dulwich/dumb.py:254:9: error[invalid-method-override] Invalid override of method `get_raw`: Definition is incompatible with `BaseObjectStore.get_raw`
+ dulwich/dumb.py:287:9: error[invalid-method-override] Invalid override of method `__contains__`: Definition is incompatible with `BaseObjectStore.__contains__`
- dulwich/file.py:316:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- dulwich/file.py:327:64: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ dulwich/hooks.py:192:9: error[invalid-method-override] Invalid override of method `execute`: Definition is inc

... (truncated 4009 lines) ...
```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
-     memo metadata = ~31MB
+     memo metadata = ~33MB

sphinx (https://github.com/sphinx-doc/sphinx)
- TOTAL MEMORY USAGE: ~273MB
+ TOTAL MEMORY USAGE: ~287MB
-     struct metadata = ~18MB
+     struct metadata = ~20MB
-     struct fields = ~18MB
+     struct fields = ~20MB
-     memo metadata = ~66MB
+     memo metadata = ~73MB

prefect (https://github.com/PrefectHQ/prefect)
- TOTAL MEMORY USAGE: ~596MB
+ TOTAL MEMORY USAGE: ~626MB
-     struct metadata = ~38MB
+     struct metadata = ~40MB
-     struct fields = ~42MB
+     struct fields = ~44MB
-     memo metadata = ~131MB
+     memo metadata = ~138MB


```

</details>




---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-11-13 22:28_

---

_Comment by @astral-sh-bot[bot] on 2025-11-13 22:35_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-method-override` | 3,484 | 0 | 0 |
| `unused-ignore-comment` | 0 | 499 | 0 |
| **Total** | **3,484** | **499** | **0** |

**[Full report with detailed diff](https://alex-basic-liskov.ecosystem-663.pages.dev/diff)** ([timing results](https://alex-basic-liskov.ecosystem-663.pages.dev/timing))




---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-11-14 19:05_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-11-14 19:05_

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-11-14 19:06_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-11-14 19:06_

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-11-14 23:22_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-11-14 23:22_

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-11-14 23:27_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-11-14 23:27_

---

_@AlexWaygood reviewed on 2025-11-17 18:56_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:554 on 2025-11-17 18:56_

This is an attempt to reduce the performance hit from this PR. A few regressions went down a few percentage points as a result of this change, but I wouldn't say this made a huge impact on perf -- it seemed to improve things fairly significantly in terms of memory usage, however.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:3847 on 2025-11-17 18:58_

(this method used to be called `qualified_name_components`, it used to be a method on `ClassLiteral`, and it used to be in `display.rs`)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/display.rs`:515 on 2025-11-17 18:58_

(moved to `class.rs` for better reusability)

---

_@AlexWaygood reviewed on 2025-11-17 18:59_

---

_Marked ready for review by @AlexWaygood on 2025-11-17 19:05_

---

_Review requested from @carljm by @AlexWaygood on 2025-11-17 19:05_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-11-17 19:05_

---

_Review requested from @dcreager by @AlexWaygood on 2025-11-17 19:05_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-11-17 19:05_

---

_Comment by @carljm on 2025-11-18 02:33_

Not sure what's up with the ConstraintSet mdtests failing only on MacOS?

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/attributes.md`:1907 on 2025-11-18 02:48_

Not necessary to decide in this PR, but I wonder if we should special-case ignoring `object.__setattr__` for Liskov purposes (and separately implement validation of a correct `__setattr__` signature). TBH I'm not even sure why typeshed has `object.__setattr__`, and I don't think there is any soundness need for subclasses of `object` to respect Liskov compatibility with the `object.__setattr__` definition in typeshed.

---

_Comment by @AlexWaygood on 2025-11-18 14:11_

> Not sure what's up with the ConstraintSet mdtests failing only on MacOS?

I have no idea. I haven't touched any code to do with constraint sets in this PR, nor have I added any MacOS-specific code! The test also passes locally for me on my MacBook, but seems to be failing very persistently in CI on this PR.

@dcreager, any idea why this PR might be causing the test to start failing here...? The only significant way I think that this PR could impact other areas of ty is that we have to fetch the type for all attributes defined in the body of every class we check (so we're doing a lot more types in every file we check).

---

_Comment by @MichaReiser on 2025-11-18 15:09_

Do we iterate over hash maps or rely on salsa ids for ordering?

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/liskov.md`:11 on 2025-11-18 18:20_

https://youtube.com/clip/UgkxowksZQfdnOH7rL3v9FItjHWy72y6AhrX?si=n5rja_tvodOpIM3I

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/liskov.md`:21 on 2025-11-18 18:20_

```suggestion
    resolves to type `str`, it can only be overridden on a subclass with exactly the same type.
```

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/liskov.md`:56 on 2025-11-18 18:21_

```suggestion
    def method(self, x: object, /): ...  # fine: `method` still accepts any argument of type `int`
```

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/liskov.md`:90 on 2025-11-18 18:22_

```suggestion
    # (the method can no longer be passed exactly one argument!)
```

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/liskov.md`:107 on 2025-11-18 18:25_

At this point it feels a bit arbitrary that we've stopped explaining the errors...

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/liskov.md`:143 on 2025-11-18 18:27_

Not that it needs to be specifically mentioned here, but I think the strongest case for this is gradual types, because it means you can have cases where each individual parent-child relationship passes Liskov checking, but the grandparent-child relationship does not:

```py
class Grandparent:
    x: int

class Parent(Grandparent):
    x: Any

class Child(Parent):
    x: str
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/liskov.rs`:25 on 2025-11-18 18:56_

I think it would be good to add some tests for method overrides involving generics. The behavior _should_ fall out from callable subtyping, but it wouldn't hurt to have some such tests for overrides specifically.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/liskov.rs`:15 on 2025-11-18 18:57_

I think this PR should idealy move this stuff out of `ide_support`? It's clearly no longer just for IDE support.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/liskov.rs`:82 on 2025-11-18 19:02_

Is there an `own_instance_member` method we could use here instead, to short-circuit this "find and then skip if not in our own scope" dance?

It feels odd (and potentially inefficient) that if we have a long inheritance chain of length N, and a method defined at the top of it (say on `object`) and then overridden at the leaf, we will do `O(N^2)` mro traversal, as we (wastefully) traverse the full MRO of every intervening type in the inheritance chain, only to find the same super-class method on `object` every time, and then skip it because it's not defined in our own scope.

Or alternatively, if we can't use an `own_instance_member`, what about checking the place table before doing the `member` lookup?

The `break` below gives a false sense of efficiency. If the method doesn't exist anywhere in the MRO, we will only traverse the MRO once, which is good -- but the same would be true if we did the "outer" loop once and did `own_instance_member` for each type in the MRO. When the method does exist somewhere up the MRO, this way does a lot more traversal.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/liskov.rs`:107 on 2025-11-18 19:04_

Arguably in this case we might ideally show both ranges, since you'd want to know a) what is the actual signature of the function? as well as b) why is this function in my MRO?

---

_@carljm reviewed on 2025-11-18 19:05_

Nice!

---

_@AlexWaygood reviewed on 2025-11-18 19:09_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/liskov.rs`:107 on 2025-11-18 19:09_

I think I came to the same realisation and actually pushed this change before you posted this comment :-)

---

_@AlexWaygood reviewed on 2025-11-18 19:10_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/liskov.md`:11 on 2025-11-18 19:10_

oh no

---

_@AlexWaygood reviewed on 2025-11-18 19:12_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/liskov.md`:11 on 2025-11-18 19:12_

```suggestion
several checks for a type checker to perform when it checks a subclass `B` of a class `A`:
```

---

_@carljm reviewed on 2025-11-18 19:13_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/liskov.md`:11 on 2025-11-18 19:13_

"Among the checks a type checker will perform"

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/attributes.md`:1907 on 2025-11-18 19:15_

Yes, I think you're right.

I was also wondering about special-case ignoring `object.__hash__`. It's obviously common to define unhashable classes in Python, and it's often the correct thing to do to add `__hash__ = None` if your class is mutable. Having a type checker yell at you every time you have to do that has never felt helpful to me.

I can propose them both (either together or separately) as followups. I'd rather keep them out of this PR, since either special case (while, IMO, well-motivated) would be a deviation from the behaviour of other type checkers.

---

_@AlexWaygood reviewed on 2025-11-18 19:15_

---

_@AlexWaygood reviewed on 2025-11-18 19:22_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/liskov.rs`:82 on 2025-11-18 19:22_

> Is there an `own_instance_member` method we could use here instead, to short-circuit this "find and then skip if not in our own scope" dance?

There is a method by that name, yes. But it emulates looking up an attribute directly in `self.__dict__` (it does not include class attributes defined directly in the class body). We'd have to call `own_class_member` as well as `own_instance_member` to do this correctly.

But looking at this again, this would have the advantage that it would include synthesized members, since `own_class_member` calls `own_synthesized_member`. I think we're currently incorrectly skipping over synthesized members (from dataclasses/namedtuples/etc.) on superclasses, because they don't appear in the symbol table for those superclasses.

So your suggestion probably brings correctness benefits as well as performance benefits. Thanks!

---

_@carljm reviewed on 2025-11-18 19:24_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/liskov.rs`:82 on 2025-11-18 19:24_

`Type::member` is Salsa-cached, but I think that doesn't help here as much as we'd hope? The internal own-member stuff that is done for each class in the MRO is not cached, so we are still doing the `N^2` walk -- but we cache the final lookup result for each class in the MRO. Which is useful if that attribute is later looked up on that superclass, but still wasted if not.

---

_Comment by @dcreager on 2025-11-19 13:30_

> @dcreager, any idea why this PR might be causing the test to start failing here...? The only significant way I think that this PR could impact other areas of ty is that we have to fetch the type for all attributes defined in the body of every class we check (so we're doing a lot more types in every file we check).

It might cause the typevar ordering to be different, which would cause the internal BDD structure to change. I'm seeing this on another PR, too, so I will try to push up a separate fix for it.

---

_Comment by @dcreager on 2025-11-19 14:01_

> It might cause the typevar ordering to be different, which would cause the internal BDD structure to change. I'm seeing this on another PR, too, so I will try to push up a separate fix for it.

https://github.com/astral-sh/ruff/pull/21524 fixes the macos CI failures I was getting on https://github.com/astral-sh/ruff/pull/21414. I was getting a slightly different test failure than you, but my hope is that they had the same underlying cause. You can cherry-pick that commit if you want to test soon, or wait till that lands on `main`.

---

_@AlexWaygood reviewed on 2025-11-20 21:29_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/liskov.md`:143 on 2025-11-20 21:29_

I'm adding this as an example, I think it's useful!

---

_@AlexWaygood reviewed on 2025-11-20 23:44_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/liskov.rs`:25 on 2025-11-20 23:44_

great point. Added lots more tests in https://github.com/astral-sh/ruff/pull/21436/commits/8c03f3f0ea3b631e6791dbcd0904c1d54703f53b.

---

_@AlexWaygood reviewed on 2025-11-20 23:46_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/liskov.rs`:15 on 2025-11-20 23:46_

I do agree, but I'm a bit loath to do that in this PR because it'll dramatically increase the merge conflicts (and because this PR is already a big PR). I'd prefer to do that as an immediate followup, if that's okay

---

_@AlexWaygood reviewed on 2025-11-20 23:54_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/liskov.rs`:82 on 2025-11-20 23:54_

Okay, I've tried to optimise this a bit. I'm a bit reticent to call `own_class_member` directly, because (at least for now) we only care about the type accessed on instances of the class. To get the class-member-as-viewed-by-instances, you'd really need to call `invoke_descriptor_protocol` (which invokes `own_class_member`) rather than `own_class_member` directly, and calling `Type::invoke_descriptor_protocol` undermines any optimisation effect we'd get from avoiding calling `Type::member()` on the nominal-instance type (it'll recurse up the class hierarchy until it finds a class that has the attribute).

For now I've moved the `place_table` check above the `Type::member` call, so that we check whether it's defined in the class body before we do the `Type::member` call. But that won't work in the future when we extend Liskov checking to also check implicit instance attributes (etc.)

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-11-20 23:55_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-11-20 23:55_

---

_Review requested from @carljm by @AlexWaygood on 2025-11-20 23:56_

---

_@AlexWaygood reviewed on 2025-11-21 00:05_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/liskov.rs`:82 on 2025-11-21 00:05_

The good news is that the perf regression on the latest version of this PR is indeed much reduced -- now at <7%!

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/diagnostic.rs`:1946 on 2025-11-21 17:06_

```suggestion
    /// The LSP states that an instance of a subtype should be substitutable for an instance of its supertype.
```

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/liskov.md`:222 on 2025-11-21 17:17_

```suggestion
    # `str` is not necessarily a supertype of `T`!
```

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/liskov.md`:468 on 2025-11-21 17:21_

As I said in Discord, I think that this case falls outside the definition of Liskov that is within the scope of a type-checker to validate, so I would not even list it here.

The scope of Liskov checking for a type checker is if the types of methods or attributes are incompatible with a base class; it's based on type signatures, not arbitrary behavior of the method. If the subclass does not override the method, there cannot be a Liskov violation of incompatible method signature.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/liskov.md`:501 on 2025-11-21 17:25_

I don't think we should emit any diagnostic here; this is not a type error. If anything, we could consider a future type-aware lint rule that should have a distinct code and severity, if it seems important in practice to alert people to this runtime behavior that is outside the type system. But I don't think this should be a TODO here.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/liskov.rs`:116 on 2025-11-21 17:30_

It's not valid to define methods on a `TypedDict` (or subclass of a `TypedDict`) at all -- we should emit a diagnostic on that (separate PR). Seems like it then shouldn't be possible to hit this case (until we are checking non-methods on the subclass).

---

_@carljm approved on 2025-11-21 17:31_

Nice!

There were some comments from previous review accidentlly left not addressed; I just unresolved those prior comments rather than adding new ones.

---

_Comment by @MichaReiser on 2025-11-21 17:33_

Ohh, I would have needed this feature today when working on ty_benchmark where claude messed up the overrides... 

---

_Merged by @AlexWaygood on 2025-11-23 18:08_

---

_Closed by @AlexWaygood on 2025-11-23 18:08_

---

_Branch deleted on 2025-11-23 18:08_

---
