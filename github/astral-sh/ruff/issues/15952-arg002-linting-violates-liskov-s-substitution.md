---
number: 15952
title: "ARG002 Linting Violates Liskov's Substitution Principle"
type: issue
state: open
author: kevdog824
labels:
  - documentation
  - rule
  - type-inference
assignees: []
created_at: 2025-02-04T22:30:19Z
updated_at: 2025-12-18T15:32:09Z
url: https://github.com/astral-sh/ruff/issues/15952
synced_at: 2026-01-07T13:12:16-06:00
---

# ARG002 Linting Violates Liskov's Substitution Principle

---

_Issue opened by @kevdog824 on 2025-02-04 22:30_

### Description

### Keyword Searches

I conducted a search on this GitHub repo's issues tab using the search `is:issue ARG002`. I found a few issues closely related to this one, but most were about specific use cases (keyword argument decorators, dunder methods, etc.).

### Software Version

I have successfully reproduced this issue on `v0.9.3` and `v0.9.4`

### Description

[`ARG002`](https://docs.astral.sh/ruff/rules/unused-method-argument/) relates to unused method arguments. In some cases, ruff will flag an unused argument that is necessary to keep as is to maintain an inherited interface/functional signature.

### Minimal Reproducible Example

```py
# myfile.py


class Interface:
    # Parent defines an interface that is dependent on arg2
    def method(self, arg1: int, arg2: int) -> int:
        return arg1 + arg2


class Implementation(Interface):
    # ruff flags unused arg2 here
    def method(self, arg1: int, arg2: int) -> int:
        return arg1 * 2


def some_method(instance: Interface) -> None:
    # The following should be allowed for both Interface and Implementation by LSP.
    # However, it would cause a TypeError if arg2 in Implementation.method was renamed
    # to "_" or "_arg2" as ruff suggests
    instance.method(arg1=1, arg2=2)
    ...

```

Running ruff CLI on the previous code

```sh
python -m ruff check myfile.py --select "ARG" --isolated
```

produces the following output

```sh
myfile.py:12:33: ARG002 Unused method argument: `arg2`
   |
10 | class Implementation(Interface):
11 |     # ruff flags unused arg2 here
12 |     def method(self, arg1: int, arg2: int) -> int:
   |                                 ^^^^ ARG002
13 |         return arg1 * 2
   |

Found 1 error.
```

The expected behavior would be to ignore the unused argument. It is required to be there (named as is) to maintain the inherited interface.

### Suggested Fix

`ARG002` linting rule should NOT apply against any argument that meets both of the following conditions:
* The method the argument belongs to has been inherited from a parent class
* The argument is NOT a positional-only parameter in the parent (i.e. `method(self, arg1: int, arg2: int, /) -> int`)

We don't need to apply this fix to positional only parameters as they can be renamed to match the current suggested pattern of `_...` without breaking the public interface

### Suggested Workarounds

* Add `ARG002` to the `ignore` list in the `tool.ruff.lint` section of your pyproject.toml to ignore the linting rule across your project
* Use the `# noqa: ARG002` directive to ignore the linting rule on a case-by-case basis
* use `del unused_var` or `_ = unused_var` at the top of the method body to get ruff to ignore the unused arguments

---

_Comment by @kevdog824 on 2025-02-04 22:40_

If [#1246](https://github.com/astral-sh/ruff/issues/1256) is implemented an alternative fix would also be changing the severity of the linting rule to warning by default

---

_Label `bug` added by @ntBre on 2025-02-04 22:49_

---

_Comment by @ntBre on 2025-02-04 22:57_

Thanks for reporting this! We might at least be able to check for methods inherited from base classes in the same file.

---

_Comment by @dylwil3 on 2025-02-04 23:00_

I don't think this is a bug, but maybe we should update the documentation/hint. The correct thing to do in this case is import `override` from `typing` and decorate your method with it:

```diff
diff --git a/a.py b/b.py
index b07e67dd7..46ac453f0 100644
--- a/a.py
+++ b/b.py
@@ -1,4 +1,5 @@
 # myfile.py
+from typing import override
 
 
 class Interface:
@@ -9,6 +10,7 @@ class Interface:
 
 class Implementation(Interface):
     # ruff flags unused arg2 here
+    @override
     def method(self, arg1: int, arg2: int) -> int:
         return arg1 * 2


```

this will satisfy the linter! [Playground link](https://play.ruff.rs/d9463952-9002-4566-bda1-93b9a9a38155)

There are similar exemptions built into the rule (but not documented) for overloads and abstract methods. I think the example in question _itself_ maybe violates Liskov substitution? Since the actual method behavior on an instance of the class would differ whether you regard that class as an instance of `Implementation` or of `Interface`? But I am very likely wrong about that - I never took a CS class 😄 

---

_Label `bug` removed by @dylwil3 on 2025-02-04 23:01_

---

_Label `documentation` added by @dylwil3 on 2025-02-04 23:01_

---

_Label `diagnostics` added by @dylwil3 on 2025-02-04 23:01_

---

_Label `diagnostics` removed by @MichaReiser on 2025-02-05 08:07_

---

_Label `rule` added by @MichaReiser on 2025-02-05 08:07_

---

_Label `type-inference` added by @MichaReiser on 2025-02-05 08:07_

---

_Comment by @kevdog824 on 2025-02-05 14:49_

> @dylwil3: I don't think this is a bug, but maybe we should update the documentation/hint. The correct thing to do in this case is import override from typing and decorate your method with it

Thanks for the `@override` tip! I haven't gone through all the ruff documentation (there's a lot of it!) but the bit I have gone through doesn't really make mention of how typing decorators like `@typing.override` and `@typing.final` effect the linting process (in general or on a rule-by-rule basis). Not to say it isn't documented, just that I hadn't encountered the documentation if it is.

I would agree in general though that documenting this situation (and the provided solution) on the [ARG002](https://docs.astral.sh/ruff/rules/unused-method-argument/) page at a minimum would be a good idea.

My only concern with this solution is what would you do in Python versions <3.12 before override was introduced? Importing it from `typing_extensions` backport? I'm not sure what ruff's philosophy is on relying on `typing_extensions` for solving reasonably common issues. We're a long ways off from the sunset of Python <= 3.11.

> @dylwil3: I think the example in question itself maybe violates Liskov substitution? Since the actual method behavior on an instance of the class would differ whether you regard that class as an instance of Implementation or of Interface? But I am very likely wrong about that - I never took a CS class 😄

I don't think the example itself violates LSP. The idea behind LSP is just that if I require something of type `A` then anything that inherits from `A` should also be able to satisfy that requirement. For example, if I have a function parameter annotated as type `Animal`, then any instance of `Dog`, `Cat`, `Horse`, etc. should be able to be passed as that function parameter without issue.

The problem I attempted to outline in the example is if we were to rename a parameter in the subclass to be prefixed with an underscore (as ruff's documentation seems to suggest) it would no longer be a perfect substitute for the annotated type. This would be a non-issue in languages like Java where keyword arguments are not allowed, so parameters can be (more or less) safely renamed in subclasses. However, in Python we can reference parameters by name when passing arguments (i.e. `method(x=1, y=2)`) unless otherwise stated with the `/` parameter (i.e. `def method(x, y, /)`). Because of that, the parameter names themselves are a part of the public interface and cannot be safely renamed in subclasses. Most of the Python code I've seen outside of the standard library doesn't utilize the `/` parameter so I imagine this is a situation that could pop up pretty frequently.

I would be willing to agree though that finding yourself in this situation is *likely* a code smell for something such as poorly chosen design/abstraction or premature optimization. Nonetheless, it is still valid Python code.

---

_Comment by @kevdog824 on 2025-02-05 15:06_

The other question I have is why does the `@override` decorator need to be used explicitly? Isn't the fact that a method is being overridden in a subclass implicit in the inheritance hierarchy? Does Python's support for multiple inheritance make it unclear (i.e. in the case where two parent classes have conflicting signatures for a method)? Is it for performance reasons within ruff that perhaps certain checks are skipped unless we are explicit about method overriding?

---

_Comment by @ntBre on 2025-02-05 16:11_

I think `@override` is just a shortcut for ruff to avoid actually inspecting the inheritance hierarchy. @MichaReiser added the `type-inference` label, I think because full type inference and multi-file diagnostics are needed to fully resolve the base classes and know that a method is being inherited. This is what I was getting at with my earlier comment that we may be able to handle the single-file case with our current infrastructure, but we definitely can't handle the fully general case of resolving all base class methods if they come from other files.

I don't think the rule currently tries to look at the base class definitions at all, even in the same file, so there's no way for it to know that a method overrides another method without the decorator, which it does check for.

And yes, I think it's pretty common in ruff to use `typing_extensions` for backports of things like this. Many of the rules I've seen explicitly handle both `typing` and `typing_extensions`.

---

_Comment by @kevdog824 on 2025-02-05 18:40_

> @ntBre: I think `@override` is just a shortcut for ruff to avoid actually inspecting the inheritance hierarchy.

I figured as much. Feels like this inspection would be difficult to implement. It even might be impossible to implement as a generalized solution. It also seems like it would be a big drag on performance that isn't justify by the marginal benefit only some users would receive. This all makes sense to me.

I'll just stick with using the `@override` decorator to remove the error during linting. Thanks all!

---

_Closed by @kevdog824 on 2025-02-05 18:40_

---

_Comment by @Tishka17 on 2025-08-12 16:27_

`@override` decorator is unavailable on older python version and some projects (like libraries) cannot use `typing_extension` as it is not stable additional dependency. Because of this, I do not see this issue as completed.

---

_Comment by @ntBre on 2025-08-12 17:13_

I think it makes sense to reopen this. The issue won't truly be closed until we can use type inference to check if the method is an override, but we could also update the rule docs to mention `@override` as a temporary measure, as @dylwil3 suggested.

We could also consider adding a `help`-level subdiagnostic now that says something like "if this method is being overriden from a parent class, consider using `@override`" too, for classes with a base class list.

---

_Reopened by @ntBre on 2025-08-12 17:13_

---

_Comment by @Avasam on 2025-08-21 06:11_

> `@override` decorator is unavailable on older python version and some projects (like libraries) cannot use `typing_extension` as it is not stable additional dependency. Because of this, I do not see this issue as completed.


```py
from typing import TYPE_CHECKING
# most type checkers will also support `if False:`
# or `TYPE_CHECKING = False` rather than import
if TYPE_CHECKING:
    from typing_extensions import override
else:
    def override(func):
        return func
```

If you wanna reduce repetition across files, write that in a module like `my_project/typing_compat.py`

And configure `ruff.toml` with
```toml
[lint]
typing-modules = ["my_project.typing_compat"]
```

See https://docs.astral.sh/ruff/settings/#lint_typing-modules

---

_Referenced in [thousandbrainsproject/tbp.monty#541](../../thousandbrainsproject/tbp.monty/pulls/541.md) on 2025-11-11 12:59_

---

_Comment by @LoicRiegel on 2025-12-18 15:32_

> but we could also update the rule docs to mention `@override` as a temporary measure

This seems to be the case already: https://docs.astral.sh/ruff/rules/unused-method-argument/#why-is-this-bad

I can take a look to add the help-level subdiagnotic

---
