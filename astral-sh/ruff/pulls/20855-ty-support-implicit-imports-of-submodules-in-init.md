```yaml
number: 20855
title: "[ty] Support implicit imports of submodules in `__init__.pyi`"
type: pull_request
state: merged
author: Gankra
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: gankra/implort3
created_at: 2025-10-14T04:59:26Z
updated_at: 2025-10-31T14:42:14Z
url: https://github.com/astral-sh/ruff/pull/20855
synced_at: 2026-01-10T16:59:49Z
```

# [ty] Support implicit imports of submodules in `__init__.pyi`

---

_Pull request opened by @Gankra on 2025-10-14 04:59_

This is a second take at the implicit imports approach, allowing `from . import submodule` in an `__init__.pyi` to create the `mypackage.submodule` attribute everyhere.

This implementation operates inside of the available_submodule_attributes subsystem instead of as a re-export rule.

The upside of this is we are no longer purely syntactic, and absolute from imports that happen to target submodules work (an intentional discussed deviation from pyright which demands a relative from import). Also we don't re-export functions or classes.

The downside(?) of this is star imports no longer see these attributes (this may be either good or bad. I believe it's not a huge lift to make it work with star imports but it's some non-trivial reworking).

I've also intentionally made `import mypackage.submodule` not trigger this rule although it's trivial to change that.

I've tried to cover as many relevant cases as possible for discussion in the new test file I've added (there are some random overlaps with existing tests but trying to add them piecemeal felt confusing and weird, so I just made a dedicated file for this extension to the rules).

Fixes https://github.com/astral-sh/ty/issues/133

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Review requested from @carljm by @Gankra on 2025-10-14 04:59_

---

_Review requested from @AlexWaygood by @Gankra on 2025-10-14 04:59_

---

_Review requested from @sharkdp by @Gankra on 2025-10-14 04:59_

---

_Review requested from @dcreager by @Gankra on 2025-10-14 04:59_

---

_Review requested from @MichaReiser by @Gankra on 2025-10-14 04:59_

---

