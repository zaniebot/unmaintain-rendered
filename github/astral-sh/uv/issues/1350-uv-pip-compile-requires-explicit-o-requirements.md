---
number: 1350
title: "`uv pip compile` requires explicit `-o requirements.txt` unlike `pip-compile`"
type: issue
state: closed
author: ThiefMaster
labels:
  - compatibility
assignees: []
created_at: 2024-02-15T21:18:23Z
updated_at: 2024-06-26T01:04:15Z
url: https://github.com/astral-sh/uv/issues/1350
synced_at: 2026-01-07T13:12:16-06:00
---

# `uv pip compile` requires explicit `-o requirements.txt` unlike `pip-compile`

---

_Issue opened by @ThiefMaster on 2024-02-15 21:18_

Not only does this require some repetition on the commandline (specifying both filenames), but it also makes it easy to get the wrong behavior when using e.g. shell redirection instead of `-o` because it won't take the existing requirements file into account.

---

With pip-tools I can do this:

    pip-compile -q && pip-compile -q requirements.dev.in

and it works great:

- When I added a new dependency to my `requirements[.dev].in`, it updates the .txt as expected
- If I didn't do anything there are no changes to the files since I did not request any upgrades nor is there anything else to change

With uv I now need to do this (twice as long!):

    uv pip compile -o requirements.txt requirements.in && uv pip compile -o requirements.dev.txt requirements.dev.in


FYI, all my tests are done on the requirements files of the [indico/indico](https://github.com/indico/indico/) repo.

---

_Label `compatibility` added by @charliermarsh on 2024-02-15 21:26_

---

_Comment by @charliermarsh on 2024-02-15 21:27_

I actually went back and forth on this -- I personally don't like this automatic behavior from pip-tools but I might be part of a small minority. @konstin strongly prefers it.

---

_Comment by @ThiefMaster on 2024-02-15 21:30_

How about an `uv pip compile --auto` that looks for `requirements*.{in,txt}`, checks if there are dependencies between them (e.g. `-c requirements.txt` in a `requirements.something.in`), and then compiles them in the proper order?

This would also be REALLY amazing for upgrades. Imagine just being able to `uv compile --auto --upgrade-package somepackage` if you want to upgrade that package :)

---

_Referenced in [astral-sh/uv#1457](../../astral-sh/uv/issues/1457.md) on 2024-02-16 09:01_

---

_Referenced in [astral-sh/uv#1875](../../astral-sh/uv/issues/1875.md) on 2024-02-22 16:41_

---

_Comment by @Julian on 2024-04-22 07:04_

I also ran into this -- I actually agree that the `pip-compile` behavior is worse and wouldn't have done it that way myself, but given it exists and that I suspect most users will be moving from it I'd have likely erred on the side of preserving it. It's probably not a good argument now that `uv` has been out for a little while though.

In my case given I support both `uv` and non-`uv` in my projects' noxfiles at the minute I basically do [this](https://github.com/bowtie-json-schema/bowtie/blob/ff7b516db5c29b32dd47e979fc6961a2b5e9da9f/noxfile.py#L368-L377).

I do think it's somewhat surprising behavior, so I would perhaps suggest considering mentioning it more prominently (in both the README and `uv pip compile --help` maybe).

---

_Comment by @charliermarsh on 2024-06-26 01:04_

I think this is unlikely to change now. It's also documented in the `PIP_COMPATIBILITY.md`.

---

_Closed by @charliermarsh on 2024-06-26 01:04_

---
