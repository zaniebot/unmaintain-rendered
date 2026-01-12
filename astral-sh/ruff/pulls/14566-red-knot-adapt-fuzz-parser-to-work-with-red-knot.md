```yaml
number: 14566
title: "red-knot: adapt fuzz-parser to work with red-knot"
type: pull_request
state: merged
author: connorskees
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: red-knot-fuzz
created_at: 2024-11-24T21:45:57Z
updated_at: 2024-11-25T13:17:43Z
url: https://github.com/astral-sh/ruff/pull/14566
synced_at: 2026-01-12T15:55:48Z
```

# red-knot: adapt fuzz-parser to work with red-knot

---

_@connorskees_

Adds a `--bin` argument to the `fuzz-parser` script, which makes it possible to fuzz either `ruff` or `red_knot`.

This fuzzing finds a few panics already in some seeds:

```sh
>>> python scripts/fuzz-parser/fuzz.py --bin red_knot 0-9
Running `cargo build --release` since no test executable was specified...
Concurrently running the fuzzer on 10 randomly generated source-code files...
Ran fuzzer successfully on seed 1                            [1/10]
Ran fuzzer successfully on seed 8                            [2/10]
Ran fuzzer successfully on seed 5                            [3/10]
Ran fuzzer successfully on seed 0                            [4/10]
Ran fuzzer successfully on seed 2                            [5/10]
Ran fuzzer successfully on seed 6                            [6/10]
Ran fuzzer successfully on seed 3                            [7/10]
Ran fuzzer on seed 7                                         [8/10]
The following code triggers a bug:

async def name_0[*name_1](name_2: name_0(), /):
    pass

Ran fuzzer on seed 9                                         [9/10]
The following code triggers a bug:

class name_5[**name_1](name_5[name_3]):
    pass

Ran fuzzer on seed 4                                        [10/10]
The following code triggers a bug:

class name_2[name_0](*name_2):
    pass

Bugs found in the following seeds:
4 7 9
```

This was tested manually by running the above command for `--bin ruff` and `--bin red_knot`.

Resolves https://github.com/astral-sh/ruff/issues/14157, though does not add the fuzzing to CI due to the existing failures.

---

_Review requested from @carljm by @connorskees on 2024-11-24 21:45_

---

_Review requested from @MichaReiser by @connorskees on 2024-11-24 21:45_

---

_Review requested from @AlexWaygood by @connorskees on 2024-11-24 21:45_

---

_Review requested from @sharkdp by @connorskees on 2024-11-24 21:45_

---

_Comment by @sharkdp on 2024-11-24 21:50_

> This fuzzing finds a few panics already in some seeds:

> `class name_2[name_0](*name_2):`

Probably similar to the panics here related to self-referential class bases: https://github.com/astral-sh/ruff/blob/ac23c9974435cfa4524ff864ec1e5e73c6b910ed/crates/red_knot_workspace/tests/check.rs#L269-L272
And/or here: https://github.com/astral-sh/ruff/issues/14333

---

_Comment by @github-actions[bot] on 2024-11-24 22:04_

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

_Label `testing` added by @AlexWaygood on 2024-11-24 22:15_

---

_Label `red-knot` added by @AlexWaygood on 2024-11-24 22:15_

---

_Comment by @MichaReiser on 2024-11-24 22:29_

Nice, thank you. Do you think it would be hard to instead test if the exit code isn't 1 or zero (panic should use something else) instead of adding the new flag to red knot?

---

_Comment by @connorskees on 2024-11-24 22:36_

Oh I've always assumed `panic!` exited with 1, but that seems not to be the case. Great suggestion, thank you, that is a lot better

---

_@dhruvmanila reviewed on 2024-11-25 03:59_

---

_Review comment by @dhruvmanila on `scripts/fuzz-parser/fuzz.py`:288 on 2024-11-25 03:59_

Can we limit this to just "ruff" or "red_knot" using the `choices` field? (https://docs.python.org/3/library/argparse.html#choices)

---

_@dhruvmanila reviewed on 2024-11-25 04:15_

---

_Review comment by @dhruvmanila on `scripts/fuzz-parser/fuzz.py`:286 on 2024-11-25 04:15_

I think we can skip this as the values from the `choices` field will be visible in `--help`
```suggestion
        help="Name of executable to test.",
```

---

_@AlexWaygood approved on 2024-11-25 13:10_

Thanks @connorskees, this is great! I pushed a few nitpicks, but nothing major.

---

_Merged by @AlexWaygood on 2024-11-25 13:12_

---

_Closed by @AlexWaygood on 2024-11-25 13:12_

---
