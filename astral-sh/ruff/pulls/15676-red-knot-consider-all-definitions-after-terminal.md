```yaml
number: 15676
title: "[red-knot] Consider all definitions after terminal statements unreachable"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
assignees: []
merged: true
base: main
head: dcreager/terminal-visibility
created_at: 2025-01-22T21:51:57Z
updated_at: 2025-05-07T15:22:57Z
url: https://github.com/astral-sh/ruff/pull/15676
synced_at: 2026-01-10T18:57:02Z
```

# [red-knot] Consider all definitions after terminal statements unreachable

---

_Pull request opened by @dcreager on 2025-01-22 21:51_

`FlowSnapshot` now tracks a `reachable` bool, which indicates whether we have encountered a terminal statement on that control flow path.  When merging flow states together, we skip any that have been marked unreachable.  This ensures that bindings that can only be reached through unreachable paths are not considered visible.

## Test Plan

The new mdtests failed (with incorrect `reveal_type` results, and spurious `possibly-unresolved-reference` errors) before adding the new visibility constraints.

---

_Label `red-knot` added by @dcreager on 2025-01-22 21:51_

---

_Review requested from @carljm by @dcreager on 2025-01-22 21:51_

---

_Review requested from @MichaReiser by @dcreager on 2025-01-22 21:51_

---

_Review requested from @AlexWaygood by @dcreager on 2025-01-22 21:51_

---

_Review requested from @sharkdp by @dcreager on 2025-01-22 21:51_

---

_Comment by @github-actions[bot] on 2025-01-22 21:57_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+5 -5 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+4 -4 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/api/flask_embed.py#L26'>examples/server/api/flask_embed.py:26:9:</a> SIM108 Use ternary operator `data = df if new == 0 else df.rolling(f"{new}D").mean()` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/api/flask_embed.py#L26'>examples/server/api/flask_embed.py:26:9:</a> SIM108 Use ternary operator `data = df if new == 0 else df.rolling(f'{new}D').mean()` instead of `if`-`else`-block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/api/flask_gunicorn_embed.py#L41'>examples/server/api/flask_gunicorn_embed.py:41:9:</a> SIM108 Use ternary operator `data = df if new == 0 else df.rolling(f"{new}D").mean()` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/api/flask_gunicorn_embed.py#L41'>examples/server/api/flask_gunicorn_embed.py:41:9:</a> SIM108 Use ternary operator `data = df if new == 0 else df.rolling(f'{new}D').mean()` instead of `if`-`else`-block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/api/standalone_embed.py#L18'>examples/server/api/standalone_embed.py:18:9:</a> SIM108 Use ternary operator `data = df if new == 0 else df.rolling(f"{new}D").mean()` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/api/standalone_embed.py#L18'>examples/server/api/standalone_embed.py:18:9:</a> SIM108 Use ternary operator `data = df if new == 0 else df.rolling(f'{new}D').mean()` instead of `if`-`else`-block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/api/tornado_embed.py#L29'>examples/server/api/tornado_embed.py:29:9:</a> SIM108 Use ternary operator `data = df if new == 0 else df.rolling(f"{new}D").mean()` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/api/tornado_embed.py#L29'>examples/server/api/tornado_embed.py:29:9:</a> SIM108 Use ternary operator `data = df if new == 0 else df.rolling(f'{new}D').mean()` instead of `if`-`else`-block
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/861917d2c50418aa2d641c57e1cfe147113ca45c/scripts/lib/sharding.py#L65'>scripts/lib/sharding.py:65:21:</a> SIM108 Use ternary operator `host = shard if "." in shard else f"{shard}.{external_host}"` instead of `if`-`else`-block
- <a href='https://github.com/zulip/zulip/blob/861917d2c50418aa2d641c57e1cfe147113ca45c/scripts/lib/sharding.py#L65'>scripts/lib/sharding.py:65:21:</a> SIM108 Use ternary operator `host = shard if "." in shard else f'{shard}.{external_host}'` instead of `if`-`else`-block
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM108 | 10 | 5 | 5 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+5 -5 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+4 -4 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/api/flask_embed.py#L26'>examples/server/api/flask_embed.py:26:9:</a> SIM108 Use ternary operator `data = df if new == 0 else df.rolling(f"{new}D").mean()` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/api/flask_embed.py#L26'>examples/server/api/flask_embed.py:26:9:</a> SIM108 Use ternary operator `data = df if new == 0 else df.rolling(f'{new}D').mean()` instead of `if`-`else`-block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/api/flask_gunicorn_embed.py#L41'>examples/server/api/flask_gunicorn_embed.py:41:9:</a> SIM108 Use ternary operator `data = df if new == 0 else df.rolling(f"{new}D").mean()` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/api/flask_gunicorn_embed.py#L41'>examples/server/api/flask_gunicorn_embed.py:41:9:</a> SIM108 Use ternary operator `data = df if new == 0 else df.rolling(f'{new}D').mean()` instead of `if`-`else`-block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/api/standalone_embed.py#L18'>examples/server/api/standalone_embed.py:18:9:</a> SIM108 Use ternary operator `data = df if new == 0 else df.rolling(f"{new}D").mean()` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/api/standalone_embed.py#L18'>examples/server/api/standalone_embed.py:18:9:</a> SIM108 Use ternary operator `data = df if new == 0 else df.rolling(f'{new}D').mean()` instead of `if`-`else`-block
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/api/tornado_embed.py#L29'>examples/server/api/tornado_embed.py:29:9:</a> SIM108 Use ternary operator `data = df if new == 0 else df.rolling(f"{new}D").mean()` instead of `if`-`else`-block
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/api/tornado_embed.py#L29'>examples/server/api/tornado_embed.py:29:9:</a> SIM108 Use ternary operator `data = df if new == 0 else df.rolling(f'{new}D').mean()` instead of `if`-`else`-block
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/861917d2c50418aa2d641c57e1cfe147113ca45c/scripts/lib/sharding.py#L65'>scripts/lib/sharding.py:65:21:</a> SIM108 Use ternary operator `host = shard if "." in shard else f"{shard}.{external_host}"` instead of `if`-`else`-block
- <a href='https://github.com/zulip/zulip/blob/861917d2c50418aa2d641c57e1cfe147113ca45c/scripts/lib/sharding.py#L65'>scripts/lib/sharding.py:65:21:</a> SIM108 Use ternary operator `host = shard if "." in shard else f'{shard}.{external_host}'` instead of `if`-`else`-block
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM108 | 10 | 5 | 5 | 0 | 0 |

