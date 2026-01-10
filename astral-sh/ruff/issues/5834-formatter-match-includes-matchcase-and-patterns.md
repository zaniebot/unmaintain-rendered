```yaml
number: 5834
title: "Formatter: `Match` (includes `MatchCase` and `Patterns`)"
type: issue
state: closed
author: konstin
labels:
  - good first issue
  - formatter
  - help wanted
assignees: []
created_at: 2023-07-17T14:14:18Z
updated_at: 2024-09-26T06:35:23Z
url: https://github.com/astral-sh/ruff/issues/5834
synced_at: 2026-01-10T11:09:48Z
```

# Formatter: `Match` (includes `MatchCase` and `Patterns`)

---

_Issue opened by @konstin on 2023-07-17 14:14_

This is a bigger project that includes the following nodes:

- [x] #6298
- [x] #6299
- [x] #6641
- [x] #6642
- [x] #6644
- [x] #6643
- [x] #6645
- [x] #6557
- [x] #6558
- [x] #6555
- [x] #6753
- [x] #6933

The match arms have the same considerations for comments as if branches or try-except branches.

---

_Label `formatter` added by @charliermarsh on 2023-07-19 01:18_

---

_Added to milestone `Formatter: Alpha` by @MichaReiser on 2023-07-31 16:09_

---

_Label `help wanted` added by @MichaReiser on 2023-07-31 16:20_

---

_Comment by @MichaReiser on 2023-07-31 16:21_

As a note. The idea isn't that this is all implemented in a single PR. I recommend to start implementing `StmtMatch` and stubbing out the match cases (if they aren't already). We can then add support for the match statements one by one

---

_Comment by @dhruvmanila on 2023-08-14 12:26_

Regarding the match patterns, I would say that `PatternMatchValue`, `PatternMatchSingleton`, `PatternMatchStar`, `PatternMatchSequence` (sequence of other patterns), and `PatternMatchOr` (similar to `PatternMatchSequence`) are good first issues and could be taken on in the order mentioned.

---

_Label `good first issue` added by @konstin on 2023-08-14 12:30_

---

_Comment by @evanrittenhouse on 2023-08-14 12:50_

I can take those pattern issues! Feel free to assign them to me

---

_Comment by @MichaReiser on 2023-08-14 13:32_

> I can take those pattern issues! Feel free to assign them to me

Awesome. Let's start with one to leave an opportunity for other contributors too. Can you pick one and comment on the issue so that I can assign it to you?

---

_Comment by @MichaReiser on 2023-08-15 12:27_

I don't think this is ready yet. We first need to land https://github.com/astral-sh/ruff/pull/6594 (@evanrittenhouse  you can work on top of that PR, as long as you don't mind that there's one failing test).

---

_Comment by @cnpryer on 2023-09-01 15:04_

Is #6933 still required for the Alpha release? Looks like it’s marked as Beta.

---

_Comment by @charliermarsh on 2023-09-01 15:06_

My impression is that #6933 shouldn't be blocking for alpha.

---

_Closed by @MichaReiser on 2023-09-02 08:20_

---
