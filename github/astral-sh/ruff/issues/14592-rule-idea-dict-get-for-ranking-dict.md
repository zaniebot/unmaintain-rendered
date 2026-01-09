---
number: 14592
title: "Rule idea: dict get for ranking dict"
type: issue
state: closed
author: entorb
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2024-11-25T19:04:59Z
updated_at: 2024-11-26T08:17:44Z
url: https://github.com/astral-sh/ruff/issues/14592
synced_at: 2026-01-07T13:12:16-06:00
---

# Rule idea: dict get for ranking dict

---

_Issue opened by @entorb on 2024-11-25 19:04_

Do you think a rule for improving code of ranking-dict increment would be worth implementing?

this
```python
if "key" not in d:
    d["key"] = 1
else:
    d["key"] += 1
```
should be simplified by
```python
d["key"] = d.get("key", 1)
```


---

_Comment by @dylwil3 on 2024-11-25 21:13_

Thanks for suggesting a new lint rule! It's always nice to have simplification ideas.

In this case I'm having a little trouble understanding the suggestion. Are the two code blocks really equivalent? It seems like the latter does not actually add 1 to the existing entry. I guess this would work:

```python
d["key"] = d.get("key",0) + 1
```
But, at least to me, that takes a little more time to understand than the if/else statement. I would think that the latter is also slightly slower (but I could be wrong, and that's not that big of a deal anyway).

Apologies if I've misunderstood the suggestion!

---

_Comment by @entorb on 2024-11-25 21:41_

Ups, yes of cause you are right, as I messed the second part up. Sorry for the confusion. Here corrected:

this
```python
if "key" not in d:
    d["key"] = 1
else:
    d["key"] += 1
```
should be simplified by
```python
d["key"] = d.get("key", 0) + 1
```



---

_Label `rule` added by @dylwil3 on 2024-11-25 22:28_

---

_Label `needs-decision` added by @dylwil3 on 2024-11-25 22:28_

---

_Comment by @dylwil3 on 2024-11-25 22:30_

Great, thank you! I think, as I said above, that I would personally find this lint confusing. But of course I'm happy to be overruled by the community 😄 

---

_Comment by @MichaReiser on 2024-11-26 08:17_

Thanks for the suggestion 

One acceptance criteria for our rules is that the rule should be useful for most Python programmers and that there's enough support in the community to consider it good practice: 

> Generally, if a lint is triggered, this should be useful to most Python (3+) programmers seeing it most of the time.

I don't think there's enough evidence for avoiding the `if`...`else` branching in the above example to be considered "simpler". 



---

_Closed by @MichaReiser on 2024-11-26 08:17_

---
