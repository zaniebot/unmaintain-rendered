---
number: 18346
title: C4 docs incorrectly label known issue
type: issue
state: closed
author: AntiSol
labels:
  - documentation
assignees: []
created_at: 2025-05-28T02:37:32Z
updated_at: 2025-06-03T19:15:58Z
url: https://github.com/astral-sh/ruff/issues/18346
synced_at: 2026-01-07T13:12:16-06:00
---

# C4 docs incorrectly label known issue

---

_Issue opened by @AntiSol on 2025-05-28 02:37_

### Summary

Hello,

After some discussion with colleagues, we've come to the conclusion that the "known problems" section on ruff's docs for [unnecessary comprehension](https://docs.astral.sh/ruff/rules/unnecessary-comprehension/) are incorrect.

The "known problems" section describes a "false positive", but the issue described isn't actually a false positive - the _warning_ about the unnecessary comprehension in the provided example (and also in every case we could come up with) is correct - the comprehension _is_ unnecessary. The issue is that ruff's suggested fix is incorrect, not that ruff will give incorrect warnings (as implied by "false positives").  A casual reader will see "false positive" and will tend to get the wrong impression.

The suggestion here would be to remove the term "false positive" from the docs, replacing it with something along the lines of "incorrect fix".

Unless there's a better example where it actually does give a false positive?

### Version

_No response_

---

_Comment by @MichaReiser on 2025-05-28 07:50_

Thanks for raising this. We made this change due to https://github.com/astral-sh/ruff/issues/13625 and @zanieb explicitly stated that this isn't a fix safety issue. I'm interested hearing their reasoning for it.

---

_Label `documentation` added by @MichaReiser on 2025-05-28 07:50_

---

_Comment by @AntiSol on 2025-05-28 08:55_

Thanks for getting back to me and for linking to the other issue. I see [@zanieb's response](https://github.com/astral-sh/ruff/issues/13625#issuecomment-2393959531)

I am also curious as to the reasoning behind the "not a fix safety problem" statement. 

As far as I can tell, if you've got unsafe fixes turned off, the output from ruff will be correct in all cases. The problem is with the suggested fix.

I agree that you'd need to know the type of `d1` in the example to be able to know what the correct fix looks like.

---

_Comment by @zanieb on 2025-06-02 16:53_

How would you write this transformation without the comprehension?

```python
d1 = {(1, 2): 3, (3, 4): 5}
d2 = {x: y for x, y in d1}
```

Perhaps as:

```python
d2 = dict(d1.keys())
```

You could argue that the comprehension is unnecessary because there's a simpler invocation, but I think it's actually clearer written as a comprehension unlike the `dict(d1)` case where you're just creating a copy and it's misleading to use a comprehension.

---

_Comment by @AntiSol on 2025-06-03 09:23_

Yeah, but the fact that you personally prefer no list comprehension in these circumstances doesn't make it a false positive - the list comprehension _is_ unnecessary. The warning is correct. Whether it should be replaced with `dict()` or not is a matter of opinion, but that discussion has no bearing on this being incorrectly labelled as a false positive. The warning is not incorrect - you just don't like what it wants you to do instead - welcome to every day of my life! 😉 

If you prefer the list comprehension, then cool (I think you're probably right, it is clearer). But to handle that you'd probably want to have either an option to this rule (`allow_comprehensions_zanieb_likes` or `allow_comprehensions_that_are_easier_to_grok`, maybe), or a separate rule to encompass your preferences. However making that work would require knowing the type of d1, which you can't always know.

And, again, none of this has any bearing on whether it's a false positive to say that the list comprehension is unnecessary. it _is_ unnecessary, and therefore not a false positive.

The wording should be changed. "you might disagree with this rule's suggestions" is more correct than "false positive".

---

_Comment by @zanieb on 2025-06-03 13:35_

I'm sorry, but I disagree.

Lint rules are _often_ subjective, and it's our role as a maintenance team to consolidate community feedback and expert opinions to determine what best practices to enforce.

The key here is that if the comprehension increases clarity, it is necessary. If it decreases clarity, as it does in the normal case, it is unnecessary. There are all sorts of cases where you could rewrite a comprehension using different syntax, that doesn't make all comprehensions "unnecessary".

---

_Comment by @AntiSol on 2025-06-03 15:53_

It's not a matter of opinion whether the comprehension is unnecessary. The rule is not called "zanieb thinks this would be clearer without a list comprehension".

You people dogmatically insist on enforcing pep8 rules in other cases, refusing to provide an option to disable them, where they destroy information and make code less clear.


But, fine, whatever, I get the message: don't bother filing ruff issues and trying to improve astral's toys.

---

_Closed by @AntiSol on 2025-06-03 15:53_

---

_Comment by @AntiSol on 2025-06-03 15:54_

(not "completed", btw, "astral aren't interested in being consistent")

---

_Comment by @charliermarsh on 2025-06-03 16:17_

> You people dogmatically insist on enforcing pep8 rules in other cases, refusing to provide an option to disable them, where they destroy information and make code less clear.

I don't know what you mean by this. We don't enable all the PEP 8 rules by default, but you can always disable rules you disagree with.

> But, fine, whatever, I get the message: don't bother filing ruff issues and trying to improve astral's tools.

Candidly, this seems like a really unfair reaction to raising an issue and getting multiple considered responses from people on the team. Open source is a huge effort, and it's draining and demoralizing for maintainers to invest time in good-faith discussions only to be repaid by snarky comments. I hope you'll reconsider your attitude in the future.


---

_Comment by @AntiSol on 2025-06-03 16:21_

> you can always disable rules you disagree with.

Oh good, so I guess you'll be able to tell me how to stop ruff format enforcing W293 then?

---
