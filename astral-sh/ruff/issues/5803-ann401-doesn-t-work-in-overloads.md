```yaml
number: 5803
title: "ANN401 doesn't work in overloads"
type: issue
state: open
author: ItsDrike
labels:
  - bug
  - documentation
assignees: []
created_at: 2023-07-16T12:43:37Z
updated_at: 2024-05-25T16:21:52Z
url: https://github.com/astral-sh/ruff/issues/5803
synced_at: 2026-01-12T15:54:45Z
```

# ANN401 doesn't work in overloads

---

_@ItsDrike_

Note that some of the points I made here were moved to be addressed from another issue: #5871, as requested. These points were not actually bugs, and this behavior was intentionally not a part of ANN401, as these weren't a part of this rule in the original `flake8-annotations` extension. The new issue is a feature request, asking for these to be added, leaving this issue purely as a bug report from one of the points I listed, which was indeed a valid bug. I've marked the points that were moved as strikethrough and you can ignore those when reading the issue description. If you're interested in those, check out the linked issue instead.

---

It seems that Ruff's implementation of ANN401 is very permissive, and probably unintentionally incomplete.

- ~~Unlike the `flake8-annotation` extension, ruff doesn't seem to consider `Any` being used as a generic argument an issue.~~
- ~~Regular variable annotations (including class variables) don't trigger ANN401~~
- The concrete implementation of an overloaded function can contain `Any` (might be intentional though)

I ran ruff with `ruff check --isolated --fix --show-source --show-fixes --select ANN401 test.py`, on version 0.0.278.

### ~~Any in generic arguments~~

<s>

```python
from typing import Any

def foo(x: dict[str, Any]) -> None:
    ...
```

</s>

~~This should be considered as an issue, but isn't. In case you'd feel like this would be too strict, there should at least be a new rule capable of enforcing this. However `flake8-annotations` would consider this as an issue.~~

### ~~Any in variable annotations~~

<s>

```python
from typing import Any, ClassVar

a: Any = 5

class Foo:
    X: ClassVar[Any] = 10
    Y: Any = 50
```

</s>

~~None of these are currently marked as violations, even though it's a clear use of the Any type as an annotation. It seems that ANN401 only triggers when used in a function annotation, variables are not checked.~~

### Any in concrete overload

I assume that this is intentional, but I didn't find any docs to support that (I looked [here](https://beta.ruff.rs/docs/rules/any-type/)).

```python
from typing import overload

@overload
def foobar(x: str) -> str:
    ...

@overload
def foobar(x: int) -> int:
    ...

def foobar(x: Any) -> Any:  # no violation
    ...
```

If it is indeed intentional, it should maybe be mentioned somewhere? As someone moving from `flake8-annotations`, suddenly seeing ruff report an unnecessary noqa: ANN401 there is surprising, and even though I think it makes sense here, it could be worth documenting that this is a special case, exempt from ANN401.

Edit note: This behavior actually does NOT make sense, and is indeed a bug see  https://github.com/astral-sh/ruff/issues/5803#issuecomment-1640567168:
> As for the overload, after thinking on it for a bit, yeah, it should probably not behave like this, even though to the outside user of that function, the `Any` type there would not be visible, it should still absolutely be a union of those types, or a supertype shared by all types in the overloads, to allow the type-checker to properly check the code of the actual implementation of that function. So that is a bug which should get fixed.

---

_Renamed from "ANN401 doesn't trigger for generic arguments" to "ANN401 doesn't trigger for variables, nor generic attributes" by @ItsDrike on 2023-07-16 13:03_

---

_Comment by @dhruvmanila on 2023-07-18 10:37_

Hey, thanks for using Ruff and opening this issue.

> Any in generic arguments

You've mentioned here that `flake8-annotations` raises a violation for the provided example  in this section but I'm unable to reproduce this. Can you provide the command you used to run `flake8` on the example? I've used the following command:

```console
flake8 --extend-select=ANN401 test.py
```