</p>
</details>




---

_Comment by @dcreager on 2025-01-22 22:07_

There are a couple of new diagnostics in the benchmark that don't look correct to me.  I need to see if I can minimize that into an mdtest to diagnose.

---

_Converted to draft by @dcreager on 2025-01-22 22:07_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:43 on 2025-01-22 22:08_

Just to clarify, since it took me a moment on first read, even though you just explained it above:
```suggestion
    return x  # no possibly-unresolved-reference diagnostic!
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:91 on 2025-01-22 22:10_

We could `return x` here as well and expect the `possibly-unresolved-reference` error?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:107 on 2025-01-22 22:12_

I don't think "nested scopes" is the right term here; Python `if` statements don't establish nested scopes. This heading would suggest a test with e.g. a nested function.
```suggestion
## `return` is terminal in nested conditionals
```

---

_@carljm approved on 2025-01-22 22:14_

Fantastic!! Love to see a feature that is easier than anticipated :)

---

_Review comment by @T-256 on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:257 on 2025-01-22 23:53_

I'm curious in which combination of `cond` and `i` it'd be `Literal["loop"]` at here.

---

_Review comment by @T-256 on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:69 on 2025-01-22 23:58_

You may want to switch to `for` loop instead or increase `i`'s value inside the loop, otherwise it's infinite loop when `i<5`.

---

_@T-256 reviewed on 2025-01-23 00:09_

---

_Comment by @dylwil3 on 2025-01-23 22:16_

I don't think you need to change anything for this PR, but just so it's on your radar: `try`/`finally` knows how to ruin any clean story. For example, the following test fails on this branch:

```py
def f():
    x = 1
    while True:
        try:
            break
        finally:
            x = 2
    reveal_type(x)  # revealed: Literal[2] 
