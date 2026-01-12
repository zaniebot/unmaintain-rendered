```yaml
number: 2397
title: detect if ruff introduces other issues and handle them/warn about it
type: issue
state: closed
author: spaceone
labels: []
assignees: []
created_at: 2023-01-31T14:22:12Z
updated_at: 2024-11-27T04:55:45Z
url: https://github.com/astral-sh/ruff/issues/2397
synced_at: 2026-01-12T15:54:42Z
```

# detect if ruff introduces other issues and handle them/warn about it

---

_@spaceone_

When I initially want to bring a large code base (or even small ones) to use `ruff` as lint tool I have the following workflow:
I am initially fixing every ruff rule individually by selecting it exclusively.
This makes sense as currently ruff sometimes introduce broken code (e.g. #2207, #2224, #2218, #2220, #2156, #2111, #2017, #1978, #1798, #1790, #1788, #2390).
So I have to carefully check each changed code.

As there is much development on the repository I need to cover the migration via reproducible steps. Also I want to reproduce it with more recent versions of ruff, which fixed some issues. And I want to separate the changes logically so that review for other people (or in the future) is easier.
So I create one git commit for each (auto)fix of the rule. I need to revert some things which introduce brokenness (e.g. issues above), I sometimes have to individually select a better fix for a certain issue or even flag them as `noqa` and I sometimes need to fix some things manually as there aren't or can't be any autofix functionality for it. OK, that's life and I can deal with it.
This kind of "script" I execute then again everytime when I rebase the branch onto the more recent upstream one.
The migration is a time consuming process, I am now working on it for two weeks. And I will probably do this as well for some other projects.

What would help me very much in this procedure is if ruff would detect that it introduces another issue by an autofix.
E.g. there is #1975 #1821, #1963, #1982, #2220, #2222, #2219 / #2394  (and some more unreported, e.g. `UP007` introduces `F401` for `typing.Optional`,  `PLR0402` introduces `I`, `COM819` introduces `UP034` (via #2389), ...).

**I would like that even when selecting only one issue to fix that it doesn't introduce other new issues.**
As ruff is very fast it could just apply `ALL` rules (even ignored ones) to the diff of the fixed version so - if there wasn't an issue prior but afterwards there is - it could as a first step simply warn on stderr about it.
For some issues it might also be correct to introduce other issues (I had one RL example, when I remember it again, I will add it here). And some are arguable (e.g. a removal of a variable or function argument should lead to the removal of a corresponding module import).
But most of the time the introduced issues aren't wanted.
So each autofix-function could tell ruff which fixers it should apply afterwards (to remove code duplication and explicit calls to other fixers). Maintaining a mapping of fixers → introduced-issues might be bad, maybe there is also some magic or general way. (or even other ideas come up)

→ So either autofix introduced issues automatically or warn about them (so they are 1. detected and 2. can also be reported as issue and then the autofixers could manually be adjusted to fix them if that makes sense in general. It would probably also help to find further "bugs" in ruff by the community which are applying the rules to their code).

I could also imagine a command line switch which decides if that should be done.

---

_Renamed from "detect if ruff introduces other issues and warn about it" to "detect if ruff introduces other issues and handle them/warn about it" by @spaceone on 2023-01-31 14:24_

---

_Comment by @dylwil3 on 2024-11-27 04:55_

I know this is an old issue, but in case anyone else has a similar question:

I think what's being asked for here can be achieved with the [`fixable` setting](https://docs.astral.sh/ruff/settings/#lint_fixable). One would enable all the rules of interest to emit diagnostics for, but restrict which fixes were used. Diagnostics would then be reported on any violations introduced by the fixes applied.

For example:

```console
➜  cat tmp.py
from a import bc
import ba
➜  ruff check tmp.py --no-cache --isolated --select I,PLR0402 --fixable PLR --fix
tmp.py:1:1: I001 Import block is un-sorted or un-formatted
  |
1 | / from a import bc
2 | | import ba
  |
  = help: Organize imports

Found 1 error.
```

(Notice that the original file did not have an `isort` violation.)

---

_Closed by @dylwil3 on 2024-11-27 04:55_

---
