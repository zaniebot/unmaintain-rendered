```yaml
number: 13683
title: "Redundant `with` around `urlopen()`"
type: issue
state: closed
author: opk12
labels: []
assignees: []
created_at: 2024-10-08T18:20:55Z
updated_at: 2024-10-09T10:58:14Z
url: https://github.com/astral-sh/ruff/issues/13683
synced_at: 2026-01-10T11:09:55Z
```

# Redundant `with` around `urlopen()`

---

_Issue opened by @opk12 on 2024-10-08 18:20_

What about a new check based on this example from the [`urllib.request` documentation](https://docs.python.org/3/library/urllib.request.html#examples), which tells that `urlopen` does not need to be used as a context manager? The first version is more verbose and has a level of indentation.

> ```
> with urllib.request.urlopen('http://www.python.org/') as f:
>     print(f.read(100).decode('utf-8'))
> ```
>
> It is also possible to achieve the same result without using the [context manager](https://docs.python.org/3/glossary.html#term-context-manager) approach.
>
> ```
> f = urllib.request.urlopen('http://www.python.org/')
> print(f.read(100).decode('utf-8'))
> ```

---

_Comment by @MichaReiser on 2024-10-09 06:30_

I'm a bit surprised that `urllib` promotes the use without a context manager. Reading through some stack overflow threads suggests that the returned object automatically calls `close` when the object gets deleted but that means that the resources are only cleaned up when the result object gets garbage collected. I don't think that's something we want to promote by a rule. @AlexWaygood do you know how the suggestion in the `urllib` documentation is to be understood?

---

_Comment by @AlexWaygood on 2024-10-09 10:58_

I'm not sure I would really describe the docs as _promoting_ the use of `urlopen` without a context manager. It mentions that it _can_ be used without a context manager (which is true: if you really want to take responsibility for closing it manually, you _can_), but the first examples given in the docs use a context manager, and most examples in the docs following this note also use a context manager.

I definitely don't think we should be recommending people to use `urlopen` without a context manager, I'm afraid. As @MichaReiser says, relying on the resource to be closed when the object is garbage-collected is extremely fragile. You have no idea exactly when it will be closed, and if the reference is only garbage-collected while Python is shutting down at the end of the programme, there's the possibility that the resource might never be closed properly.

Thanks for the rule suggestion @opk12! However, if there's a bug here, it's in the CPython `urlopen` docs, which could perhaps be clearer that although using the function without a `with` statement is possible, it's definitely not recommended.

---

_Closed by @AlexWaygood on 2024-10-09 10:58_

---
