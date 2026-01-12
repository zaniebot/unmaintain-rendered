```yaml
number: 16422
title: "[red-knot] Switch to a handwritten parser for mdtest error assertions"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: alex/mdtest-error-msg
created_at: 2025-02-27T20:26:40Z
updated_at: 2025-02-28T11:37:08Z
url: https://github.com/astral-sh/ruff/pull/16422
synced_at: 2026-01-12T15:55:54Z
```

# [red-knot] Switch to a handwritten parser for mdtest error assertions

---

_@AlexWaygood_

## Summary

I keep on making mistakes when writing mdtests that the framework makes hard to debug. The latest iteration here was when I added a test that looked like this while working on https://github.com/astral-sh/ruff/pull/16321:

````md
```py
class TestIter:
    def __next__(self) -> int:
        return 42

class Test:
    def __iter__(self) -> TestIter | int:
        return TestIter()

# error: [not-iterable] "Object of type `Test` may not be iterable because its `__iter__` method returns an object of type `TestIter | int`, which may not have a `__next__` method
for x in Test():
    reveal_type(x)  # revealed: int
```
````

Notice the mistake there? It took me an annoying amount of time to figure it out. The error in the test is that the asserted error message is missing its closing `"` character. But mdtest doesn't tell you that if you apply this diff to `main`:

```diff
diff --git a/crates/red_knot_python_semantic/resources/mdtest/loops/for.md b/crates/red_knot_python_semantic/resources/mdtest/loops/for.md
index 394c104b9..c32cbda7e 100644
--- a/crates/red_knot_python_semantic/resources/mdtest/loops/for.md
+++ b/crates/red_knot_python_semantic/resources/mdtest/loops/for.md
@@ -286,7 +286,7 @@ class Test:
     def __iter__(self) -> TestIter | int:
         return TestIter()
 
-# error: [not-iterable] "Object of type `Test` may not be iterable because its `__iter__` method returns an object of type `TestIter | int`, which may not have a `__next__` method"
+# error: [not-iterable] "Object of type `Test` may not be iterable because its `__iter__` method returns an object of type `TestIter | int`, which may not have a `__next__` method
 for x in Test():
     reveal_type(x)  # revealed: int
```

Instead it says:

```
failures:

---- mdtest__loops_for stdout ----

for.md - For loops - Union type as iterator where one union element has no `__next__` method

  crates/red_knot_python_semantic/resources/mdtest/loops/for.md:290 unexpected error: [not-iterable] "Object of type `Test` may not be iterable because its `__iter__` method returns an object of type `TestIter | int`, which may not have a `__next__` method"

To rerun this specific test, set the environment variable: MDTEST_TEST_FILTER="for.md - For loops - Union type as iterator where one union element has no `__next__` method"
MDTEST_TEST_FILTER="for.md - For loops - Union type as iterator where one union element has no `__next__` method" cargo test -p red_knot_python_semantic --test mdtest -- mdtest__loops_for
```

This PR switches our parsing of `# error:` assertion comments from regexes to a custom parser. This allows us to explicitly reject malformed assertion messages like this rather than misinterpreting them. Helpful messages are now given to the user in this and many other cases when a malformed assertion message is identified. Assertion messages are also now parsed lazily when matching assertion messages to the diagnostics that are emitted: this allows us to print tailored error messages to the terminal about the malformed assertion messages and recover from them, which would be harder if they were parsed eagerly.

## Test Plan

I added many unit tests to the `red_knot_test` crate. I also manually checked that if you apply the diff given above, mdtest now produces this error message:

```
failures:

---- mdtest__loops_for stdout ----

for.md - For loops - Union type as iterator where one union element has no `__next__` method

  crates/red_knot_python_semantic/resources/mdtest/loops/for.md:290 invalid assertion: expected '"' to be the final character in an assertion with an error message
  crates/red_knot_python_semantic/resources/mdtest/loops/for.md:290 unexpected error: 10 [not-iterable] "Object of type `Test` may not be iterable because its `__iter__` method returns an object of type `TestIter | int`, which may not have a `__next__` method"

To rerun this specific test, set the environment variable: MDTEST_TEST_FILTER="for.md - For loops - Union type as iterator where one union element has no `__next__` method"
MDTEST_TEST_FILTER="for.md - For loops - Union type as iterator where one union element has no `__next__` method" cargo test -p red_knot_python_semantic --test mdtest -- mdtest__loops_for
```

---

_Label `testing` added by @AlexWaygood on 2025-02-27 20:26_

---

_Label `red-knot` added by @AlexWaygood on 2025-02-27 20:26_

---

_Review requested from @carljm by @AlexWaygood on 2025-02-27 20:26_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-02-27 20:26_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-02-27 20:26_

