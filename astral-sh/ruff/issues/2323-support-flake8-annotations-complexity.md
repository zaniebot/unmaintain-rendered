```yaml
number: 2323
title: Support flake8-annotations-complexity
type: issue
state: open
author: Crocmagnon
labels:
  - plugin
assignees: []
created_at: 2023-01-29T10:00:49Z
updated_at: 2026-01-12T18:39:21Z
url: https://github.com/astral-sh/ruff/issues/2323
synced_at: 2026-01-12T19:26:11Z
```

# Support flake8-annotations-complexity

---

_@Crocmagnon_

I'm using https://github.com/best-doctor/flake8-annotations-complexity to keep my type annotations from going wild, and would love to see support in ruff :)

There are two config options (https://github.com/best-doctor/flake8-annotations-complexity/blob/master/flake8_annotations_complexity/checker.py#L27-L38) and two checks (https://github.com/best-doctor/flake8-annotations-complexity/blob/master/flake8_annotations_complexity/ast_helpers.py#L59-L69).

---

_Label `plugin` added by @charliermarsh on 2023-01-29 16:30_

---

_Comment by @karpa4o4 on 2023-01-31 13:29_

@charliermarsh, hi. can i take this issue? 

---

_Comment by @charliermarsh on 2023-01-31 17:31_

@karpa4o4 - Sure, this seems reasonable to include.

---

_Comment by @darikoneil on 2025-12-19 22:24_

Is anyone still on this?

---

_Comment by @ntBre on 2025-12-22 14:32_

@darikoneil I don't think so.

---

_Comment by @danjones1618 on 2026-01-06 23:01_

I've started working on this in https://github.com/astral-sh/ruff/pull/22427

---

_Comment by @danjones1618 on 2026-01-06 23:31_

A couple of discussion points/questions for this rule(s):

## 1. single rule vs multiple rules
The plugin only implements one rule, `TAE002`, which flags for any annotation. This means there is no differentiation between annotation complexity in function/method arguments vs parameter declarations.

By splitting into two rules, function parameters can be configured to warn at a lower complexity than variable annotations. Is this desired or is it better to align with the plugin?

## 2. The `Annotated` type

The `Annotated` type introduced by [PEP-593](https://peps.python.org/pep-0593/) allows adding additional metadata that can be queried at runtime. This is used by libraries such as Pydantic to provide contextual configuration.

`flake8-annotations-complexity` does not make any special considerations for this type therefore using an `Annotated` type increases the reported complexity. Given the default configuration, a type of `Annotated[dict[str, Model], Field(...)]` would become an error. This is a likely annotation in a application. Should the `Annotated` type be ignored for the purpose of complexity calculation? Perhaps, with a configuration flag to disable this behaviour?

## 3. PEP-604 union syntax

[PEP-604](https://peps.python.org/pep-0604/) provides syntactic sugar for `Union[int, str]` as `int | str`. `flake8-annoations-complexity` does not increase complexity with the PEP-604 syntax. This seems like an oversight in the plugin as opposed to a feature therefore the new syntax should be reported as increasing complexity.

---

_Comment by @darikoneil on 2026-01-08 00:59_

> A couple of discussion points/questions for this rule(s):
> 
> ## 1. single rule vs multiple rules
> The plugin only implements one rule, `TAE002`, which flags for any annotation. This means there is no differentiation between annotation complexity in function/method arguments vs parameter declarations.
> 
> By splitting into two rules, function parameters can be configured to warn at a lower complexity than variable annotations. Is this desired or is it better to align with the plugin?

I don't particularly feel strongly about this one.

> ## 2. The `Annotated` type
> The `Annotated` type introduced by [PEP-593](https://peps.python.org/pep-0593/) allows adding additional metadata that can be queried at runtime. This is used by libraries such as Pydantic to provide contextual configuration.
> 
> `flake8-annotations-complexity` does not make any special considerations for this type therefore using an `Annotated` type increases the reported complexity. Given the default configuration, a type of `Annotated[dict[str, Model], Field(...)]` would become an error. This is a likely annotation in a application. Should the `Annotated` type be ignored for the purpose of complexity calculation? Perhaps, with a configuration flag to disable this behaviour?

As far as I understand, `Annotated` is intended to be treated as type `T` and the major type checkers all consider `Annotated[T, meta]` as `T`. For that reason, I think the complexity count should be calculated only on `T`. Being said, eliminating nested chaos is really in the *spirit* of annotations complexity rules, so I wouldn't necessarily having it as an option.


> ## 3. PEP-604 union syntax
> [PEP-604](https://peps.python.org/pep-0604/) provides syntactic sugar for `Union[int, str]` as `int | str`. `flake8-annoations-complexity` does not increase complexity with the PEP-604 syntax. This seems like an oversight in the plugin as opposed to a feature therefore the new syntax should be reported as increasing complexity.

This is actually kind of funny, because I think whether `Union[int, str]` (or `tuple[str, int]`) should be considered the same complexity as `str | int` is pretty debatable. For example, `str | bytes | PathLike` vs something like `list[tuple[int]]`.



---

_Comment by @ntBre on 2026-01-12 18:39_

> A couple of discussion points/questions for this rule(s):
> 
> ## 1. single rule vs multiple rules
> The plugin only implements one rule, `TAE002`, which flags for any annotation. This means there is no differentiation between annotation complexity in function/method arguments vs parameter declarations.
> 
> By splitting into two rules, function parameters can be configured to warn at a lower complexity than variable annotations. Is this desired or is it better to align with the plugin?
> 
> ## 2. The `Annotated` type
> The `Annotated` type introduced by [PEP-593](https://peps.python.org/pep-0593/) allows adding additional metadata that can be queried at runtime. This is used by libraries such as Pydantic to provide contextual configuration.
> 
> `flake8-annotations-complexity` does not make any special considerations for this type therefore using an `Annotated` type increases the reported complexity. Given the default configuration, a type of `Annotated[dict[str, Model], Field(...)]` would become an error. This is a likely annotation in a application. Should the `Annotated` type be ignored for the purpose of complexity calculation? Perhaps, with a configuration flag to disable this behaviour?
> 
> ## 3. PEP-604 union syntax
> [PEP-604](https://peps.python.org/pep-0604/) provides syntactic sugar for `Union[int, str]` as `int | str`. `flake8-annoations-complexity` does not increase complexity with the PEP-604 syntax. This seems like an oversight in the plugin as opposed to a feature therefore the new syntax should be reported as increasing complexity.

I'd be curious to hear from @MichaReiser and @amyreese on these, but my initial reaction would probably be:
1. stick with a single rule for now, we can split them later if needed
2. probably not count the `Annotated` layer and treat the annotation as the first element in the subscript
3. I think I probably would count this as additional complexity, but only one layer/level like the subscript version (assuming that _is_ how it works, I haven't looked at the PR yet)

I'm also wondering if we should just make this a `RUF` rule instead of adding a new linter for one rule, but that kind of relates to point (1) as well. We don't have to answer that now and can recode it before landing the PR anyway.

---
