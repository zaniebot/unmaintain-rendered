```yaml
number: 14698
title: "regression: ruff deletes imports; successive checks fail due to missing import"
type: issue
state: closed
author: WhyNotHugo
labels:
  - bug
assignees: []
created_at: 2024-12-01T11:41:16Z
updated_at: 2024-12-02T13:28:28Z
url: https://github.com/astral-sh/ruff/issues/14698
synced_at: 2026-01-12T15:54:54Z
```

# regression: ruff deletes imports; successive checks fail due to missing import

---

_@WhyNotHugo_

Running `ruff check --force-exclude --fix --exit-non-zero-on-fix` on my repository deletes some imports that are actually used.

The next time that I run `ruff check .`, it fails because of missing imports. The missing imports are the ones deleted by ruff.

## Reproduction steps

- git clone https://github.com/WhyNotHugo/django-afip/
- cd django-afip
- ruff check --force-exclude --fix --exit-non-zero-on-fix

## Observed result

`ruff` deletes imports which are in the `if TYPE_CHECKING` block:

```diff
diff --git a/django_afip/serializers.py b/django_afip/serializers.py
index 34fa8b4..bc2eb57 100644
--- a/django_afip/serializers.py
+++ b/django_afip/serializers.py
@@ -11,13 +11,6 @@ if TYPE_CHECKING:
     from datetime import date
     from datetime import datetime
 
-    from django.db.models import QuerySet
-
-    from django_afip.models import AuthTicket
-    from django_afip.models import Optional
-    from django_afip.models import Receipt
-    from django_afip.models import Tax
-    from django_afip.models import Vat
 
 
```

Running `ruff check` now fails:

```sh
> ruff check
django_afip/serializers.py:43:30: F821 Undefined name `AuthTicket`
   |
42 | @typing.no_type_check  # zeep's dynamic types cannot be type-checked
43 | def serialize_ticket(ticket: AuthTicket):  # noqa: ANN201
   |                              ^^^^^^^^^^ F821
44 |     return f.FEAuthRequest(
45 |         Token=ticket.token,
   |

django_afip/serializers.py:52:43: F821 Undefined name `QuerySet`
   |
51 | @typing.no_type_check  # zeep's dynamic types cannot be type-checked
52 | def serialize_multiple_receipts(receipts: QuerySet[Receipt]):  # noqa: ANN201
   |                                           ^^^^^^^^ F821
53 |     receipts = receipts.all().order_by("receipt_number")
   |

django_afip/serializers.py:52:52: F821 Undefined name `Receipt`
   |
51 | @typing.no_type_check  # zeep's dynamic types cannot be type-checked
52 | def serialize_multiple_receipts(receipts: QuerySet[Receipt]):  # noqa: ANN201
   |                                                    ^^^^^^^ F821
53 |     receipts = receipts.all().order_by("receipt_number")
   |

django_afip/serializers.py:69:32: F821 Undefined name `Receipt`
   |
68 | @typing.no_type_check  # zeep's dynamic types cannot be type-checked
69 | def serialize_receipt(receipt: Receipt):  # noqa: ANN201
   |                                ^^^^^^^ F821
70 |     taxes = receipt.taxes.all()
71 |     vats = receipt.vat.all()
   |

django_afip/serializers.py:128:24: F821 Undefined name `Tax`
    |
127 | @typing.no_type_check  # zeep's dynamic types cannot be type-checked
128 | def serialize_tax(tax: Tax):  # noqa: ANN201
    |                        ^^^ F821
129 |     return f.Tributo(
130 |         Id=tax.tax_type.code,
    |

django_afip/serializers.py:139:24: F821 Undefined name `Vat`
    |
138 | @typing.no_type_check  # zeep's dynamic types cannot be type-checked
139 | def serialize_vat(vat: Vat):  # noqa: ANN201
    |                        ^^^ F821
140 |     return f.AlicIva(
141 |         Id=vat.vat_type.code,
    |

django_afip/serializers.py:148:34: F821 Undefined name `Optional`
    |
147 | @typing.no_type_check  # zeep's dynamic types cannot be type-checked
148 | def serialize_optional(optional: Optional):  # noqa: ANN201
    |                                  ^^^^^^^^ F821
149 |     return f.Opcional(
150 |         Id=optional.optional_type.code,
    |

Found 7 errors.
```

