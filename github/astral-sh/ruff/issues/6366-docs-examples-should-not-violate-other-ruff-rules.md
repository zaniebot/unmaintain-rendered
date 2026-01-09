---
number: 6366
title: docs examples should not violate (other) ruff rules
type: issue
state: closed
author: mmarras
labels:
  - documentation
assignees: []
created_at: 2023-08-05T14:12:50Z
updated_at: 2023-08-05T16:05:35Z
url: https://github.com/astral-sh/ruff/issues/6366
synced_at: 2026-01-07T13:12:15-06:00
---

# docs examples should not violate (other) ruff rules

---

_Issue opened by @mmarras on 2023-08-05 14:12_

This repo is all about following consistent style, so I suggest even the docs should be educative and holistic. Therefore it is imho an anti-pattern to explain how to fix one ruff flag while introducing another. 

Spotted when copying solution from this one https://beta.ruff.rs/docs/rules/django-model-without-dunder-str/:

[Example](https://beta.ruff.rs/docs/rules/django-model-without-dunder-str/#example)

```python
from django.db import models


class MyModel(models.Model):
    field = models.CharField(max_length=255)
```
Use instead:


```python 
from django.db import models


class MyModel(models.Model):
    field = models.CharField(max_length=255)

    def __str__(self):
        return f"{self.field}"
```



`def __str__(self)` method there has no dock-string triggering D105. So I'm solving DJ008 but introducing D105. Obviously easy to fix, but that's not the point.

---

_Comment by @zanieb on 2023-08-05 15:03_

Hi! I appreciate the sentiment here but I don't think we should be adding docstrings into examples. I think the examples should be brief and not include extraneous content. I agree in general they should also not have violations of other lints, but I'm not sure it's avoidable in practice.

Curious to hear others' thoughts here :)

---

_Comment by @charliermarsh on 2023-08-05 15:11_

Also appreciate the issue + sentiment! I think there's a balance here with respect to keeping the example focused on the issue. My instinct is that adding docstrings to every example would probably detract from clarity, although I understand that "examples from the docs leading to lint errors" is imperfect.

Perhaps, as a rule of thumb, the "good" examples shouldn't contain errors from other rules _in the same category_. For example, if the "good" example above contained a new, different flake8-django error, that would strike me as clearly bad.

---

_Label `documentation` added by @charliermarsh on 2023-08-05 15:11_

---

_Label `needs-decision` added by @charliermarsh on 2023-08-05 15:11_

---

_Label `needs-decision` removed by @charliermarsh on 2023-08-05 15:11_

---

_Comment by @tjkuson on 2023-08-05 15:24_

IMO, the examples should be just examples of a rule per se, followed by how you could consider addressing that rule to help the reader understand its functionality and intent.

For example, [`static-join-to-f-string`](https://beta.ruff.rs/docs/rules/static-join-to-f-string/) has an example that won't even run by itself (it contains two undefined names) and doesn't do anything meaningful (the string is never assigned or passed to a function). Fixing that would make the code more holistically correct, but I think less useful as an example. (This would be more of an issue for more complex rules.)

Also, some rules are opinionated, and others are context-dependent; adhering to them wouldn't be useful. Further, even if you wanted to check against every rule, some are contradictory.

---

_Comment by @charliermarsh on 2023-08-05 15:53_

Okay, I'm going to err on the side of closing for now since there seems to be some consensus that making our positive examples error-free is not necessarily a goal.

---

_Closed by @charliermarsh on 2023-08-05 15:53_

---

_Comment by @mmarras on 2023-08-05 15:58_

Nice discussion here, appreciate it!

In my book, the issue is not about if examples run alone, or not. Imho they don’t have to. I think that would be obvious in those cases. 
But if I would copy that line of the “correct” static string and it would introduce another error, I would be annoyed. It could be that we are covered in that regard with Charlie’s argument of no lint errors of the same type (DJXXX for my OP), but I’m not sure it is sufficient. I would still be annoyed and frankly think people writing the doc were careless (if I would not know better of course) if it introduced another lint error. 

I won’t have any hard feelings, if this will proceed without fixing this, but I think I’m not pragmatic enough to follow the argument that a function in an example shouldn’t have a docstring for sake of clarity/simplicity. It can be a dummy docstring of course. 
Here is why: if you make a linter trying to “help” people write clean uniform and consistent looking code I would assume you were an advocate for following the linting rules. After all consistent means applying the rules everywhere (unless we make explicit that we have a reason to break them). Examples are where we are supposed to learn. By repetition. Some may even say, none of the ruff example show a docstring, clearly it’s not mandatory if such a popular tool like ruff doesn’t use it. Isn’t that a missed teaching/learning opportunity? And think about who is going to consult these examples? Likely not the veteran programmer who knows when to break the rules but people who are still learning their ways trying to understand things better (not saying that veteran programmers don’t practice continuous learning). 

Regarding the last issue raised, concerning contradicting rules: I think that’s another issue altogether. If I don’t explicitly exclude any ruff rules and two are contradictory, I would want to be informed there is no consent on this rule. I don’t recall I ran into the issue before so don’t know how it is currently handled, but I believe you that this corner case exists. And yes in that case the example should tell people that there is a contradicting rule, so as to be able to make an informed decision what to follow or be less adamant about following it. 

---
