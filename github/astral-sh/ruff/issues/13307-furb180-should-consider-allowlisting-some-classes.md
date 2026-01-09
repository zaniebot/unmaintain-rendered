---
number: 13307
title: "`FURB180` should consider allowlisting some classes"
type: issue
state: open
author: robsdedude
labels:
  - question
assignees: []
created_at: 2024-09-10T12:41:59Z
updated_at: 2025-06-13T21:24:17Z
url: https://github.com/astral-sh/ruff/issues/13307
synced_at: 2026-01-07T13:12:15-06:00
---

# `FURB180` should consider allowlisting some classes

---

_Issue opened by @robsdedude on 2024-09-10 12:41_

Please consider
```python
import abc
from typing_extensions import Protocol


class Foo(Protocol, metclass=abc.ABCMeta):
    ...
```
Now this rule will re-write the class to `class Foo(Protocol, abc.ABC): ...`. Well, that won't work at runtime :) `Protocol` checks that it only inherits from other Protocols.

Not sure what the fix is here.

---

_Comment by @AlexWaygood on 2024-09-10 13:23_

Hmm, what's the purpose of using `metaclass=abc.ABCMeta` here? Just inheriting from `Protocol` sets the metaclass as `_ProtocolMeta`, which is a subclass of `ABCMeta`

---

_Renamed from "`FURB180` should consider whitelisting some classes" to "`FURB180` should consider allowlisting some classes" by @MichaReiser on 2024-09-10 13:26_

---

_Comment by @robsdedude on 2024-09-11 10:41_

That's a fair point. But I'm a fan of being explicit here.. Imagine someone not being super familiar with Python (or just the `typing` module). They might easily not know that this is the case. Further, you have to dig into CPython's code to be sure about this. The docs are somewhat vague. They mention "abstract base class" (but not `abc.ABC`) when following `Protocol`'s inheritance of `Generic`. Lastly,  being explicit makes sure that the assumption is true and remains true in the future (or at least explodes with a metaclass conflict if Python decides to change this).

Regardless whether you agree with my approach or don't think it's worth supporting, the auto-fix does break the code and the documentation is incorrect.

>  Inheriting from the `ABC` wrapper class is semantically identical to setting `metaclass=abc.ABCMeta`, but more succinct.

It's not exactly semantically identical. Only in most cases.

---

_Label `question` added by @charliermarsh on 2024-09-18 03:27_

---

_Comment by @robsdedude on 2025-05-17 07:52_

I am considering crafting a PR for this. Here are my suggestions in descending order or preference:

 1. Introduce a configuration option to allow `metclass=abc.ABCMeta` if all base classes are in the configure list. By default, the list would be `typing.Protocol`, `typing_extensions.Protocol`.
 2. Hard code `Protocol` as an exception to the rule
 3. Simply mark the fix as unsafe (if the class has any super-class(es)).

Maybe 3 should happen anyway as this fix cannot be blindly applied without breaking any code.

Before I invest time I'd like to know which option(s) you prefer.

PS: if the label "question" means "user question", I don't think the issue is labeled quite right. It's not really that I don't know how to use the tool, but more that I am of the opinion that this rule needs some small fixing.

---

_Referenced in [astral-sh/ruff#18148](../../astral-sh/ruff/pulls/18148.md) on 2025-05-17 11:06_

---

_Referenced in [astral-sh/ruff#18149](../../astral-sh/ruff/pulls/18149.md) on 2025-05-17 11:32_

---

_Comment by @dylwil3 on 2025-06-12 15:34_

Could you say a bit more @robsdedude about how this configuration might be used other than the `Protocol` example?

I feel like the original issue raised along with Alex's comment has been addressed now that the fix is considered unsafe. I don't necessarily find this justification in response to Alex's point super convincing:

> Imagine someone not being super familiar with Python (or just the typing module). They might easily not know that this is the case.

If someone is not super familiar with Python, would they be writing code like the original example in the first place?

Anyway, I'm open to the idea of the configuration option, I just don't have a clear picture of the use-cases.

---

_Comment by @robsdedude on 2025-06-13 21:23_

@dylwil3 there's also a discussion in parallel on the linked [PR](https://github.com/astral-sh/ruff/pull/18148), in case you've not seen that. To not spread the high-level discussion about whether the option is even desired, I'll address parts of that [MichaReiser's comment](https://github.com/astral-sh/ruff/pull/18148#issuecomment-2965287966) here as well.

---

> [I] don't necessarily find this justification in response to Alex's point super convincing
> > Imagine someone not being super familiar with Python (or just the typing module). They might easily not know that this is the case.

Yeah, they would 😇 that "they" was me. I mean, I wouldn't necessarily say I'm not familiar with Python, but before looking it up, I wouldn't have know whether `Protocol` inherits from `ABC`/has `ABCMeta` (or a subclass) as its metaclass or not.

But even after finding out, I still want to keep that `ABCMeta` there for the aforementioned reason
> being explicit makes sure that the assumption is true and remains true in the future (or at least explodes with a metaclass conflict if Python decides to change this).

---

> I just don't have a clear picture of the use-cases.
Besides `Protocol`, I figured there could be any user-defined class or class defined in a 3rd party package that alters runtime behavior depending on subclasses' bases (validates bases, changes behavior depending on present bases, ...), and if that class requires you to use `ABCMeta` instead of `ABC`, this option would help.

Imagine a class that like `Protocol` validates that no other bases are present, but unlike `Protocol` does not inherit from `ABC`. This option would allow users to allow-list such classes instead of heaving to slap a `# noqa` at every sub-class definition. So in this case there isn't even a work-around of the sorts "just omit ABC all together", just `noqa`.

---

From @MichaReiser's comment on the PR 
> More specifically, options have a non-zero cost because users need to understand them.

That's a good point I've not considered.

Less than before but still I'd still like to see the option become a thing because I like the core of `FURB180`, but I disagree with having to work around it here in order to be explicit with my class being an abstract base class without having to use `noqa` comments.


---
