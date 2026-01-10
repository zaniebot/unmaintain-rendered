---
number: 170
title: Support remaining Flake8 rules
type: issue
state: closed
author: charliermarsh
labels: []
assignees: []
created_at: 2022-09-12T19:17:25Z
updated_at: 2023-02-03T18:28:28Z
url: https://github.com/astral-sh/ruff/issues/170
synced_at: 2026-01-10T01:22:37Z
---

# Support remaining Flake8 rules

---

_Issue opened by @charliermarsh on 2022-09-12 19:17_

# Summary

Mega-issue to track support for the remaining Flake8 rules. This only includes rules that are (1) relevant for Python 3 and (2) relevant when using + compatible with Black.

If we can cross out these checks, then we can elevate ruff to an 0.1.0 release (with additional testing).

## pycodestyle

- [x] `E721` ("do not compare types, use `isinstance()`")

## PyFlakes

- [x] `F402` ("import module from line N shadowed by loop variable")
- [x] `F405` ("name may be undefined, or defined from star imports: module")
- [x] `F406` ("`from module import *` only allowed at module level")
- [x] `F632` ("use ==/!= to compare str, bytes, and int literals")
- [x] `F633` ("use of >> is invalid with print function")
- [x] `F722` ("syntax error in forward annotation")
- [ ] `F811` ("redefinition of unused name from line N")

## String formatting

N.B. These are the least important to me, in part because I think they're low-value while requiring a lot of custom logic. I would be fine with an 0.1.0 release that didn't include these.

- [x] `F501` ("invalid % format literal")
- [x] `F502` ("% format expected mapping but got sequence")
- [x] `F503` ("% format expected sequence but got mapping")
- [x] `F504` ("% format unused named arguments")
- [x] `F505` ("% format missing named arguments")
- [x] `F506` ("% format mixed positional and named arguments")
- [x] `F507` ("% format mismatch of placeholder and argument count")
- [x] `F508` ("% format with * specifier requires a sequence")
- [x] `F509` ("% format with unsupported format character")
- [x] `F521` (".format(...) invalid format string")
- [x] `F522` (".format(...) unused named arguments")
- [x] `F523` (".format(...) unused positional arguments")
- [x] `F524` (".format(...) missing argument")
- [x] `F525` (".format(...) mixing automatic and manual numbering")


---

_Referenced in [cnpryer/huak#99](../../cnpryer/huak/issues/99.md) on 2022-09-13 01:59_

---

_Comment by @charliermarsh on 2022-09-14 13:15_

Ignoring the string-formatting issues, there's a set of issues here that require us to track the parents for every binding, not just for current node. That is: we need to be able to get `getParent` on any binding, so we can do things like "determine whether two nodes are in different branches of an if/else".


---

_Added to milestone `Release 0.1.0` by @charliermarsh on 2022-09-15 13:26_

---

_Label `enhancement` added by @charliermarsh on 2022-09-15 13:49_

---

_Comment by @harupy on 2022-09-18 03:16_

@charliermarsh I would like to work on F402.

---

_Referenced in [astral-sh/ruff#221](../../astral-sh/ruff/pulls/221.md) on 2022-09-18 08:40_

---

_Comment by @charliermarsh on 2022-09-21 15:21_

Removed one rule that Flake8 no longer supports (my list was stale), and another that I don't think is worth supporting.

---

_Comment by @charliermarsh on 2022-09-21 15:27_

Also removing syntax error in doctest.

---

_Comment by @Jackenmen on 2022-09-21 15:27_

Hmm, I wanted F811 but I guess that both isort and usort offer the same functionality so it's better to leave it to them.

---

_Comment by @charliermarsh on 2022-09-21 15:37_

(Doing F405 now.)

---

_Comment by @charliermarsh on 2022-09-21 16:13_

Yeah we can revisit F811... I can see the usefulness. It's a bit tricky to get right and not a huge value-add so wanted to cut for 0.1.0.

---

_Comment by @cnpryer on 2022-10-06 23:56_

> This only includes rules that are (1) relevant for Python 3 and (2) relevant when using + compatible with Black.

I hope this is the right place for this. I noticed that #68 was closed, so I figured the idea is to consolidate rules here.

Are E302 and W292 considered part of pre 0.1.0 scope?

---

_Comment by @charliermarsh on 2022-10-07 00:27_

If they're important to you, they can be! I thought those were made obsolete by Black, but if you need E302 and W292 I'm happy to implement.

---

_Comment by @cnpryer on 2022-10-07 01:28_

That's my bad! I totally forgot about this snippet from the readme.

> As a project, ruff is designed to be used alongside Black and, as such, will defer implementing stylistic lint rules that are obviated by autoformatting.

I think I'm just conditioned to expect my linting and formatting tools to be less coupled together like this.

Also I'm not opposed to grab those! I just want to make sure that it's aligned with Ruff goals.

---

_Comment by @charliermarsh on 2022-10-07 01:40_

No problem at all! Right now, it's more of a prioritization decision than a strictly philosophical one. I'm not opposed to including stylistic rules, I just intentionally deprioritized those rules that were made irrelevant by Black.

Would of course welcome a PR on either of those if you ever feel up for it, but no pressure :)


---

_Referenced in [astral-sh/ruff#339](../../astral-sh/ruff/pulls/339.md) on 2022-10-07 05:40_

---

_Comment by @charliermarsh on 2022-11-28 02:32_

Thanks to @olliemath, we now support all of the string parsing rules! I'm going to close this out -- we're only missing `F811`, and I don't plan to implement it right now.

---

_Closed by @charliermarsh on 2022-11-28 02:32_

---

_Comment by @NyanKiyoshi on 2022-12-02 08:39_

> Thanks to @olliemath, we now support all of the string parsing rules! I'm going to close this out -- we're only missing `F811`, and I don't plan to implement it right now.

@charliermarsh, would it be possible to have a ticket to track the implementation of `F811`?

---

_Comment by @charliermarsh on 2022-12-02 14:25_

@NyanKiyoshi - Done! #996

---

_Comment by @charliermarsh on 2022-12-02 14:30_

I put off `F811` because it requires a bunch of logic to detect whether statements are defined in different `if` branches, etc. But it's certainly doable.

---
