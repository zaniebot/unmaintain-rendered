```yaml
number: 20390
title: ban-while-true
type: issue
state: open
author: Richienb
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-09-15T00:46:09Z
updated_at: 2025-09-16T17:18:13Z
url: https://github.com/astral-sh/ruff/issues/20390
synced_at: 2026-01-10T11:09:59Z
```

# ban-while-true

---

_Issue opened by @Richienb on 2025-09-15 00:46_

### Summary

This rule would ban the `while True`/`break` pattern.

This is part of a style guide.
I presume the reason is to avoid accidental crashes.

---

_Comment by @MeGaGiGaGon on 2025-09-15 01:40_

> This is part of a style guide.

Which style guide? I checked [PEP 8](https://peps.python.org/pep-0008/) but `while True`s are not mentioned, so is this part of an existing style guide? Or an existing linter? Or would it be part of a new style guide in ruff itself?

---

_Comment by @Richienb on 2025-09-15 01:50_

> Which style guide?

The one for the university I attend.

---

_Label `rule` added by @ntBre on 2025-09-15 12:42_

---

_Label `needs-decision` added by @ntBre on 2025-09-15 12:42_

---

_Comment by @mscheifer on 2025-09-15 15:43_

Can you link to the style guide?

---

_Comment by @eli-schwartz on 2025-09-15 19:37_

> I presume the reason is to avoid accidental crashes.

Which crashes?

---

_Comment by @Richienb on 2025-09-16 01:41_

> Can you link to the style guide?

@mscheifer Unfortunately, I don't think the style guide is published

> Which crashes?

@eli-schwartz What I mean is that I think the purpose is to make it easier to avoid accidentally writing infinitely looping code if the condition is not `True`, but something actually falsifiable.

The encouraged pattern is:
```py
condition = do()
while condition:
    condition = do()
```

---

_Comment by @eli-schwartz on 2025-09-16 02:03_

> [@eli-schwartz](https://github.com/eli-schwartz) What I mean is that I think the purpose is to make it easier to avoid accidentally writing infinitely looping code if the condition is not `True`, but something actually falsifiable.

That's all fine and well, but "crash" has a fairly specific meaning. Using it to refer to infinitely looping code feels wrong.

Plus, infinitely looping code isn't necessarily wrong. It could be the main event loop of an application, which gets exited by:

```python
while True:
    if not do():
        print('finished')
        sys.exit(0)
```

Your "encouraged pattern" doesn't provide a mechanism for detecting whether `def do():` is capable of yielding a falsy value, which would be vital for breaking out of the loop...

---

_Comment by @MeGaGiGaGon on 2025-09-16 07:13_

If the goal is to prevent hangs from infinite loops, then just banning `while True`s seems very limited to me. You can just as easily crash programs with a `while stack:`  loop if your stack grows infinitely. Why are they not considered a hazard, while `while True` is?

One place I could see this being useful is instead checking for a specific pattern. This may be what OP intended, but just wasn't clear on. It would be specifically targeting this pattern:
```py
while True:
    if not condition:
        break
    ...
```
Since that should be turned into
```py
while condition:
    ...
```

There could also be an equivalent transformation for loops that look like
```py
while True:
    x = some_function_or_method_call()
    if not x:
        break
    ...
```
Turning them into
```py
while (x :=some_function_or_method_call()):
    ...
```

---

_Comment by @MichaReiser on 2025-09-16 07:46_

I agree that outright banning `while True` doesn't seem to be a very useful rule (other than enforcing a very specific style guide of which Ruff only has few rules). 

The clippy counterparts seem more useful to me:

* [`never_loop`](https://rust-lang.github.io/rust-clippy/stable/index.html#never_loop): Loops that always exit in the first iteration
* [`while_let_loop`](https://rust-lang.github.io/rust-clippy/stable/index.html#never_loop) Nested `while .. loop` that can be simplified to a single `while`
* [`infinite_loop`](https://rust-lang.github.io/rust-clippy/stable/index.html#never_loop) loop that never exits

---
