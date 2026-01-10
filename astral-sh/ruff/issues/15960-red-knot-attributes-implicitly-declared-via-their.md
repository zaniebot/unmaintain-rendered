```yaml
number: 15960
title: "[red-knot] Attributes implicitly declared via their parameter types"
type: issue
state: closed
author: sharkdp
labels:
  - help wanted
  - ty
assignees: []
created_at: 2025-02-05T10:55:47Z
updated_at: 2025-02-18T21:43:13Z
url: https://github.com/astral-sh/ruff/issues/15960
synced_at: 2026-01-10T11:09:57Z
```

# [red-knot] Attributes implicitly declared via their parameter types

---

_Issue opened by @sharkdp on 2025-02-05 10:55_

The idea of this ticket is to support the following use case.

```py
class C:
    def __init__(self, x: int | None):
        self.x = x

reveal_type(C().x)  # Should be `int | None`, is currently `Unknown | int | None`
```

We have pre-existing tests with `TODO` comments for this scenario. The goal of this issue it to get rid of these (there are more `TODO`s that would be resolved by this feature):

https://github.com/astral-sh/ruff/blob/7ca778f4923590503b1be7958c4e44fcab1e0f98/crates/red_knot_python_semantic/resources/mdtest/attributes.md?plain=1#L33-L34

Part of: #14164 

---

_Renamed from "Support attributes which are implicitly "declared" via their parameter types (`self.x = param`)" to "[red-knot] Attributes implicitly declared via their parameter types" by @sharkdp on 2025-02-05 10:56_

---

_Label `red-knot` added by @sharkdp on 2025-02-05 10:56_

---

_Label `help wanted` added by @sharkdp on 2025-02-05 13:52_

---

_Comment by @mishamsk on 2025-02-10 01:19_

@sharkdp I was looking into this issue, and not sure your intent tbh. Here is the top of the test where I've added mypy types alongside red_knot current output:

```python
class C:
    def __init__(self, param: int | None, flag: bool = False) -> None:
        value = 1 if flag else "a"
        self.inferred_from_value = value
        self.inferred_from_other_attribute = self.inferred_from_value
        self.inferred_from_param = param
        self.declared_only: bytes
        self.declared_and_bound: bool = True
        if flag:
            self.possibly_undeclared_unbound: str = "possibly set in __init__"

c_instance = C(1)

reveal_type(c_instance.inferred_from_value)  # revealed: Unknown | Literal[1, "a"]
# MYPY: Revealed type is "Union[builtins.int, builtins.str]"

# TODO: Same here. This should be `Unknown | Literal[1, "a"]`
reveal_type(c_instance.inferred_from_other_attribute)  # revealed: Unknown
# MYPY: Revealed type is "Union[builtins.int, builtins.str]"

# TODO: should be `int | None`
reveal_type(c_instance.inferred_from_param)  # revealed: Unknown | int | None
# MYPY: Revealed type is "Union[builtins.int, None]"
```

putting aside the fact that knot prefers to infer literals, where pyright/mypy would infer a type - I am not sure about two things:
* why special treatment for `inferred_from_param` (no `Unknown` in the union), but not for for `inferred_from_value`?
* why adding `Unknown` in the first place?

I guess, you want to try to distinguish "declaration" (explicit type annotation by user) versus pure inference. But I'd say it is hard to draw a line here. You suggest to infer the RHS of `inferred_from_param` assignment as "declaration". But what about this then:

```
declared: int = 42

class C:
    def __init__(self):
        self.special_attr = declared
```

and what if we import a function with declared return type? And so on ad nauseam.

IMHO, mypy/pyright behavior - avoiding `Unknown` and trusting inference in the method make more practical sense. Injecting unknown would mean that any downstream usage would allow anything to be assigned to the attribute. Whoever writes the class is encapsulating some logic by creating the class, they "own" that logic, including types of attributes. If an attribute is accessed outside of the class, it can be considered a public contract => imho type checker should not account for a possibility of some value assigned outside from class/method body to be valid, unless it is compatible.

So I'd just remove this:

https://github.com/mishamsk/ruff/blob/main/crates/red_knot_python_semantic/src/types.rs#L4190-L4194

However, this will bring us back to the downsides of eagerly inferring literals, since after this change this (and all similar cases) will stop being fine:

```python
# This assignment is not fine anymore fine, as we infer `Literal[1, "a"]` for `inferred_from_value`.
# error: [invalid-assignment] "Object of type `Literal["value set on instance"]` is not assignable to attribute `inferred_from_value` of type `Literal[1, "a"]`"
c_instance.inferred_from_value = "value set on instance"
```

LMK

---

_Comment by @sharkdp on 2025-02-11 12:50_

> why adding `Unknown` in the first place?

