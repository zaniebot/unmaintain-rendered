```yaml
number: 1073
title: Support remaining pycodestyle rules
type: issue
state: closed
author: horseinthesky
labels:
  - plugin
assignees: []
created_at: 2022-12-05T19:17:35Z
updated_at: 2023-01-31T17:28:32Z
url: https://github.com/astral-sh/ruff/issues/1073
synced_at: 2026-01-12T15:54:41Z
```

# Support remaining pycodestyle rules

---

_@horseinthesky_

Would be great to have these:
```
E201 whitespace after '['
E202 whitespace before ')'
E203 whitespace before ','
E211 whitespace before '('
E221 multiple spaces before operator
E301 expected 1 blank line, found 0
E302 expected 2 blank lines, found 1
E303 too many blank lines (4)
E401 multiple imports on one line
```

---

_Label `rule` added by @charliermarsh on 2022-12-05 20:02_

---

_Comment by @nanthony007 on 2022-12-06 18:40_

I'm going to try to work on this one rule at a time. Just for clarity E201 is "whitespace after '('" according to pycodestyle docs [here](https://pycodestyle.pycqa.org/en/latest/intro.html#error-codes).

---

_Comment by @horseinthesky on 2022-12-06 18:43_

> 

Awesome!

Docs mention E201 3 times with (, [ and {. So I assume it's just whitespace after any of them.

---

_Comment by @nanthony007 on 2022-12-06 20:17_

@charliermarsh First time contributing and I've never worked with ASTs before. I think I have some code in place but would welcome support on this. I could open a PR to show what I have currently, if that is acceptable.

Basically stuck on the AST logic implementation and believe I have the rest in place.

---

_Comment by @charliermarsh on 2022-12-06 20:24_

@nanthony007 - Happy to help! Do you want to put up a draft PR with questions we can discuss in the context of the code? Or what would be most effective?


---

_Comment by @nanthony007 on 2022-12-06 20:29_

I can put up a PR and we can go through it there I think.

I was thinking that since the E3 series seems to be whitespace oriented it might be worth implementing them as a part of this issue as well.

---

_Comment by @vytas7 on 2022-12-15 15:54_

Run into this as well when testing `ruff` on a relatively large, mostly legacy code base where it provides an immense speed boost over `flake8`; but we'll have to wait until basic stuff along the lines of `E302` is supported.

Don't get me wrong, I do think `ruff` is fantastic, and I'm planning to adopt it soon both in various hobby projects and at work, but maybe "near-parity with Flake8" is a bit misleading for the time being?

---

_Comment by @charliermarsh on 2022-12-15 16:20_

@vytas7 - Appreciate it!

We prioritized the rules based on the assumption that Ruff would be used alongside Black -- so we just haven't focused on implementing rules like `E302` that are made obsolete by an autoformatter. (It doesn't mean we won't implement them, just that they weren't / haven't been prioritized.) That's one of the key assumptions baked into any "parity with Flake8" claims. I hope it doesn't come across as misleading, it's all documented in the [FAQ](https://github.com/charliermarsh/ruff#how-does-ruff-compare-to-flake8), definitely not my intent.


---

_Comment by @rochacbruno on 2022-12-26 14:12_

I was also looking for it, those messages are useful when teaching.

I will need to use ruff + flake8 for a while

---

_Label `rule` removed by @charliermarsh on 2022-12-31 18:15_

---

_Label `plugin` added by @charliermarsh on 2022-12-31 18:15_

---

_Comment by @charliermarsh on 2023-01-31 17:28_

Closing in favor of #2402, which is the same issue but includes the rule tabulation up top.

---

_Closed by @charliermarsh on 2023-01-31 17:28_

---