```
(it gives a revealed type of `Literal[1]` instead)

---

_Comment by @carljm on 2025-01-23 23:08_

Yeah, we can handle `finally` in this PR or as a separate follow up PR, but it probably will need some special handling.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:69 on 2025-01-24 19:35_

Fair point!  I originally had this as `while True` (i.e. a purposefully infinite loop), but our flow analysis was smart enough to see that we'd never leave the loop.  `while i < 5` is not something we can currently detect as infinite, even though it's clear that `i` is never modified.  That might change with @carljm's fixed point work, so I've changed it to a `for` loop so that we don't get a regression if/when that changes.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:257 on 2025-01-24 19:38_

At runtime it couldn't, but per above, our flow analysis couldn't detect that.  It couldn't see that `i` is not modified, so it had to assume there was some control flow path that executed the `while` body at least once but not an infinite number of times.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:91 on 2025-01-24 19:39_

Done.  And it turns out to be `unresolved`, not `possibly-unresolved`, since we can now see that the only binding is blocked by the `return` statement!

---

_@dcreager reviewed on 2025-01-24 19:44_

> For example, the following test fails on this branch:

Thanks @dylwil3!  I added that as a failing test case.  I'm going to poke at it briefly to see if it's easy to add for this PR

---

_@dcreager reviewed on 2025-01-24 19:59_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:257 on 2025-01-24 19:59_

Ooh, this is now incorrect.  It should be `Literal["before", "loop", "continue"]`:

If `i <= 0`, the loop body won't execute, and `x == "before"`.
If `i > 0 and cond`, the loop body will execute `i` times, each time assigning `"loop"`.
If `i > 0 and not cond`, the loop body will execute `i` times, each time assigning `"continue"`.

The `continue` statement should mark its flow as unreachable _for when we join the `if` branches_.  But it should _not_ be considered unreachable when we join with the `for` loop's continuation and exit paths.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:257 on 2025-01-27 21:22_

After discussing in Discord, we've decided that this incorrect result is not because of the new terminal statement handling, but because we're not currently handling _any_ loop back-links (including `continue` statements) pending fixpoint support in salsa.  cf https://github.com/astral-sh/ruff/issues/14160

---

_@dcreager reviewed on 2025-01-27 21:24_

---

_Marked ready for review by @dcreager on 2025-01-27 21:25_

---

_Comment by @carljm on 2025-01-27 22:03_

> We can use the new statically known branches feature to address

Nit: update the PR description to match how the PR currently works.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:23 on 2025-01-27 22:12_

Nit: "unreachable" is a possibly-confusing term here, since (in terms of control flow) the assignment `x = "unreachable"` certainly is reachable. But that binding is not visible from later in the function. Sometimes we've carelessly used the term "reachable" to refer to a binding that is visible from later in control flow, but I've tried to avoid that usage in favor of "visible" or "reaching" (that is, the definition _reaches_ a later point in control flow, not "is reachable from" a later point in control flow -- control flow flows forward, not backward.)

All that said, it clearly doesn't matter too much what word we use in this string in this test :) But I would probably favor `"terminal"` instead?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:56 on 2025-01-27 22:17_

More terminology nits! "Loop scope" not really a thing in Python, probably best to avoid possible confusion.

A more accurate phrasing might be "`continue` is terminal in local control flow, but represents a jump back to top-of-loop" -- but this is too much to squeeze into a heading. The heading could perhaps just be one word: `continue`, and we can elaborate in prose?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:86 on 2025-01-27 22:17_

same comment as above

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:51 on 2025-01-27 22:17_

same comment as above

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:112 on 2025-01-27 22:20_

Maybe you have it below and I haven't gotten there yet, but somewhere I'd like to see a test showing we understand that if we have terminals in both `if` and `else` branches, that translates to the merged flow after the if/else also being terminal.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:141 on 2025-01-27 22:26_

This test looks fine. I think we should have at least a couple additional tests showing correct handling of terminals in `try/except` blocks in various cases. Some examples that come to mind:

- `raise` or `return` inside a `try` block (bindings from before the `raise` are visible in `except` and `finally` and beyond the whole `try` statement, bindings from after the `raise` are not)
- `return` inside an `except` block means bindings in that block are not visible later

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:589 on 2025-01-27 22:27_

We should be able to at least eliminate this `Unknown`, because there is no other scope which can assign to `x`:

```suggestion
        # TODO eliminate Unknown
        reveal_type(x)  # revealed: Unknown | Literal[1, 2, 3]
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:187 on 2025-01-27 22:28_

