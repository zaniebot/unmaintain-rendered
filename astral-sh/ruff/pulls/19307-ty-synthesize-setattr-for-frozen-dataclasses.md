```yaml
number: 19307
title: "[ty] synthesize __setattr__ for frozen dataclasses"
type: pull_request
state: merged
author: thejchap
labels:
  - ty
assignees: []
merged: true
base: main
head: thejchap/adv-frozen-dataclasses
created_at: 2025-07-13T12:37:46Z
updated_at: 2025-07-18T18:26:46Z
url: https://github.com/astral-sh/ruff/pull/19307
synced_at: 2026-01-10T17:58:13Z
```

# [ty] synthesize __setattr__ for frozen dataclasses

---

_Pull request opened by @thejchap on 2025-07-13 12:37_

## Summary

Synthesize a `__setattr__` method with a return type of `Never` for frozen dataclasses.

https://docs.python.org/3/library/dataclasses.html#frozen-instances
https://docs.python.org/3/library/dataclasses.html#dataclasses.FrozenInstanceError

### Related
https://github.com/astral-sh/ty/issues/111
https://github.com/astral-sh/ruff/pull/17974#discussion_r2108527106
https://github.com/astral-sh/ruff/pull/18347#discussion_r2128174665

WIP

## Test Plan

New Markdown tests

---

