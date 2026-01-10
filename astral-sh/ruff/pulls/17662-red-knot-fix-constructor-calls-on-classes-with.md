```yaml
number: 17662
title: "[red-knot] Fix constructor calls on classes with metaclasses "
type: pull_request
state: closed
author: mishamsk
labels:
  - ty
assignees: []
base: main
head: 17462-fix-constructor-calls-on-classes-with-metaclasses
created_at: 2025-04-28T01:50:44Z
updated_at: 2025-04-30T15:27:34Z
url: https://github.com/astral-sh/ruff/pull/17662
synced_at: 2026-01-10T19:03:00Z
```

# [red-knot] Fix constructor calls on classes with metaclasses 

---

_Pull request opened by @mishamsk on 2025-04-28 01:50_

## Summary

Fixes #17462

Notes:

* We can't get rid of META_CLASS_NO_TYPE_FALLBACK as it is necessary to figure out if a given meta-class has an explicit `__call__` method defined, not inherited from type. This is used in `into_callable` implementation
* I decided that it isn't worth it to create a separate `NO_META_CLASS_FALLBACK` policy flag, and instead hard-code the logic into descriptor protocol, that if `MRO_NO_OBJECT_FALLBACK` is used, we should assume that the lookup is being done for a symbol that is always present on `object` and hence we also assume that it can never be looked up on a meta-class

The last point is not 100% correct, since data-descriptors should theoretically override lookup even for attributes present on `object`. But as of today, `MRO_NO_OBJECT_FALLBACK` is only used for `__new__` which can't be overridden even by a data-descriptor on a meta-class (I've checked runtime behavior).

## Test Plan

Added the failing code example from #17462 to mdtest


---

_Review requested from @carljm by @mishamsk on 2025-04-28 01:50_

---

_Review requested from @AlexWaygood by @mishamsk on 2025-04-28 01:50_

---

_Review requested from @sharkdp by @mishamsk on 2025-04-28 01:50_

---

_Review requested from @dcreager by @mishamsk on 2025-04-28 01:50_

---

