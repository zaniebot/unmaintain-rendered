---
number: 21976
title: Is rule categorization a real blocker?
type: issue
state: open
author: worldmind
labels:
  - question
assignees: []
created_at: 2025-12-14T09:34:17Z
updated_at: 2025-12-15T10:11:38Z
url: https://github.com/astral-sh/ruff/issues/21976
synced_at: 2026-01-07T13:12:16-06:00
---

# Is rule categorization a real blocker?

---

_Issue opened by @worldmind on 2025-12-14 09:34_

### Question

Hi! Ruff is a cool tool, thank you for it! But I have a feeling that many interesting new rules (even with PRs) are blocked by ["Rule categorization"](https://github.com/astral-sh/ruff/issues/1774) and, as I probably have already mentioned somewhere, I am not sure that it's a good enough reason - yes, some people enable "ALL" rules and may get some rules not needed for them, but why this is a problem? If you decided to enable all rules it means you are ready to revisit newly added rules during ruff upgrade (even if a rule is good in general it doesn't mean it can be easily enabled in a big existing codebase). Even if you are trying to make default rule set sensible for all, every project is unique and anyway developers will disable some rules which are good by themselves, but doesn't fit (yet) to a particular project. Of course it would be nice to have a categorization, but as years passed and it's not added yet it's likely not that easy or not a priority, so, shouldn't be a blocker in that case.

A bit related topic - I heard you want to make own complexity metric instead of implementing all existing metrics from different linters, it sounds a bit related - also attempt to make something perfect for all instead of giving people tools for configure what they want in their particular case and it also not yet implemented.
So, my point - maybe you are trying to take too much and instead of building "perfect linter for all" it would be more realistic to make "perfect linter's building blocks" from which everyone can make, perfect for them, linter.

As I am a fun of classic liberal political philosophy and Austrian economy school it reminds me "central planing" vs "free market spontaneous order" debates - one of the points of classic liberal doctrine is that central planner just can't get all information about every particular case and thus can't make good decisions and entrepreneurs can do that because they know information about exact place/time etc. I guess it can be applied not only to economy.

### Version

_No response_

---

_Label `question` added by @worldmind on 2025-12-14 09:34_

---

_Comment by @MichaReiser on 2025-12-15 07:38_

Thanks for the kind words.

We're aware that many issues are currently blocked on rule categorization and it's our plan to resolve rule categorization in the coming months, to unblock many of those decisions. 

> So, my point - maybe you are trying to take too much and instead of building "perfect linter for all" it would be more realistic to make "perfect linter's building blocks" from which everyone can make, perfect for them, linter.

I can see how this is appealing, and we'd make a step in that direction if we were to add plugin support. I also think it's a perfectly valid strategy for a linter, but it's not the experience we want for Ruff. 

In my experience, most users want a working linter that comes with good defaults. They want to build their product and Ruff should have their back. They don't want to spend a lot of time configuring their linter. The same's true for me. If I set up a new Rust project, I want to get into coding and not configuring a linter, formatter, ... There are other people that maintain those tools that know much better than I what a good default rule set should be and I can turn off/on some rules that I disagree with -- if I happen to care enough.

I'd also argue that Ruff already does pretty well at providing the necessary building blocks. You can disable all rules and configure them as you like. The only thing you can't is get access to Rules that haven't been added to Ruff because Ruff lacks a plugin system and, you're right, we have been somewhat defensive about what rules we want to add to Ruff. There are mainly two reasons:

* Adding a rule is some work today and a lot of work in the future. We have to review the rule, document it, maybe even add a fix. But we also need to maintain the rule. This includes answering questions about how the rule works, justifying why the rule exists, keep the code working, ...
*  The challenge with Ruff is that users expect to be able to enable all rules and have them work seamlessly together (rightfully so!). This does mean that we can't have conflicting rules, or it results in infinite fix loops or frustrating experiences where Ruff tells you to fix some code, only to then tell you that your now-fixed code is all wrong again.

Short answer. We need and want to solve rule categorization and we may add plugin support in a future version. Both should resolve the issues you're raising.

---

_Comment by @worldmind on 2025-12-15 10:11_

Thank you for detailed answer! Of course it would be cool in all senses to have categories for rules and plugins, just wasn't clear will it happen. And of course you shouldn't support rules which you are not interested in, but if you will not be able to find a good for you approach for plugins, you can still add those rules, just mark them as unsupported (still some effort for review of course).
Maybe it's also some rule's category, similar to preview rules now.

---