---

_Comment by @github-actions[bot] on 2025-02-27 20:41_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review comment by @carljm on `crates/red_knot_test/src/matcher.rs`:141 on 2025-02-28 00:47_

Seems a little odd that you only get assertion-parsing errors if there is at least one diagnostic that could match the assertion, but I think in practice it's fine?

---

_@carljm approved on 2025-02-28 00:49_

👍 

---

_@dhruvmanila reviewed on 2025-02-28 06:28_

---

_Review comment by @dhruvmanila on `crates/red_knot_test/src/assertion.rs`:328 on 2025-02-28 06:28_

nit: you could implement [`FromStr`](https://doc.rust-lang.org/stable/std/str/trait.FromStr.html) instead

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/assertion.rs`:260 on 2025-02-28 08:15_

Not necessarily something for this PR but two common errors that I made in the past:

* I wrote `error[code]:` which is wrong
* I misspelled `error` or `revealed`
* Too much or not enough whitespace between `#` and the pragma keyword` 

The second one is probably easy to detect (error on any comment of the form `<pragma>:` and allowlist legit pragmas (`fmt`, `type`, `knot`)

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/assertion.rs`:260 on 2025-02-28 08:18_

I think it would be good to make this method be less strict about the amount of whitespace between `#` and the keyword and colon

```rust
let comment = comment.strip_prefix('#')?;
let (code, rest) = comment.split_once(':')?;

match code.trim():
	"revealed" => Some(Self::Revealed(rest.trim()))
	"error" => Some(Self::Error(rest.trim())),
	_ => None
}
```

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/assertion.rs`:423 on 2025-02-28 08:22_

I'm not sure if `memmem` is worth using here, considering that this reads through like 20 bytes. It might just be easier to use `self.eat_while(|c| c != ']')`

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/assertion.rs`:432 on 2025-02-28 08:23_

```suggestion
                        [self.offset().to_usize()..self.comment_source.trim_end().len() - 1];
```

This could panic if the string doesn't end with a `"` but the last character is a multi bytes character. Why not `self.eath_while(|c| c != '"')`?

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/assertion.rs`:400 on 2025-02-28 08:26_

Nit: I prefer to use `bump` over `first` because it guarantees that each iteration progresses. It's otherwise very easy to forget bumping the current character or checking if the cursor isn't eof.


---

_Review comment by @MichaReiser on `crates/red_knot_test/src/matcher.rs`:141 on 2025-02-28 08:27_

Would it be hard to force-parse unmatched assertions?

---

_@MichaReiser approved on 2025-02-28 08:27_

Nice

---

_@AlexWaygood reviewed on 2025-02-28 10:11_

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/assertion.rs`:328 on 2025-02-28 10:11_

This was my first thought too, but unfortunately I don't think it works because of the fact that `ErrorAssertion` is generic over a lifetime :-( Or at least, I can't seem to make it work

---

_@AlexWaygood reviewed on 2025-02-28 11:01_

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/assertion.rs`:260 on 2025-02-28 11:01_

> The second one is probably easy to detect (error on any comment of the form `<pragma>:` and allowlist legit pragmas (`fmt`, `type`, `knot`)

Hah, you would _think_ so... I've just spent a bit of time playing around with it, and it seems surprisingly hard to shoehorn into our current architecture :-(

I'm making the whitespace handling more lenientm though; that's easy to fix

---

_@AlexWaygood reviewed on 2025-02-28 11:10_

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/assertion.rs`:432 on 2025-02-28 11:10_

> This could panic if the string doesn't end with a `"` but the last character is a multi bytes character.

good point, thanks!

> Why not `self.eath_while(|c| c != '"')`?

that doesn't work because some of our revealed types and error messages include `"` characters (e.g. `Literal["foo"]`). But I can do something else.

---

_@AlexWaygood reviewed on 2025-02-28 11:26_

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/matcher.rs`:141 on 2025-02-28 11:26_

I don't think it fits neatly into the current architecture. I'm going to defer it for now.

---

_@AlexWaygood reviewed on 2025-02-28 11:26_

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/matcher.rs`:141 on 2025-02-28 11:26_

I'll add a TODO

---

_@AlexWaygood reviewed on 2025-02-28 11:28_

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/matcher.rs`:141 on 2025-02-28 11:28_

https://github.com/astral-sh/ruff/pull/16422/commits/5203341f8d1e261e657cb4591348756e4d1a08fc

---

_Merged by @AlexWaygood on 2025-02-28 11:33_

---

_Closed by @AlexWaygood on 2025-02-28 11:33_

---

_Branch deleted on 2025-02-28 11:33_

---