This is a good question. Adding `Unknown` is a deliberate deviation from the behavior of existing type checkers. Since we will have to explain this to users eventually, I wrote [some documentation](https://github.com/astral-sh/ruff/blob/main/crates/red_knot_python_semantic/resources/mdtest/doc/public_type_undeclared_symbols.md) that explains our reasoning. Let me know if that makes sense or not.

> why special treatment for `inferred_from_param` (no `Unknown` in the union), but not for for `inferred_from_value`?

This is also a good question. It _is_ a special treatment. And it is _not_ consistent with the behavior of code that looks very similar, as you pointed out. But we anticipate that use cases like
```py
class C:
    def __init__(self, x: int | None = None):
        self.x = x
```
are very common. Forcing users to duplicate all type annotations does not seem like an appealing solution:
```py
class C:
    def __init__(self, x: int | None = None):
        self.x: int | None = x
```

> I guess, you want to try to distinguish "declaration" (explicit type annotation by user) versus pure inference.

Yes

> You suggest to infer the RHS of `inferred_from_param` assignment as "declaration".

Correct. We would treat that first example above just like the second example, and pretend that `self.x = x` would be a declaration.

> I'd say it is hard to draw a line here.

And I would agree with you.

I think we would currently try to draw the line right there. If we see `self.attr = param`, where `param` is a declared parameter of a method, that is supported. Everything else is not. So for both of these attributes, we would still union with `Unknown` (and I can completely understand why that might be surprising):
```py
class C:
    def __init__(self, x: int | None = None, y: str = ""):
        self.x = 0 if x is None else x
        self.y = y + "!"

reveal_type(C().x)  # Unknown | int
reveal_type(C().y)  # Unknown | str
```

Interested to hear your thoughts.

---

_Comment by @mishamsk on 2025-02-11 13:35_

I forgot to add one more example, which I believe[d] to be common. Here is an actual piece from a pretty big commercial project:

```py
class Graph:
    def __init__(self, graph: nx.MultiDiGraph | None = None) -> None:
        self._graph = graph or nx.MultiDiGraph()
```

so, I'd expect `self._graph` to be of `type[nx.MultiDiGraph]`, not `type[nx.MultiDiGraph] | None`.

Fun fact, when I searched for this pattern now, I only found two instances (out of thousands of classes), and the second of the two had explicit declaration for the attribute without the None.

I still think that such pattern, that allows optional attribute and applies a default is very common. I think I saw it in, say, `unicorn` - I can research if necessary.

At the very least, I would special case for this.

In general - I am not sure what are the benefits of adding unknown in class context for the reasons I outlined above (class is a kind of a contract, and all internal code should be considered as implementing/assuming that contract). But I trust you have good reasons for that.

I can implement this special case - let me know if you think the `None` should be special cased as well. The rule maybe - if the inferred type == param_type - None => assume declared as such

---

_Comment by @sharkdp on 2025-02-11 14:33_

> I forgot to add one more example, which I believe[d] to be common.

This seems to be the same like my `C().x` example above, or am I missing something?

> I'd expect `self._graph` to be of `type[nx.MultiDiGraph]`, not `type[nx.MultiDiGraph] | None`.

Assuming you mean "`self._graph` to be of type `nx.MultiDiGraph`, not type `nx.MultiDiGraph | None`": yes, we would eliminate `None` from the union due to type-narrowing in that Boolean expression. But we would then union with `Unknown` to arrive at `Unknown | nx.MultiDiGraph` for the instance variable `_graph`; that is, unless we decide to add an extended special case for something like this.

> At the very least, I would special case for this.

> I can implement this special case - let me know if you think the None should be special cased as well. The rule maybe - if the inferred type == param_type - None => assume declared as such

Let's go without any additional special casing for now and keep that idea in mind (or in a ticket, once this is resolved).

> In general - I am not sure what are the benefits of adding unknown in class context […] But I trust you have good reasons for that.

I tried to explain our reasons in the linked document.

> class is a kind of a contract, and all internal code should be considered as implementing/assuming that contract

So, in your opinion, what does the contract of this "old style dataclass" (we are talking about unannotated code here) say about the type of `timezone`? That it is `None` and remains `None` forever?
```py
class Timestamp:
    timezone = None

    def __init__(self, hours, minutes):
        self.hours = hours
        self.minutes = minutes
```

---

_Comment by @mishamsk on 2025-02-12 03:19_

@sharkdp thanks a lot for the doc link, it helped indeed. If you think there are other such docs that will help me understand overarching red_knot goals/strategy that guide such decisions - would really appreciate them. The more I know, the easier it is for me to align with the core team.

> So, in your opinion, what does the contract of this "old style dataclass" (we are talking about unannotated code here) say about the type of timezone? That it is None and remains None forever?

I want and will answer later. I want to do a small research and gather stats.

For now I've opened a PR #16111. I really really hope that this time I did exactly what you'd expected 🤞

---

_Comment by @sharkdp on 2025-02-13 12:17_

As discussed privately, we decided to *not* implement this feature for now. I will leave the ticket open because I want to reflect that decision in our `attributes.md` test suite.

---

_Assigned to @sharkdp by @sharkdp on 2025-02-13 12:17_

---

_Closed by @sharkdp on 2025-02-18 21:43_

---

_Closed by @sharkdp on 2025-02-18 21:43_

---
