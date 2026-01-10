---
number: 143
title: Add proper support for class decorators
type: issue
state: open
author: sharkdp
labels:
  - runtime semantics
assignees: []
created_at: 2025-04-02T07:28:48Z
updated_at: 2026-01-08T08:31:25Z
url: https://github.com/astral-sh/ty/issues/143
synced_at: 2026-01-10T01:51:14Z
---

# Add proper support for class decorators

---

_Issue opened by @sharkdp on 2025-04-02 07:28_

Similar to function decorators implemented in https://github.com/astral-sh/ruff/pull/17017, we need to support class decorators properly.

---

_Comment by @AlexWaygood on 2025-04-02 09:41_

Mypy's actually on version 1.15.0, and still doesn't properly support class decorators, FWIW! https://mypy-play.net/?mypy=latest&python=3.12&gist=5b5798e2bce1a8a90af1e777d4305294

I agree that we definitely should add support for these at some point (ideally before red-knot becomes generally available), but I'm not sure that full support for class decorators _needs_ to be a before-alpha issue or even a before-beta issue. They're _much_ less important than function/method decorators.

(We'll obviously need to add support for specific decorators such as `@typing.final`, `@typing.runtime_checkable`, `@dataclasses.dataclass`, `typing.dataclass_transform`, though.)

---

_Renamed from "[red-knot] Add proper support for class decorators" to "Add proper support for class decorators" by @MichaReiser on 2025-05-07 15:25_

---

_Label `runtime semantics` added by @AlexWaygood on 2025-05-11 07:54_

---

_Comment by @mwaskom on 2026-01-07 18:34_

Just mentioning that we at @modal-labs are watching this issue as decorator wrapping (including of classes) is quite central to the design of our Python API and we'd love have `ty` work out of the box for our users. I added some specific comments about a pattern that we are hoping to see working in this issue: https://github.com/astral-sh/ty/issues/2379

---

_Comment by @BarrensZeppelin on 2026-01-08 08:18_

@AlexWaygood When you wrote
> But I would not use the return type of the decorator as the class's type in this first PR. I would defer that to a followup change, as I expect this to be much more disruptive and harder to pull off.

Did you mean that it's more disruptive for the ecosystem results (i.e. `mypy-primer`), or that it is hard to implement in `ty`?

If it's the former, would you consider adding a configuration option that opts-in to the "correct" behaviour?
I also have a code base that I cannot migrate from `pyright` due to this issue. 🙂 

---

_Comment by @AlexWaygood on 2026-01-08 08:29_

@BarrensZeppelin thanks for letting us know that this impacts you! That's helpful for us in determining how to prioritise this issue.

We certainly intend to add proper support for class decorators at some point, that's why this issue is still open. Once we have that support, there won't be any reason to have a config option. My comment there was just advising Charlie to make an easy incremental change in one specific PR rather than trying to do everything (including the harder changes) all in one PR.

A config option wouldn't really help if adding support for this feature led to ty crashing all the time, which is what we were seeing in the very early version of Charlie's PR at the time and what prompted that comment from me. If that was the effect of the change, we would not be able to land the change with or without the config option.

That doesn't necessarily mean that the crashes are hard to fix, though — AFAIK we haven't looked too hard at what it would take to implement this feature. Again, I was really just advising an incremental approach in Charlie's work.

---
