```yaml
number: 16548
title: print MDTEST_TEST_FILTER value in single-quotes (and escaped)
type: pull_request
state: merged
author: ericmarkmartin
labels:
  - ty
assignees: []
merged: true
base: main
head: mdtest-escape-backticks
created_at: 2025-03-07T07:35:41Z
updated_at: 2025-03-08T15:37:24Z
url: https://github.com/astral-sh/ruff/pull/16548
synced_at: 2026-01-12T15:55:55Z
```

# print MDTEST_TEST_FILTER value in single-quotes (and escaped)

---

_@ericmarkmartin_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

If an mdtest fails, the error output will include an example command that can be run to re-run just the failing test, e.g

```
To rerun this specific test, set the environment variable: MDTEST_TEST_FILTER="sync.md - With statements - Context manager with non-callable `__exit__` attribute"
MDTEST_TEST_FILTER="sync.md - With statements - Context manager with non-callable `__exit__` attribute" cargo test -p red_knot_python_semantic --test mdtest -- mdtest__with_sync
```
This is very helpful, but because we're printing the envvar value surrounded in double-quotes, the bits between backticks in this example get interpreted as a shell interpolation. When running this in zsh, for example, I see

```console
❯ MDTEST_TEST_FILTER="sync.md - With statements - Context manager with non-callable `__exit__` attribute" cargo test -p red_knot_python_semantic --test mdtest -- mdtest__with_sync  
zsh: command not found: __exit__
   Compiling red_knot_python_semantic v0.0.0 (/home/ericmarkmartin/Development/ruff/crates/red_knot_python_semantic)
   Compiling red_knot_test v0.0.0 (/home/ericmarkmartin/Development/ruff/crates/red_knot_test)
    Finished `test` profile [unoptimized + debuginfo] target(s) in 6.09s
     Running tests/mdtest.rs (target/debug/deps/mdtest-149b8f9d937e36bc)

running 1 test
test mdtest__with_sync ... ok
```
[^1]

This is a minor annoyance which we can solve by using single-quotes instead of double-quotes for this string. To do so safely, we also escape single-quotes possibly contained within the string.

There is a [shell-quote](https://github.com/allenap/shell-quote) crate, which seems to handle all this escaping stuff for you but fixing this issue perfectly isn't a big deal (if there are more things to escape we can deal with it then), so adding a new dependency (even a dev one) seemed overkill.

[^1]: The filter does still work---it turns out that the filter `MDTEST_TEST_FILTER="sync.md - With statements - Context manager with non-callable  attribute"` (what you get after the failed interpolation) is still good enough

## Test Plan
<!-- How was it tested? -->

I broke the ``## Context manager with non-callable `__exit__` attribute`` test by deleting the error assertion, then successfully ran the new command it printed out.



---

_Review requested from @carljm by @ericmarkmartin on 2025-03-07 07:35_

---

_Review requested from @MichaReiser by @ericmarkmartin on 2025-03-07 07:35_

---

_Review requested from @AlexWaygood by @ericmarkmartin on 2025-03-07 07:35_

---

_Review requested from @sharkdp by @ericmarkmartin on 2025-03-07 07:35_

---

_Label `red-knot` added by @AlexWaygood on 2025-03-07 07:37_

---

_Comment by @MichaReiser on 2025-03-07 08:04_

Thanks. I verified that this doesn't break fish shell. 



---

_Merged by @MichaReiser on 2025-03-07 08:04_

---

_Closed by @MichaReiser on 2025-03-07 08:04_

---

_Branch deleted on 2025-03-08 15:37_

---
