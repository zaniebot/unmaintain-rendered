```yaml
number: 17563
title: "[red-knot] Add mdtests for `global` statement"
type: pull_request
state: merged
author: ntBre
labels:
  - ty
assignees: []
merged: true
base: main
head: brent/global-test-suite
created_at: 2025-04-22T19:49:15Z
updated_at: 2025-04-23T21:18:45Z
url: https://github.com/astral-sh/ruff/pull/17563
synced_at: 2026-01-10T19:33:02Z
```

# [red-knot] Add mdtests for `global` statement

---

_Pull request opened by @ntBre on 2025-04-22 19:49_

## Summary

This is a first step toward `global` support in red-knot (#15385). I went through all the matches for `global` in the `mypy/test-data` directory, but I didn't find anything too interesting that wasn't already covered by @carljm's suggestions on Discord. I still pulled in a couple of cases for a little extra variety. I also included a section from the [PLE0118](https://docs.astral.sh/ruff/rules/load-before-global-declaration/) tests in ruff that will become syntax errors once astral-sh/ruff#17463 is merged and we handle `global` statements.

I don't think I figured out how to use `@Todo` properly, so please let me know if I need to fix that. I hope this is a good start to the test suite otherwise.


---

_Label `red-knot` added by @ntBre on 2025-04-22 19:49_

---

_Comment by @github-actions[bot] on 2025-04-22 19:53_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Marked ready for review by @ntBre on 2025-04-22 20:01_

---

_Review requested from @carljm by @ntBre on 2025-04-22 20:01_

---

_Review requested from @AlexWaygood by @ntBre on 2025-04-22 20:01_

---

_Review requested from @sharkdp by @ntBre on 2025-04-22 20:01_

---

_Review requested from @dcreager by @ntBre on 2025-04-22 20:01_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/scopes/global.md`:35 on 2025-04-23 00:00_

`@Todo` is our display styling for an internal type we use to represent the result of some operation we haven't implemented yet. We don't use this for todo comments in tests, there we just use `TODO: `

```suggestion
    # TODO: error: [invalid-assignment] "Object of type `Literal[""]` is not assignable to `int`"
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/scopes/global.md`:51 on 2025-04-23 00:03_

And typically (for easier reading of the later diff that fixes the TODO!) we try to put these messages as close as we can to the spot where something will change:

```suggestion
A `global` statement causes lookup to skip any bindings in intervening scopes:

```py
x = 1

def outer():
    x = ""

    def inner():
        global x
        # TODO should be `Unknown | Literal[1]`
        reveal_type(x)  # revealed: Unknown | Literal[""]
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/scopes/global.md`:47 on 2025-04-23 00:04_

Nit: you could use declarations here (`x: int = 1` and `x: str = ""`) to get rid of the `Unknown |` in the types, which is not really relevant to the point of this test and kinda just noise? But it's fine either way.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/scopes/global.md`:58 on 2025-04-23 00:08_

Nit: in this case I probably wouldn't call out the pyright/mypy behavior difference. The reason is that both of them do narrow the global type of `int | None` to eliminate the `None`, based on the local assignment. The fact that narrowing is done at all is the key behavior this test asserts, and it is in line with both mypy and pyright, which typically means we don't bother commenting on their behavior.

The question of whether `x = 1` narrows to `int` or to `Literal[1]` is really a separate issue of how eager type checkers are to use literal types, which may be relevant for us to comment on somewhere, but probably not here.

(Another way of saying this is that we could write this same test using non-literal types -- e.g. `x: C | None` at global scope and `x = C()` in the function -- and in that case we would see identical behavior from mypy and pyright, and the test would still be testing the same thing we care about here. The literal-types difference is something unrelated to `global`.)

```suggestion
assignment.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/scopes/global.md`:82 on 2025-04-23 00:27_

This is a useful test case, but despite happening to use a `global` statement, it's not related to `global`. The same case could be constructed without `global`, just making `g` a local in `f` and using an `assert` (or `if` with early return, or whatever) to narrow its type instead. In other words, the test is really about how nested functions can or cannot make use of narrowed types in the outer scope. Which is a topic we do need to address, but it's not related to `global`. So I don't think we should include this test here.
```suggestion
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/scopes/global.md`:102 on 2025-04-23 00:29_

This is a pretty basic unremarkable test for how `global` should work -- it's fine that you happened to get it from mypy, but I don't think it's useful to preserve that information here.

```suggestion
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/scopes/global.md`:117 on 2025-04-23 00:31_

Again not sure the provenance information (from ruff tests) is useful here.

Typically we would go ahead and put a TODO comment right above each and every specific line that should error, rather than a single blanket TODO comment here. That makes it much clearer to a future contributor exactly where errors should and should not appear (and on which line(s)), and makes the TODO-fixing diff much simpler to review for correctness.

---

_@carljm reviewed on 2025-04-23 00:32_

Awesome, thank you! A few comments, mostly just about adapting to our "house style" for TODO comments in mdtests.

---

_@ntBre reviewed on 2025-04-23 14:24_

---

_Review comment by @ntBre on `crates/red_knot_python_semantic/resources/mdtest/scopes/global.md`:35 on 2025-04-23 14:24_

Ohhh, I see. I thought it was mdtest magic, but it's part of the actual diagnostics for now. Thank you!

---

_Comment by @ntBre on 2025-04-23 15:01_

Thanks for the review, it was very helpful! I think this should be in a much better state now.

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-04-23 15:02_

---

_@carljm approved on 2025-04-23 18:28_

Looks great, thank you!

---

_Merged by @ntBre on 2025-04-23 21:18_

---

_Closed by @ntBre on 2025-04-23 21:18_

---

_Branch deleted on 2025-04-23 21:18_

---
