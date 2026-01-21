```yaml
number: 22778
title: "[ty] Fix assignment in decorated method causing Unknown fallback"
type: pull_request
state: open
author: carljm
labels:
  - ty
  - ecosystem-analyzer
assignees: []
base: main
head: cjm/attrs-in-decorated-methods
created_at: 2026-01-20T21:59:48Z
updated_at: 2026-01-21T03:59:07Z
url: https://github.com/astral-sh/ruff/pull/22778
synced_at: 2026-01-21T04:56:55Z
```

# [ty] Fix assignment in decorated method causing Unknown fallback

---

_@carljm_

Fixes https://github.com/astral-sh/ty/issues/2566

Fix a bug where assigning to an instance attribute in a decorated method (like @cache) incorrectly resulted in `Unknown | int` instead of just `int` when the attribute was explicitly declared in `__init__`.

The bug was in the `is_valid_scope` closure in `implicit_attribute_inner`. When looking up class members for the descriptor protocol, it calls `implicit_attribute` with `MethodDecorator::ClassMethod`. The previous code relied on the final evaluated type of the method (after all decorators are applied) to determine if it's a classmethod. For methods wrapped by `@cache`, the final type becomes `_lru_cache_wrapper[...]` instead of `FunctionLiteral`, which caused incorrect behavior.

The fix changes the approach to directly iterate through the decorators on the AST node and check if any of their types evaluate to `classmethod` or `staticmethod`. This is more reliable because it checks the original decorator types, not the final wrapped result.

Also removes the assumption that unknown decorators might be classmethod/staticmethod aliases. This caused attributes defined in methods with unknown decorators to pollute class-level lookups and potentially override instance-level attributes.


---

_Label `ty` added by @carljm on 2026-01-20 21:59_

---