_Comment by @github-actions[bot] on 2025-07-13 12:59_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
attrs (https://github.com/python-attrs/attrs)
+ error[invalid-assignment] tests/test_next_gen.py:267:13: Property `c` defined in `C` is read-only
- Found 636 diagnostics
+ Found 637 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Comment by @thejchap on 2025-07-13 14:32_

@sharkdp i could use some help understanding your comment [here](https://github.com/astral-sh/ruff/pull/18347#discussion_r2128174665)

>  It returns an AttributeError if the attribute doesn't exist, and it returns a FrozenInstanceError if it's an existing attribute

did you mean to say _raises_ instead _returns_? and if so, can you maybe expand a bit on your idea for differentiating based on the function body (which doesn't exist in the ast since it's synthesized), since as far as i understand it from [this code](https://github.com/thejchap/ruff/blob/dd1a59ea5bb19ab02c85d18a93fd7a80f67731fd/crates/ty_python_semantic/resources/mdtest/function/return_type.md?plain=1#L22) a `raise` is considered to have a return type of `Never`, which would look the same regardless of the function body? apologies if im missing something obvious...

also as an aside, it looks like at runtime a `FrozenInstanceError` gets thrown regardless of if the attribute exists or not

```py
from dataclasses import dataclass

@dataclass(frozen=True)
class Test:
    b: int

test = Test(1)
test.a = 2
```

```bash
Traceback (most recent call last):
  File "/Users/justinchapman/src/ruff/../test.py", line 8, in <module>
    test.a = 2  # This will raise an error because the dataclass is frozen
    ^^^^^^
  File "<string>", line 4, in __setattr__
dataclasses.FrozenInstanceError: cannot assign to field 'a'
```

---

_Label `ty` added by @MichaReiser on 2025-07-13 14:37_

---

_Comment by @sharkdp on 2025-07-14 08:28_

> > It returns an AttributeError if the attribute doesn't exist, and it returns a FrozenInstanceError if it's an existing attribute
> 
> did you mean to say _raises_ instead _returns_?

Yes

> can you maybe expand a bit on your idea for differentiating based on the function body (which doesn't exist in the ast since it's synthesized), since as far as i understand it from [this code](https://github.com/thejchap/ruff/blob/dd1a59ea5bb19ab02c85d18a93fd7a80f67731fd/crates/ty_python_semantic/resources/mdtest/function/return_type.md?plain=1#L22) a `raise` is considered to have a return type of `Never`, which would look the same regardless of the function body? apologies if im missing something obvious...

My idea was that we could distinguish between calling a user-defined `__setattr__` method and the synthesized `__setattr__` method that you implemented here. This maybe more difficult than I imagined, since we only (easily) have access to the return type here. One way that would work (but I don't think I like that idea) would be to "tag" the `Never` return type. `Type::Never` could internally track some metadata (like a bit field) that would allow us to track the "origin" of that type. User-defined methods would just return a "normal" `Type::Never(NeverMetadata::default())`, but the synthesized dataclass `__setattr__` could return `Type::Never(NeverMetadata::FROZEN_DATACLASS_SETATTR)` or similar. The metadata wouldn't change the behavior of the `Never` type in any way. But here, when checking the return type of `__setattr__`, we could use that flag to distinguish the different origins of `Never`.

Another way to do this would be to look up `__setattr__` as an attribute (in addition to calling `try_call_dunder`). Instead of getting some `Type::FunctionLiteral`, we should see the function-like `Type::Callable` that would be evidence for it being a synthesized `__setattr__` method.

All of this would only be a way to provide a nicer error message. Something that mentions "frozen dataclass" instead of "custom `__setattr__` with a return type of `Never`", which might be really confusing for users.

> also as an aside, it looks like at runtime a `FrozenInstanceError` gets thrown regardless of if the attribute exists or not

Ah, interesting. I think I probably just tried accessing an unknown member, not setting it. I think it would also be fine to emit the invalid-assignment diagnostic here instead of unresolved-attribute. What's more important is the thing above.

---

_Closed by @thejchap on 2025-07-18 00:16_

---

_Reopened by @thejchap on 2025-07-18 00:16_

---

_Renamed from "[ty] synthesize __setattr__ and __delattr__ for frozen dataclasses" to "[ty] synthesize __setattr__ for frozen dataclasses" by @thejchap on 2025-07-18 00:45_

---

_Marked ready for review by @sharkdp on 2025-07-18 09:21_

---

_Review requested from @carljm by @sharkdp on 2025-07-18 09:21_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-07-18 09:21_

---

_Review requested from @sharkdp by @sharkdp on 2025-07-18 09:21_

---

_Review requested from @dcreager by @sharkdp on 2025-07-18 09:21_

---

_@sharkdp approved on 2025-07-18 09:23_

Thank you very much for this. Look, this found a new true positive!

```diff
attrs (https://github.com/python-attrs/attrs)
+ error[invalid-assignment] tests/test_next_gen.py:267:13: Property `c` defined in `C` is read-only
```

 I made a few minor updates to bring this into a mergeable state. Most importantly, we can not skip the whole check for the `frozen` flag, and can rely on `__setattr__` alone. This leads to a big code improvement.

---

_Merged by @sharkdp on 2025-07-18 09:35_

---

_Closed by @sharkdp on 2025-07-18 09:35_

---

_Comment by @thejchap on 2025-07-18 13:50_

@sharkdp ah, awesome - thanks! i had planned to work on it a bit more, but if its good with you its good with me :) i appreciate you getting it merged

is there anything else in https://github.com/astral-sh/ty/issues/111 that you think would be good to work on next?

one thing this is still missing i believe is emitting the same error for `__delattr__` (mypy doesn't do this, but at runtime you get the same `dataclasses.FrozenInstanceError: cannot delete field 'a'`)

---

_Comment by @sharkdp on 2025-07-18 18:26_

> i had planned to work on it a bit more

I noticed that it was still in draft, but it seemed like a good and consistent contribution. You're certainly welcome to add something on top of this! 

> one thing this is still missing i believe is emitting the same error for `__delattr__` (mypy doesn't do this, but at runtime you get the same `dataclasses.FrozenInstanceError: cannot delete field 'a'`)

Adding that would be great. I noticed that you already added a test for it.



> is there anything else in [astral-sh/ty#111](https://github.com/astral-sh/ty/issues/111) that you think would be good to work on next?

Adding support for more of the synthesized methods/arguments would certainly be a good next step. I haven't looked closely at this, and maybe some of these might not require any special casing at all. The next thing I'd do would probably be to write a few more tests for these, even if they are initially still failing. That should give us an idea what to focus on next.

Support for dataclass fields is another interesting area, but we're already working on this.

---