```suggestion
        # TODO: Literal[1, 2, 3]
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:200 on 2025-01-27 22:28_

```suggestion
        # TODO: Literal[1, 2, 3]
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:216 on 2025-01-27 22:32_

A return at the end of the function isn't really an "early return", contrary to the test title.

Is this test testing some code that was added in this PR? It doesn't clearly seem to test anything about terminality of `return`.

This test intersects with two known-incorrect areas (closed-over vars in scopes with `return` statements, modeling of eagerly-executing nested scopes), has no `reveal_type` (so asserts nothing more than "no diagnostics here") and doesn't demonstrate any TODOs. This makes me question its value as a test.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:707 on 2025-01-27 23:44_

There is also the possibility that neither `self` nor `snapshot` is reachable. This code will correctly result in `self` still being marked unreachable in that case, but it seems a little odd that we keep the visible definitions state from `self` in that case. The logical extension of the idea that an unreachable state takes no part in a merge should be that in case neither are reachable, we reset `self` to a state with `reachable: false` and no visible definitions, right?

Not sure if it practically makes a difference, though; since the new state is still unreachable its visible definitions shouldn't "go" anywhere anyway, even if `self` is later merged into another state. I guess it will make a difference to the types we reveal in the following unreachable code?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:731 on 2025-01-27 23:45_

If we've gotten past the top-of-function checks, we know that _both_ of the two states were reachable, right? So this comment understates what we know here.

---

_Review comment by @carljm on `crates/ruff_benchmark/benches/red_knot.rs`:44 on 2025-01-27 23:45_

🎉 

---

_@carljm approved on 2025-01-27 23:45_

---

_Comment by @sharkdp on 2025-01-28 12:01_

I haven't followed the full conversation on this topic, so maybe I have missed this being discussed somewhere. I'm also not sure if this is out-of-scope for this PR or out-of-scope in general, but I was curious how the interplay between statically-known branches and terminal statements worked, and it looks like this is something that this approach does not handle (yet)?
```py
def _(cond: bool):
    x = "a"
    if cond:
        x = "b"
        if True:
            return

    reveal_type(x)  # revealed: "a", "b"; should be "a"
```
I understand that this is probably difficult to handle with our "delayed" handling of statically-known branches, but it seems worth to mention as a limitation, because pyright can handle situations like this.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:23 on 2025-01-28 14:34_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:51 on 2025-01-28 14:34_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:56 on 2025-01-28 14:43_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:86 on 2025-01-28 14:48_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:216 on 2025-01-28 14:53_

This is a minimal reproduction of an error I was getting in the `tomllib` benchmark test.  I thought to put it here to catch it earlier in the CI process, but since it's redundant with the `tomllib` test I'll remove it.  (Maybe a better way to handle this is to move the assertions out of the benchmark and into a new test case that also analyzes `tomllib`?  That way the benchmark is only concerned with performance, and the test with correctness.)

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:112 on 2025-01-28 15:06_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:707 on 2025-01-28 15:21_

> but it seems a little odd that we keep the visible definitions state from `self` in that case

If we want this, I think it would be best to add an invariant that marking a flow as unreachable clears out all of its definitions, and update `mark_unreachable` and `restore` to maintain that invariant.  Then this code in `merge` would do the right thing as you describe.

> I guess it will make a difference to the types we reveal in the following unreachable code?