_Comment by @astral-sh-bot[bot] on 2026-01-20 22:01_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-20 22:03_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
parso (https://github.com/davidhalter/parso)
+ parso/python/errors.py:415:18: warning[possibly-missing-attribute] Attribute `add_block` may be missing on object of type `Unknown | _Context | None`
+ parso/python/errors.py:416:24: warning[possibly-missing-attribute] Attribute `blocks` may be missing on object of type `Unknown | _Context | None`
+ parso/python/errors.py:431:28: warning[possibly-missing-attribute] Attribute `parent_context` may be missing on object of type `Unknown | _Context | None`
+ parso/python/errors.py:432:13: warning[possibly-missing-attribute] Attribute `close_child_context` may be missing on object of type `Unknown | None`
+ parso/python/errors.py:470:32: warning[possibly-missing-attribute] Attribute `add_context` may be missing on object of type `Unknown | _Context | None`
+ parso/python/errors.py:489:9: warning[possibly-missing-attribute] Attribute `finalize` may be missing on object of type `Unknown | _Context | None`
- parso/python/pep8.py:290:22: warning[possibly-missing-attribute] Attribute `get_latest_suite_node` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:290:22: warning[possibly-missing-attribute] Attribute `get_latest_suite_node` may be missing on object of type `Unknown | IndentationNode | None`
- parso/python/pep8.py:385:37: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:385:37: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `Unknown | IndentationNode | None`
- parso/python/pep8.py:414:16: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:414:16: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | IndentationNode | None`
- parso/python/pep8.py:415:20: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:415:20: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | IndentationNode | None`
- parso/python/pep8.py:418:35: warning[possibly-missing-attribute] Attribute `indentation` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:418:35: warning[possibly-missing-attribute] Attribute `indentation` may be missing on object of type `Unknown | IndentationNode | None`
- parso/python/pep8.py:419:54: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:419:54: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | IndentationNode | None`
- parso/python/pep8.py:431:16: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:431:16: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | IndentationNode | None`
- parso/python/pep8.py:433:41: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:433:41: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `Unknown | IndentationNode | None`
- parso/python/pep8.py:436:41: warning[possibly-missing-attribute] Attribute `indentation` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:436:41: warning[possibly-missing-attribute] Attribute `indentation` may be missing on object of type `Unknown | IndentationNode | None`
- parso/python/pep8.py:460:49: warning[possibly-missing-attribute] Attribute `bracket_indentation` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:460:49: warning[possibly-missing-attribute] Attribute `bracket_indentation` may be missing on object of type `Unknown | IndentationNode | None`
- parso/python/pep8.py:462:49: warning[possibly-missing-attribute] Attribute `indentation` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:462:49: warning[possibly-missing-attribute] Attribute `indentation` may be missing on object of type `Unknown | IndentationNode | None`
- parso/python/pep8.py:464:29: warning[possibly-missing-attribute] Attribute `get_latest_suite_node` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:464:29: warning[possibly-missing-attribute] Attribute `get_latest_suite_node` may be missing on object of type `Unknown | IndentationNode | None`
- parso/python/pep8.py:471:36: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:471:36: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | IndentationNode | None`
- parso/python/pep8.py:486:40: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:486:40: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | IndentationNode | None`
- parso/python/pep8.py:492:42: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:492:42: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | IndentationNode | None`
- parso/python/pep8.py:498:42: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:498:42: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | IndentationNode | None`
- parso/python/pep8.py:507:40: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:507:40: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | IndentationNode | None`
- parso/python/pep8.py:513:42: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:513:42: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | IndentationNode | None`
- parso/python/pep8.py:537:24: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:537:24: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | IndentationNode | None`
- parso/python/pep8.py:538:41: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:538:41: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `Unknown | IndentationNode | None`
- parso/python/pep8.py:541:27: warning[possibly-missing-attribute] Attribute `indentation` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:541:27: warning[possibly-missing-attribute] Attribute `indentation` may be missing on object of type `Unknown | IndentationNode | None`
- Found 201 diagnostics
+ Found 207 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- testing/test_terminal.py:2029:33: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 407 diagnostics
+ Found 406 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- tests/test_closespider.py:29:18: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_closespider.py:29:18: error[unresolved-attribute] Object of type `Spider | None` has no attribute `meta`
- tests/test_closespider.py:39:18: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_closespider.py:39:18: error[unresolved-attribute] Object of type `Spider | None` has no attribute `meta`
- tests/test_closespider.py:58:18: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_closespider.py:58:18: error[unresolved-attribute] Object of type `Spider | None` has no attribute `meta`
- tests/test_closespider.py:76:18: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_closespider.py:76:18: error[unresolved-attribute] Object of type `Spider | None` has no attribute `meta`
- tests/test_closespider.py:86:18: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
- tests/test_closespider.py:88:36: warning[possibly-missing-attribute] Attribute `exception_cls` may be missing on object of type `Unknown | Spider | None`
+ tests/test_closespider.py:86:18: error[unresolved-attribute] Object of type `Spider | None` has no attribute `meta`
+ tests/test_closespider.py:88:36: error[unresolved-attribute] Object of type `Spider | None` has no attribute `exception_cls`
- tests/test_closespider.py:98:18: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_closespider.py:98:18: error[unresolved-attribute] Object of type `Spider | None` has no attribute `meta`
- tests/test_closespider.py:108:18: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_closespider.py:108:18: error[unresolved-attribute] Object of type `Spider | None` has no attribute `meta`
- tests/test_contracts.py:534:16: warning[possibly-missing-attribute] Attribute `visited` may be missing on object of type `Unknown | Spider | None`
+ tests/test_contracts.py:534:16: error[unresolved-attribute] Object of type `Spider | None` has no attribute `visited`
- tests/test_crawl.py:79:20: warning[possibly-missing-attribute] Attribute `urls_visited` may be missing on object of type `Unknown | Spider | None`
- tests/test_crawl.py:127:16: warning[possibly-missing-attribute] Attribute `t1` may be missing on object of type `Unknown | Spider | None`
- tests/test_crawl.py:128:16: warning[possibly-missing-attribute] Attribute `t2` may be missing on object of type `Unknown | Spider | None`
- tests/test_crawl.py:129:16: warning[possibly-missing-attribute] Attribute `t2` may be missing on object of type `Unknown | Spider | None`
- tests/test_crawl.py:129:36: warning[possibly-missing-attribute] Attribute `t1` may be missing on object of type `Unknown | Spider | None`
- tests/test_crawl.py:135:16: warning[possibly-missing-attribute] Attribute `t1` may be missing on object of type `Unknown | Spider | None`
- tests/test_crawl.py:136:16: warning[possibly-missing-attribute] Attribute `t2` may be missing on object of type `Unknown | Spider | None`
- tests/test_crawl.py:137:16: warning[possibly-missing-attribute] Attribute `t2_err` may be missing on object of type `Unknown | Spider | None`
- tests/test_crawl.py:138:16: warning[possibly-missing-attribute] Attribute `t2_err` may be missing on object of type `Unknown | Spider | None`
- tests/test_crawl.py:138:40: warning[possibly-missing-attribute] Attribute `t1` may be missing on object of type `Unknown | Spider | None`
- tests/test_crawl.py:143:16: warning[possibly-missing-attribute] Attribute `t1` may be missing on object of type `Unknown | Spider | None`
- tests/test_crawl.py:144:16: warning[possibly-missing-attribute] Attribute `t2` may be missing on object of type `Unknown | Spider | None`
- tests/test_crawl.py:145:16: warning[possibly-missing-attribute] Attribute `t2_err` may be missing on object of type `Unknown | Spider | None`
- tests/test_crawl.py:146:16: warning[possibly-missing-attribute] Attribute `t2_err` may be missing on object of type `Unknown | Spider | None`
- tests/test_crawl.py:146:40: warning[possibly-missing-attribute] Attribute `t1` may be missing on object of type `Unknown | Spider | None`
- tests/test_crawl.py:243:16: warning[possibly-missing-attribute] Attribute `visited` may be missing on object of type `Unknown | Spider | None`
- tests/test_crawl.py:252:16: warning[possibly-missing-attribute] Attribute `visited` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:79:20: error[unresolved-attribute] Object of type `Spider | None` has no attribute `urls_visited`
+ tests/test_crawl.py:127:16: error[unresolved-attribute] Object of type `Spider | None` has no attribute `t1`
+ tests/test_crawl.py:128:16: error[unresolved-attribute] Object of type `Spider | None` has no attribute `t2`
+ tests/test_crawl.py:129:16: error[unresolved-attribute] Object of type `Spider | None` has no attribute `t2`
+ tests/test_crawl.py:129:36: error[unresolved-attribute] Object of type `Spider | None` has no attribute `t1`
+ tests/test_crawl.py:135:16: error[unresolved-attribute] Object of type `Spider | None` has no attribute `t1`
+ tests/test_crawl.py:136:16: error[unresolved-attribute] Object of type `Spider | None` has no attribute `t2`
+ tests/test_crawl.py:137:16: error[unresolved-attribute] Object of type `Spider | None` has no attribute `t2_err`
+ tests/test_crawl.py:138:16: error[unresolved-attribute] Object of type `Spider | None` has no attribute `t2_err`
+ tests/test_crawl.py:138:40: error[unresolved-attribute] Object of type `Spider | None` has no attribute `t1`
+ tests/test_crawl.py:143:16: error[unresolved-attribute] Object of type `Spider | None` has no attribute `t1`
+ tests/test_crawl.py:144:16: error[unresolved-attribute] Object of type `Spider | None` has no attribute `t2`
+ tests/test_crawl.py:145:16: error[unresolved-attribute] Object of type `Spider | None` has no attribute `t2_err`
+ tests/test_crawl.py:146:16: error[unresolved-attribute] Object of type `Spider | None` has no attribute `t2_err`
+ tests/test_crawl.py:146:40: error[unresolved-attribute] Object of type `Spider | None` has no attribute `t1`
+ tests/test_crawl.py:243:16: error[unresolved-attribute] Object of type `Spider | None` has no attribute `visited`
+ tests/test_crawl.py:252:16: error[unresolved-attribute] Object of type `Spider | None` has no attribute `visited`
- tests/test_crawl.py:323:31: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
- tests/test_crawl.py:324:34: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
- tests/test_crawl.py:326:39: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
- tests/test_crawl.py:329:39: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
- tests/test_crawl.py:332:39: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
- tests/test_crawl.py:335:39: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:323:31: error[unresolved-attribute] Object of type `Spider | None` has no attribute `meta`
+ tests/test_crawl.py:324:34: error[unresolved-attribute] Object of type `Spider | None` has no attribute `meta`
+ tests/test_crawl.py:326:39: error[unresolved-attribute] Object of type `Spider | None` has no attribute `meta`
+ tests/test_crawl.py:329:39: error[unresolved-attribute] Object of type `Spider | None` has no attribute `meta`
+ tests/test_crawl.py:332:39: error[unresolved-attribute] Object of type `Spider | None` has no attribute `meta`
+ tests/test_crawl.py:335:39: error[unresolved-attribute] Object of type `Spider | None` has no attribute `meta`
- tests/test_crawl.py:343:42: error[invalid-argument-type] Argument to function `get_engine_status` is incorrect: Expected `ExecutionEngine`, found `Unknown | ExecutionEngine | None`
+ tests/test_crawl.py:343:42: error[invalid-argument-type] Argument to function `get_engine_status` is incorrect: Expected `ExecutionEngine`, found `ExecutionEngine | None`
- tests/test_crawl.py:351:43: warning[possibly-missing-attribute] Attribute `name` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:351:43: warning[possibly-missing-attribute] Attribute `name` may be missing on object of type `Spider | None`
- tests/test_crawl.py:359:45: error[invalid-argument-type] Argument to function `format_engine_status` is incorrect: Expected `ExecutionEngine`, found `Unknown | ExecutionEngine | None`
+ tests/test_crawl.py:359:45: error[invalid-argument-type] Argument to function `format_engine_status` is incorrect: Expected `ExecutionEngine`, found `ExecutionEngine | None`
- tests/test_crawl.py:374:43: warning[possibly-missing-attribute] Attribute `name` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:374:43: warning[possibly-missing-attribute] Attribute `name` may be missing on object of type `Spider | None`
- tests/test_crawl.py:634:16: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
- tests/test_crawl.py:641:16: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
- tests/test_crawl.py:654:16: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
- tests/test_crawl.py:664:22: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
- tests/test_crawl.py:673:22: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
- tests/test_crawl.py:681:16: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
- tests/test_crawl.py:682:27: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
- tests/test_crawl.py:683:16: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
- tests/test_crawl.py:683:56: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
- tests/test_crawl.py:687:17: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
- tests/test_crawl.py:688:15: warning[possibly-missing-attribute] Attribute `full_response_length` may be missing on object of type `Unknown | Spider | None`
- tests/test_crawl.py:695:16: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
- tests/test_crawl.py:696:27: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
- tests/test_crawl.py:697:27: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
- tests/test_crawl.py:698:27: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
- tests/test_crawl.py:699:16: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
- tests/test_crawl.py:701:34: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
- tests/test_crawl.py:703:17: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
- tests/test_crawl.py:704:15: warning[possibly-missing-attribute] Attribute `full_response_length` may be missing on object of type `Unknown | Spider | None`
- tests/test_crawl.py:711:16: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
- tests/test_crawl.py:712:27: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
- tests/test_crawl.py:713:16: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
- tests/test_crawl.py:713:59: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
- tests/test_crawl.py:721:16: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
- tests/test_crawl.py:722:27: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
- tests/test_crawl.py:723:27: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
- tests/test_crawl.py:724:27: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
- tests/test_crawl.py:725:16: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
- tests/test_crawl.py:727:37: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:634:16: error[unresolved-attribute] Object of type `Spider | None` has no attribute `meta`
+ tests/test_crawl.py:641:16: error[unresolved-attribute] Object of type `Spider | None` has no attribute `meta`
+ tests/test_crawl.py:654:16: error[unresolved-attribute] Object of type `Spider | None` has no attribute `meta`
+ tests/test_crawl.py:664:22: error[unresolved-attribute] Object of type `Spider | None` has no attribute `meta`
+ tests/test_crawl.py:673:22: error[unresolved-attribute] Object of type `Spider | None` has no attribute `meta`
+ tests/test_crawl.py:681:16: error[unresolved-attribute] Object of type `Spider | None` has no attribute `meta`
+ tests/test_crawl.py:682:27: error[unresolved-attribute] Object of type `Spider | None` has no attribute `meta`
+ tests/test_crawl.py:683:16: error[unresolved-attribute] Object of type `Spider | None` has no attribute `meta`
+ tests/test_crawl.py:683:56: error[unresolved-attribute] Object of type `Spider | None` has no attribute `meta`
+ tests/test_crawl.py:687:17: error[unresolved-attribute] Object of type `Spider | None` has no attribute `meta`
+ tests/test_crawl.py:688:15: error[unresolved-attribute] Object of type `Spider | None` has no attribute `full_response_length`
+ tests/test_crawl.py:695:16: error[unresolved-attribute] Object of type `Spider | None` has no attribute `meta`
+ tests/test_crawl.py:696:27: error[unresolved-attribute] Object of type `Spider | None` has no attribute `meta`
+ tests/test_crawl.py:697:27: error[unresolved-attribute] Object of type `Spider | None` has no attribute `meta`
+ tests/test_crawl.py:698:27: error[unresolved-attribute] Object of type `Spider | None` has no attribute `meta`
+ tests/test_crawl.py:699:16: error[unresolved-attribute] Object of type `Spider | None` has no attribute `meta`
+ tests/test_crawl.py:701:34: error[unresolved-attribute] Object of type `Spider | None` has no attribute `meta`
+ tests/test_crawl.py:703:17: error[unresolved-attribute] Object of type `Spider | None` has no attribute `meta`
+ tests/test_crawl.py:704:15: error[unresolved-attribute] Object of type `Spider | None` has no attribute `full_response_length`
+ tests/test_crawl.py:711:16: error[unresolved-attribute] Object of type `Spider | None` has no attribute `meta`
+ tests/test_crawl.py:712:27: error[unresolved-attribute] Object of type `Spider | None` has no attribute `meta`
+ tests/test_crawl.py:713:16: error[unresolved-attribute] Object of type `Spider | None` has no attribute `meta`
+ tests/test_crawl.py:713:59: error[unresolved-attribute] Object of type `Spider | None` has no attribute `meta`
+ tests/test_crawl.py:721:16: error[unresolved-attribute] Object of type `Spider | None` has no attribute `meta`
+ tests/test_crawl.py:722:27: error[unresolved-attribute] Object of type `Spider | None` has no attribute `meta`
+ tests/test_crawl.py:723:27: error[unresolved-attribute] Object of type `Spider | None` has no attribute `meta`
+ tests/test_crawl.py:724:27: error[unresolved-attribute] Object of type `Spider | None` has no attribute `meta`
+ tests/test_crawl.py:725:16: error[unresolved-attribute] Object of type `Spider | None` has no attribute `meta`
+ tests/test_crawl.py:727:37: error[unresolved-attribute] Object of type `Spider | None` has no attribute `meta`
- tests/test_downloadermiddleware.py:305:56: error[invalid-argument-type] Argument to bound method `download` is incorrect: Expected `Spider`, found `Unknown | Spider | None`
+ tests/test_downloadermiddleware.py:305:56: error[invalid-argument-type] Argument to bound method `download` is incorrect: Expected `Spider`, found `Spider | None`
- tests/test_downloaderslotssettings.py:75:17: warning[possibly-missing-attribute] Attribute `downloader` may be missing on object of type `Unknown | ExecutionEngine | None`
+ tests/test_downloaderslotssettings.py:75:17: warning[possibly-missing-attribute] Attribute `downloader` may be missing on object of type `ExecutionEngine | None`
- tests/test_downloaderslotssettings.py:76:17: warning[possibly-missing-attribute] Attribute `times` may be missing on object of type `Unknown | Spider | None`
+ tests/test_downloaderslotssettings.py:76:17: error[unresolved-attribute] Object of type `Spider | None` has no attribute `times`
+ tests/test_engine_loop.py:48:17: error[unresolved-attribute] Object of type `BaseScheduler` has no attribute `pause`
- tests/test_engine_loop.py:48:17: warning[possibly-missing-attribute] Attribute `_slot` may be missing on object of type `Unknown | ExecutionEngine | None`
+ tests/test_engine_loop.py:48:17: warning[possibly-missing-attribute] Attribute `_slot` may be missing on object of type `ExecutionEngine | None`
- tests/test_engine_loop.py:48:17: warning[possibly-missing-attribute] Attribute `scheduler` may be missing on object of type `Unknown | _Slot | None`
+ tests/test_engine_loop.py:48:17: warning[possibly-missing-attribute] Attribute `scheduler` may be missing on object of type `_Slot | None`
- tests/test_engine_loop.py:48:17: warning[possibly-missing-attribute] Attribute `pause` may be missing on object of type `Unknown | BaseScheduler`
- tests/test_engine_loop.py:49:17: warning[possibly-missing-attribute] Attribute `_slot` may be missing on object of type `Unknown | ExecutionEngine | None`
+ tests/test_engine_loop.py:49:17: warning[possibly-missing-attribute] Attribute `_slot` may be missing on object of type `ExecutionEngine | None`
- tests/test_engine_loop.py:49:17: warning[possibly-missing-attribute] Attribute `scheduler` may be missing on object of type `Unknown | _Slot | None`
+ tests/test_engine_loop.py:49:17: warning[possibly-missing-attribute] Attribute `scheduler` may be missing on object of type `_Slot | None`
+ tests/test_engine_loop.py:55:17: error[unresolved-attribute] Object of type `BaseScheduler` has no attribute `unpause`
- tests/test_engine_loop.py:55:17: warning[possibly-missing-attribute] Attribute `_slot` may be missing on object of type `Unknown | ExecutionEngine | None`
+ tests/test_engine_loop.py:55:17: warning[possibly-missing-attribute] Attribute `_slot` may be missing on object of type `ExecutionEngine | None`
- tests/test_engine_loop.py:55:17: warning[possibly-missing-attribute] Attribute `scheduler` may be missing on object of type `Unknown | _Slot | None`
+ tests/test_engine_loop.py:55:17: warning[possibly-missing-attribute] Attribute `scheduler` may be missing on object of type `_Slot | None`
- tests/test_engine_loop.py:55:17: warning[possibly-missing-attribute] Attribute `unpause` may be missing on object of type `Unknown | BaseScheduler`
+ tests/test_engine_loop.py:64:17: error[unresolved-attribute] Object of type `BaseScheduler` has no attribute `pause`
- tests/test_engine_loop.py:64:17: warning[possibly-missing-attribute] Attribute `_slot` may be missing on object of type `Unknown | ExecutionEngine | None`
+ tests/test_engine_loop.py:64:17: warning[possibly-missing-attribute] Attribute `_slot` may be missing on object of type `ExecutionEngine | None`
- tests/test_engine_loop.py:64:17: warning[possibly-missing-attribute] Attribute `scheduler` may be missing on object of type `Unknown | _Slot | None`
+ tests/test_engine_loop.py:64:17: warning[possibly-missing-attribute] Attribute `scheduler` may be missing on object of type `_Slot | None`
- tests/test_engine_loop.py:64:17: warning[possibly-missing-attribute] Attribute `pause` may be missing on object of type `Unknown | BaseScheduler`
- tests/test_engine_loop.py:65:17: warning[possibly-missing-attribute] Attribute `_slot` may be missing on object of type `Unknown | ExecutionEngine | None`
+ tests/test_engine_loop.py:65:17: warning[possibly-missing-attribute] Attribute `_slot` may be missing on object of type `ExecutionEngine | None`
- tests/test_engine_loop.py:65:17: warning[possibly-missing-attribute] Attribute `scheduler` may be missing on object of type `Unknown | _Slot | None`
+ tests/test_engine_loop.py:65:17: warning[possibly-missing-attribute] Attribute `scheduler` may be missing on object of type `_Slot | None`
+ tests/test_engine_loop.py:71:44: error[unresolved-attribute] Object of type `BaseScheduler` has no attribute `unpause`
- tests/test_engine_loop.py:71:44: warning[possibly-missing-attribute] Attribute `_slot` may be missing on object of type `Unknown | ExecutionEngine | None`
+ tests/test_engine_loop.py:71:44: warning[possibly-missing-attribute] Attribute `_slot` may be missing on object of type `ExecutionEngine | None`
- tests/test_engine_loop.py:71:44: warning[possibly-missing-attribute] Attribute `scheduler` may be missing on object of type `Unknown | _Slot | None`
+ tests/test_engine_loop.py:71:44: warning[possibly-missing-attribute] Attribute `scheduler` may be missing on object of type `_Slot | None`
- tests/test_engine_loop.py:71:44: warning[possibly-missing-attribute] Attribute `unpause` may be missing on object of type `Unknown | BaseScheduler`
+ tests/test_pqueues.py:101:9: error[invalid-assignment] Object of type `MockEngine` is not assignable to attribute `engine` of type `ExecutionEngine | None`
- tests/test_proxy_connect.py:110:27: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_proxy_connect.py:110:27: error[unresolved-attribute] Object of type `Spider | None` has no attribute `meta`
- tests/test_request_attribute_binding.py:80:20: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
- tests/test_request_attribute_binding.py:89:23: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
- tests/test_request_attribute_binding.py:106:19: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
- tests/test_request_attribute_binding.py:139:20: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
- tests/test_request_attribute_binding.py:172:20: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
- tests/test_request_attribute_binding.py:195:20: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_request_attribute_binding.py:80:20: error[unresolved-attribute] Object of type `Spider | None` has no attribute `meta`
+ tests/test_request_attribute_binding.py:89:23: error[unresolved-attribute] Object of type `Spider | None` has no attribute `meta`
+ tests/test_request_attribute_binding.py:106:19: error[unresolved-attribute] Object of type `Spider | None` has no attribute `meta`
+ tests/test_request_attribute_binding.py:139:20: error[unresolved-attribute] Object of type `Spider | None` has no attribute `meta`
+ tests/test_request_attribute_binding.py:172:20: error[unresolved-attribute] Object of type `Spider | None` has no attribute `meta`
+ tests/test_request_attribute_binding.py:195:20: error[unresolved-attribute] Object of type `Spider | None` has no attribute `meta`
- tests/test_request_cb_kwargs.py:166:20: warning[possibly-missing-attribute] Attribute `checks` may be missing on object of type `Unknown | Spider | None`
- tests/test_request_cb_kwargs.py:167:20: warning[possibly-missing-attribute] Attribute `checks` may be missing on object of type `Unknown | Spider | None`
+ tests/test_request_cb_kwargs.py:166:20: error[unresolved-attribute] Object of type `Spider | None` has no attribute `checks`
+ tests/test_request_cb_kwargs.py:167:20: error[unresolved-attribute] Object of type `Spider | None` has no attribute `checks`
- tests/test_request_left.py:40:16: warning[possibly-missing-attribute] Attribute `caught_times` may be missing on object of type `Unknown | Spider | None`
- tests/test_request_left.py:46:16: warning[possibly-missing-attribute] Attribute `caught_times` may be missing on object of type `Unknown | Spider | None`
- tests/test_request_left.py:52:16: warning[possibly-missing-attribute] Attribute `caught_times` may be missing on object of type `Unknown | Spider | None`
- tests/test_request_left.py:58:16: warning[possibly-missing-attribute] Attribute `caught_times` may be missing on object of type `Unknown | Spider | None`
+ tests/test_request_left.py:40:16: error[unresolved-attribute] Object of type `Spider | None` has no attribute `caught_times`
+ tests/test_request_left.py:46:16: error[unresolved-attribute] Object of type `Spider | None` has no attribute `caught_times`
+ tests/test_request_left.py:52:16: error[unresolved-attribute] Object of type `Spider | None` has no attribute `caught_times`
+ tests/test_request_left.py:58:16: error[unresolved-attribute] Object of type `Spider | None` has no attribute `caught_times`
+ tests/test_scheduler.py:98:9: error[invalid-assignment] Object of type `MockEngine` is not assignable to attribute `engine` of type `ExecutionEngine | None`
- tests/test_spidermiddleware_httperror.py:208:20: warning[possibly-missing-attribute] Attribute `skipped` may be missing on object of type `Unknown | Spider | None`
- tests/test_spidermiddleware_httperror.py:209:16: warning[possibly-missing-attribute] Attribute `parsed` may be missing on object of type `Unknown | Spider | None`
- tests/test_spidermiddleware_httperror.py:210:16: warning[possibly-missing-attribute] Attribute `failed` may be missing on object of type `Unknown | Spider | None`
+ tests/test_spidermiddleware_httperror.py:208:20: error[unresolved-attribute] Object of type `Spider | None` has no attribute `skipped`
+ tests/test_spidermiddleware_httperror.py:209:16: error[unresolved-attribute] Object of type `Spider | None` has no attribute `parsed`
+ tests/test_spidermiddleware_httperror.py:210:16: error[unresolved-attribute] Object of type `Spider | None` has no attribute `failed`
- tests/test_spidermiddleware_httperror.py:223:16: warning[possibly-missing-attribute] Attribute `parsed` may be missing on object of type `Unknown | Spider | None`
- tests/test_spidermiddleware_httperror.py:224:16: warning[possibly-missing-attribute] Attribute `skipped` may be missing on object of type `Unknown | Spider | None`
- tests/test_spidermiddleware_httperror.py:225:16: warning[possibly-missing-attribute] Attribute `failed` may be missing on object of type `Unknown | Spider | None`
- tests/test_spidermiddleware_httperror.py:238:16: warning[possibly-missing-attribute] Attribute `parsed` may be missing on object of type `Unknown | Spider | None`
- tests/test_spidermiddleware_httperror.py:239:16: warning[possibly-missing-attribute] Attribute `failed` may be missing on object of type `Unknown | Spider | None`
- tests/test_spidermiddleware_httperror.py:250:16: warning[possibly-missing-attribute] Attribute `parsed` may be missing on object of type `Unknown | Spider | None`
- tests/test_spidermiddleware_httperror.py:251:16: warning[possibly-missing-attribute] Attribute `failed` may be missing on object of type `Unknown | Spider | None`
+ tests/test_spidermiddleware_httperror.py:223:16: error[unresolved-attribute] Object of type `Spider | None` has no attribute `parsed`
+ tests/test_spidermiddleware_httperror.py:224:16: error[unresolved-attribute] Object of type `Spider | None` has no attribute `skipped`
+ tests/test_spidermiddleware_httperror.py:225:16: error[unresolved-attribute] Object of type `Spider | None` has no attribute `failed`
+ tests/test_spidermiddleware_httperror.py:238:16: error[unresolved-attribute] Object of type `Spider | None` has no attribute `parsed`
+ tests/test_spidermiddleware_httperror.py:239:16: error[unresolved-attribute] Object of type `Spider | None` has no attribute `failed`
+ tests/test_spidermiddleware_httperror.py:250:16: error[unresolved-attribute] Object of type `Spider | None` has no attribute `parsed`
+ tests/test_spidermiddleware_httperror.py:251:16: error[unresolved-attribute] Object of type `Spider | None` has no attribute `failed`
- Found 1789 diagnostics
+ Found 1791 diagnostics

ignite (https://github.com/pytorch/ignite)
+ tests/ignite/metrics/test_precision.py:461:17: warning[possibly-missing-attribute] Attribute `device` may be missing on object of type `int | Unknown`
+ tests/ignite/metrics/test_precision.py:462:24: warning[possibly-missing-attribute] Attribute `device` may be missing on object of type `int | Unknown`
+ tests/ignite/metrics/test_precision.py:462:48: warning[possibly-missing-attribute] Attribute `device` may be missing on object of type `int | Unknown`
+ tests/ignite/metrics/test_precision.py:467:21: warning[possibly-missing-attribute] Attribute `device` may be missing on object of type `int | Unknown`
+ tests/ignite/metrics/test_precision.py:468:28: warning[possibly-missing-attribute] Attribute `device` may be missing on object of type `int | Unknown`
+ tests/ignite/metrics/test_precision.py:468:54: warning[possibly-missing-attribute] Attribute `device` may be missing on object of type `int | Unknown`
+ tests/ignite/metrics/test_precision.py:471:24: warning[possibly-missing-attribute] Attribute `device` may be missing on object of type `int | Unknown`
+ tests/ignite/metrics/test_precision.py:471:68: warning[possibly-missing-attribute] Attribute `device` may be missing on object of type `int | Unknown`
+ tests/ignite/metrics/test_precision.py:471:89: warning[possibly-missing-attribute] Attribute `device` may be missing on object of type `int | Unknown`
+ tests/ignite/metrics/test_precision.py:494:17: warning[possibly-missing-attribute] Attribute `device` may be missing on object of type `int | Unknown`
+ tests/ignite/metrics/test_precision.py:495:24: warning[possibly-missing-attribute] Attribute `device` may be missing on object of type `int | Unknown`
+ tests/ignite/metrics/test_precision.py:495:48: warning[possibly-missing-attribute] Attribute `device` may be missing on object of type `int | Unknown`
+ tests/ignite/metrics/test_precision.py:500:21: warning[possibly-missing-attribute] Attribute `device` may be missing on object of type `int | Unknown`
+ tests/ignite/metrics/test_precision.py:501:28: warning[possibly-missing-attribute] Attribute `device` may be missing on object of type `int | Unknown`
+ tests/ignite/metrics/test_precision.py:501:54: warning[possibly-missing-attribute] Attribute `device` may be missing on object of type `int | Unknown`
+ tests/ignite/metrics/test_precision.py:504:24: warning[possibly-missing-attribute] Attribute `device` may be missing on object of type `int | Unknown`
+ tests/ignite/metrics/test_precision.py:504:68: warning[possibly-missing-attribute] Attribute `device` may be missing on object of type `int | Unknown`
+ tests/ignite/metrics/test_precision.py:504:89: warning[possibly-missing-attribute] Attribute `device` may be missing on object of type `int | Unknown`
+ tests/ignite/metrics/test_recall.py:463:17: warning[possibly-missing-attribute] Attribute `device` may be missing on object of type `int | Unknown`
+ tests/ignite/metrics/test_recall.py:464:24: warning[possibly-missing-attribute] Attribute `device` may be missing on object of type `int | Unknown`
+ tests/ignite/metrics/test_recall.py:464:48: warning[possibly-missing-attribute] Attribute `device` may be missing on object of type `int | Unknown`
+ tests/ignite/metrics/test_recall.py:469:21: warning[possibly-missing-attribute] Attribute `device` may be missing on object of type `int | Unknown`
+ tests/ignite/metrics/test_recall.py:470:28: warning[possibly-missing-attribute] Attribute `device` may be missing on object of type `int | Unknown`
+ tests/ignite/metrics/test_recall.py:470:54: warning[possibly-missing-attribute] Attribute `device` may be missing on object of type `int | Unknown`
+ tests/ignite/metrics/test_recall.py:473:24: warning[possibly-missing-attribute] Attribute `device` may be missing on object of type `int | Unknown`
+ tests/ignite/metrics/test_recall.py:473:68: warning[possibly-missing-attribute] Attribute `device` may be missing on object of type `int | Unknown`
+ tests/ignite/metrics/test_recall.py:473:89: warning[possibly-missing-attribute] Attribute `device` may be missing on object of type `int | Unknown`
+ tests/ignite/metrics/test_recall.py:497:17: warning[possibly-missing-attribute] Attribute `device` may be missing on object of type `int | Unknown`
+ tests/ignite/metrics/test_recall.py:498:24: warning[possibly-missing-attribute] Attribute `device` may be missing on object of type `int | Unknown`
+ tests/ignite/metrics/test_recall.py:498:48: warning[possibly-missing-attribute] Attribute `device` may be missing on object of type `int | Unknown`
+ tests/ignite/metrics/test_recall.py:503:21: warning[possibly-missing-attribute] Attribute `device` may be missing on object of type `int | Unknown`
+ tests/ignite/metrics/test_recall.py:504:28: warning[possibly-missing-attribute] Attribute `device` may be missing on object of type `int | Unknown`
+ tests/ignite/metrics/test_recall.py:504:54: warning[possibly-missing-attribute] Attribute `device` may be missing on object of type `int | Unknown`
+ tests/ignite/metrics/test_recall.py:507:24: warning[possibly-missing-attribute] Attribute `device` may be missing on object of type `int | Unknown`
+ tests/ignite/metrics/test_recall.py:507:68: warning[possibly-missing-attribute] Attribute `device` may be missing on object of type `int | Unknown`
+ tests/ignite/metrics/test_recall.py:507:89: warning[possibly-missing-attribute] Attribute `device` may be missing on object of type `int | Unknown`
+ tests/ignite/metrics/test_ssim.py:176:12: warning[possibly-missing-attribute] Attribute `device` may be missing on object of type `Unknown | None`
- Found 2034 diagnostics
+ Found 2071 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- examples/contrib/search.py:68:23: warning[possibly-missing-attribute] Attribute `findall` may be missing on object of type `Unknown | Pattern[str] | None`
+ examples/contrib/search.py:68:23: warning[possibly-missing-attribute] Attribute `findall` may be missing on object of type `Unknown | None | Pattern[str]`
- examples/contrib/search.py:73:41: warning[possibly-missing-attribute] Attribute `findall` may be missing on object of type `Unknown | Pattern[str] | None`
+ examples/contrib/search.py:73:41: warning[possibly-missing-attribute] Attribute `findall` may be missing on object of type `Unknown | None | Pattern[str]`
- examples/contrib/search.py:75:45: warning[possibly-missing-attribute] Attribute `findall` may be missing on object of type `Unknown | Pattern[str] | None`
+ examples/contrib/search.py:75:45: warning[possibly-missing-attribute] Attribute `findall` may be missing on object of type `Unknown | None | Pattern[str]`
- examples/contrib/search.py:78:49: warning[possibly-missing-attribute] Attribute `findall` may be missing on object of type `Unknown | Pattern[str] | None`
+ examples/contrib/search.py:78:49: warning[possibly-missing-attribute] Attribute `findall` may be missing on object of type `Unknown | None | Pattern[str]`
- examples/contrib/search.py:82:50: warning[possibly-missing-attribute] Attribute `findall` may be missing on object of type `Unknown | Pattern[str] | None`
+ examples/contrib/search.py:82:50: warning[possibly-missing-attribute] Attribute `findall` may be missing on object of type `Unknown | None | Pattern[str]`
- mitmproxy/tools/console/flowview.py:350:33: error[invalid-assignment] Object of type `Unknown | None` is not assignable to `Message`
- Found 2143 diagnostics
+ Found 2142 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
+ tests/unittests/sources/test_azure.py:2878:9: error[invalid-assignment] Object of type `Literal["md"]` is not assignable to attribute `metadata` of type `dict[Unknown, Unknown]`
- Found 1168 diagnostics
+ Found 1169 diagnostics

meson (https://github.com/mesonbuild/meson)
- mesonbuild/interpreter/interpreter.py:1184:79: error[invalid-argument-type] Argument to bound method `initialize_from_top_level_project_call` is incorrect: Expected `dict[OptionKey, str | int | list[str]]`, found `list[str]`
+ mesonbuild/interpreter/interpreter.py:1181:9: error[invalid-assignment] Object of type `list[str]` is not assignable to attribute `project_default_options` of type `dict[OptionKey, str | int | list[str]]`
- mesonbuild/interpreter/interpreter.py:1190:72: error[invalid-argument-type] Argument to bound method `initialize_from_subproject_call` is incorrect: Expected `dict[OptionKey, str | int | list[str]]`, found `list[str]`
- Found 2154 diagnostics
+ Found 2153 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
- discord/utils.py:243:54: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 540 diagnostics
+ Found 539 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/integrations/prefect-kubernetes/prefect_kubernetes/jobs.py:499:16: error[invalid-return-type] Return type does not match returned value: expected `dict[str, Any]`, found `Unknown | dict[Unknown, Unknown] | None`
+ src/integrations/prefect-kubernetes/prefect_kubernetes/jobs.py:499:16: error[invalid-return-type] Return type does not match returned value: expected `dict[str, Any]`, found `Unknown | None | dict[Unknown, Unknown]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
- src/prefect/infrastructure/provisioners/container_instance.py:751:17: error[invalid-argument-type] Argument to bound method `read_block_document_by_name` is incorrect: Expected `str`, found `Unknown | str | None`
+ src/prefect/infrastructure/provisioners/container_instance.py:751:17: error[invalid-argument-type] Argument to bound method `read_block_document_by_name` is incorrect: Expected `str`, found `Unknown | None | str`
- src/prefect/server/orchestration/global_policy.py:460:13: warning[possibly-missing-attribute] Attribute `state_details` may be missing on object of type `(State & @Todo) | None`
+ src/prefect/server/orchestration/global_policy.py:460:13: warning[possibly-missing-attribute] Attribute `state_details` may be missing on object of type `State | None`
- src/prefect/testing/fixtures.py:434:27: warning[possibly-missing-attribute] Attribute `events` may be missing on object of type `Unknown | EventsClient`
- src/prefect/testing/fixtures.py:442:113: warning[possibly-missing-attribute] Attribute `events` may be missing on object of type `Unknown | EventsClient`
- src/prefect/testing/fixtures.py:448:26: warning[possibly-missing-attribute] Attribute `pop_events` may be missing on object of type `Unknown | EventsClient`
-

... (truncated 143 lines) ...
```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
-     struct fields = ~12MB
+     struct fields = ~11MB

sphinx (https://github.com/sphinx-doc/sphinx)
- TOTAL MEMORY USAGE: ~301MB
+ TOTAL MEMORY USAGE: ~287MB


```

</details>




---

_Marked ready for review by @carljm on 2026-01-20 22:06_

---

_Review requested from @AlexWaygood by @carljm on 2026-01-20 22:06_

---

_Review requested from @sharkdp by @carljm on 2026-01-20 22:06_

---

_Review requested from @dcreager by @carljm on 2026-01-20 22:06_

---

_Label `ecosystem-analyzer` added by @carljm on 2026-01-20 22:06_

---

_Comment by @astral-sh-bot[bot] on 2026-01-20 22:12_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `possibly-missing-attribute` | 80 | 94 | 42 |
| `unresolved-attribute` | 94 | 0 | 0 |
| `invalid-argument-type` | 18 | 2 | 10 |
| `no-matching-overload` | 21 | 0 | 0 |
| `invalid-assignment` | 6 | 1 | 5 |
| `not-subscriptable` | 7 | 0 | 0 |
| `invalid-return-type` | 0 | 1 | 5 |
| `not-iterable` | 3 | 0 | 0 |
| `unsupported-operator` | 0 | 0 | 3 |
| `unused-ignore-comment` | 0 | 2 | 0 |
| `parameter-already-assigned` | 0 | 1 | 0 |
| **Total** | **229** | **101** | **65** |


**[Full report with detailed diff](https://338b5a1e.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://338b5a1e.ty-ecosystem-ext.pages.dev/timing))



---

_Comment by @carljm on 2026-01-21 01:27_

The new diagnostics in the ecosystem report look like true positives. Home-assistant is relying on a pattern like this:

```py
class CoinbaseData:
      def __init__(self, client, exchange_base):
          self.accounts = None

      @Throttle(MIN_TIME_BETWEEN_UPDATES)  # <- Unknown decorator
      def update(self):
          self.accounts = get_accounts(self.client)      # <- Assigns list
```

This code implicitly relies on the `update` method being called before the `accounts` attribute is ever used. But this isn't an invariant we can guarantee; the new diagnostic is correct.

The reason why `None` wasn't previously included in the type of `self.accounts` in this particular case is a bit complex, and looks like it has to do with Salsa cycles introduced by the use of `.class_member(...)` in `is_valid_scope`. The new version infers the types of decorators directly, without needing to go through the entire `class_member()` call, and is thus less likely to introduce cycles -- a side benefit!

The new parso diagnostics have the same basic cause, and are also true positives.

---

_Comment by @carljm on 2026-01-21 03:59_

The sklearn diagnostics are similar to home-assistant and parso; they generally follow the pattern "we understand more types as something other than Unknown, so we get more diagnostics". The one wrinkle with sklearn is that some of the new types we understand also trigger #350.

---
