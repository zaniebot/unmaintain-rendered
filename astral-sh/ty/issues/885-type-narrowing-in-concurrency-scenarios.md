```yaml
number: 885
title: Type narrowing in concurrency scenarios
type: issue
state: open
author: pfaion
labels:
  - question
assignees: []
created_at: 2025-07-24T17:44:33Z
updated_at: 2025-07-25T20:03:10Z
url: https://github.com/astral-sh/ty/issues/885
synced_at: 2026-01-12T15:54:24Z
```

# Type narrowing in concurrency scenarios

---

_@pfaion_

### Question

Hey all,

I write a lot of async code and frequently run into cases where the type inference (both of `ty` and other type checkers) does not tell the full picture. I'm wondering what `ty`'s perspective on that is. Apologies if I'm mis-using terminology here, I'm still relatively new to a lot of this, feel free to correct me!

Consider this example:

```python
from typing import reveal_type

field: int | None = None

async def process() -> None:
    global field
    if field is None:
        return
    await asyncio.sleep(1)        # (1)
    reveal_type(field)            # (2) Revealed type: `int`
```

It's easy to imagine a case where a different coroutine takes control during the await at `(1)`, changes the `field` to `None` again and then we resume at `(2)` and could theoretically get `int | None` again??

I got confused thinking about this. It feels to me that for async code we at least have clear boundaries for where you could argue to reset the narrowing. But thinking further, does that mean any code accessing shared state from multiple threads can never use type narrowing?

Is there any clear statement in the typing spec about concurrency?

Do you have strategies for dealing with these situations? Because it can lead to type-mismatch-related crashes at runtime that the static type checker did not warn about.

---

_Label `question` added by @pfaion on 2025-07-24 17:44_

---

_Comment by @carljm on 2025-07-24 23:36_

This is a great question, and one we've definitely discussed. It seems untenable to refuse to do narrowing that may be unsafe in a multi-threaded context, because it would emit too many false positives on code that isn't running multi-threaded. You're right that if we exclude multi-threading and consider only async, it's more plausible to detect where narrowing should be discarded (but I'm not aware of any existing type checker that does this.) Even if this were done correctly, it could lead to a lot of false positives. It's plausible to imagine it as an opt-in strictness setting.

The spec doesn't currently really speak to type narrowing at all, but the consensus of existing type checkers is to ignore this problem.

At the moment I would say this issue is low on our priority list, but we'd be interested to collaborate with other type checker authors in the future to think about how it could be better handled in the Python type system.

---

_Comment by @pfaion on 2025-07-25 07:15_

I'd be very interested in something like a strict mode for async code. I think it might even be possible to do some sort of inference on _how_ to reset the narrowing? I.e. it should be possible to see which state is accessed from other async code and only reset that accordingly? I think that might still produce a lot of false positives, but then you could start writing your types around that. I have a feeling, there must be a better way 😉

Noted that it's low on the priority list though. Is there any way to get involved in something like this for myself?

---

_Comment by @jelle-openai on 2025-07-25 15:50_

Type narrowing is technically unsafe in many cases; it's fully safe only with function-local variables (that cannot be reassigned by `nonlocal` variables in inner scopes), and with immutable objects (like narrowing a `Final` global or a tuple member). You don't usually even need concurrency for this; it's enough that you are able to call into arbitrary Python code that might mutate the object under you. But as Carl said, unsafe type narrowing is hugely important in practice for type checking a lot of Python code; lots of code relies on patterns like doing `if self.some_attr is not None:` and then assuming that `self.some_attr` stays not None.

I can see a few options here for your use case:

- Type checkers could introduce a "sound narrowing" mode that refuses to do any potentially unsafe narrowing. This would be difficult to use for most users, but those who care about heavily concurrent code could opt into it.
- We could introduce a new type qualifier (`Volatile`?) that marks certain objects as not being subject to narrowing. In your example above, perhaps you could annotate `field: Volatile[int | None] = None`. The semantics would be that the `if field is None:` check does not narrow the local type of `field`; it stays `int | None`.

If you're interested in getting this into the standardized type system, you'd probably need to work on a prototype in some type checker (showing that the feature is useful) and a PEP.

---

_Comment by @pfaion on 2025-07-25 19:16_

Right, any "shared mutable state" will have that issue. Maybe another reason to generally avoid this.

Thanks for the answers and suggestions. I have no illusions, it will be hard to find the time to follow up on that. But who knows 😅

---

_Comment by @pfaion on 2025-07-25 19:28_

For completeness sake, I guess this would be such an example. Pretty sure code like that is everywhere. It really irks me that the type checker essentially reports something wrong here.

```python
from typing import reveal_type
class Foo:
    val: int | None = None
    def change(self):
        self.val = 42

foo = Foo()
assert foo.val is None
reveal_type(foo.val)  # Type of "foo.val" is "None"
foo.change()
reveal_type(foo.val)  # Type of "foo.val" is "None"
```

---

_Comment by @carljm on 2025-07-25 20:03_

If unsoundness in the Python type system bothers you, you'll probably be interested in https://github.com/JelleZijlstra/unsoundness :)

Specifically, https://github.com/JelleZijlstra/unsoundness/blob/main/examples/narrowing/attribute_narrowing.py captures this form of unsoundness.

---