_Comment by @github-actions[bot] on 2025-10-14 05:01_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-14 05:02_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
asynq (https://github.com/quora/asynq)
- asynq/async_task.py:78:24: error[unresolved-attribute] Module `asynq` has no member `scheduler`
- asynq/async_task.py:115:9: error[unresolved-attribute] Module `asynq` has no member `scheduler`
+ asynq/async_task.py:318:18: warning[possibly-missing-attribute] Attribute `traceback` may be missing on object of type `(Unknown & ~None) | FutureBase[Unknown]`
- asynq/contexts.py:59:19: error[unresolved-attribute] Module `asynq` has no member `scheduler`
+ asynq/contexts.py:59:19: error[unresolved-attribute] Module `asynq.scheduler` has no member `_state`
+ asynq/tests/test_debug.py:30:32: error[invalid-argument-type] Argument to function `dump_error` is incorrect: Expected `BaseException`, found `None`
+ asynq/tests/test_debug.py:40:46: error[invalid-argument-type] Argument to function `format_error` is incorrect: Expected `BaseException`, found `None`
- asynq/tests/test_debug.py:30:9: error[unresolved-attribute] Module `asynq` has no member `debug`
- asynq/tests/test_debug.py:40:21: error[unresolved-attribute] Module `asynq` has no member `debug`
- asynq/tests/test_debug.py:43:5: error[unresolved-attribute] Module `asynq` has no member `debug`
- asynq/tests/test_debug.py:46:25: error[unresolved-attribute] Module `asynq` has no member `debug`
- asynq/tests/test_debug.py:49:17: error[unresolved-attribute] Module `asynq` has no member `debug`
- asynq/tests/test_debug.py:57:17: error[unresolved-attribute] Module `asynq` has no member `debug`
- asynq/tests/test_debug.py:62:5: error[unresolved-attribute] Module `asynq` has no member `debug`
- asynq/tests/test_debug.py:65:17: error[unresolved-attribute] Module `asynq` has no member `debug`
- asynq/tests/test_debug.py:80:5: error[unresolved-attribute] Module `asynq` has no member `debug`
- asynq/tests/test_debug.py:81:17: error[unresolved-attribute] Module `asynq` has no member `debug`
- asynq/tests/test_debug.py:87:5: error[unresolved-attribute] Module `asynq` has no member `debug`
- asynq/tests/test_debug.py:88:17: error[unresolved-attribute] Module `asynq` has no member `debug`
- asynq/tests/test_debug.py:100:13: error[unresolved-attribute] Module `asynq` has no member `debug`
- asynq/tests/test_debug.py:115:32: error[unresolved-attribute] Module `asynq` has no member `debug`
+ asynq/tests/test_debug.py:199:60: error[invalid-argument-type] Argument to function `extract_tb` is incorrect: Expected `TracebackType`, found `None | @Todo`
- asynq/tests/test_debug.py:139:9: error[unresolved-attribute] Module `asynq` has no member `debug`
- asynq/tests/test_debug.py:199:37: error[unresolved-attribute] Module `asynq` has no member `debug`
- asynq/tests/test_debug.py:249:31: error[unresolved-attribute] Module `asynq` has no member `debug`
+ asynq/tests/test_debug.py:249:53: error[invalid-argument-type] Argument to function `format_tb` is incorrect: Expected `TracebackType`, found `None | @Todo`
- asynq/tests/test_debug.py:259:26: error[unresolved-attribute] Module `asynq` has no member `debug`
+ asynq/tests/test_debug.py:306:38: error[invalid-argument-type] Argument to function `filter_traceback` is incorrect: Expected `list[str]`, found `list[LiteralString]`
+ asynq/tests/test_debug.py:323:38: error[invalid-argument-type] Argument to function `filter_traceback` is incorrect: Expected `list[str]`, found `list[LiteralString]`
+ asynq/tests/test_debug.py:355:38: error[invalid-argument-type] Argument to function `filter_traceback` is incorrect: Expected `list[str]`, found `list[LiteralString]`
+ asynq/tests/test_debug.py:383:38: error[invalid-argument-type] Argument to function `filter_traceback` is incorrect: Expected `list[str]`, found `list[LiteralString]`
- asynq/tests/test_debug.py:306:9: error[unresolved-attribute] Module `asynq` has no member `debug`
- asynq/tests/test_debug.py:309:19: error[unresolved-attribute] Module `asynq` has no member `debug`
- asynq/tests/test_debug.py:323:9: error[unresolved-attribute] Module `asynq` has no member `debug`
- asynq/tests/test_debug.py:355:9: error[unresolved-attribute] Module `asynq` has no member `debug`
- asynq/tests/test_debug.py:383:9: error[unresolved-attribute] Module `asynq` has no member `debug`
- Found 166 diagnostics
+ Found 150 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- tornado/test/circlerefs_test.py:146:24: error[unresolved-attribute] Module `tornado` has no member `testing`
- tornado/test/circlerefs_test.py:147:18: error[unresolved-attribute] Module `tornado` has no member `httpserver`
- tornado/test/circlerefs_test.py:205:18: error[unresolved-attribute] Module `tornado` has no member `concurrent`
- tornado/test/escape_test.py:217:22: error[unresolved-attribute] Module `tornado` has no member `escape`
+ tornado/test/escape_test.py:217:51: error[invalid-argument-type] Argument to function `linkify` is incorrect: Expected `bool`, found `Unknown | list[Unknown | str] | bool | str | ((href) -> Unknown)`
+ tornado/test/escape_test.py:217:51: error[invalid-argument-type] Argument to function `linkify` is incorrect: Expected `str | ((str, /) -> str)`, found `Unknown | list[Unknown | str] | bool | str | ((href) -> Unknown)`
+ tornado/test/escape_test.py:217:51: error[invalid-argument-type] Argument to function `linkify` is incorrect: Expected `bool`, found `Unknown | list[Unknown | str] | bool | str | ((href) -> Unknown)`
+ tornado/test/escape_test.py:217:51: error[invalid-argument-type] Argument to function `linkify` is incorrect: Expected `list[str]`, found `Unknown | list[Unknown | str] | bool | str | ((href) -> Unknown)`
- tornado/test/import_test.py:66:23: error[unresolved-attribute] Module `tornado` has no member `ioloop`
- tornado/test/import_test.py:66:52: error[unresolved-attribute] Module `tornado` has no member `util`
- tornado/test/import_test.py:67:23: error[unresolved-attribute] Module `tornado` has no member `gen`
- tornado/test/import_test.py:67:49: error[unresolved-attribute] Module `tornado` has no member `util`
- tornado/test/import_test.py:68:23: error[unresolved-attribute] Module `tornado` has no member `util`
- tornado/test/util_test.py:296:56: error[unresolved-attribute] Module `tornado` has no member `escape`
- tornado/test/util_test.py:302:56: error[unresolved-attribute] Module `tornado` has no member `escape`
- tornado/web.py:1380:25: error[unresolved-attribute] Module `tornado` has no member `locale`
- tornado/web.py:1401:29: error[unresolved-attribute] Module `tornado` has no member `locale`
- tornado/web.py:1404:43: error[unresolved-attribute] Module `tornado` has no member `locale`
- tornado/web.py:1414:61: error[unresolved-attribute] Module `tornado` has no member `locale`
- tornado/web.py:2242:24: error[unresolved-attribute] Module `tornado` has no member `netutil`
- tornado/websocket.py:139:24: error[unresolved-attribute] Module `tornado` has no member `web`
- tornado/websocket.py:222:22: error[unresolved-attribute] Module `tornado` has no member `web`
- tornado/websocket.py:364:23: error[unresolved-attribute] Module `tornado` has no member `escape`
- tornado/websocket.py:758:39: error[unresolved-attribute] Module `tornado` has no member `web`
- tornado/websocket.py:1097:23: error[unresolved-attribute] Module `tornado` has no member `escape`
- tornado/websocket.py:1098:19: error[unresolved-attribute] Module `tornado` has no member `escape`
- Found 352 diagnostics
+ Found 334 diagnostics

dragonchain (https://github.com/dragonchain/dragonchain)
- dragonchain/lib/dao/transaction_dao.py:77:16: error[unresolved-attribute] Module `redis` has no member `exceptions`
- dragonchain/lib/database/redis.py:364:48: error[unresolved-attribute] Module `redis` has no member `client`
- dragonchain/lib/database/redisearch.py:184:12: error[unresolved-attribute] Module `redis` has no member `exceptions`
- dragonchain/lib/database/redisearch.py:347:12: error[unresolved-attribute] Module `redis` has no member `exceptions`
- dragonchain/lib/database/redisearch.py:361:24: error[unresolved-attribute] Module `redis` has no member `exceptions`
- dragonchain/lib/database/redisearch.py:382:12: error[unresolved-attribute] Module `redis` has no member `exceptions`
- dragonchain/lib/database/redisearch.py:431:12: error[unresolved-attribute] Module `redis` has no member `exceptions`
- dragonchain/lib/database/redisearch.py:436:12: error[unresolved-attribute] Module `redis` has no member `exceptions`
- dragonchain/lib/database/redisearch.py:449:16: error[unresolved-attribute] Module `redis` has no member `exceptions`
- dragonchain/lib/database/redisearch_utest.py:84:49: error[unresolved-attribute] Module `redis` has no member `exceptions`
- dragonchain/webserver/lib/blocks.py:52:12: error[unresolved-attribute] Module `redis` has no member `exceptions`
- dragonchain/webserver/lib/transactions.py:68:12: error[unresolved-attribute] Module `redis` has no member `exceptions`
- Found 443 diagnostics
+ Found 431 diagnostics

cki-lib (https://gitlab.com/cki-project/cki-lib)
- cki_lib/timeout.py:23:20: error[unresolved-attribute] Module `multiprocessing` has no member `context`
- Found 187 diagnostics
+ Found 186 diagnostics

bokeh (https://github.com/bokeh/bokeh)
+ src/bokeh/server/views/ws.py:111:25: warning[possibly-missing-attribute] Attribute `websocket_origins` may be missing on object of type `Unknown | Application`
+ src/bokeh/server/views/ws.py:151:47: warning[possibly-missing-attribute] Attribute `sign_sessions` may be missing on object of type `Unknown | Application`
+ src/bokeh/server/views/ws.py:152:51: warning[possibly-missing-attribute] Attribute `secret_key` may be missing on object of type `Unknown | Application`
+ src/bokeh/server/views/ws.py:158:13: warning[possibly-missing-attribute] Attribute `io_loop` may be missing on object of type `Unknown | Application`
+ src/bokeh/server/views/ws.py:212:31: warning[possibly-missing-attribute] Attribute `new_connection` may be missing on object of type `Unknown | Application`
+ src/bokeh/server/views/ws.py:284:32: error[invalid-argument-type] Argument to bound method `send` is incorrect: Expected `WebSocketClientConnectionWrapper`, found `Self@send_message`
+ src/bokeh/server/views/ws.py:309:13: warning[possibly-missing-attribute] Attribute `client_lost` may be missing on object of type `Unknown | Application`
+ src/bokeh/server/views/ws.py:314:29: warning[possibly-missing-attribute] Attribute `consume` may be missing on object of type `Unknown | None | Receiver`
+ src/bokeh/server/views/ws.py:323:26: warning[possibly-missing-attribute] Attribute `handle` may be missing on object of type `Unknown | None | ProtocolHandler`
- Found 603 diagnostics
+ Found 612 diagnostics

zulip (https://github.com/zulip/zulip)
- scripts/setup/generate_secrets.py:177:31: error[unresolved-attribute] Module `redis` has no member `exceptions`
- Found 3313 diagnostics
+ Found 3312 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Comment by @codspeed-hq[bot] on 2025-10-14 05:10_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/gankra%2Fimplort3?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #20855 will **not alter performance**

<sub>Comparing <code>gankra/implort3</code> (e479b50) with <code>main</code> (735ec0c)</sub>



### Summary

`✅ 52` untouched  





---

_Comment by @github-actions[bot] on 2025-10-14 05:20_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "Support implicit imports of submodules in `__init__.pyi`" to "[ty] Support implicit imports of submodules in `__init__.pyi`" by @AlexWaygood on 2025-10-14 07:04_

---

_Label `ty` added by @AlexWaygood on 2025-10-14 07:04_

---

_Label `ecosystem-analyzer` added by @MichaReiser on 2025-10-14 07:10_

---

_Comment by @github-actions[bot] on 2025-10-14 07:15_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `unresolved-attribute` | 0 | 61 | 1 |
| `invalid-argument-type` | 13 | 0 | 0 |
| `possibly-missing-attribute` | 9 | 0 | 0 |
| **Total** | **22** | **61** | **1** |

**[Full report with detailed diff](https://gankra-implort3.ecosystem-663.pages.dev/diff)** ([timing results](https://gankra-implort3.ecosystem-663.pages.dev/timing))


---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index.rs`:285 on 2025-10-14 07:48_

How is this different from `imported_modules`? 

I also suggest using a named struct here. It's non obvious what the semantic meaning of the tuple fields are.


It might be worth using `Name` instead of `String`, to reduce the amount of heap allocations (or use `ModuleName`?)

---

_@MichaReiser reviewed on 2025-10-14 07:51_

---

_@MichaReiser reviewed on 2025-10-14 07:54_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:9878 on 2025-10-14 07:54_

Should we move this check into `maybe_imported_relative_submodules_of_stub_package`, given that `maybe_imported_relative_submodules_of_stub_package` is cached? Is it worth caching `maybe_imported_relative_submodules_of_stub_package`?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/import/nonstandard_conventions.md`:11 on 2025-10-14 12:57_

nit: it's not really a side effect! Just the direct impact of the import system working as designed: importing any object `X` (whether that object is a class, function, module, or anything else) into a module `M` stores `X` in the global namespace of `M` and makes it available as an attribute on `M` when `M` is imported.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/import/nonstandard_conventions.md`:30 on 2025-10-14 12:58_

```suggestion
> If an `__init__.pyi` for `mypackage` contains a `from...import` targeting a submodule, then that
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/import/nonstandard_conventions.md`:33 on 2025-10-14 12:59_

nit: I'd spell these as

```suggestion
## Relative `from` Import of Direct Submodule in `__init__`
```

throughout

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/import/nonstandard_conventions.md`:205 on 2025-10-14 13:03_

I think we _should_ try to support this, at least in non-stub files (but possibly not in stubs). This is exactly what the original examples in https://github.com/astral-sh/ty/issues/133 were about, before that issue got kinda hijacked by examples that were really about exemptions we should carve out for the stub file re-export convention 😄

We don't need to add support for this in this PR, but it might be good to add a TODO here, I think?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/import/nonstandard_conventions.md`:209 on 2025-10-14 13:04_

Maybe add "But we could consider improving our handling in the future", or something, to make it clear that there's no reason they _shouldn't_, this just reflects our current implementation?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/import/nonstandard_conventions.md`:306 on 2025-10-14 13:05_

Again, I think it would be good to at least support this for non-stub files eventually; it's probably worth adding a TODO here to make it clear that this isn't the desired end state

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/import/nonstandard_conventions.md`:311 on 2025-10-14 13:07_

hmmmm, we could consider changing this in the future, too

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/semantic_index.rs`:104 on 2025-10-14 13:11_

Explicitly listing the exported symbols in `__all__` is another standard escape hatch here too

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/semantic_index.rs`:123 on 2025-10-14 13:12_

nit: we usually return `Box<[]>` for Salsa queries like this, just to store a tiny bit less data in the Salsa cache (and since it's immutable anyway, so a `Vec` doesn't really get you anything)

```suggestion
) -> Box<[ModuleName]> {
    let Some(file) = importing_module.file(db) else {
        return Box::default();
    };
    if !file.is_package_stub(db) {
        return Box::default();
    }
```

---

_@AlexWaygood reviewed on 2025-10-14 13:17_

Nice, this looks like a great step forward in terms of the semantics. Thanks for writing such comprehensive tests!

---

_Comment by @Gankra on 2025-10-14 14:37_

Analysis of ecosystem failures:

# croniter 

This case would be fixed by introducing a rule that non-matching asname aliases disable the submodule init rule. I briefly discussed this with Alex and he said something to the effect of "well adding the submodule even if there's an alias is the runtime behaviour" and then I forgot to add a test for it mentioning it as a distinct case.

**EDIT: I pushed this change with tests**

Failure:

```text
paasta (https://github.com/yelp/paasta)
paasta_tools/cli/cmds/validate.py
[error] call-non-callable - :890:12 - Object of type `<module 'croniter.croniter'>` is not callable 
```

In [validate.py](https://github.com/yelp/paasta/blob/8c21e65cf7fd7581e6bef492fe9910c0bf5cc1ea/paasta_tools/cli/cmds/validate.py#L890):



In [typeshed's `croniter/__init__.pyi`](https://github.com/python/typeshed/blob/main/stubs/croniter/croniter/__init__.pyi) we
have the `croniter.croniter` module now masking the `croniter` class in the `croniter` package:

```py
from croniter import croniter
...
iter = croniter(cron_schedule, starting_from)
```

```py
# import that we now consider as introducing the submodule attr
from . import croniter as croniter_m
# explicit re-export of the class (that now gets shadowed by our "submodule attrs win ties" rule)
from .croniter import croniter as croniter
```


# scikit

This case is harder for me to wrap my head around, largely because I can't give a coherent explanation for why it
worked before. As in, I was under the impression that `from xyz import lobpcg` was not a valid re-export of the lobpcg
function, and that listing it in `__all__` would only affect star imports (of which there appear to be none).

Failure:

```
scikit-learn (https://github.com/scikit-learn/scikit-learn)
sklearn/manifold/_spectral_embedding.py
[error] call-non-callable - :415:28 - Object of type `<module 'scipy.sparse.linalg._eigen.lobpcg'>` is not callable
[error] call-non-callable - :449:32 - Object of type `<module 'scipy.sparse.linalg._eigen.lobpcg'>` is not callable
```

In `_spectral_embedding.py`:

```py
from scipy.sparse.linalg import lobpcg
...
_, diffusion_map = lobpcg(laplacian, X, M=M, tol=tol, largest=False)
...
```

In [scipy-stubs' `sparse/linalg/__init__.pyi`](https://github.com/scipy/scipy-stubs/blob/master/scipy-stubs/sparse/linalg/__init__.pyi):

```py
# im
from ._eigen import lobpcg

__all__ = [
    ...
    "lobpcg",
    ...
]
```

In [scipy-stubs' `sparse/linalg/_eigen/__init__.pyi` ](https://github.com/scipy/scipy-stubs/blob/master/scipy-stubs/sparse/linalg/_eigen/__init__.pyi)

```py
# importing the lobpcg function from the submodule of the same name
from .lobpcg import lobpcg

__all__ = [..., "lobpcg", ...]
```

---

_Comment by @AlexWaygood on 2025-10-14 14:45_

> As in, I was under the impression that `from xyz import lobpcg` was not a valid re-export of the lobpcg
> function, and that listing it in `__all__` would only affect star imports (of which there appear to be none).

Ah, no, `__all__` does also impact which symbols are considered explicitly re-exported from a stub file as well as impacting `*` imports. If you have an imported symbol in a stub file that does not have a redundant alias but _is_ listed in `__all__`, that still counts as an explicit re-export of that symbol

---

_Comment by @Gankra on 2025-10-14 14:51_

Ah ok I had a nagging feeling you *did* say that but I was like "no Aria you always want `__all__` to do more than it does". 

Ok then my guess is that the issue is `from ._eigen import lobpcg` (in [`linalg/__init__.pyi`](https://github.com/scipy/scipy-stubs/blob/master/scipy-stubs/sparse/linalg/__init__.pyi) is an unfortunate syntactic ambiguity here. 

It is simultaneously a valid import of the `scipy.sparse.linalg._eigen.lobpcg` *module* and of the *function* it re-exports. And presumably tie-break logic kicks in and we are sad? This is an extra fun case because it's even in the syntactic form pyright wants!

---

_@Gankra reviewed on 2025-10-14 15:16_

---

_Review comment by @Gankra on `crates/ty_python_semantic/resources/mdtest/import/nonstandard_conventions.md`:11 on 2025-10-14 15:16_

That sounds like a side-effect to me? As in, state has been mutated.

---

_@Gankra reviewed on 2025-10-14 15:16_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/types.rs`:9878 on 2025-10-14 15:16_

Yes good call, and done!

---

_@Gankra reviewed on 2025-10-14 15:16_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/types.rs`:9878 on 2025-10-14 15:16_

(This fixed most of the perf regressions)

---

_@Gankra reviewed on 2025-10-14 15:20_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/semantic_index.rs`:285 on 2025-10-14 15:20_

I originally had this as ModuleName but it created some odd error-handling contortions in the code that get undone anyway because some of the APIs we invoke take a `str` and then themselves parse it as a ModuleName.

---

_@AlexWaygood reviewed on 2025-10-14 15:23_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/import/nonstandard_conventions.md`:11 on 2025-10-14 15:23_

But that mutation of state is the _primary_ thing an `import` statement in Python does, so I don't see it as a _side_ effect 😄 it's the intended purpose, not something that happens by accident as a result of an operation that's intended to do something else

---

_Comment by @Gankra on 2025-10-14 15:34_

I believe the re-export version of this PR avoids the linalg issue by virtue of it not adding anything to the special set of submodule attributes that gets higher priority (not necessarily an argument for it being a better approach, just noting it).

---

_Comment by @Gankra on 2025-10-14 17:22_

I've put up a reduced test case for the scipy issue (as reduced as I could get it).

Also I think addressed all review comments.

---

_@MichaReiser reviewed on 2025-10-16 07:45_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index.rs`:284 on 2025-10-16 07:45_

Could you update the comment so that it explains how this field is different from `imported_modules`? 

I assume we can't really unify the two (I'm asking because I want to avoid that we store redundant information in the semantic index, which is one of the main contributor to ty's memory usage)

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index.rs`:284 on 2025-10-16 07:56_

I don't think this needs to be an `Arc`, as we have no `#[salsa::tracked]` query that returns the maybe imported modules directly

```suggestion
    maybe_imported_modules: FxHashSet<MaybeModuleImport>,
```

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/builder.rs`:1281 on 2025-10-16 07:57_

Let's call `shrink_to_fit` before passing the modules to `SemanticIndex`

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/builder.rs`:1568 on 2025-10-16 08:00_

Would it be sufficient to only collect the import statements at the module level (skip if we're inside a function or class)? In which case, I even wonder if this has to be inside `semantic_index` or if it could just be its own salsa query that traverses the top-level statements of a module without traversing into inner blocks.

---

_@MichaReiser reviewed on 2025-10-16 08:01_

---

_@Gankra reviewed on 2025-10-16 14:12_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/semantic_index.rs`:284 on 2025-10-16 14:12_

Whether we can unify the two I believe hinges on whether we actually want `from...import` and `import` to ever be treated differently for the purposes of import/re-export idioms.

---

_@Gankra reviewed on 2025-10-16 14:13_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/semantic_index.rs`:284 on 2025-10-16 14:13_

Actually I think I could probably unify them still...

---

_@Gankra reviewed on 2025-10-16 14:15_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/semantic_index/builder.rs`:1568 on 2025-10-16 14:15_

I was going off of the fact that we apply no such logic when handling `imported_modules`. I agree anything but top-level feels a biiit dubious (and I believe pyright agrees with that).

---

_@Gankra reviewed on 2025-10-16 14:19_

---

_Review comment by @Gankra on `crates/ty_python_semantic/resources/mdtest/import/nonstandard_conventions.md`:205 on 2025-10-16 14:19_

Agreed that supporting this would be pretty natural, although I'm a bit nervous about making this extension/idiom "too broad" and undermining the whole idea of re-exports having to be explicit.

---

_@Gankra reviewed on 2025-10-16 14:22_

---

_Review comment by @Gankra on `crates/ty_python_semantic/resources/mdtest/import/nonstandard_conventions.md`:306 on 2025-10-16 14:22_

Given the rest of our handling of non-stubs I agree that this should work for non-stubs (and *only* non-stubs).
(Definitely out of scope for this PR though)

---

_@Gankra reviewed on 2025-10-16 14:23_

---

_Review comment by @Gankra on `crates/ty_python_semantic/resources/mdtest/import/nonstandard_conventions.md`:311 on 2025-10-16 14:23_

Agreed, I think fixing this could be in-scope for this PR without too much work.

---

_@AlexWaygood reviewed on 2025-10-16 14:30_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/import/nonstandard_conventions.md`:205 on 2025-10-16 14:30_

> Agreed that supporting this would be pretty natural, although I'm a bit nervous about making this extension/idiom "too broad" and undermining the whole idea of re-exports having to be explicit.

yeah, I do agree with that hesitancy for stub files!

For runtime files, though, I think we should just model the runtime semantics as closely as is reasonable. Failing to be explicit about re-exports in a runtime file is at best something a linter should be concerned with, not a type checker, arguably. In runtime files, a type checker should focus on things that are actually going to cause runtime errors (or are unsound in some type-theoretic way). Things that are in poor taste because they rely on implicit behaviour don't really fall into that bucket IMO.

---

_@Gankra reviewed on 2025-10-16 14:36_

---

_Review comment by @Gankra on `crates/ty_python_semantic/resources/mdtest/import/nonstandard_conventions.md`:205 on 2025-10-16 14:36_

I should add some tests for imports at non-global scope (inside functions), which, are a more muddy situation.

---

_@AlexWaygood reviewed on 2025-10-17 10:27_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/import/nonstandard_conventions.md`:11 on 2025-10-17 10:27_

I think this might reveal an interesting difference in perspective that stems from how dynamic Python is (so much happens at runtime; so little happens at compile time compared to, e.g., Rust).

In Python, I think the statement `from x import y` in a module `z` is best conceptualised as an explicit command to the interpreter that says "At runtime, at this point in the global namespace, lookup the symbol `y` in the `x` module/package and store a reference to it in the global namespace of `z`". All other consequences of the `import` statement, such as it then being available for use elsewhere in the module `z`, or being available as an attribute on the module `z`, are downstream from that. `from x import y` just desugars to something like this, which is executed at runtime just like all other statements or expressions in the module's global namespace:

```py
try:
    y = __import__("x.y").y
except ImportError:  # not a submodule; look it up as an attribute instead
    y = __import__("x").y
```

If `y` is loaded into the global namespace of an `z/__init__.py` file, it will be available from all code in that file and as an attribute on the `z` package, just like any other symbol that's loaded into the global namespace of that file. So I don't really view the state mutation here as fundamentally different to what happens with any other import in Python, or as a "side effect" of the import system -- this is just how imports work in Python, and it's the _primary_ effect of them IMO

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/import/nonstandard_conventions.md`:205 on 2025-10-17 10:35_

I still think it would be great to add a TODO comment here (https://github.com/astral-sh/ruff/pull/20855#discussion_r2429085956)

Since this is what the original examples in github.com/astral-sh/ty/issues/133 were about, I think this PR arguably doesn't actually close that issue -- rather than being about implicit imports of submodules in `__init__.py` files (what that issue was originally about), to my mind this PR is really about carving out exemptions to the explicit re-export convention for stub files in `__init__.pyi` files. It's still a good and important change for us to make! But something slightly different to what github.com/astral-sh/ty/issues/133 was originally about.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/import/nonstandard_conventions.md`:405 on 2025-10-17 10:36_

I still think a TODO comment here would be great to make it clear that this isn't asserting our intended behaviour, just capturing our current behaviour (https://github.com/astral-sh/ruff/pull/20855#discussion_r2429093216)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/import/nonstandard_conventions.md`:432 on 2025-10-17 10:38_

I know you signalled this in the prose description, but I think a TODO comment next to this assertion would also be great -- it can be confusing for people working on features if existing "assertions" start "breaking" when they improve semantics. Having a TODO comment right next to the assertion is really helpful for that.

---

_@AlexWaygood reviewed on 2025-10-17 10:39_

Nice, I still think this looks great in terms of the semantics! I think we should aim to land this more-or less as-is, notwithstanding some edge cases in scipy

---

_Comment by @MichaReiser on 2025-10-24 14:28_

What's the status of this PR? Are you waiting on any more input/feedback? Is it blocked on anything?

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-10-30 18:44_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-10-30 18:44_

---

_Label `ecosystem-analyzer` removed by @Gankra on 2025-10-31 02:07_

---

_Label `ecosystem-analyzer` added by @Gankra on 2025-10-31 02:08_

---

_Label `ecosystem-analyzer` removed by @Gankra on 2025-10-31 02:40_

---

_Label `ecosystem-analyzer` added by @Gankra on 2025-10-31 02:40_

---

_Comment by @Gankra on 2025-10-31 02:51_

Nice, the optimization I wanted to try ended up actually being a fix for the `from .x import x` issue by virtue of it dropping support for that form altogether. Now only `from . import x` or `from mypackage import x` actually work. I noticed in practice that the users of this idiom are in fact only `from . import x` and not the other form, so, this works well!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/import/nonstandard_conventions.md`:573 on 2025-10-31 14:13_

(this could also be changed in the future, as discussed, but no need to do that in this PR of course!)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/semantic_index/builder.rs`:1281 on 2025-10-31 14:16_

this looks like it's still unaddressed

---

_@AlexWaygood approved on 2025-10-31 14:18_

Thank you!!

---

_Merged by @Gankra on 2025-10-31 14:29_

---

_Closed by @Gankra on 2025-10-31 14:29_

---

_Branch deleted on 2025-10-31 14:29_

---
