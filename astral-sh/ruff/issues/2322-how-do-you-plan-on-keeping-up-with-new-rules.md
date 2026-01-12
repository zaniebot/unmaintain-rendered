```yaml
number: 2322
title: How do you plan on keeping up with new rules/fixes in core ?
type: issue
state: closed
author: Crocmagnon
labels:
  - question
assignees: []
created_at: 2023-01-29T09:56:31Z
updated_at: 2023-01-29T21:11:10Z
url: https://github.com/astral-sh/ruff/issues/2322
synced_at: 2026-01-12T15:54:42Z
```

# How do you plan on keeping up with new rules/fixes in core ?

---

_@Crocmagnon_

This is more a general question rather than a feature request/bug report:

How do you plan on keeping up to date with evolving projects you reimplemented in core, like pyupgrade for example?

---

_Comment by @charliermarsh on 2023-01-29 18:12_

The goal is to maintain parity as those projects evolve, to the degree that we can. We've already reimplemented some upstream fixes from `flake8-bugbear`, for example.

Many of the plugins we've reimplemented, though, are pretty "stable", in that they tend not to add many additional rules. E.g., many of them haven't seen new releases in the past few years. That's not true of _all_ of them, but at least a handful.

`pyupgrade` is maybe an interesting case, because we don't _really_ support pre-Python 3.7 code. So I'd probably not prioritize anything that was added to `pyupgrade` that was focused on 2-to-3 conversions. (We do have some of that, like `six` import removal, but in hindsight I wouldn't have prioritized adding it.) On the other hand, there's a lot of stuff in `pyupgrade` that's useful for the ongoing maintenance of Python 3 code, like the type annotation rules. So I _would_ pull in new rules of that nature.


---

_Closed by @charliermarsh on 2023-01-29 18:12_

---

_Label `question` added by @charliermarsh on 2023-01-29 18:12_

---

_Comment by @charliermarsh on 2023-01-29 18:12_

Hopefully that answers your question, but obviously happy to field any follow-ups :)

---

_Comment by @Crocmagnon on 2023-01-29 20:59_

Great! Thanks for your answer 😊

Yeah my question about pyupgrade was more focused on the ongoing development for new Python versions as I don’t see this tool be « finished » like maybe isort or some other could be (I mean once you’ve sorted your imports everything else is bug fixing, … right ? 😁)

And thanks for ruff! It’s an amazing piece of software 😊 I incorporated it in my own projects coming from flakeheaven and the transition was smooth. I don’t have large codebases but it feels a lot snappier already.

---

_Comment by @charliermarsh on 2023-01-29 21:11_

Amazing, so glad to hear that it's working for you :)

---
