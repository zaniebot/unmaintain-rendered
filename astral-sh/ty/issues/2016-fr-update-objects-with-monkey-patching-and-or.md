```yaml
number: 2016
title: "FR: Update objects with monkey patching and/or setattr"
type: issue
state: closed
author: deanm0000
labels: []
assignees: []
created_at: 2025-12-17T15:02:45Z
updated_at: 2025-12-17T18:43:19Z
url: https://github.com/astral-sh/ty/issues/2016
synced_at: 2026-01-10T01:53:59Z
```

# FR: Update objects with monkey patching and/or setattr

---

_Issue opened by @deanm0000 on 2025-12-17 15:02_

### Summary

It would be great if we could update objects and the ty would be aware of those updates. As an example of a real use case, take [polars's](https://docs.pola.rs/api/python/stable/reference/api.html) ability to extend the API. While this feature works great at runtime, it doesn't work with type checking as you can see... https://i.imgur.com/fX4EGpR.png

More generically here's a more minimal example

```python
class Foo:
    def __init__(self, thing:str):
        self.bar=thing


my_foo=Foo("abc")
setattr(my_foo, "abz", "hello") # it'd be great if it would update what it expects of `my_foo` here
my_foo.abz="hello" # this syntax too, if possible

print(my_foo.abz) # As-is it gives, Object of type `Foo` has no attribute `abz`

setattr(Foo, "abz", "hello") # also updating the class itself
Foo.abz="hello" 

print(my_foo.abz) # Same message as above
```

In rust, this is kind of like implementing your own trait on an imported struct/enum and rust-analyzer knows about the new trait like this

<img width="973" height="561" alt="Image" src="https://github.com/user-attachments/assets/3ec3faf7-154e-4b6e-afc2-c61482e599d7" />


---

_Comment by @sharkdp on 2025-12-17 15:09_

> In rust, this is kind of like implementing your own trait on an imported struct/enum and rust-analyzer knows about the new trait like this

You could do the same thing in Python by subclassing from both `Foo` and a protocol that describes this dynamic `abz` member.

```py
class Foo:
    def __init__(self, thing:str):
        self.bar=thing

class HasAbzMember:
    abz: str

class FooWithAbz(Foo, HasAbzMember): ...

my_foo=FooWithAbz("abc")
setattr(my_foo, "abz", "hello")

reveal_type(my_foo.abz)  # str
```
https://play.ty.dev/eeaab3b9-b39d-4e25-b759-d136554d7731

---

_Comment by @deanm0000 on 2025-12-17 16:02_

For the generic case, subclassing works but in the case of polars, one of its desirable features is the ability to chain Expr expressions like `pl.col("a").sum().add(5).div(2)`

Subclassing would break chaining, for example

```python
import polars as pl

class MyExpr(pl.Expr):
    def __init__(self, x):
        self._pyexpr=pl.col(x)._pyexpr
    def new_thing(self):
        return self +5

def mycol(x:str):
    return MyExpr(x)
a=mycol("a") ## is a MyExpr

b=a.sum() ## sum returns Expr
b2 = b.new_thing() ## doesn't work because b is an Expr

c=a.new_thing() ## works b/c a is still MyExpr
```

Also, with subclassing you can't import multiple plugins so if I want to use both `MyExpr` and `FranksExpr` then I'd be out of luck.

I realize this looks like a very niche use case but I think if the feature existed other libraries focused on interop would make use of the feature.

---

_Comment by @carljm on 2025-12-17 18:43_

We already support this locally, using regular assignment syntax: https://play.ty.dev/5237acdf-5f51-453b-a96f-3ad94916ab07

```py
class Foo:
    pass

f = Foo()
f.bar = "baz"  # emits an error

reveal_type(f.bar)  # still reveals `Literal['baz']`
```

I think emitting an error on `f.bar = "baz"` is the right thing to do, as that attribute is not part of the type `Foo` (and it may well be a typo or a mistake), but if you suppress that error, we'll still respect the result of the assignment.

Recognizing `setattr` with fixed known attribute name would be reasonably easy, but doesn't seem very useful -- if the attribute name is known, why use `setattr` in the first place? (Unless it's just a workaround to avoid the type error on the assignment -- but I think if we added support to recognized fixed-name setattr assignments, we'd emit that error on them, too.) If the attribute name is not statically known, then there's not much we can do with that `setattr()` call.

But I'm not sure any of this really helps your use case, because we don't support any non-local effects here. If you dynamically add an attribute to `Foo` itself, we aren't going to start respecting that attribute on all instances of `Foo` everywhere. I think this is something we are unlikely to ever support, unless it were carefully designed as a new type system feature in a PEP.

I'm going to close this issue for now because I don't think it's currently actionable for us, but not closed to more discussion of the idea! Thanks for the suggestion.

---

_Closed by @carljm on 2025-12-17 18:43_

---