_Comment by @github-actions[bot] on 2025-04-28 01:53_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
parso (https://github.com/davidhalter/parso)
- error[lint:missing-argument] /tmp/mypy_primer/projects/parso/parso/grammar.py:183:16: No argument provided for required parameter `dct` of bound method `__new__`
- Found 80 diagnostics
+ Found 79 diagnostics

kopf (https://github.com/nolar/kopf)
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/kopf/kopf/_kits/webhooks.py:541:20: Argument to this function is incorrect: Expected `str`, found `Literal[WebhookServer]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/kopf/kopf/_kits/webhooks.py:541:20: No arguments provided for required parameters `bases`, `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/kopf/kopf/_kits/webhooks.py:679:22: Argument to this function is incorrect: Expected `str`, found `Literal[WebhookNgrokTunnel]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/kopf/kopf/_kits/webhooks.py:679:22: No arguments provided for required parameters `bases`, `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/kopf/kopf/_kits/webhooks.py:682:22: Argument to this function is incorrect: Expected `str`, found `Literal[WebhookServer]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/kopf/kopf/_kits/webhooks.py:682:22: No arguments provided for required parameters `bases`, `namespace` of bound method `__new__`
- Found 136 diagnostics
+ Found 130 diagnostics

jinja (https://github.com/pallets/jinja)
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/docs/examples/cache_extension.py:30:25: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/jinja/docs/examples/cache_extension.py:39:59: Too many positional arguments to bound method `__new__`: expected 2, got 4
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/environment.py:816:67: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/environment.py:817:37: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/environment.py:817:58: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/ext.py:136:64: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/jinja/src/jinja2/ext.py:157:13: Too many positional arguments to bound method `__new__`: expected 2, got 5
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/ext.py:160:13: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/ext.py:533:42: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/ext.py:536:33: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/ext.py:541:31: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/jinja/src/jinja2/ext.py:543:69: Too many positional arguments to bound method `__new__`: expected 2, got 5
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/ext.py:560:20: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/ext.py:564:21: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/ext.py:566:40: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/ext.py:571:16: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/ext.py:582:16: No arguments provided for required parameters `bases`, `d` of bound method `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/ext.py:582:31: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/ext.py:595:20: No arguments provided for required parameters `bases`, `d` of bound method `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/ext.py:595:32: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/ext.py:596:16: No arguments provided for required parameters `bases`, `d` of bound method `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/ext.py:596:31: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/ext.py:624:19: No arguments provided for required parameters `bases`, `d` of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/ext.py:626:16: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/ext.py:626:39: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:231:47: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:234:55: Too many positional arguments to bound method `__new__`: expected 2, got 3
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:234:61: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:253:40: Too many positional arguments to bound method `__new__`: expected 2, got 6
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:253:70: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:257:25: No arguments provided for required parameters `bases`, `d` of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:257:25: No arguments provided for required parameters `bases`, `d` of bound method `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:257:34: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:257:34: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:265:24: No arguments provided for required parameters `bases`, `d` of bound method `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:265:33: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:274:16: No arguments provided for required parameters `bases`, `d` of bound method `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:274:27: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:291:16: No arguments provided for required parameters `bases`, `d` of bound method `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:291:48: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:294:16: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:297:16: No arguments provided for required parameters `bases`, `d` of bound method `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:297:28: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:329:16: No arguments provided for required parameters `bases`, `d` of bound method `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:329:30: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:346:16: No arguments provided for required parameters `bases`, `d` of bound method `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:346:30: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:358:16: No arguments provided for required parameters `bases`, `d` of bound method `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:358:29: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:365:16: No arguments provided for required parameters `bases`, `d` of bound method `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:365:33: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:423:16: No arguments provided for required parameters `bases`, `d` of bound method `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:423:32: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:438:16: No arguments provided for required parameters `bases`, `d` of bound method `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:438:34: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:444:16: No arguments provided for required parameters `bases`, `d` of bound method `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:444:28: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:451:16: No arguments provided for required parameters `bases`, `d` of bound method `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:451:29: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:492:55: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:532:50: Too many positional arguments to bound method `__new__`: expected 2, got 3
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:532:57: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:541:42: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:550:43: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:557:20: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:557:48: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:581:41: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:602:16: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:602:35: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:621:43: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:632:20: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:632:55: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:635:20: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:635:55: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:651:24: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:651:69: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:653:24: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:653:42: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:659:61: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:661:56: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:669:20: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:669:46: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:672:20: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:672:45: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:752:42: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:764:16: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:764:34: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:777:49: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:779:16: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:779:34: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:820:45: Too many positional arguments to bound method `__new__`: expected 2, got 3
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:820:53: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:824:19: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:824:49: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:825:45: Too many positional arguments to bound method `__new__`: expected 2, got 3
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:825:53: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:836:49: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:837:45: Too many positional arguments to bound method `__new__`: expected 2, got 3
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:837:53: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:870:16: No arguments provided for required parameters `bases`, `d` of bound method `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:870:28: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:917:61: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:933:39: Too many positional arguments to bound method `__new__`: expected 2, got 5
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:933:69: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:953:29: Too many positional arguments to bound method `__new__`: expected 2, got 6
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:953:65: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:990:25: Too many positional arguments to bound method `__new__`: expected 2, got 6
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:990:61: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:993:20: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:993:36: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:1009:29: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:1009:58: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:1017:34: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:1017:66: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:1047:18: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/src/jinja2/parser.py:1047:50: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/tests/test_ext.py:158:16: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/tests/test_ext.py:163:25: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/tests/test_ext.py:165:25: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/tests/test_ext.py:492:24: No arguments provided for required parameters `bases`, `d` of bound method `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/tests/test_ext.py:492:36: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/tests/test_ext.py:501:67: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/tests/test_ext.py:718:24: No arguments provided for required parameters `bases`, `d` of bound method `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/jinja/tests/test_ext.py:718:43: Argument `lineno` does not match any known parameter of bound method `__new__`
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/jinja/tests/test_idtracking.py:9:9: Too many positional arguments to bound method `__new__`: expected 2, got 6
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/tests/test_idtracking.py:9:10: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/tests/test_idtracking.py:14:12: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/tests/test_idtracking.py:41:19: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/tests/test_idtracking.py:41:33: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/jinja/tests/test_idtracking.py:41:70: Too many positional arguments to bound method `__new__`: expected 2, got 4
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/jinja/tests/test_idtracking.py:47:9: Too many positional arguments to bound method `__new__`: expected 2, got 4
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/tests/test_idtracking.py:49:13: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/tests/test_idtracking.py:51:21: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/tests/test_idtracking.py:53:21: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/tests/test_idtracking.py:55:21: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/tests/test_idtracking.py:59:50: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/tests/test_idtracking.py:61:13: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/tests/test_idtracking.py:63:21: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/tests/test_idtracking.py:65:21: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/jinja/tests/test_idtracking.py:74:37: Too many positional arguments to bound method `__new__`: expected 2, got 6
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/tests/test_idtracking.py:80:29: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/tests/test_idtracking.py:85:42: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/jinja/tests/test_idtracking.py:86:41: Too many positional arguments to bound method `__new__`: expected 2, got 5
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/jinja/tests/test_idtracking.py:93:25: Too many positional arguments to bound method `__new__`: expected 2, got 4
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/jinja/tests/test_idtracking.py:104:9: Too many positional arguments to bound method `__new__`: expected 2, got 6
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/tests/test_idtracking.py:105:13: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/tests/test_idtracking.py:107:21: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/tests/test_idtracking.py:109:21: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/tests/test_idtracking.py:112:27: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/jinja/tests/test_idtracking.py:112:61: Too many positional arguments to bound method `__new__`: expected 2, got 3
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/tests/test_idtracking.py:113:13: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/tests/test_idtracking.py:113:27: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/tests/test_idtracking.py:123:13: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/tests/test_idtracking.py:125:21: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/jinja/tests/test_idtracking.py:129:25: Too many positional arguments to bound method `__new__`: expected 2, got 5
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/tests/test_idtracking.py:133:21: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/tests/test_idtracking.py:137:13: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/tests/test_idtracking.py:137:27: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/jinja/tests/test_idtracking.py:139:9: Too many positional arguments to bound method `__new__`: expected 2, got 4
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/tests/test_idtracking.py:143:12: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/tests/test_idtracking.py:145:13: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/tests/test_idtracking.py:145:27: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/tests/test_idtracking.py:213:12: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/tests/test_idtracking.py:217:64: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/jinja/tests/test_idtracking.py:218:17: Too many positional arguments to bound method `__new__`: expected 2, got 4
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/tests/test_idtracking.py:237:12: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/tests/test_idtracking.py:239:59: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/tests/test_idtracking.py:242:64: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/jinja/tests/test_idtracking.py:243:17: Too many positional arguments to bound method `__new__`: expected 2, got 4
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/jinja/tests/test_idtracking.py:265:9: Too many positional arguments to bound method `__new__`: expected 2, got 6
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/tests/test_idtracking.py:268:57: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/jinja/tests/test_idtracking.py:269:17: Too many positional arguments to bound method `__new__`: expected 2, got 4
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/tests/test_idtracking.py:272:27: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/jinja/tests/test_idtracking.py:272:61: Too many positional arguments to bound method `__new__`: expected 2, got 3
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/tests/test_idtracking.py:279:12: No argument provided for required parameter `d` of bound method `__new__`
- error[lint:missing-argument] /tmp/mypy_primer/projects/jinja/tests/test_idtracking.py:280:49: No argument provided for required parameter `d` of bound method `__new__`
- Found 413 diagnostics
+ Found 237 diagnostics

pyodide (https://github.com/pyodide/pyodide)
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyodide/pyodide-build/pyodide_build/recipe/spec.py:150:27: Argument to this function is incorrect: Expected `str`, found `Literal[_SourceSpec]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/pyodide/pyodide-build/pyodide_build/recipe/spec.py:150:27: No arguments provided for required parameters `bases`, `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyodide/pyodide-build/pyodide_build/recipe/spec.py:151:25: Argument to this function is incorrect: Expected `str`, found `Literal[_BuildSpec]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/pyodide/pyodide-build/pyodide_build/recipe/spec.py:151:25: No arguments provided for required parameters `bases`, `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyodide/pyodide-build/pyodide_build/recipe/spec.py:152:39: Argument to this function is incorrect: Expected `str`, found `Literal[_RequirementsSpec]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/pyodide/pyodide-build/pyodide_build/recipe/spec.py:152:39: No arguments provided for required parameters `bases`, `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyodide/pyodide-build/pyodide_build/recipe/spec.py:153:23: Argument to this function is incorrect: Expected `str`, found `Literal[_TestSpec]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/pyodide/pyodide-build/pyodide_build/recipe/spec.py:153:23: No arguments provided for required parameters `bases`, `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyodide/pyodide-build/pyodide_build/recipe/spec.py:154:25: Argument to this function is incorrect: Expected `str`, found `Literal[_AboutSpec]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/pyodide/pyodide-build/pyodide_build/recipe/spec.py:154:25: No arguments provided for required parameters `bases`, `namespace` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyodide/pyodide-build/pyodide_build/recipe/spec.py:155:25: Argument to this function is incorrect: Expected `str`, found `Literal[_ExtraSpec]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/pyodide/pyodide-build/pyodide_build/recipe/spec.py:155:25: No arguments provided for required parameters `bases`, `namespace` of bound method `__new__`
- Found 1259 diagnostics
+ Found 1247 diagnostics

bokeh (https://github.com/bokeh/bokeh)
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bokeh/src/bokeh/server/server.py:397:16: Argument to this function is incorrect: Expected `str`, found `Literal[_ServerOpts]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/bokeh/src/bokeh/server/server.py:397:16: No argument provided for required parameter `class_dict` of bound method `__new__`
- Found 1851 diagnostics
+ Found 1849 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dd-trace-py/ddtrace/internal/gitmetadata.py:104:18: Argument to this function is incorrect: Expected `str`, found `Literal[GitMetadataConfig]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/dd-trace-py/ddtrace/internal/gitmetadata.py:104:18: No arguments provided for required parameters `bases`, `ns` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dd-trace-py/ddtrace/internal/remoteconfig/client.py:66:10: Argument to this function is incorrect: Expected `str`, found `Literal[RemoteConfigClientConfig]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/dd-trace-py/ddtrace/internal/remoteconfig/client.py:66:10: No arguments provided for required parameters `bases`, `ns` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dd-trace-py/ddtrace/settings/_agent.py:158:10: Argument to this function is incorrect: Expected `str`, found `Literal[AgentConfig]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/dd-trace-py/ddtrace/settings/_agent.py:158:10: No arguments provided for required parameters `bases`, `ns` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dd-trace-py/ddtrace/settings/_database_monitoring.py:18:14: Argument to this function is incorrect: Expected `str`, found `Literal[DatabaseMonitoringConfig]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/dd-trace-py/ddtrace/settings/_database_monitoring.py:18:14: No arguments provided for required parameters `bases`, `ns` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dd-trace-py/ddtrace/settings/_telemetry.py:28:10: Argument to this function is incorrect: Expected `str`, found `Literal[TelemetryConfig]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/dd-trace-py/ddtrace/settings/_telemetry.py:28:10: No arguments provided for required parameters `bases`, `ns` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dd-trace-py/ddtrace/settings/asm.py:280:10: Argument to this function is incorrect: Expected `str`, found `Literal[ASMConfig]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/dd-trace-py/ddtrace/settings/asm.py:280:10: No arguments provided for required parameters `bases`, `ns` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dd-trace-py/ddtrace/settings/code_origin.py:29:10: Argument to this function is incorrect: Expected `str`, found `Literal[CodeOriginConfig]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/dd-trace-py/ddtrace/settings/code_origin.py:29:10: No arguments provided for required parameters `bases`, `ns` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dd-trace-py/ddtrace/settings/crashtracker.py:122:10: Argument to this function is incorrect: Expected `str`, found `Literal[CrashtrackingConfig]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/dd-trace-py/ddtrace/settings/crashtracker.py:122:10: No arguments provided for required parameters `bases`, `ns` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dd-trace-py/ddtrace/settings/dynamic_instrumentation.py:132:10: Argument to this function is incorrect: Expected `str`, found `Literal[DynamicInstrumentationConfig]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/dd-trace-py/ddtrace/settings/dynamic_instrumentation.py:132:10: No arguments provided for required parameters `bases`, `ns` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dd-trace-py/ddtrace/settings/errortracking.py:52:10: Argument to this function is incorrect: Expected `str`, found `Literal[ErrorTrackingConfig]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/dd-trace-py/ddtrace/settings/errortracking.py:52:10: No arguments provided for required parameters `bases`, `ns` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dd-trace-py/ddtrace/settings/exception_replay.py:24:10: Argument to this function is incorrect: Expected `str`, found `Literal[ExceptionReplayConfig]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/dd-trace-py/ddtrace/settings/exception_replay.py:24:10: No arguments provided for required parameters `bases`, `ns` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dd-trace-py/ddtrace/settings/live_debugging.py:16:10: Argument to this function is incorrect: Expected `str`, found `Literal[LiveDebuggerConfig]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/dd-trace-py/ddtrace/settings/live_debugging.py:16:10: No arguments provided for required parameters `bases`, `ns` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dd-trace-py/ddtrace/settings/profiling.py:455:10: Argument to this function is incorrect: Expected `str`, found `Literal[ProfilingConfig]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/dd-trace-py/ddtrace/settings/profiling.py:455:10: No arguments provided for required parameters `bases`, `ns` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dd-trace-py/ddtrace/settings/symbol_db.py:40:10: Argument to this function is incorrect: Expected `str`, found `Literal[SymbolDatabaseConfig]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/dd-trace-py/ddtrace/settings/symbol_db.py:40:10: No arguments provided for required parameters `bases`, `ns` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dd-trace-py/ddtrace/settings/third_party.py:23:10: Argument to this function is incorrect: Expected `str`, found `Literal[ThirdPartyDetectionConfig]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/dd-trace-py/ddtrace/settings/third_party.py:23:10: No arguments provided for required parameters `bases`, `ns` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dd-trace-py/tests/debugging/exception/test_replay.py:19:17: Argument to this function is incorrect: Expected `str`, found `Literal[ExceptionReplayConfig]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/dd-trace-py/tests/debugging/exception/test_replay.py:19:17: No arguments provided for required parameters `bases`, `ns` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dd-trace-py/tests/debugging/exception/test_replay.py:26:17: Argument to this function is incorrect: Expected `str`, found `Literal[ExceptionReplayConfig]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/dd-trace-py/tests/debugging/exception/test_replay.py:26:17: No arguments provided for required parameters `bases`, `ns` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dd-trace-py/tests/debugging/exception/test_replay.py:31:17: Argument to this function is incorrect: Expected `str`, found `Literal[ExceptionReplayConfig]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/dd-trace-py/tests/debugging/exception/test_replay.py:31:17: No arguments provided for required parameters `bases`, `ns` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dd-trace-py/tests/debugging/exception/test_replay.py:319:18: Argument to this function is incorrect: Expected `str`, found `Literal[ExceptionReplayConfig]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/dd-trace-py/tests/debugging/exception/test_replay.py:319:18: No arguments provided for required parameters `bases`, `ns` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dd-trace-py/tests/debugging/exploration/_config.py:123:10: Argument to this function is incorrect: Expected `str`, found `Literal[ExplorationConfig]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/dd-trace-py/tests/debugging/exploration/_config.py:123:10: No arguments provided for required parameters `bases`, `ns` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dd-trace-py/tests/debugging/test_config.py:23:26: Argument to this function is incorrect: Expected `str`, found `Literal[DynamicInstrumentationConfig]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/dd-trace-py/tests/debugging/test_config.py:23:26: No arguments provided for required parameters `bases`, `ns` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dd-trace-py/tests/debugging/test_config.py:47:12: Argument to this function is incorrect: Expected `str`, found `Literal[DynamicInstrumentationConfig]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/dd-trace-py/tests/debugging/test_config.py:47:12: No arguments provided for required parameters `bases`, `ns` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dd-trace-py/tests/debugging/test_config.py:51:12: Argument to this function is incorrect: Expected `str`, found `Literal[DynamicInstrumentationConfig]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/dd-trace-py/tests/debugging/test_config.py:51:12: No arguments provided for required parameters `bases`, `ns` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dd-trace-py/tests/internal/bytecode_injection/framework_injection/_config.py:63:10: Argument to this function is incorrect: Expected `str`, found `Literal[InjectionConfig]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/dd-trace-py/tests/internal/bytecode_injection/framework_injection/_config.py:63:10: No arguments provided for required parameters `bases`, `ns` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dd-trace-py/tests/internal/symbol_db/test_config.py:6:9: Argument to this function is incorrect: Expected `str`, found `Literal[SymbolDatabaseConfig]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/dd-trace-py/tests/internal/symbol_db/test_config.py:6:9: No arguments provided for required parameters `bases`, `ns` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dd-trace-py/tests/internal/test_database_monitoring.py:17:18: Argument to this function is incorrect: Expected `str`, found `Literal[DatabaseMonitoringConfig]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/dd-trace-py/tests/internal/test_database_monitoring.py:17:18: No arguments provided for required parameters `bases`, `ns` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dd-trace-py/tests/internal/test_database_monitoring.py:22:18: Argument to this function is incorrect: Expected `str`, found `Literal[DatabaseMonitoringConfig]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/dd-trace-py/tests/internal/test_database_monitoring.py:22:18: No arguments provided for required parameters `bases`, `ns` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dd-trace-py/tests/internal/test_database_monitoring.py:28:13: Argument to this function is incorrect: Expected `str`, found `Literal[DatabaseMonitoringConfig]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/dd-trace-py/tests/internal/test_database_monitoring.py:28:13: No arguments provided for required parameters `bases`, `ns` of bound method `__new__`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dd-trace-py/tests/profiling/collector/test_memalloc.py:347:14: Argument to this function is incorrect: Expected `str`, found `Literal[ProfilingConfig]`
- error[lint:missing-argument] /tmp/mypy_primer/projects/dd-trace-py/tests/profiling/collector/test_memalloc.py:347:14: No arguments provided for required parameters `bases`, `ns` of bound method `__new__`
- Found 6877 diagnostics
+ Found 6819 diagnostics

```
</details>


---

_Comment by @mishamsk on 2025-04-28 02:07_

mypy_primer fixes seem legit!

---

_Label `red-knot` added by @MichaReiser on 2025-04-28 06:11_

---

_@AlexWaygood reviewed on 2025-04-28 10:44_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/class.rs`:893 on 2025-04-28 10:44_

Could you explain why we still need to add some special casing for `type` specifically here? I see some tests fail in `is_subtype_of.md` without this, but I don't understand why we need to `continue` if the class is `type` but not if it's some `type` subclass. Adding a comment here that explains this would be very helpful.

---

_@mishamsk reviewed on 2025-04-28 14:06_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/src/types/class.rs`:893 on 2025-04-28 14:06_

@AlexWaygood yeah, I initially removed it and then stumbled on failing tests and figured out it is not possible (at least without refactoring into_callable logic). I've updated the policy bit flags enum docs and mentioned the reason in the description of the PR, but agree that adding additional comment at the use site would be helpful for future readers. I'll wait for other reviews and will add.

Here is my line from the PR for the underlying reason:

> We can't get rid of META_CLASS_NO_TYPE_FALLBACK as it is necessary to figure out if a given meta-class has an explicit `__call__` method defined, not inherited from type. This is used in `into_callable` implementation

---

_@AlexWaygood reviewed on 2025-04-28 14:39_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/class.rs`:893 on 2025-04-28 14:39_

> Here is my line from the PR for the underlying reason:

I see -- I didn't realise that that note from your PR description was referring to this, since this method is called `Type::class_member_from_mro` rather than `Type::into_callable` :-)

---

_@mishamsk reviewed on 2025-04-28 17:59_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/src/types/class.rs`:893 on 2025-04-28 17:59_

I'll do better with wording! Thanks for pointing this out. I've just pushed a commit with a comment, hopefully it is clear enough. Let me know, I can continue exercising my English if it doesn't!

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/call/constructor.md`:333 on 2025-04-29 16:44_

```suggestion
purposes of type checking we use special lookup logic that ignores `object.__new__` as its runtime
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/call/constructor.md`:334 on 2025-04-29 16:44_

```suggestion
behavior is not expressible in typeshed. This should not cause false-positives when `__new__` is defined on
```

---

_Renamed from "[red-knot] Fix constructor calls on classes with meta-classes " to "[red-knot] Fix constructor calls on classes with metaclasses " by @AlexWaygood on 2025-04-29 17:22_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2647 on 2025-04-29 17:31_

This match on these specific type variants here looks suspicious to me; I think I need a clearer statement of why this is correct? Tests fail without it, so it's clearly not redundant.

It _seems_ like we are using "is `self` one of these three variants" as a proxy for "are we currently looking up an attribute on the meta type", but I don't think that's really a correct proxy? Or at least, if it's correct today, it's making assumptions that could be invalidated in future. (For instance, `Instance(<builtins.type>)` could definitely be a meta-type -- I can't remember if we normalize that to `SubclassOf(<builtins.object>)` currently.)

I'd have to spend more time digging into details to make this more concrete, but it feels like there should be some place higher up the call chain where we should know for certain that we are about to look up the attribute on the meta type, and _that's_ the place where we should be making a decision about this case (and if necessary, passing some policy or context down to here to reflect that fact).

---

_@carljm reviewed on 2025-04-29 17:31_

Thanks for working on this! I'm not yet quite convinced by the correctness of the fix, though...

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/call/constructor.md`:335 on 2025-04-29 17:33_

```suggestion
We lookup `__new__` methods using the descriptor protocol, which technically allows it to be found on a
metaclass if `__new__` is not defined anywhere in a class's MRO. At runtime this is never the case,
since all classes have `object` in their MRO, and `object` has a `__new__` method. However, for the
purposes of type checking we use special lookup logic that ignores `object.__new__` as its runtime
behavior is not expressible in typeshed. This does not cause false-positives when `__new__` is defined on
a metaclass.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:204 on 2025-04-29 17:33_

```suggestion
        /// For example, this is used to find if a metaclass implements a `__call__` method.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/class.rs`:882 on 2025-04-29 17:33_

```suggestion
                    // Some metaclass methods such as `__call__` need special treatment depending on
```

---

_@AlexWaygood reviewed on 2025-04-29 17:34_

Some minor spelling and grammar nits :-)

---

_Comment by @sharkdp on 2025-04-30 11:26_

@mishamsk While reviewing this, I needed to dig deeper into how class construction work, and found some things that were problematic with the current way we handle things. There are two main things that we need to distinguish:

- how attributes are looked up
- whether or not `__get__` is potentially called on descriptor attributes

The two are not unrelated. A normal attribute lookup works like this:

1. Look up the attribute on the meta type; if it's a data descriptor, call `__get__` and use the return type
2. Otherwise, use a fallback
  a. instance attributes, if accessing on an instance
  b. class attributes with a potential `__get__(None, cls)` call, if accessing on a class
4. If the fallback is not available, call `__get__` if the attribute on the metaclass is a non-data descriptor
5. Otherwise, return `Unbound`.

For *implicit dunder calls* like `__call__`, `__iter__`, etc., the attribute lookup works in the same way, except that the fallback step 2 is skipped (this is why we pass `MemberLookupPolicy::NO_INSTANCE_FALLBACK` in `try_call_dunder`). This means that implicit dunder calls are *always* accessed on the meta type.

For `__new__`, the situation seems to be completely different. We still "invoke the descriptor protocol" in the sense that we potentially call `__get__`, but the way attribute lookup works is different from the two cases above. Instead of going straight to the meta type, `__new__` is called directly on the class that is being constructed. In a sense, it's as if only step 2b of the protocol above would be executed.

This explains why it's problematic to use `try_call_dunder` for that implicit `__new__` call (originating from `type.__call__`). I am working on an alternative approach, so maybe hold off on further work on this PR. I hope that's okay for you.

---

_Comment by @mishamsk on 2025-04-30 12:22_

@sharkdp yes, I agree with your assessment and I was thinking that Carl's comment basically implies that I should "fix" the root cause (improve descriptor protocol modeling) instead of doing a band-aid. But if you are already working on it, I'll wait. Although I wonder if there is going to be anything left from this PR after the change. Maybe tests & some comments...

I'll see if it makes sense for me to do other follow-ups that I had without waiting. I need to tackle `__call__` in constructor and remove the calling logic from infer (per one of your suggestions to the original PR). Keep me posted (or better mention me on the PR with your changes, so I could see when it is merged)

---

_Comment by @sharkdp on 2025-04-30 13:06_

> @sharkdp yes, I agree with your assessment and I was thinking that Carl's comment basically implies that I should "fix" the root cause (improve descriptor protocol modeling) instead of doing a band-aid. But if you are already working on it, I'll wait. Although I wonder if there is going to be anything left from this PR after the change. Maybe tests & some comments...

The PR with the alternative approach is here: https://github.com/astral-sh/ruff/pull/17733. My intention was to write review feedback for your PR, but by the time I had understood what was going on, I had written so much new code for my analysis that it seemed easier to open a PR myself. Sorry for that! I think this PR was still valuable for exploration of possible options. For example, I agree with you that we still need the `META_CLASS_NO_TYPE_FALLBACK` policy parameter (for looking up `__call__` on meta types without looking up `type.__call__`).



> I'll see if it makes sense for me to do other follow-ups that I had without waiting. I need to tackle `__call__` in constructor

I think this would be a great follow-up, yes! Especially since my PR now reveals a few new false positives on enum construction (see PR description).

> and remove the calling logic from infer (per one of your suggestions to the original PR)

Not exactly sure what that refers to, but reducing technical debt always sounds like a good idea.

---

_Comment by @sharkdp on 2025-04-30 15:27_

Closing this in favor of #17733. @mishamsk Thank you very much!

---

_Closed by @sharkdp on 2025-04-30 15:27_

---