## Expected results

No change; ruff should not delete imports which are being used.

## Additional details

- Using `ruff 0.8.1` from PyPI
- The issue is not reproducible with ruff 0.7.3

---

_Comment by @WhyNotHugo on 2024-12-01 11:43_

Also not an issue on `ruff 0.8.0`; only `0.8.1` is affected.

---

_Comment by @Daverball on 2024-12-01 11:52_

Looks like this regression was caused by #14615

If you remove the `@no_type_check` decorator, then `F401` stops triggering for those imports.

---

_Comment by @charliermarsh on 2024-12-01 12:49_

Is this incorrect? The spec says that "Functions with the `@no_type_check` decorator should be treated as having no annotations."

---

_Comment by @InSyncWithFoo on 2024-12-01 12:58_

> The spec says that "Functions with the `@no_type_check` decorator should be treated as having no annotations."

I think what it means is that we should treat its parameters (and return type) as `Any`. Consider this snippet, in which `Foo` and `Bar` are used in instantiations, the results of which are possibly inspected at runtime:

```python
from typing import no_type_check
from library import Foo, Bar  # Magic classes

@no_type_check
def f(v: Foo()) -> Bar(): ...

def frobnicate(f):  # More magic
	...; inspect.signature(f); ...
```


---

_Comment by @Daverball on 2024-12-01 12:59_

> Is this incorrect? The spec says that "Functions with the `@no_type_check` decorator should be treated as having no annotations."

What the typing spec says is irrelevant in this case, since the annotations will still be stored by the compiler and will be accessible at runtime. So the annotations should only really be ignored as far as type checkers are concerned.

Now, why you would annotate a function that is marked with `@no_type_check`, is an entirely different question. It does seem like a strange thing to do, but it doesn't really change that Ruff is causing runtime errors if it pretends the annotations don't exist at all and ends up removing unused imports, that aren't actually unused.

---

_Comment by @charliermarsh on 2024-12-01 13:03_

> What the typing spec says is irrelevant in this case, since the annotations will still be stored by the compiler and will be accessible at runtime. So the annotations should only really be ignored as far as type checkers are concerned.

I don't really agree, but from a practical standpoint, is does seem wrong to be removing imports here. I'm not sure where to draw the line though. Should _this_ raise a lint error? That was the initiating use-case.

```
from typing import no_type_check

@no_type_check
def f(v: Foo) -> Bar: ...  # Foo and Bar don't exist. Should we raise a lint error?
```

---

_Comment by @Daverball on 2024-12-01 13:06_

> I don't really agree, but from a practical standpoint, is does seem wrong to be removing imports here. I'm not sure where to draw the line though. Should _this_ raise a lint error? That was the initiating use-case.
> 
> ```
> from typing import no_type_check
> 
> @no_type_check
> def f(v: Foo) -> Bar: ...  # Foo and Bar don't exist. Should we raise a lint error?
> ```

Yes, absolutely it should. You could maybe argue that it shouldn't if you use `from __future__ import annotations` or are on Python 3.14, but even then it is somewhat questionable. But in most cases this will cause a `NameError` at runtime.

---

_Comment by @charliermarsh on 2024-12-01 13:07_

Does your opinion change given:

```
from typing import no_type_check

@no_type_check
def f(v: "Foo") -> "Bar": ...  # Foo and Bar don't exist. Should we raise a lint error?
```

---

_Comment by @Daverball on 2024-12-01 13:09_

This is functionally equivalent to the `from __future__ import annotations` case. You could certainly make a case for it, but I don't think you will find many people that would find that behavior useful.

---

_Comment by @Daverball on 2024-12-01 13:27_

While annotations were originally intended as a general purpose feature, the typing use-case is now so prevalent, that you can't really expect to use them for anything else without using `typing.Annotated`, since otherwise you will cause interoperability issues with runtime and static type checkers and make linters less powerful, since they now cannot make any assumptions about what's a forward reference and what isn't.

