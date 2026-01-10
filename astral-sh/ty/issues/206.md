```yaml
number: 206
title: Diagnostic for conflicting declared attribute types
type: issue
state: open
author: sharkdp
labels:
  - help wanted
  - typing semantics
  - attribute access
assignees: []
created_at: 2025-02-05T11:58:19Z
updated_at: 2025-06-04T16:05:44Z
url: https://github.com/astral-sh/ty/issues/206
synced_at: 2026-01-10T02:34:09Z
```

# Diagnostic for conflicting declared attribute types

---

_Issue opened by @sharkdp on 2025-02-05 11:58_

We should emit a diagnostic when we see conflicting declared types of attributes, either between the declaration in the class body and declarations in a method, or between annotated assignments in different methods. We have existing tests for this scenario (see `TODO` comments):

https://github.com/astral-sh/ruff/blob/eb08345fd5434ed3db243233df6dd91757a6af3b/crates/red_knot_python_semantic/resources/mdtest/attributes.md?plain=1#L212-L226

part of: https://github.com/astral-sh/ruff/issues/14164

---

_Renamed from "Emit a diagnostic when we see conflicting declared types between class body and `__init__` (see TODO in `own_instance_member`)" to "[red-knot] Diagnostic for conflicting declared attribute types" by @sharkdp on 2025-02-05 12:01_

---

_Label `help wanted` added by @sharkdp on 2025-02-05 12:01_

---

_Comment by @smokyabdulrahman on 2025-02-10 17:52_

I am not sure if this is helpful, but here is what `pyright` reports for the mentioned test case:

```python
class C:
    """
    Diagnostics:
    Declaration "z" is obscured by a declaration of the same name [reportRedeclaration]
    """

    z: int

    def __init__(self) -> None:
        self.x = get_int()
        """
        Diagnostics:
        Declaration "y" is obscured by a declaration of the same name [reportRedeclaration]
        """
        self.y: int = 1

    def other_method(self):
        self.x = get_str()

        self.y: str = "a"

        self.z: str = "a"
```

I am trying to learn/understand the technical stuff behind parsers by reading (books, blogs) and contributing to tools I love that has things to do with parsers (ruff ❤).

is this issue about adding a new rule? or just fixing/refactoring `red-knot` tests mentioned (modify the markdown file and mention the correct error/reveal messages)?

---

_Comment by @sharkdp on 2025-02-10 20:37_

> is this issue about adding a new rule?

Yes, this is about emitting a new diagnostic (we can probably reuse [conflicting-declarations](https://github.com/astral-sh/ruff/blob/f30fac632685c1edd200005d110cf407992bd69a/crates/red_knot_python_semantic/src/types/diagnostic.rs#L103-L109)) when we see something like this.

We do want to report them in the same places where pyright reports *"declaration … is obscured by …"*, but I think we want to use a slightly different wording, as we generally allow re-declarations of symbols:
```py
x: int = 1
x: str = "foo"
```
What's not allowed though, is to have multiple visible conflicting declarations:
```py
def _(flag: bool):
    if flag:
        x: str
    else:
        x: int

    x = 1  # error: [conflicting-declarations]
```

For the case mentioned in this issue, we want something similar, but for declarations of *attributes*.

---

_Comment by @carljm on 2025-02-10 21:16_

(To be clear, though, this issue is _not_ about adding a new _ruff_ rule, it's about a new red-knot diagnostic. Although they are in the same codebase, red-knot is separate from ruff and shares only the parser/AST with it.)

---

_Comment by @smokyabdulrahman on 2025-02-11 17:30_

Thank you all for the information. All clear.

I will give this issue a shot.

---

_Comment by @smokyabdulrahman on 2025-02-13 21:39_

While trying to dive deep into the code, I've found the following comments where I think the `report_lint` should happen.

https://github.com/astral-sh/ruff/blob/cb8b23d60915206d9ad74fd351a36dd0d4ed1231/crates/red_knot_python_semantic/src/types.rs#L4323-L4329

Also, I've found this TODO:
https://github.com/astral-sh/ruff/blob/cb8b23d60915206d9ad74fd351a36dd0d4ed1231/crates/red_knot_python_semantic/src/types.rs#L4224-L4237

However, I believe that `type.rs` doesn't have the context to report the diagnostic for a reason.

Could you please point me to a similar example where a similar rule had been tackled?

My thought process is to implement some logic in:
https://github.com/astral-sh/ruff/blob/cb8b23d60915206d9ad74fd351a36dd0d4ed1231/crates/red_knot_python_semantic/src/types/infer.rs#L899-L904

and:
https://github.com/astral-sh/ruff/blob/cb8b23d60915206d9ad74fd351a36dd0d4ed1231/crates/red_knot_python_semantic/src/types/infer.rs#L929-L934

or to check here if the target is an instance member of class, and check all the symbol declarations in the scope of the class body, by calling `symbol_from_declarations` and report the conflicting declarations if any.
https://github.com/astral-sh/ruff/blob/cb8b23d60915206d9ad74fd351a36dd0d4ed1231/crates/red_knot_python_semantic/src/types/infer.rs#L2121-L2125

---

_Comment by @sharkdp on 2025-02-14 15:23_

> While trying to dive deep into the code, I've found the following comments where I think the `report_lint` should happen.

This is the place where we would end up if there would be conflicting declarations *in the class body*. This is just one possible source for declarations of an attribute.

> Also, I've found this TODO:

This is the other place, correct. Here, we loop over declarations of attributes in methods. We currently return early, but we probably need to change the loop logic to collect all declared types, and then emit a diagnostic in case there are more than one.

Finally, we also need to consider the option that there is a declaration on the class body, and a conflicting declaration in a method.

--

> However, I believe that `type.rs` doesn't have the context to report the diagnostic for a reason.

We are currently in the process of changing some infrastructure to make this easier. Maybe it would be good to  split this task into two parts: (1) *collect* all conflicting declared types so we would in principle be able to emit a diagnostic — assuming we could do it from anywhere, and (2) actually create a new diagnostic and make the required changes to be able to emit it.

I think you could already start with part 1, and for part 2, it might make sense to wait for those changes to the diagnostics infrastructure.

Let me know if I failed to answer any of the questions that would be required for part 1.

---

_Comment by @smokyabdulrahman on 2025-02-15 22:21_

Thank you so much, this made it clearer.
I will try and approach part 1.

---

_Label `help wanted` added by @MichaReiser on 2025-05-07 15:22_

---

_Renamed from "[red-knot] Diagnostic for conflicting declared attribute types" to "Diagnostic for conflicting declared attribute types" by @MichaReiser on 2025-05-07 15:26_

---

_Label `typing semantics` added by @AlexWaygood on 2025-05-11 07:50_

---

_Label `attribute access` added by @AlexWaygood on 2025-05-11 08:05_

---

_Comment by @lipefree on 2025-06-04 16:05_

I can try to do this one since the last PR was closed. I guess there is enough information in this issue and the closed PR for me to start investigating !

---
