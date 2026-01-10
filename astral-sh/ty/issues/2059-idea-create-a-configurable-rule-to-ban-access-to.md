```yaml
number: 2059
title: "[idea] Create a configurable rule to ban access to certain APIs"
type: issue
state: closed
author: danielgafni
labels:
  - wish
  - lint
assignees: []
created_at: 2025-12-18T10:38:10Z
updated_at: 2025-12-18T12:42:58Z
url: https://github.com/astral-sh/ty/issues/2059
synced_at: 2026-01-10T01:54:00Z
```

# [idea] Create a configurable rule to ban access to certain APIs

---

_Issue opened by @danielgafni on 2025-12-18 10:38_

I have the following use case: I'm using [Narwhals](https://narwhals-dev.github.io/narwhals/) for my library code, and accessing `narwhals.LazyFrame.columns` emits a `PerformanceWarning` that advises to access `narwhals.LazyFrame.collect_schema().names()` instead. 

It would be nice if I could ban `narwhals.LazyFrame.columns` access in my codebase and configure it in `ty.toml/pyproject.toml`, similarly to how [banned-module-level-imports](https://docs.astral.sh/ruff/rules/banned-module-level-imports/) works in Ruff. 

Example: 

```toml
[rules]
banned-apis = [
  "narwhals.LazyFrame.columns"
]
```

this should fail:
```py
import narwhals as nw
from narwhals.typing import Frame

def my_func(df: Frame): 
    # Frame is `nw.LazyFrame[Any] | nw.DataFrame[Any]`
    # so this should fail because nw.LazyFrame.columns is banned
    _ = df.columns  
```

This should pass: 

```py
import narwhals as nw
from narwhals.typing import Frame

def my_func(df: Frame): 
    if isinstance(df, nw.DataFrame):
        columns = df.columns  # nw.DataFrame.columns is not banned
    elif isinstance(df, nw.LazyFrame):
        columns = df.collect_schema().names()
    else:
        raise NotImplementedError()
```

---

Another very common use case would be banning deprectated APIs (at object method/attribute level)

---

_Comment by @MichaReiser on 2025-12-18 10:40_

Thanks for the suggestion. We don't plan on adding any rules for now that overlap with what Ruff already provides. 

---

_Closed by @MichaReiser on 2025-12-18 10:40_

---

_Comment by @danielgafni on 2025-12-18 10:41_

@MichaReiser this is different from Ruff. It doesn't target a **module**, it would allow targeting **methods**, **attributes**, and so on, which can only be recognized by `ty` as it requires typing information. Ruff cannot handle this. 

Maybe take a look at my example use case again. 

---

_Comment by @MichaReiser on 2025-12-18 11:05_

I think https://docs.astral.sh/ruff/settings/#lint_flake8-tidy-imports_banned-api does what you want

But it's certainly more limited than what ty could do here.

---

_Comment by @danielgafni on 2025-12-18 11:17_

Yes, I think the scope of `banned-api` in `flake8-tidy-imports` are module members at most. 

I'm looking for a more sophisticated algorithm here, it should respect the actual types and not just import paths. 

It should work with the rest of the type system such as `Union` and others.  

Perhaps this example may shed more light: 

```py
import narwhals as nw
from narwhals.typing import Frame

def my_func(df: Frame): 
    # Frame is `nw.LazyFrame[Any] | nw.DataFrame[Any]`
    # so this should fail because nw.LazyFrame.columns is banned
    _ = df.columns  
```

This should pass: 

```py
import narwhals as nw
from narwhals.typing import Frame

def my_func(df: Frame): 
    if isinstance(df, nw.DataFrame):
        columns = df.columns  # nw.DataFrame.columns is not banned
    elif isinstance(df, nw.LazyFrame):
        columns = df.collect_schema().names()
    else:
        raise NotImplementedError()
```

As you can see, this requires digging into types rather just import paths. 

Do you mind reopening the issue in this case? 

---

_Label `wish` added by @MichaReiser on 2025-12-18 11:23_

---

_Label `lint` added by @MichaReiser on 2025-12-18 11:23_

---

_Comment by @MichaReiser on 2025-12-18 11:25_

I'm still leaning towards closing this issue because I'd like to keep our issues actionable. The reason this issue isn't actionable is that we first need to decide whether we want Ruff to start using ty's semantic analysis or accept more lint-like rules in ty. This is something on our mind and we plan to tackle this after the stable release.

---

_Comment by @danielgafni on 2025-12-18 11:32_

Thank you, this makes a lot of sense. 

I've noticed your beta release blog post mentioned features such as "dead code analysis" and a few others, these are going to be part of `ty`?  

---

_Comment by @MichaReiser on 2025-12-18 11:37_

Adding dead-code analysis is something we plan to do in the near future, as Ruff doesn't support it today. I believe there's an issue for it somewhere

---

_Comment by @sharkdp on 2025-12-18 12:42_

We can already recognize parts of the code as being reachable or not (https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/unreachable.md), but we do not have a special lint rule for it (see #1948), and we also don't support "graying it out" in editors (see #784)

---