Using `no_type_check` to get around that seems like a fragile hack to me. The recommended way to do this, would be to use `Annotated[Any, "not_a_forward_reference"]`. I know that rubs some people the wrong way,  because of how verbose it is, and there have been some attempts to propose alternate syntax because of it. Which I think is the more productive way forward, rather than encourage patterns that may cause friction with the rest of the ecosystem.

---

_Comment by @WhyNotHugo on 2024-12-01 14:40_

> [...] why you would annotate a function that is marked with @no_type_check, is an entirely different question.

In this particular case, some of the variables used in this function are runtime-generated types, so there is no correct annotation for them. The type hints are hints for developers, so they understand what types these functions take as parameters.

These hints also get rendered into generated documentation.

---

_Comment by @MichaReiser on 2024-12-01 15:31_

We should align Ruff's behavior with what other type checkers do for best compatibility. 

Pyright does not raise any error for the example given by @charliermarsh ([playground](https://pyright-play.net/?code=GYJw9gtgBALgngBwJYDsDmUkQWEMopgD68CApkQMYAWZlA1gLABQLAAoSYhTXU8wBMywKMAAUANwBcUAGJgwASigBaAHxQAQgEMQMgHSGoUAMRyFUbSgFbdUAWBQByfGQAeSAM4x9UAMrUYACuADY2AO5kUCDaXlHaUCGoriDgIAD8QA))

```
from typing import no_type_check

@no_type_check
def f(v: Foo) -> Bar: ...  # Foo and Bar don't exist. Should we raise a lint error?
```

which matches my expectations after reading the specification. But we should definitely avoid removing imports if they're referenced from functions annotated with `no_type_check`

---

_Label `bug` added by @MichaReiser on 2024-12-01 15:31_

---

_Comment by @Daverball on 2024-12-01 16:16_

@MichaReiser Ruff in many ways captures much more detailed semantic state when it comes to things other than types compared to other type checkers. As far as I am aware all other type checkers currently ignore runtime/typing context, as in they consider everything to be in the same context. A lot of the lint rules rely on statically inferrable behavior based on the object model, rather than the type system. Type checkers typically rely on linters to detect some of these other issues.

As such I don't think it is sufficient to look at what other type checkers do here. You're hiding a statically detectable `NameError`, that would otherwise be caught by pyflakes.

But rereading the docs, I've come around on the case where forward references are used either implicitly or explicitly. `typing.get_type_hints` will explicitly ignore any classes or functions that have been decorated with `no_type_check` and return an empty dictionary, so there's no danger of a `NameError` and it is even explicitly mentioned in conjunction with `Annotated` that it allows arbitrary annotations to be used.

In order to avoid bad interactions with other rules I would still recommend treating them like normal forward references, just with the caveat, that no F821 will be emitted for missing bindings. I.e. a narrow exception to the rule, rather than changing the state of the semantic model by not visiting those nodes at all. The additional state for making the more narrow exception is already being tracked through the `NO_TYPE_CHECK` flag. So it would only be a matter of reverting the change to `visit_type_definition` and adding additional code to F821 to detect these kinds of forward references.

Edit: It might also be feasible to switch from skipping to calling `visit_non_type_definition` to reduce the chance of interacting badly with type definition related rules, but that might cause issues for things like:

```python
from typing import no_type_check, TYPE_CHECKING

if TYPE_CHECKING:
    from foo import Foo

@no_type_check
def bar(a: list["Foo"]) -> None: ...
```

So it's probably not really a good idea to do that either, since you're just changing where the problem may occur. There's similar kinds of issues in `flake8-type-checking` with deciding whether or not to flag something as requiring a forward reference or not where we have soft-references to deal with that sort of thing. With a soft reference we always assume the code is correct and don't emit an error in either direction, i.e. it's both considered a reference for things that check for references and not a reference for things that check for references that don't point anywhere in a given context.

---

_Comment by @eli-schwartz on 2024-12-01 16:24_

It's not clear to me whether the discussion thus far has accounted for the fact that the imports which are deleted are inside of a `if TYPE_CHECKING:` block.

I would personally find it a bit surprising to see imports which cannot be resolved at runtime due to being imported under `TYPE_CHECKING`, which are simultaneously used as `@no_type_checking`. Deleting the imports won't produce NameError any way you slice it.

If they're not used for typing, and they're not used for *not-typing*, then semantically speaking, what precisely are they used for?

---

_Comment by @MichaReiser on 2024-12-02 08:26_

@Daverball I don't think it's important whether Ruff's model is more or less complicated than that of type checkers. What's important is that Ruff's behavior matches users' expectations and, to some extent, the typing spec. The typing spec isn't very clear about most details when it comes to `no_type_checking` and the user expectation is mainly defined by what existing tools with `@no_type_check` support do. [Pyflakes](https://github.com/PyCQA/pyflakes/issues/595) does not support for `@no_type_check`. 

> As such I don't think it is sufficient to look at what other type checkers do here. You're hiding a statically detectable NameError, that would otherwise be caught by pyflakes.

That's true, but that's the decorator's intent. It's a very broad `noqa` suppression that applies to typing errors throughout the function. And it should probably be handled the same where some rules test if they're in a `no_type_check` block and, if so, don't emit their diagnostic (or skip running entirely)


---

_Comment by @Daverball on 2024-12-02 09:10_

> That's true, but that's the decorator's intent. It's a very broad `noqa` suppression that applies to typing errors throughout the function. And it should probably be handled the same where some rules test if they're in a `no_type_check` block and, if so, don't emit their diagnostic (or skip running entirely)

That's only true as far as type errors are concerned. A `NameError` is not a type error, the only reason type checkers still tend to highlight most of them, is because it's free for them to do so. They don't know what the type is without being able to look it up, so they might as well emit an error, rather than silently infer `Any`. But type checkers generally don't go out of their way to ensure that all potential `NameError`s are highlighted, since they know there are already linters that cover that part well enough.

So I agree that the decorator's intent is to silence any errors relating to the validity of those annotations as types, but annotations are a general purpose construct and they can store arbitrary expressions. It just so happens that that space has mostly been taken over by type hints.

So semantically a name lookup still occurs if there are no forward references, so removing that fact from the semantic model is a bug, plain and simple. The case with forward references is more subtle. Implicit forward references look like regular code, so I think people's expectation is that the load is still simulated, even if it may never happen at runtime. If you want to put an actual plain string into the annotation, then use a string, don't abuse an implicit forward reference for that.

With explicit forward references I'm fine with skipping the load, but I think it's still more robust to do it anyways and just silence any errors related to failed name lookups (or invalid type expressions). That preserves the annotations as pure documentation without enforcement use-case.

---

_Comment by @MichaReiser on 2024-12-02 09:16_

I get the feeling that we're saying the same but are talking past each other :) 

* Keep analysing type annotations in `@no_type_check` blocks as we did before #14615 
* Stop emitting specific diagnostics inside `@no_type_check` blocks. E.g. any diagnostic related to invalid type annotations, or even possible `NameError`s


---

_Comment by @MichaReiser on 2024-12-02 12:11_

We should probably revert this change and redo the implementation

---

_Comment by @Daverball on 2024-12-02 13:15_

Edit: For some reason Eli's comment is rendered at the bottom of this issue for me, so I apologize if this confusion had already been cleared up in the meantime.

@eli-schwartz This was addressed implicitly through discussion about the desired semantics of `@no_type_check` with the involvement of implicit/explicit forward references.

In the given example the annotations are still used for documentation. If you remove the imports entirely, you now would have to guess what these symbols mean. That's counter-intuitive, especially with an implicit forward reference introduced through `from __future__ import annotations`, which still reads like a regular python expression, rather than a string. With explicit string annotations it would be easier to justify, but it also seems unnecessary, since we can stop F821/F722 from triggering without changing the semantics of annotation expressions within `@no_type_check`.

You generally probably wouldn't want ruff to destroy an explicit reference, just because there's a chance it will never be used. An explicit reference is always better than no reference at all, since it makes the code easier to understand.

---

_Closed by @MichaReiser on 2024-12-02 13:28_

---
