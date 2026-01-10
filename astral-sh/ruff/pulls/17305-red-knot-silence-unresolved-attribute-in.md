```yaml
number: 17305
title: "[red-knot] Silence `unresolved-attribute` in unreachable code"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/unreachable-attribute-expressions
created_at: 2025-04-09T08:34:22Z
updated_at: 2025-04-10T16:32:55Z
url: https://github.com/astral-sh/ruff/pull/17305
synced_at: 2026-01-10T19:40:37Z
```

# [red-knot] Silence `unresolved-attribute` in unreachable code

---

_Pull request opened by @sharkdp on 2025-04-09 08:34_

## Summary

Basically just repeat the same thing that we did for `unresolved-reference`, but now for attribute expressions.

We now also handle the case where the unresolved attribute (or the unresolved reference) diagnostic originates from a stringified type annotation.

And I made the evaluation of reachability constraints lazy (will only be evaluated right before we are about to emit a diagnostic).

## Test Plan

New Markdown tests for stringified annotations.

---

_Label `red-knot` added by @sharkdp on 2025-04-09 08:34_

---

_Comment by @github-actions[bot] on 2025-04-09 08:36_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
zipp (https://github.com/jaraco/zipp)
+ warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/zipp/zipp/compat/py310.py:10:23: Unused blanket `type: ignore` directive
- Found 2 diagnostics
+ Found 3 diagnostics

pyp (https://github.com/hauntsaninja/pyp)
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/pyp/pyp.py:604:16: Type `<module 'ast'>` has no attribute `unparse`
- Found 20 diagnostics
+ Found 19 diagnostics

pybind11 (https://github.com/pybind/pybind11)
- dependency graph cycle querying try_metaclass_(Id(26c02)); set cycle_fn/cycle_initial to fixpoint iterate
+ dependency graph cycle querying try_metaclass_(Id(30c04)); set cycle_fn/cycle_initial to fixpoint iterate
- dependency graph cycle querying try_metaclass_(Id(2b012)); set cycle_fn/cycle_initial to fixpoint iterate
+ dependency graph cycle querying try_metaclass_(Id(1e018)); set cycle_fn/cycle_initial to fixpoint iterate
- dependency graph cycle querying member_lookup_with_policy_(Id(1c0aa)); set cycle_fn/cycle_initial to fixpoint iterate
+ dependency graph cycle querying member_lookup_with_policy_(Id(19cf5)); set cycle_fn/cycle_initial to fixpoint iterate

werkzeug (https://github.com/pallets/werkzeug)
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/_reloader.py:176:34: Type `<module 'sys'>` has no attribute `orig_argv`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/debug/__init__.py:106:22: Type `<module 'winreg'>` has no attribute `OpenKey`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/debug/__init__.py:107:21: Type `<module 'winreg'>` has no attribute `HKEY_LOCAL_MACHINE`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/debug/__init__.py:110:21: Type `<module 'winreg'>` has no attribute `KEY_READ`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/debug/__init__.py:110:39: Type `<module 'winreg'>` has no attribute `KEY_WOW64_64KEY`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/debug/__init__.py:114:39: Type `<module 'winreg'>` has no attribute `QueryValueEx`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/debug/__init__.py:114:39: Type `<module 'winreg'>` has no attribute `QueryValueEx`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/debug/__init__.py:114:39: Type `<module 'winreg'>` has no attribute `QueryValueEx`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/debug/__init__.py:116:37: Type `<module 'winreg'>` has no attribute `REG_SZ`
- Found 768 diagnostics
+ Found 759 diagnostics

rich (https://github.com/Textualize/rich)
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/rich/_win32_console.py:12:35: Type `<module 'ctypes'>` has no attribute `WinDLL`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/rich/_windows.py:20:32: Type `<module 'ctypes'>` has no attribute `WinDLL`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/rich/_windows.py:56:27: Type `<module 'sys'>` has no attribute `getwindowsversion`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/rich/traceback.py:464:38: Type `BaseException` has no attribute `exceptions`
- Found 793 diagnostics
+ Found 789 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/scrapy/tests/CrawlerProcess/asyncio_enabled_reactor_same_loop.py:8:35: Type `<module 'asyncio'>` has no attribute `WindowsSelectorEventLoopPolicy`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/scrapy/tests/CrawlerProcess/asyncio_enabled_reactor_different_loop.py:8:35: Type `<module 'asyncio'>` has no attribute `WindowsSelectorEventLoopPolicy`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/scrapy/scrapy/utils/reactor.py:84:17: Type `<module 'asyncio'>` has no attribute `WindowsSelectorEventLoopPolicy`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/scrapy/scrapy/utils/reactor.py:86:18: Type `<module 'asyncio'>` has no attribute `WindowsSelectorEventLoopPolicy`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/scrapy/tests/test_crawler.py:892:61: Type `<module 'signal'>` has no attribute `SIGBREAK`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/scrapy/tests/test_crawler.py:906:61: Type `<module 'signal'>` has no attribute `SIGBREAK`
- Found 1570 diagnostics
+ Found 1564 diagnostics

```
</details>


---

_Comment by @MichaReiser on 2025-04-09 08:59_

The new error in `zipp` is interesting... It raises questions whether we should raise unused ignore for `type: ignore` and in which cases

```py
text_encoding = (
    io.text_encoding  # type: ignore[unused-ignore, attr-defined]
    if sys.version_info > (3, 10)
    else _text_encoding
)
```

---

_Renamed from "[red-knot] Silence unresolved-attribute diagnostics" to "[red-knot] Silence `unresolved-attribute` diagnostics in unreachable code" by @sharkdp on 2025-04-10 13:45_

---

_Renamed from "[red-knot] Silence `unresolved-attribute` diagnostics in unreachable code" to "[red-knot] Silence `unresolved-attribute` in unreachable code" by @sharkdp on 2025-04-10 13:46_

---

_Marked ready for review by @sharkdp on 2025-04-10 14:05_

---

_Review requested from @carljm by @sharkdp on 2025-04-10 14:05_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-04-10 14:05_

---

_Review requested from @dcreager by @sharkdp on 2025-04-10 14:05_

---

_Comment by @sharkdp on 2025-04-10 14:07_

> The new error in `zipp` is interesting... It raises questions whether we should raise unused ignore for `type: ignore` and in which cases

@MichaReiser Is this something specific to this PR, or just a general thought? Anything that should be done about it?

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:364 on 2025-04-10 14:24_

```suggestion
    /// be unreachable. Use [`super::SemanticIndex::is_expression_reachable`] for the global
    /// analysis.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:379 on 2025-04-10 14:25_

```suggestion
                    .expect("`is_expression_reachable` should only be called on expressions with recorded reachability"),
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:937 on 2025-04-10 14:29_

```suggestion
        self.bindings_by_use.shrink_to_fit();
        self.expression_reachability.shrink_to_fit();
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4536 on 2025-04-10 14:35_

it looks like computing `bound_on_instance` might be somewhat expensive, but we're doing it eagerly at the top of this branch whether or not `report_unresolved_attribute` resolves to `true`. Maybe turn that block of code into a lazy closure, so that we only evaluate it if we actually need to?

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:7478 on 2025-04-10 14:36_

```suggestion
    /// stringified.
    ///
    /// This variant wraps a [`ScopedExpressionId`] that allows us to retrieve
    /// the original [`ast::ExprStringLiteral`] node which created the string annotation
```

---

_@AlexWaygood approved on 2025-04-10 14:37_

Nice, this LGTM

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:4536 on 2025-04-10 14:56_

Thanks. It's enough to just move that definition into the `if report_unresolved_attribute` branch.

---

_@sharkdp reviewed on 2025-04-10 14:56_

---

_Comment by @sharkdp on 2025-04-10 15:15_

I manually checked that nothing changed in pybind11, so I'm ignoring the spurious cycle-panic-diff there.

---

_Merged by @sharkdp on 2025-04-10 15:15_

---

_Closed by @sharkdp on 2025-04-10 15:15_

---

_Branch deleted on 2025-04-10 15:15_

---

_Comment by @MichaReiser on 2025-04-10 16:32_

> > The new error in `zipp` is interesting... It raises questions whether we should raise unused ignore for `type: ignore` and in which cases
> 
> @MichaReiser Is this something specific to this PR, or just a general thought? Anything that should be done about it?

I'll create a separate issue for it

---
