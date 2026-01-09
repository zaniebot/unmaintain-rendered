---
number: 8382
title: "Feature request: new rule (and auto fix?) keyword only parameters"
type: issue
state: open
author: tiangolo
labels:
  - rule
assignees: []
created_at: 2023-10-31T17:27:57Z
updated_at: 2024-02-06T12:08:43Z
url: https://github.com/astral-sh/ruff/issues/8382
synced_at: 2026-01-07T13:12:15-06:00
---

# Feature request: new rule (and auto fix?) keyword only parameters

---

_Issue opened by @tiangolo on 2023-10-31 17:27_

This is something I like to do and request people do as well, but I haven't seen a linter that implements my (personal) rule.

I'm a bit ashamed of coming just to add a new issue to request a new non-existing rule/fix just because I would want it, but anyway, worth trying. 😅 

## Description

I try to avoid callables (functions, methods) with more than two positional parameters/arguments.

I told @Kludex about it a while ago, and he built https://github.com/Kludex/kwonly-transformer which is very close to what I want.

There is also https://github.com/vchaptsev/flake8-keyword-arguments which is a bit closer but only implements the rule/error, not the fix.

## Specification (?)

Here's the description of what I would like.

It applies both to creating a function/method signature and to calling it.

* If there are at most 2 parameters/arguments, it's fine to leave them positional.
* If there are more than 2 parameters/arguments:
    * If at most 2 are required and the rest are optional, the two parameters can be positional but the rest should be keyword only.
    * If there are more than 2 parameters/arguments required, all of them have to be keyword only.

I tend to be a bit stricter than that, but I would think that's generalizable enough that it could be useful to others. For example, if there's one required parameter and the rest are optional, I would try to allow only the first one to be positional, all the others would be required to be keyword only. The rationale is that normally it's a function that applies to a specific object and has some parameters/modifications for how to apply it.

The reason for 2 parameters and allowing 2 in most places is mainly for the cases when an operation applies to two things, one is in some way the origin/source and the other is the destiny, so it can be considered starting from the first and going to the second (going left to right, the same direction as when writing or reading code).

Maybe that could be a configurable threshold, could be 2, or could be 0 for people who don't want any positional argument... don't know, for me, it would be fine if it was fixed at 2.

## Other ideas

It could be stricter and with more rules, with other things like, if all parameters/arguments are optional, they should all be required, but that (and other potential extra rules) is probably more cumbersome and less flexible.

## Auto Fix

Of course, it would be great if it could not just show the error but also auto fix it, but not sure how difficult that is, it would change the calls and hopefully also the function signature.

---

_Comment by @zanieb on 2023-10-31 17:37_

Thanks for the comprehensive suggestion! There's some discussion at https://github.com/astral-sh/ruff/discussions/8137 already too.

I agree with the general sentiment that positional argument use should be limited.

We may have a hard time with the fix, as we don't perform fixes across multiple files yet.

---

_Label `rule` added by @zanieb on 2023-10-31 17:37_

---

_Comment by @tiangolo on 2023-11-01 09:29_

Ah! yes, it seems that's related to the same, sorry, I only checked on issues and not on discussions. 😅  Feel free to close this one!

About the fix, get it, maybe just fixing the calls (and not the signature) would already do most of the work. But in any case, even just the rule detecting it would help a lot.

---

_Referenced in [astral-sh/ruff#8907](../../astral-sh/ruff/issues/8907.md) on 2023-11-29 23:13_

---

_Comment by @jack-mcivor on 2024-02-06 12:08_

FWIW I found a couple of additional issues that I think are the same

- https://github.com/astral-sh/ruff/issues/3269
- https://github.com/astral-sh/ruff/issues/4235

---

_Referenced in [waltsims/k-wave-python#564](../../waltsims/k-wave-python/pulls/564.md) on 2025-03-16 15:21_

---
