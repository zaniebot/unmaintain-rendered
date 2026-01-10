```yaml
number: 4056
title: Check for no side-effects during import
type: issue
state: closed
author: worldmind
labels: []
assignees: []
created_at: 2023-04-21T08:34:40Z
updated_at: 2023-04-22T15:18:40Z
url: https://github.com/astral-sh/ruff/issues/4056
synced_at: 2026-01-10T11:09:46Z
```

# Check for no side-effects during import

---

_Issue opened by @worldmind on 2023-04-21 08:34_

Initial issue: ruff removed imports which looks unused, but they are actually needed and to be honest I also tried to remove those imports and realized they are needed only after tests starts failing.
That gave me idea, but I am not sure is it even possible to implement, if it's fully stupid question just close it.
I am wondering about check that there is no side effects during import: usually when you do import you just want to bring some names to current namespace, but in practice sometimes, in addition to that, you can get some side-effects, for example if you are using django's [receiver decorator](https://docs.djangoproject.com/en/4.2/topics/signals/) in imported file, import will also register django signal handler(s).
As for me, good practice is not having any side-effects, so, if it's possible good to have such check.

---

_Comment by @JonathanPlasse on 2023-04-22 14:54_

Can you share a minimal reproducible example of this problem to see if we can avoid the false positive?

---

_Comment by @dhruvmanila on 2023-04-22 15:09_

I think what the author means is for ruff to detect if a specific import is having a _side effect_ or not. The only reason the import is being done is for the side effect and is not used anywhere in the module. Ruff will mark this as unused import and thus remove it.

As far as I know that would require runtime analysis which ruff isn't capable of doing currently. The best option would be to use `noqa` for that. Another option would for ruff to resolve the import and check if there are any _patterns_ for side effects in that module but that'll quite a challenge with Python's dynamism.

---

_Comment by @charliermarsh on 2023-04-22 15:18_

Hmm, yeah, unfortunately I think this is probably unfixable. Even if an imported module contains some runnable code, it’s hard to know whether any of that code is significant. I’d generally suggest trying to avoid side-effects on imports as much as possible, but when not possible, I’d have to recommend a noqa here.

---

_Closed by @charliermarsh on 2023-04-22 15:18_

---
