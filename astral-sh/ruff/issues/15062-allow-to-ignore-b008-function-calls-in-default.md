```yaml
number: 15062
title: Allow to ignore B008 (function calls in default arguments) in cases where the function is a method of a local variable
type: issue
state: open
author: beneyal
labels:
  - type-inference
assignees: []
created_at: 2024-12-19T12:43:47Z
updated_at: 2024-12-21T13:49:19Z
url: https://github.com/astral-sh/ruff/issues/15062
synced_at: 2026-01-10T11:09:56Z
```

# Allow to ignore B008 (function calls in default arguments) in cases where the function is a method of a local variable

---

_Issue opened by @beneyal on 2024-12-19 12:43_

Hey all,

First, I want to thank the maintainers of this tool for doing a spectacular job, Ruff is a joy to use! 🚀 

I'll begin the issue with a small reproduction:

```python
class A:
    pass


class B:
    def f(self) -> A:
        return A()


def foo(b: B) -> None:
    def bar(x: A = b.f()) -> None:  # Do not perform function call `b.f` in argument defaults; instead, perform the call within the function, or read the default from a module-level singleton variable [B008]
        print(x)

    bar(A())
```

When using flake8-bugbear's `extend-immutable-calls` section, I could ignore `b.f`. In Ruff, I can't, because I don't have a fully qualified path to it. I tried adding `B.f` to `extend-immutable-calls`, but it didn't work.

Is there a way to ignore such a thing using `extend-immutable-calls`? Thanks! 🙏 

---

_Comment by @MichaReiser on 2024-12-20 09:11_

Thanks for the kind words. 

Hmm, no, I don't think there's a way to do it today. Ruff's code that is used to resolve a name (e.g. `B.b`) doesn't understand following arguments but it would be required to understand that `b` is typed as `B`. 

Relevant code

https://github.com/astral-sh/ruff/blob/c0b7c36d435441788b5a1f2330a66613656a3e47/crates/ruff_python_semantic/src/model.rs#L938

---

_Label `type-inference` added by @MichaReiser on 2024-12-20 09:11_

---

_Comment by @beneyal on 2024-12-21 13:01_

Oh, okay, thanks! Should I keep the issue open as a "feature request", or just close it?

---

_Comment by @MichaReiser on 2024-12-21 13:49_

I'd leave it open for now. It might be possible to add (limited) support for it with Ruff's current model. 

---
