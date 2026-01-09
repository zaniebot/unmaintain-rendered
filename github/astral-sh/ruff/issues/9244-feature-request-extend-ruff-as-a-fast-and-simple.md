---
number: 9244
title: " Feature Request: Extend Ruff as a fast and simple SQL formatting and linting tool"
type: issue
state: open
author: OneCyrus
labels:
  - core
assignees: []
created_at: 2023-12-22T09:34:15Z
updated_at: 2024-10-03T02:33:43Z
url: https://github.com/astral-sh/ruff/issues/9244
synced_at: 2026-01-07T13:12:15-06:00
---

#  Feature Request: Extend Ruff as a fast and simple SQL formatting and linting tool

---

_Issue opened by @OneCyrus on 2023-12-22 09:34_

i think there's a huge overlapping userbase of python users who work in the data engineering space. the current lanscape of good tooling to format and lint sql files is not great and something like ruff for those scenarios could be huge.

basically ruff to replace sqlfmt and sqlfluff would be awesome.

https://sqlfluff.com/
https://sqlfmt.com/

---

_Comment by @zanieb on 2023-12-24 17:11_

We're definitely interested in this, but it's a serious endeavor and we'll need to dedicate some time to the design :)


---

_Label `core` added by @charliermarsh on 2023-12-26 22:04_

---

_Comment by @gregorywaynepower on 2023-12-28 05:29_

@OneCyrus It looks like [Sleek](https://github.com/nrempel/sleek) may be up your alley for the time being, at least as far as formatting goes. It was easier to set up than sqlfmt.

---

_Referenced in [astral-sh/ruff#10227](../../astral-sh/ruff/issues/10227.md) on 2024-03-04 12:42_

---

_Referenced in [astral-sh/ruff#10738](../../astral-sh/ruff/issues/10738.md) on 2024-04-02 13:36_

---

_Comment by @ashrub-holvi on 2024-04-22 07:08_

and https://github.com/pre-commit/mirrors-prettier

>       Archived
> prettier made some changes that breaks plugins entirely

---

_Comment by @KangOl on 2024-08-08 12:19_

There is now a Rust rewrite of sqlfluff named `sqruff`.
 - announce: https://www.quary.dev/blog/sqruff-launch
 - repo: https://github.com/quarylabs/sqruff


---

_Comment by @benfdking on 2024-09-06 16:00_

Hey 👋, Ben here from Quary, I just saw this! Yep, we would be happy to integrate https://github.com/quarylabs/sqruff in a way that makes sense, feels like there could be a nice way of doing this!  

---

_Comment by @takeda on 2024-10-03 00:24_

If this is implemented would that make ruff format SQL within the strings, or this feature only asks to format SQL files separately?

Slightly unrelated, but what's the recommended way to write python code with raw SQL?

---

_Comment by @gregorywaynepower on 2024-10-03 01:31_

> If this is implemented would that make ruff format SQL within the strings, or this feature only asks to format SQL files separately?
> 
> What's the recommended way to write python code with raw SQL?

@benfdking I'd also be pretty intrigued on what y'all's intended outcome for this. (I really enjoy sqruff by the way, I can completely understand sqruff being a completely separate tool.)

---

_Comment by @jordandakota on 2024-10-03 02:33_

I also enjoy sqruff due to its speed, but there's still quite a few things that feel not implemented. It would be good to get more hands on deck. I've been working on quite a few fixes as I get it integrated into my project I'll be submitting a pr for, but would still be great to have it built into ruff.

---

_Referenced in [astral-sh/ruff#15152](../../astral-sh/ruff/issues/15152.md) on 2024-12-27 06:55_

---
