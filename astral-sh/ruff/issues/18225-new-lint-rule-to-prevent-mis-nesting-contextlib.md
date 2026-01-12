```yaml
number: 18225
title: "New lint rule to prevent mis-nesting `contextlib.ExitStack`"
type: issue
state: open
author: Zac-HD
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-05-20T16:10:50Z
updated_at: 2025-05-21T05:33:01Z
url: https://github.com/astral-sh/ruff/issues/18225
synced_at: 2026-01-12T15:54:56Z
```

# New lint rule to prevent mis-nesting `contextlib.ExitStack`

---

_@Zac-HD_

### Summary

I really like context managers: they make resource and lifetime management easy, and the `with` and `async with` statements ensure that their entries and exits form a last-in, first-out stack structure.

Unfortunately, `contextlib.ExitStack` makes it easy to accidentally violate this structure, because context managers are entered by calling `stack.enter_context(cm)`, and exited at the close of the `with ExitStack() as stack:` block.  If you call the `.enter_context()` method _after_ entering (but before exiting) another context manager, they won't be correctly nested.  For example:

```python
from contextlib import ExitStack, contextmanager

@contextmanager
def ctx(tag):
    print(f"entering ctx {tag}")
    yield
    print(f" exiting ctx {tag}")

with (
    ExitStack() as stack,
    ctx(1),  # entering ctx 1
):
    stack.enter_context(ctx(2))  # entering ctx 2
    with ctx(3):  # entering ctx 3
        stack.enter_context(ctx(4))  # entering ctx 4
    # exiting ctx 3
# exiting ctx 1
# exiting ctx 4
# exiting ctx 2
```

In this example we'd _want_ to exit 4, 3, 2, 1 in the reverse order of entry, and that's not at all what we see.

Having *multiple* ExitStacks is a recipe for trouble: you must never push something onto stack1 after opening stack2.  

```python
with ExitStack() as stack1, ExitStack() as stack2:
    # No use of stack1 can ever be valid in this context!
    # Emit a lint warning on any use of stack1.enter_context.
```

The `.push()`, `.close()`, and `.pop_all()` methods also offer some very sharp edges, but are used very rarely, and so I haven't seen code which would benefit from a lint rule (and the rule would have to be _rather_ complicated for them, too).  All of the discussion above applies equally to `AsyncExitStack` and its' `.enter_async_context(acm)` method, which has identical semantics for the purpose of this discussion.

<details>
<summary>test cases for the linter</summary>

Note that this file only tests `ExitStack`; `AyncExitStack` is identical but should test both `astack.enter_context()` and `await astack.enter_async_context()`.

```python
from contextlib import ExitStack, contextmanager

@contextmanager
def resource(name):
    print(f"Opening {name}")
    yield
    print(f"Closing {name}")


# Basic error case: Using enter_context after entering another context manager
def test_basic_error():
    with ExitStack() as stack:
        stack.enter_context(resource("A"))
        with resource("B"):
            stack.enter_context(resource("C"))  # ERROR
        # OK to enter another context after "B" has closed though
        stack.enter_context(resource("D"))


# Error case: Using one ExitStack after opening another
def test_multiple_stacks_error():
    with ExitStack() as stack1:
        stack1.enter_context(resource("A"))
        with ExitStack() as stack2:
            stack2.enter_context(resource("B"))
            stack1.enter_context(resource("C"))  # ERROR


# Error case: ExitStack opened before another context, multi-element `with`
def test_interleaved_stacks_error():
    with (
        ExitStack() as stack1,
        resource("A"),
    ):
        # might want to error above, resource-A makes stack1 unuseable.
        stack1.enter_context(resource("B"))  # ERROR


# Error case: Multiple ExitStacks interleaved
def test_interleaved_stacks_error():
    with ExitStack() as stack1, ExitStack() as stack2:
        stack1.enter_context(resource("A"))  # ERROR
        stack2.enter_context(resource("B"))
        stack1.enter_context(resource("C"))  # ERROR


# Correct usage: Separate, non-interleaved ExitStacks
def test_separate_stacks():
    with ExitStack() as stack1:
        stack1.enter_context(resource("A"))
        stack1.enter_context(resource("B"))

    with ExitStack() as stack2:
        stack2.enter_context(resource("C"))
        stack2.enter_context(resource("D"))


# Error case: Using enter_context after close() on the same stack
def test_after_close_error():
    stack = ExitStack()
    stack.enter_context(resource("A"))
    stack.close()
    stack.enter_context(resource("B"))  # ERROR


# Error case: Interleaved contexts still problematic despite later close()
def test_interleaved_with_close_error():
    with ExitStack() as stack1:
        stack1.enter_context(resource("A"))

        with resource("B"):
            stack1.enter_context(resource("C"))  # ERROR
            stack1.close()


# Correct usage: Using one stack after properly closing another
def test_after_properly_closed():
    stack1 = ExitStack()
    stack1.enter_context(resource("A"))
    stack1.close()

    stack2 = ExitStack()
    stack2.enter_context(resource("B"))
    stack2.close()


# Correct usage: Making multiple ExitStacks safe with close()
def test_close_before_next_stack():
    with ExitStack() as stack1:
        stack1.enter_context(resource("A"))
        stack1.close()

        with ExitStack() as stack2:
            stack2.enter_context(resource("B"))

    # It's even OK if you close after `with`, iff before enter_context
    with ExitStack() as stack3:
        stack3.enter_context(resource("C"))
        with ExitStack() as stack4:
            stack3.close()  # makes the next line safe
            stack4.enter_context(resource("D"))

# Correct usage: Close one context before entering another with a new stack
def test_new_stack_after_close():
    stack = ExitStack()
    stack.enter_context(resource("A"))
    stack.enter_context(resource("B"))
    stack.close()

    stack = ExitStack()  # Need a new stack object after closing
    stack.enter_context(resource("C"))
    stack.close()
```
</details>

---

_Label `rule` added by @ntBre on 2025-05-20 20:34_

---

_Label `needs-decision` added by @ntBre on 2025-05-20 20:34_

---

_Comment by @ntBre on 2025-05-20 20:36_

Thanks for the suggestion and the test cases! This sounds like a pretty reasonable rule to me, just adding `needs-decision` in case anyone else wants to weigh in :)

---