It's also mentioned in the plugin README that this rule isn't supported for nested dynamic types (https://github.com/sco1/flake8-annotations#dynamic-typing-caveats):

> Nested dynamic types (e.g. `typing.Tuple[typing.Any]`) and redefinition (e.g. `from typing import Any as Foo`) will not be identified.

_Ruff identifies redefinition_

---

> It seems that ANN401 only triggers when used in a function annotation, variables are not checked.

Yes, this is true as is the case for `flake8-annotations`:

> Only function declarations are considered by this plugin; type annotations in function/module bodies are not checked

---

> Any in concrete overload

This seems like a bug as it should be raising violations similar to the plugin. Thanks for noticing this!

---

Overall, I think we should improve the documentation to be clear on where this rule will be checked and enable it on `@overload` implementation function.

---

_Label `bug` added by @dhruvmanila on 2023-07-18 10:40_

---

_Label `documentation` added by @dhruvmanila on 2023-07-18 10:40_

---

_Comment by @charliermarsh on 2023-07-18 11:55_

(Omitting Any in a concrete overload might be desirable, is that not a common pattern?)

---

_Comment by @dhruvmanila on 2023-07-18 12:53_

I'm not sure if it's a common pattern as the concrete annotation would probably be a union of all possible types. Although, `flake8-annotations` does raise violation in this case.

---

_Comment by @ItsDrike on 2023-07-18 16:39_

Oh, you're actually right, it seems that `flake8-annotations` also doesn't check this, huh, I've only noticed this when moving our code-base to ruff, as Ruff has reported unused noqa comments. Apparently this was never an issue, and those comments were just added because someone assumed it would be.

That's pretty interesting, so in that case, I'd like to make this a feature request, as I think there is value in having a linter rule capable of finding use of `Any` in those nested type definitions, and it would be really nice if we were able to detect variable annotations containing `Any` too.

I'm not sure if the `flake8-annotations`'s ANN401 rule should be modified to include this, as that would not be compatible with the original extension then, so new rules would probably be needed here.

As for the overload, after thinking on it for a bit, yeah, it should probably not behave like this, even though to the outside user of that function, the `Any` type there would not be visible, it should still absolutely be a union of those types, or a supertype shared by all types in the overloads, to allow the type-checker to properly check the code of the actual implementation of that function. So that is a bug which should get fixed.

---

_Comment by @dhruvmanila on 2023-07-18 17:47_

> That's pretty interesting, so in that case, I'd like to make this a feature request, as I think there is value in having a linter rule capable of finding use of `Any` in those nested type definitions, and it would be really nice if we were able to detect variable annotations containing `Any` too.

If possible can you open a new issue for this (better for tracking)? Otherwise I can open it later as well :)

> As for the overload, after thinking on it for a bit, yeah, it should probably not behave like this, even though to the outside user of that function, the `Any` type there would not be visible, it should still absolutely be a union of those types, or a supertype shared by all types in the overloads, to allow the type-checker to properly check the code of the actual implementation of that function. So that is a bug which should get fixed.

I agree. Any thoughts here? /cc @charliermarsh @zanieb 

---

_Comment by @zanieb on 2023-07-18 19:05_

Regarding generic arguments, `dict[str, Any]` is fairly common in my experience. Anytime you're working with `kwargs` (outside of the definition itself) e.g.

```python
from typing import reveal_type

def foo(**kwargs):
    reveal_type(kwargs)  # dict[str, Any]
```

It seems better to use a generic that's partially complete than not at all. It would probably be reasonable to allow people to opt-in to enforcing narrower types though. I see a less controversial diagnostic for cases like `list[Any]` (either write `list` or narrow the generic) where _all_ of the generic arguments are `Any`.

Variable annotations seem totally reasonable to address.

I agree best practice for the overloaded function is a union of all overload types. I think we should raise a violation here and consider implementing an autofix. Type checkers will not detect issues within the function body otherwise.

---

_Comment by @ItsDrike on 2023-07-18 20:42_

Alright, I've opened a separate issue for the feature request, leaving this one just as a bug report.

By the way, I've also noticed another behavior that Ruff doesn't pick up on, this one might be harder to address though, and perhaps something that should be left for a type checker to identify like with [mypy's dynamic typing disallowing options](https://mypy.readthedocs.io/en/stable/command_line.html#disallow-dynamic-typing) , and I would understand if it was not something you'll want to fix:

```python
from typing import TypeAlias, Any

MyAny: TypeAlias = Any

def foo(x: MyAny) -> None:
    ...
```

In this code, Ruff doesn't report any annotation issues, even though `x` is actually annotated with the `Any` type, it's just hidden behind a type alias.

---

_Comment by @dhruvmanila on 2023-07-19 01:04_

> Alright, I've opened a separate issue for the feature request, leaving this one just as a bug report.

Thanks!

> In this code, Ruff doesn't report any annotation issues, even though `x` is actually annotated with the `Any` type, it's just hidden behind a type alias.

Yes, type aliases are a known issue across multiple rules as they're difficult to detect reliably.

---

_Comment by @ItsDrike on 2023-07-22 18:28_

> Yes, type aliases are a known issue across multiple rules as they're difficult to detect reliably.

That's totally fair, though it should probably be mentioned in the rule's docs, to make this clear.

---

_Renamed from "ANN401 doesn't trigger for variables, nor generic attributes" to "ANN401 doesn't work in overloads" by @ItsDrike on 2023-07-31 09:39_

---