Yes, that's exactly right.  (In the sense that that's what the code does, not necessarily that that's what we _want_ it to do :sweat_smile:)  For now, I was punting on this, because this PR isn't currently addressing what we want to do for unreachable code.  (Note that in the mdtests I've tried to not put in any `reveal_type`s in unreachable positions.)

I think there are a couple of issues at play here.  One is that, not even considering the merge, what do we want to report in the unreachable code within the same block after a terminal statement?

```py
x = 2
return
reveal_type(x)  # ???
```

Should it be an `unresolved-reference` error?  Or should it act as if the terminal statement weren't there, and show what `x` would be if control flow could somehow make it to that point?  Or should we silence all diagnostics completely in unreachable code?

Whatever we choose, we should have the same result for

```py
if cond:
    x = 2
    return
else:
    x = 3
    return
reveal_type(x)  # ???
```

If we decide that we want the first case to reveal `Literal[2]`, then we'd want this case to reveal `Literal[2, 3]` — which means that we actually _do_ want to merge all of the flows, even if they're unreachable, and it's just the visibility of the relevant symbols that needs to be tracked/adjusted somehow.


---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:731 on 2025-01-28 15:23_

Yes good catch!  Stale comment from a previous iteration of this function.

---

_@dcreager reviewed on 2025-01-28 15:29_

> but I was curious how the interplay between statically-known branches and terminal statements worked

That's a good example @sharkdp!  Before I was also adding a `~AlwaysTrue` visibility constraint when we encountered a terminal statement, which (edit: I think) would handle your example.  I removed it because it seemed to be interacting incorrectly with `continue` and `break`.  (The new visibility constraint should apply to the rest of the current flow, but should _not_ apply when we jump back to the beginning of the loop.)  But @carljm and I convinced each other in Discord that the issue with `continue` is that we haven't implemented the jump back to top-of-loop yet (pending fixpoint support in salsa) — and I think that would solve the visibility constraint issue too...

---

_@carljm reviewed on 2025-01-28 16:47_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:707 on 2025-01-28 16:47_

Yes, that all makes sense, thanks for clarifying!

I think we should tackle this separately as a later problem; maybe file an issue for it? I don't think it's urgent, and I'm happy with doing the "least work" for now, even if it's less consistent (that is, neither eagerly clearing definitions when a branch becomes unreachable, nor merging definitions from two unreachable branches just so we can have fully consistent types in unreachable code).

Whatever we do for unreachable code should look consistent whether the origin of that unreachability is in terminals or in statically-known branches (that is, code under an `if False` should behave similarly to code after a `return`), which may place some additional constraints on how we handle it.

---

_@carljm reviewed on 2025-01-28 16:52_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:216 on 2025-01-28 16:52_

I think it's fine (even good) to take cases that we find from tomllib (or any other testing on real code), distill them down to their simplest form that illustrates a potential regression, and include them as mdtests. So that's not an issue. I think my question here really is trying to understand what the regression was (what did we do wrong in this example in some earlier version of this PR?) and clarify what behavior the test is trying to demonstrate (maybe just with some prose, maybe by adding a `reveal_type`, maybe both).

---

_@carljm reviewed on 2025-01-28 16:58_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:707 on 2025-01-28 16:58_

Top-of-head thoughts on what behaviors we do/don't want (not for action now, just for consideration in writing up the issue):

- I definitely don't think it would be useful to issue undefined-reference diagnostics for names used in unreachable code that would have been defined were the branch reachable.
- In some sense I think `Never` is the "right" type for all expressions in unreachable code?
- But I suspect that the most useful behavior is to check the unreachable code (and still raise diagnostics in it as normal), as if it were reachable.
- We should look into mypy and pyright behavior here.

---

_@dcreager reviewed on 2025-01-28 17:39_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:216 on 2025-01-28 17:39_

When I was using visibility constraints, this was a regression because:

- the `return` statement would mark the `x` parameter as non-visible for the remainder of the flow;
- list comprehensions would resolve free variables as of the end of the containing scope,
- which is technically after the `return` statement, and so the body of the list comprehension wouldn't see the `x` formal parameter as a visible definition.

It's the a lack of an `unresolved-reference` error that shows that the regression isn't there anymore.

Talking through it in detail like this, I think this is superfluous with the "Early returns and nested functions" tests, because it was the "closed-over vars in scopes with `return` statements" part that was relevant, and the "modeling of eagerly-executing nested scopes" was a red herring.


---

_@AlexWaygood reviewed on 2025-01-28 17:44_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:707 on 2025-01-28 17:44_

Mypy has terrible UX if it sees a `reveal_type` in a block of code it considers unreachable: it just doesn't emit any diagnostic for the `reveal_type` call at all. This has been the source of many bug reports at mypy over the years, because the rule that tells you off for having unreachable code in the first place is disabled by default, even if you opt into mypy's `--strict` flag. We shouldn't do what mypy does!

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:141 on 2025-01-28 18:23_

Added several `try`- and `raise`-related tests

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:90 on 2025-01-28 18:24_

Here we're considering the `"test"` assignment visible even though we return immediately after.  Is that because we assume an exception might occur after the assignment but before the `return`?  (It looks like we take a flow snapshot after every definition in the `try` body.  Should we instead take snapshots after each statement that could in theory raise an exception?  i.e. actual `raise`s plus anything that desugars into a call?)

---

_@dcreager reviewed on 2025-01-28 18:26_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:90 on 2025-01-28 18:41_

In practice almost everything in Python desugars into a call, so currently our over-approximation is to take a snapshot after every definition. But yes, I think in future we should aim to refine that to better handle cases where an exception really isn't possible.

---

_@carljm reviewed on 2025-01-28 18:41_

---

_@AlexWaygood reviewed on 2025-01-28 18:46_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:90 on 2025-01-28 18:46_

theoretically even a statement like `x = 3` _could_ raise an exception, since there's always the possibility of a `KeyboardInterrupt` terminating the current scope. But we probably want to pretend that `KeyboardInterrupt` doesn't exist, since most real-world code pretends that `KeyboardInterrupt` doesn't exist. It will be annoying for users if we explain to them that the reason red-knot is complaining that some variable in their code might be undefined is because they haven't accounted for the possibility of a `KeyboardInterrupt` leading to the `try` block being terminated before it ran to completion.

TL;DR: I agree with Carl. We should add a more sophisticated analysis of exactly which statements are actually capable of raising exceptions at some point. We just haven't gotten to it yet.

---

_@dcreager reviewed on 2025-01-28 20:39_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:90 on 2025-01-28 20:39_

:+1: Thanks for the details!  I was mostly wanting to confirm my understanding of the result and that there wasn't a bug I needed to fix.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:624 on 2025-01-28 21:33_

I've added @sharkdp's example as an mdtest.  I think we can support it by tracking reachability with a visibility constraint, instead of a Rust boolean, but I want to tackle that in a follow-on PR.

---

_@dcreager reviewed on 2025-01-28 21:33_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:127 on 2025-01-28 21:38_

I think one more assignment here would add a useful dimension to this test; it should also not be a possible value for `x` at the end of the scope:

```suggestion
    else:
        x = "terminal0"
        if cond2:
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:142 on 2025-01-28 21:38_

```suggestion
are likely visible after the loop body, since loops do not introduce new scopes. (Statically
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:263 on 2025-01-28 21:41_

```suggestion
likely visible after the loop body, since loops do not introduce new scopes. (Statically known
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:379 on 2025-01-28 21:47_

This is currently describing `finally` as if it were `else`. We don't really need to discuss `finally` here, as it's not directly connected to `raise` -- `finally` occurs after any `except` or `else`, regardless of the presence of a `raise`.

Also, the last phrase about `try` not introducing a new scope doesn't seem to me clearly related to why definitions from before `raise` are visible after the `try`; I suggested an adjusted wording.

```suggestion
jump to one of the `except` clauses (if it matches the value being raised), or to the `else`
clause (if none match). Currently, we assume definitions from before the `raise` are visible in all
`except` and `else` clauses. (In the future, we might analyze the `except` clauses to see which
ones match the value being raised, and limit visibility to those clauses.) Definitions from before
the `raise` are not visible in any `else` clause, but are visible in `except` clauses or after the
containing `try` statement (since control flow may have passed through an `except`).
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:397 on 2025-01-28 21:51_

I don't think this TODO is accurate, since `reveal_type` is a call, and I don't think we'd special-case it to assume it can't raise? So at the very least `"else"` is a possible value here.

I think it's accurate to say that `"before"` is not possible here, but only if we understand that boolean-testing a value of type `bool` is a special case that doesn't execute a `__bool__` method.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:400 on 2025-01-28 21:52_

`"else"` is also a possible value here, even after we model possible-exception-raising-locations more accurately, due to the `reveal_type` call.

And for the same reason, I don't think there's any scenario where we'd ever eliminate `"raise"` as a possibility here, because a non-ValueError could have been raised by the `reveal_type` call before the `raise` statement.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:406 on 2025-01-28 21:56_

It seems a little inconsistent to make TODO comments above but a non-TODO explanatory comment for precisely the same thing here. Let's either change this to `# TODO: Literal["raise", "else"]`, or else remove all the `TODO` in this function and replace them with explanatory comments about our over-approximation of possible jumps from within the `try` block. (I don't feel strongly about which way we go; to me our current behavior is not exactly _wrong_, given that some exceptions can occur pretty much anywhere, but it's not ideal and pretty likely we'll want to change it, so it's reasonable either to mark it as an outright `TODO`, or just comment on it for the benefit of someone possibly changing it in future.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:410 on 2025-01-28 21:57_

If in future we decided that no exception can be raised until the `raise` statement or `reveal_type` calls above, then `"before"` would also be absent from this list, right? So this line deserves the same `TODO` or explanatory comment that we use above.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:433 on 2025-01-28 21:58_

I think all of the comments above apply here as well

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:450 on 2025-01-28 22:00_

```suggestion
        # TODO: Literal["raise1", "raise2"]
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:454 on 2025-01-28 22:01_

I realize we are intentionally not asserting about types _within_ unreachable code, but can we still verify that we consider this code unreachable by adding an assignment to `x` here and verifying it is not a possible value for `x` in the end?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:459 on 2025-01-28 22:02_

Same comments as above about TODO vs explanatory comment, and about `"before"` as removable from both of these in future

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:489 on 2025-01-28 22:03_

All the same comments as above (though in this case both `"else1"` and `"else2"` should be possible in the `except` clauses)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:519 on 2025-01-28 22:04_

Same comments as in previous test

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:549 on 2025-01-28 22:05_

Same comments as in above cases (in this case `"else"` should be possible in both `except` clauses)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:382 on 2025-01-28 22:09_

AFAICT from the TODOs below, it looks like the only problem you're referring to here is our over-approximation of the possible location where an exception could be raised. Let's describe this a bit more clearly, to save future us from wondering what we meant here. (Also re-wording to avoid making it specific to `raise` statements, since it's really about too many jumps from places that _aren't_ `raise` statements at all, and to avoid describing it as incorrect, since technically (given `KeyboardInterrupt`) the current behavior is correct, just likely not preferable.
```suggestion
Currently we assume that an exception could be raised anywhere within a `try` block; the TODOs below reflect
cases where we could implement a more precise understanding of where exceptions (barring `KeyboardInterrupt`
and `MemoryError`) can and cannot actually be raised.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:552 on 2025-01-28 22:10_

Nit: the test below does not have a terminal statement in a `finally` block, it has a terminal statement in a `try` block with an associated `finally` clause.
```suggestion
## Terminal in `try` with `finally` clause
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:554 on 2025-01-28 22:13_

This also seems overly broad; we could be more specific to save future-us from possible confusion about what we meant
```suggestion
TODO: we don't yet model that a `break` or `continue` in a `try` block will jump to a `finally` clause before it
jumps to end/start of the loop.
```

---

_@carljm approved on 2025-01-28 22:15_

Some comments on the new tests, but the behavior looks good for this PR!

---

_@dcreager reviewed on 2025-01-28 22:16_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:707 on 2025-01-28 22:16_

https://github.com/astral-sh/ruff/issues/15797

---

_Review comment by @Cryptomonkey1979 on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:519 on 2025-01-28 23:45_

FxHashMap::default


---

_Review comment by @Cryptomonkey1979 on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:482 on 2025-01-29 02:41_

Debug


---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:382 on 2025-01-29 13:30_

A couple of points, though the tl;dr is that I like your edit:

- The part about the jump behavior was less about "exceptions can come from anywhere" and more about "a `raise` definitely doesn't execute the `else` clause".  The latter should be something we can model regardless of how approximate our exception tracking is.  But we're actually giving the correct result below in the `else` `reveal_type`, so you're right that this isn't accurately a `TODO`!  That said, I think it's coincidence that we're giving the correct result in the `else` clause — `"raise"` isn't included because we're treating `raise` the same as `return`, not because we know that `raise` skips the `else` clause.  (And `"raise"` _is_ included in the `except` clauses not because we know the `raise` statement jumps there — with this PR we think the `raise` skips everything since it's terminal! — but because our approximation thinks an unrelated exception might occur just after the assignment.)

- I had written this (and the TODOs below) describing the goal of a less approximate exception tracking strategy.  But that deserves discussion about what we'd want that to look like, so I like your suggestion to describe this in terms of what we're currently doing instead.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:397 on 2025-01-29 14:19_

> I don't think this TODO is accurate, since `reveal_type` is a call, and I don't think we'd special-case it to assume it can't raise? So at the very least `"else"` is a possible value here.

Ah, I was actually thinking we _would_ try to do that somehow!  But per above, that deserves discussion, so I'll back out the _assumption_ that we'd try to do that.

> I think it's accurate to say that `"before"` is not possible here, but only if we understand that boolean-testing a value of type `bool` is a special case that doesn't execute a `__bool__` method.

I removed the TODO entirely, leaving `"before"` as a potential possibility here too, so that we're not making _any_ assumptions about how we might make exception tracking less approximate.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:400 on 2025-01-29 14:23_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:406 on 2025-01-29 14:25_

Done.  See above, wrote the commentary based on what exception tracking currently does, not what we might change it to do in the future.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:410 on 2025-01-29 14:29_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:433 on 2025-01-29 14:29_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:454 on 2025-01-29 14:30_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:459 on 2025-01-29 14:30_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:489 on 2025-01-29 14:31_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:519 on 2025-01-29 14:31_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:549 on 2025-01-29 14:31_

Done

---

_@dcreager reviewed on 2025-01-29 14:34_

---

_@carljm reviewed on 2025-01-29 17:47_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:382 on 2025-01-29 17:47_

Ah, the first bullet point here is something I hadn't fully understood! It sort of seems like the current "right behavior by accident" might suffice until/unless we implement tighter understandings of where exceptions can be raised, at which point we might also need better understanding of what `raise` actually does. Certainly wouldn't object to adding some text to record this context for future.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:384 on 2025-01-29 17:52_

Rewording this slightly since there are no longer TODOs below about this!

```suggestion
Currently we assume that an exception could be raised anywhere within a `try` block. We may want to
implement a more precise understanding of where exceptions (barring `KeyboardInterrupt` and
`MemoryError`) can and cannot actually be raised.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:382 on 2025-01-29 17:58_

So I tried to write up an edit describing this, and I kind of ended up concluding that we may never need to implement any special understanding of where `raise` can jump to? Even if we tighten up our understanding of where exceptions can be raised, it seems like the only thing we'll need to do is maintain two things: 1) understanding `raise` as terminal, as we do already in this PR, and 2) still understanding `raise` as "a point where an exception can be raised", as we do in this PR.

(2) seems unlikely to be something we'd miss in that future where we're adding more understanding of exception points, so I'm thinking maybe we don't need to document this any more than it is already.

---

_@carljm approved on 2025-01-29 17:59_

One minor edit, but looks land-ready to me!

---

_@dcreager reviewed on 2025-01-29 18:31_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:382 on 2025-01-29 18:31_

> Even if we tighten up our understanding of where exceptions can be raised, it seems like the only thing we'll need to do is maintain two things: 1) understanding `raise` as terminal, as we do already in this PR, and 2) still understanding `raise` as "a point where an exception can be raised", as we do in this PR.

Ah yes, that sounds right!

> so I'm thinking maybe we don't need to document this any more than it is already.

:+1: 

---

_@dcreager reviewed on 2025-01-29 18:35_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/terminal_statements.md`:382 on 2025-01-29 18:35_

> still understanding `raise` as "a point where an exception can be raised", as we do in this PR.

The only place where this might fall over is that a `raise` can _only_ raise an exception, whereas a call _can_ but _doesn't have to_ raise one.  So calls could jump to `except` or `else`, while `raise` could only jump to `except`.

No, wait!  Calls can jump to `except` or flow through to the next statement, and _the end of the `try` block_ flows to `else`.  So yes, you're right, `raise` being terminal within the `try` block would correctly encode that it can't "jump" to `else`.  (Nothing actually "jumps" there, in fact.)

---

_Merged by @dcreager on 2025-01-29 19:06_

---

_Closed by @dcreager on 2025-01-29 19:06_

---

_Branch deleted on 2025-01-29 19:06_

---

_@Cryptomonkey1979 reviewed on 2025-02-26 07:26_

---
