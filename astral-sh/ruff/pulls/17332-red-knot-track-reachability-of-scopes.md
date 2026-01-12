```yaml
number: 17332
title: "[red-knot] Track reachability of scopes"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/scope-reachability
created_at: 2025-04-10T11:08:59Z
updated_at: 2025-04-10T13:09:27Z
url: https://github.com/astral-sh/ruff/pull/17332
synced_at: 2026-01-12T15:56:01Z
```

# [red-knot] Track reachability of scopes

---

_@sharkdp_

## Summary

Track the reachability of nested scopes within their parent scopes. We use this as an additional requirement for emitting `unresolved-reference` diagnostics (and in the future, `unresolved-attribute` and `unresolved-import`). This means that we only emit `unresolved-reference` for a given use of a symbol if the use itself is reachable (within its own scope), *and if the scope itself is reachable*. For example, no diagnostic should be emitted for the use of `x` here:

```py
if False:
    x = 1

    def f():
        print(x)  # this use of `x` is reachable inside the `f` scope,
                  # but the whole `f` scope is not reachable.
```

There are probably more fine-grained ways of solving this problem, but they require a more sophisticated understanding of nested scopes (see astral-sh/ty#210, in particular https://github.com/astral-sh/ty/issues/210). But it doesn't seem completely unreasonable to silence *this specific kind of error* in unreachable scopes.

## Test Plan

Observed changes in reachability tests and ecosystem.

---

_Label `red-knot` added by @sharkdp on 2025-04-10 11:09_

---

_Comment by @github-actions[bot] on 2025-04-10 11:11_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
mypy_primer (https://github.com/hauntsaninja/mypy_primer)
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/utils.py:33:24: Name `ILLEGAL_PATH_CHARS` used when not defined
- Found 709 diagnostics
+ Found 708 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/middleware/shared_data.py:139:45: Name `fnmatch` used when not defined
- Found 770 diagnostics
+ Found 769 diagnostics

rich (https://github.com/Textualize/rich)
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:31:17: Name `WindowsCoordinates` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:31:47: Name `WindowsCoordinates` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:38:32: Name `_win32_console` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:44:34: Name `CURSOR_SIZE` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:48:13: Name `_win32_console` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:50:26: Name `StubScreenBufferInfo` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:52:13: Name `_win32_console` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:60:16: Name `LegacyWindowsTerm` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:61:40: Name `WindowsCoordinates` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:61:63: Name `CURSOR_Y` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:61:77: Name `CURSOR_X` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:64:16: Name `LegacyWindowsTerm` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:65:36: Name `WindowsCoordinates` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:66:17: Name `SCREEN_HEIGHT` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:66:36: Name `SCREEN_WIDTH` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:71:16: Name `LegacyWindowsTerm` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:87:16: Name `LegacyWindowsTerm` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:103:47: Name `DEFAULT_STYLE_ATTRIBUTE` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:111:16: Name `LegacyWindowsTerm` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:128:16: Name `LegacyWindowsTerm` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:145:16: Name `LegacyWindowsTerm` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:162:16: Name `LegacyWindowsTerm` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:179:16: Name `LegacyWindowsTerm` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:200:16: Name `LegacyWindowsTerm` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:202:17: Name `WindowsCoordinates` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:202:40: Name `CURSOR_Y` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:204:39: Name `SCREEN_WIDTH` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:207:27: Name `DEFAULT_STYLE_ATTRIBUTE` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:207:59: Name `SCREEN_WIDTH` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:218:16: Name `LegacyWindowsTerm` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:222:39: Name `SCREEN_WIDTH` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:222:54: Name `CURSOR_X` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:222:70: Name `CURSOR_POSITION` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:226:13: Name `DEFAULT_STYLE_ATTRIBUTE` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:227:20: Name `SCREEN_WIDTH` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:227:35: Name `CURSOR_X` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:228:19: Name `CURSOR_POSITION` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:239:16: Name `LegacyWindowsTerm` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:242:17: Name `WindowsCoordinates` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:242:36: Name `CURSOR_Y` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:245:39: Name `CURSOR_X` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:248:27: Name `DEFAULT_STYLE_ATTRIBUTE` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:248:59: Name `CURSOR_X` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:255:18: Name `WindowsCoordinates` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:256:16: Name `LegacyWindowsTerm` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:266:18: Name `WindowsCoordinates` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:267:16: Name `LegacyWindowsTerm` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:277:18: Name `WindowsCoordinates` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:278:16: Name `LegacyWindowsTerm` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:288:16: Name `LegacyWindowsTerm` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:293:34: Name `WindowsCoordinates` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:293:57: Name `CURSOR_Y` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:293:75: Name `CURSOR_X` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:300:16: Name `LegacyWindowsTerm` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:305:34: Name `WindowsCoordinates` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:305:57: Name `CURSOR_Y` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:305:75: Name `CURSOR_X` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:312:16: Name `LegacyWindowsTerm` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:317:34: Name `WindowsCoordinates` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:317:57: Name `CURSOR_Y` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:317:71: Name `CURSOR_X` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:324:33: Name `StubScreenBufferInfo` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:325:30: Name `COORD` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:325:36: Name `SCREEN_WIDTH` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:325:54: Name `CURSOR_Y` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:330:16: Name `LegacyWindowsTerm` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:334:34: Name `WindowsCoordinates` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:334:57: Name `CURSOR_Y` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:341:16: Name `LegacyWindowsTerm` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:344:34: Name `WindowsCoordinates` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:344:53: Name `CURSOR_Y` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:351:16: Name `LegacyWindowsTerm` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:354:34: Name `WindowsCoordinates` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:354:57: Name `CURSOR_Y` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:354:71: Name `CURSOR_X` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:361:35: Name `StubScreenBufferInfo` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:362:30: Name `COORD` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:362:39: Name `CURSOR_Y` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:367:16: Name `LegacyWindowsTerm` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:371:20: Name `WindowsCoordinates` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:371:43: Name `CURSOR_Y` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:371:61: Name `SCREEN_WIDTH` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:376:16: Name `LegacyWindowsTerm` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:385:48: Name `CURSOR_SIZE` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:389:16: Name `LegacyWindowsTerm` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:398:48: Name `CURSOR_SIZE` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:402:16: Name `LegacyWindowsTerm` used when not defined
- warning[lint:unresolved-reference] /tmp/mypy_primer/projects/rich/tests/test_win32_console.py:409:16: Name `LegacyWindowsTerm` used when not defined
- Found 886 diagnostics
+ Found 798 diagnostics

```
</details>


---

_Marked ready for review by @sharkdp on 2025-04-10 11:19_

---

_Review requested from @carljm by @sharkdp on 2025-04-10 11:19_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-04-10 11:19_

---

_Review requested from @dcreager by @sharkdp on 2025-04-10 11:19_

---

_Comment by @AlexWaygood on 2025-04-10 11:38_

> ```diff
> werkzeug (https://github.com/pallets/werkzeug)
> - warning[lint:unresolved-reference] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/middleware/shared_data.py:139:45: Name `fnmatch` used when not defined
> - Found 770 diagnostics
> + Found 769 diagnostics
> ```

This one is interesting -- looks like a buggy type annotation in `werkzeug` to me. Based on the type annotation of the `disallow` parameter, I think we're correct in thinking that the `if disallow is not None` branch is unreachable... but I can't imagine that you're really only allowed to pass `None` in to the `disallow` parameter (https://github.com/pallets/werkzeug/blob/7868bef5d978093a8baa0784464ebe5d775ae92a/src/werkzeug/middleware/shared_data.py#L103-L139)

---

_Comment by @sharkdp on 2025-04-10 11:40_

> This one is interesting -- looks like a buggy type annotation in `werkzeug` to me. Based on the type annotation of the `disallow` parameter, I think we're correct in thinking that the `if disallow is not None` branch is unreachable... but I can't imagine that you're really only allowed to pass `None` in to the `disallow` parameter (https://github.com/pallets/werkzeug/blob/7868bef5d978093a8baa0784464ebe5d775ae92a/src/werkzeug/middleware/shared_data.py#L103-L139)

Came to the exact same conclusion. I was also confused at first.


Edit: this might explain it. Maybe `None` was the only thing ever passed in at runtime. Let's just say it's Carl's fault.

![image](https://github.com/user-attachments/assets/369dea2b-ee45-46f0-9d40-9298de714381)


---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index.rs`:256 on 2025-04-10 11:41_

nit

```suggestion
        self.parent_scope_id(scope_id)
            .is_none_or(|parent_scope_id| {
                if !self.is_scope_reachable(db, parent_scope_id) {
                    return false;
                }

                let parent_use_def = self.use_def_map(parent_scope_id);
                let reachability = self.scope(scope_id).reachability();

                parent_use_def.is_reachable(db, reachability)
            })
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:364 on 2025-04-10 11:44_

Maybe it's worth adding a doc-comment here to note that this doesn't give you the "full" picture, because it's also important to consider whether the enclosing scope is reachable (and that `SemanticIndex::is_symbol_use_reachable` puts the two pieces of the puzzle together to give you the full picture)?

---

_@AlexWaygood approved on 2025-04-10 11:46_

Nice, this looks excellent

---

_@AlexWaygood reviewed on 2025-04-10 11:48_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index.rs`:256 on 2025-04-10 11:48_

it might also be worth adding a comment saying something along the lines of "if `self.parent_scope_id()` is `None`, it's the global scope, which is always reachable"

---

_@sharkdp reviewed on 2025-04-10 11:51_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:364 on 2025-04-10 11:51_

That's a good suggestion. I'll add this in a follow up I'm already working on, where this function will change anyway.

---

_Merged by @sharkdp on 2025-04-10 11:56_

---

_Closed by @sharkdp on 2025-04-10 11:56_

---

_Branch deleted on 2025-04-10 11:56_

---

_Comment by @carljm on 2025-04-10 13:09_

Love this!

---
