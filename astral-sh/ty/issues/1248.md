```yaml
number: 1248
title: better gradual guarantee for un-annotated dict (and other container?) literals
type: issue
state: open
author: carljm
labels:
  - needs-decision
assignees: []
created_at: 2025-09-24T16:10:32Z
updated_at: 2026-01-08T18:37:30Z
url: https://github.com/astral-sh/ty/issues/1248
synced_at: 2026-01-10T01:56:40Z
```

# better gradual guarantee for un-annotated dict (and other container?) literals

---

_Issue opened by @carljm on 2025-09-24 16:10_

In the ecosystem, we see this kind of pattern:

```py
def f(a: int, b: str): ...

x = { "a": 1, "b": "2" }

# Expected `int` found `Unknown | int | str`
f(x["a"], x["b"])
# or
f(**x)
```

In this case, an un-annotated dict literal is used implicitly as a heterogeneous `TypedDict`. We (along with mypy and pyrefly) throw errors on these calls, because we infer the type of `x` as `dict[str, Unknown | int | str]`. (Pyrefly infers `dict[str, int | str]`, mypy `dict[str, object]`.)

Pyright avoids this problem by falling back to `dict[str, Unknown]` rather than inferring a union value type, when the dict contents look heterogeneous.

A similar problem can occur with list literals (e.g. `x = [1, "a"]; f(x[0], x[1])`). We do see this in the ecosystem too, but it's less common than with dictionaries (probably because tuples are an attractive alternative, and implicit heterogeneity is supported for tuples).

Perhaps ideally this would be solved by inferring a more precise heterogeneous "implicit TypedDict" type for these literals, but this gets very difficult to handle correctly with mutations.

If we do implement a "gradual mode" vs "strict mode", it may be worth emulating pyright's behavior in the "gradual mode".

---

_Label `needs-decision` added by @carljm on 2025-09-24 16:10_

---

_Label `gradual-guarantee` added by @carljm on 2025-09-24 16:10_

---

_Comment by @sharkdp on 2025-09-25 08:13_

FWIW, I think we should prioritize this. [2,471 new diagnostics](https://github.com/astral-sh/ruff/pull/20523#issuecomment-3325399343) is a massive amount. That averages to 17 new diagnostics per ecosystem project. They are obviously not evenly distributed, but a lot of projects are affected. I also see these errors in some local Python codebases (mypy_primer, ecosystem-analyzer) that previously had very few diagnostics. It may also not be immediately clear to users what is even going on:

```py
state = {"name": "ABC", "counter": 0}

state["counter"] += 1  # Operator `+=` is unsupported between objects of type `str` and `Literal[1]`

print(state["name"].lower())  # Attribute `lower` on type `Unknown | str | int` may be missing
```


---

_Added to milestone `Beta` by @carljm on 2025-09-25 14:33_

---

_Comment by @carljm on 2025-09-25 14:34_

Tentatively putting this in the beta milestone, but it's still not clear to me if we should unconditionally do this, or make it subject to a config parameter that would also toggle the unioning with `Unknown` that we do.

---

_Comment by @sharkdp on 2025-09-29 14:03_

Instead of falling back to `Unknown`, we could potentially return [`AnyOf[ValueType1, ValueType2, …]`](https://github.com/python/typing/issues/566)? This is similarly forgiving to `Unknown`, but would allow us to use auto-completions when typing e.g. `state["name"].lo<CURSOR>`.

This would require a change to the aggregation logic [here](https://github.com/astral-sh/ruff/blob/00c8851ef8d643434340577f1b1ee65b5e728ac1/crates/ty_python_semantic/src/types/ide_support.rs#L107), where we currently reduce the completions for a union type to the *intersection* of the completions on the element types. This logic is correct, but we did already question whether unioning would be the more practically useful way of combining completions  when we originally introduced it.

Another way to achieve similar results was suggested by @carljm, where we could track two different types for each expression. The "correct" type and a "best guess" type for LSP use cases. For the strong-gradual-guarantee case, we could return `Unknown` as the "correct" type, and `int | str` as the "best guess" type.

---

_Comment by @sharkdp on 2025-09-30 07:19_

Where ty's current behavior definitely differs from mypy, pyrefly is when you explicitly annotate with `dict[str, Any]` in an attempt to get rid of these errors. Because we always prefer the inferred type (see also #136):

```py
from typing import Any

state: dict[str, Any] = {"name": "ABC", "counter": 0}

state["counter"] += 1  # Still an error!
print(state["name"].lower())  # Still an error!
```

---

_Comment by @sharkdp on 2025-10-17 07:18_

With the latest changes in https://github.com/astral-sh/ruff/pull/20927, my last comment here is not relevant anymore. So I will move this from Beta to GA. Feel free to move it back if anyone disagrees.

---

_Removed from milestone `Beta` by @sharkdp on 2025-10-17 07:18_

---

_Added to milestone `GA` by @sharkdp on 2025-10-17 07:18_

---

_Comment by @carljm on 2025-12-18 00:13_

Since I filed this issue, [Pyrefly seems to have implemented](https://pyrefly.org/sandbox/?project=N4IgZglgNgpgziAXKOBDAdgEwEYHsAeAdAA4CeS4ATrgLYAEALqcROgOZ0Q3G6UN2UYANxiooAfSbEYAHXRzMMMHTAAKVIk7oGAGjrZNcBpQCUmwhblz8dALx1gdGSFTPNARj3PsbpyABMznQAvlbo%2BADa3s4AunZ0-nR0AMR0MJTUlHDWhACuxJioDDCqwNFICcEmSanouGkZvHq56HC4LZhhgiJikswl%2BNUpdJgQAMYMEUaUeqz8AD500zFy3aISUgNRLrFDqXOrwut90qqR5TF7S8ZhtfXpmXB0uOhpEAwAFul0Y2JQiGE1OcdiAYnpgT5QSY5KleHI1AAqBGDOQgHQgXIMaBwEjkRAgVIAVSxUHepBULQmEBe2XkWCUKl4NCK4nQuRo2HSZ00c2qAFoAHzXSgA15JAQwBi5SivMDOABy7M5Ioc%2BFCIFR6LIgjAUFIhAYtCgFFSAAVSDq9UsMDgCD8XpA2NKitT0IQYXQAMowGB0D4MBjEOCIAD0Ie1Sj1hF4bBDMHQIcwuDGcBDYwdECdlBdLxDjModFQQlQ0FQ2Fg9vQjudWJez2Itda7vQZE%2BLz5IiyrvizgAzIR3IENegQMF0agqSIAGLQGAUNBYPBEMijoA) a version of the "infer implicit TypedDict" approach for dict literals with heterogeneous value types. It prevents some mutations that would change the type of a specific key, but allows others unsoundly:

```py
from typing import reveal_type

def f(a: int, b: str): ...

x = { "a": 1, "b": "2" }  # shows `dict[str, int | str]` in inlay hint

x["b"] = 2  # error, even though the "official" type is dict[str, int | str]
x.update({"b": 2})  # no error, unsound

reveal_type(x)  # dict[str, int | str]
reveal_type(x["a"])  # int
reveal_type(x["b"])  # str

# no errors on either call:

f(x["a"], x["b"])
# or
f(**x)
```


---

_Label `gradual-guarantee` removed by @AlexWaygood on 2025-12-19 12:07_

---

_Removed from milestone `Stable` by @carljm on 2026-01-08 18:37_

---

_Added to milestone `Pre-stable 1` by @carljm on 2026-01-08 18:37_

---
