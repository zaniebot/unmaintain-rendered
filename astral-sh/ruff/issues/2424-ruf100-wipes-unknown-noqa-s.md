```yaml
number: 2424
title: "RUF100 wipes unknown noqa's"
type: issue
state: closed
author: henryiii
labels:
  - question
assignees: []
created_at: 2023-01-31T22:49:31Z
updated_at: 2023-04-13T16:17:09Z
url: https://github.com/astral-sh/ruff/issues/2424
synced_at: 2026-01-12T15:54:42Z
```

# RUF100 wipes unknown noqa's

---

_@henryiii_

I have a custom flake8 plugin that's specific for a project (it wouldn't make sense to port it to Ruff ;) ). I've moved everything else to Ruff, and this still is much faster than flake8 used to be running everything! But I'm unable to enable RUF100 since this is an unknown directive.

Would it make sense to have a user-settable "known-noqa" or "ignore-noqa" setting that could tell RUF100 to ignore these noqa statements? Or should I just add RUF100 to the noqa? Another idea that would help in some cases (including this one) would be to split unknown and unused noqa cleanup.

---

_Comment by @charliermarsh on 2023-01-31 22:54_

We have a setting for this! You should be able to mark codes that Ruff doesn't know about, but should respect, via [`external`](https://github.com/charliermarsh/ruff#external).

---

_Label `question` added by @charliermarsh on 2023-01-31 22:54_

---

_Comment by @charliermarsh on 2023-01-31 22:54_

LMK if that works for what you're describing. If not, happy to figure something out :)

---

_Comment by @henryiii on 2023-02-01 03:37_

That's exactly what I was looking for, thanks! Sorry for not finding it, I assumed it would be mentioned in RUF 100's description, I guess.

---

_Closed by @henryiii on 2023-02-01 03:37_

---

_Comment by @Agalin on 2023-04-13 16:17_

What about support for globs/regex in `external`? Right now if I'm using dlint in my project I'd need to add 38 separate rules to the list and update it each time dlint changes something.

---
