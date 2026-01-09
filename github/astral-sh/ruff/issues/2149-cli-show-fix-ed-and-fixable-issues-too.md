---
number: 2149
title: "CLI: Show `--fix`ed and fixable issues too?"
type: issue
state: closed
author: akx
labels:
  - cli
assignees: []
created_at: 2023-01-25T07:57:52Z
updated_at: 2023-02-24T20:29:54Z
url: https://github.com/astral-sh/ruff/issues/2149
synced_at: 2026-01-07T13:12:14-06:00
---

# CLI: Show `--fix`ed and fixable issues too?

---

_Issue opened by @akx on 2023-01-25 07:57_

I think it would be useful if the CLI listed the things it `--fix`es, or can fix; right now you get something like

```
Found 8 error(s).
1 potentially fixable with the --fix option.
```
and if you run `--fix`, you'll get one error less and need look at the changes that have been applied and then figure out why. Instead, I think it'd be nicer if the flow was

```
$ ruff .
file.py:2:9: NOM001 Ran out of pancakes in `__init__` (fixable)
Found 1 error(s).
1 potentially fixable with the --fix option.
$ ruff --fix .
file.py:2:9: NOM001 Ran out of pancakes in `__init__` (fixed)
Found 1 error(s).
1 fixed with the --fix option.
$
```


---

_Comment by @not-my-profile on 2023-01-25 09:13_

I disagree ... I think it's better to follow the "rule of silence" from the Unix philosophy ([1](http://www.catb.org/~esr/writings/taoup/html/ch01s06.html),[2](http://www.linfo.org/rule_of_silence.html)):

> When a program has nothing surprising to say, it should say nothing.

(Besides any self-respecting programmer uses version control and will be able to see the fixes in detail by doing e.g. a `git diff`.)

So I'd rather keep the current default behavior. However I'd be alright with showing this output when a `--verbose` flag is supplied ... however in order to do that we'd probably have to firstly rename our current `--verbose` flag to debug ... which would be more fitting anyway since it currently literally prints [DEBUG] messages that are completely uninteresting for regular users.

---

_Comment by @charliermarsh on 2023-01-25 12:22_

I actually made a change in #1701, a while ago, to include the list of fixes in a slightly different format (just telling you how many violations were fixed in each file, closer to Black's output, where it tells you which files are reformatted). But I was worried it was too verbose and wanted to get more feedback before sending it out (so it was reverted in #1711).


---

_Comment by @akx on 2023-01-25 12:49_

How about `--show-fixes` instead of `--verbose`? 

---

_Comment by @not-my-profile on 2023-01-25 13:02_

I don't think this is important enough to warrant a dedicated command-line flag ... every flag we add complicates the UI a bit and makes it harder to for the user to find the flags they're actually looking for.

---

_Comment by @not-my-profile on 2023-01-26 06:24_

I just opened #2188 for the larger issue of `-v` being user-unfriendly.

---

_Label `cli` added by @charliermarsh on 2023-01-26 14:54_

---

_Referenced in [astral-sh/ruff#2188](../../astral-sh/ruff/issues/2188.md) on 2023-01-26 14:56_

---

_Referenced in [astral-sh/ruff#2452](../../astral-sh/ruff/issues/2452.md) on 2023-02-02 12:26_

---

_Comment by @aberres on 2023-02-24 19:51_

My 2cent: We call `ruff --fix` from `pre-commit` - I'd love to see a hint of what was changed to have a closer look. It is not always obvious when just a single line has new changes.

---

_Comment by @charliermarsh on 2023-02-24 20:00_

The `--show-fixes` flag exists which helps with this a bit.

---

_Comment by @aberres on 2023-02-24 20:24_

Ah, great, thank you. This is close enough.

---

_Comment by @charliermarsh on 2023-02-24 20:29_

Actually gonna close this because I think `--show-fixes` helped enough.

---

_Closed by @charliermarsh on 2023-02-24 20:29_

---
