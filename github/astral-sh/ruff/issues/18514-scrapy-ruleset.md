---
number: 18514
title: Scrapy ruleset
type: issue
state: open
author: Gallaecio
labels:
  - plugin
  - needs-decision
assignees: []
created_at: 2025-06-06T20:53:58Z
updated_at: 2025-06-06T21:37:47Z
url: https://github.com/astral-sh/ruff/issues/18514
synced_at: 2026-01-07T13:12:16-06:00
---

# Scrapy ruleset

---

_Issue opened by @Gallaecio on 2025-06-06 20:53_

### Summary

Would it make sense to add rules for the Scrapy ecosystem to ruff? Or would it be best to keep separate (e.g. revive [flake8-scrapy](https://github.com/stummjr/flake8-scrapy)), and maybe implement it as a plugin [once plugin support comes](https://github.com/astral-sh/ruff/issues/283)?

I thought adding them to ruff made sense, but then I saw and [old comment](https://github.com/astral-sh/ruff/issues/283#issuecomment-1307850818) that made me doubt:

> [Some plugins] don't make sense to include directly in Ruff (e.g., Django-specific stuff […])

Scrapy was originally inspired by Django, and is way more niche, so if rules for Django do not make sense in ruff, rules for Scrapy definitely don’t. And even if Django rules do make sense, Scrapy rules may not.

So, [as the documentation recommends](https://docs.astral.sh/ruff/contributing/#the-basics), this is my issue to ask before writing a PR for a new lint rule set.

Related: https://github.com/scrapy/scrapy/issues/4421

---

_Comment by @ntBre on 2025-06-06 21:17_

We actually do have [Django rules](https://docs.astral.sh/ruff/rules/#flake8-django-dj) now, as well as some other package-specific rules like numpy, pandas, and airflow. So I'd say we take it on a case-by-case basis, weighted by factors like the package's popularity/demand for the rules and interest of contributors to add the rules (which it sounds like we might have here 🙂).

It's not a top priority for us to add new rules right now, but we can definitely keep the issue open and collect this kind of feedback.

Do you have a sense for the number and kind of rules you'd want to add? If there are rules that could be useful more generally, we could also consider adding individual RUF rules instead of a whole new plugin namespace too.


---

_Label `plugin` added by @ntBre on 2025-06-06 21:17_

---

_Label `needs-decision` added by @ntBre on 2025-06-06 21:17_

---

_Comment by @Gallaecio on 2025-06-06 21:31_

Thank you for the quick feedback!

> weighted by factors like […] interest of contributors to add the rules

To be honest, this is more of a pet project of mine, and I can easily be pulled off it by other priorities, so I wouldn’t put too much weight on the fact that a Scrapy core contributor opened this ticket.

> Do you have a sense for the number and kind of rules you'd want to add?

The number or rules would potentially be in the 10-50 range, I think. As for the kind of rules, https://github.com/scrapy/scrapy/issues/4421 has some ideas.

I cannot think of any rule that would be useful outside of the Scrapy ecosystem off the top of my head, and I agree that it would be best to add those to existing rule namespaces.

> It's not a top priority for us to add new rules right now.

Then maybe it is best if I first go with a revival of [flake8-scrapy](https://github.com/stummjr/flake8-scrapy).

When things change, be it a grown interested to have Scrapy rules in Ruff itself or [plugin support](https://github.com/astral-sh/ruff/issues/283), we can migrate the flake8 plugin accordingly.

---
