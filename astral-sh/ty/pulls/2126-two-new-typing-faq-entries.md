```yaml
number: 2126
title: two new typing FAQ entries
type: pull_request
state: merged
author: carljm
labels: []
assignees: []
merged: true
base: main
head: cjm/faq
created_at: 2025-12-20T00:43:31Z
updated_at: 2025-12-20T17:00:15Z
url: https://github.com/astral-sh/ty/pull/2126
synced_at: 2026-01-10T02:34:11Z
```

# two new typing FAQ entries

---

_Pull request opened by @carljm on 2025-12-20 00:43_

Add new FAQ entries about checking rule code docs, and `Top` types.


---

_Review requested from @AlexWaygood by @carljm on 2025-12-20 00:43_

---

_Review comment by @sharkdp on `docs/reference/typing-faq.md`:301 on 2025-12-20 08:05_

Maybe
```suggestion
See also the [discussion
```


---

_@sharkdp approved on 2025-12-20 08:06_

Thank you!

---

_@AlexWaygood reviewed on 2025-12-20 12:01_

---

_Review comment by @AlexWaygood on `docs/reference/typing-faq.md`:255 on 2025-12-20 12:01_

I already have a mini-FAQ for this rule in the docs for this rule at https://docs.astral.sh/ty/reference/rules/#invalid-method-override, under the "Common issues" subheading. That isn't rendered very well right now (https://github.com/astral-sh/ty/issues/1884), but I think it's useful for all the docs relating to this rule to be in one place. Another advantage of putting FAQs there is that we link to that page straight from diagnostics printed to the terminal (following https://github.com/astral-sh/ruff/pull/21502).

So I would be inclined to improve the FAQ section at https://docs.astral.sh/ty/reference/rules/#invalid-method-override, and link to that page from this document, rather than having Liskov-related FAQs in both documents.

---

_@carljm reviewed on 2025-12-20 16:07_

---

_Review comment by @carljm on `docs/reference/typing-faq.md`:255 on 2025-12-20 16:07_

How do we feel about linking to open GH issues from rules docs?

---

_@carljm reviewed on 2025-12-20 16:12_

---

_Review comment by @carljm on `docs/reference/typing-faq.md`:255 on 2025-12-20 16:12_

> improve the FAQ section

This will be a separate PR, since it's a separate repo.

> and link to that page from this document

I added an FAQ here that does this in a very generic way, for all rule codes -- lmk what you think.

---

_@AlexWaygood reviewed on 2025-12-20 16:35_

---

_Review comment by @AlexWaygood on `docs/reference/typing-faq.md`:255 on 2025-12-20 16:35_

> How do we feel about linking to open GH issues from rules docs?

I think that's fine? Doesn't feel materially different to me to linking to GH issues from the FAQs doc here

---

_@zanieb reviewed on 2025-12-20 16:41_

---

_Review comment by @zanieb on `docs/reference/typing-faq.md`:255 on 2025-12-20 16:41_

We don't do it a lot, but I do think it's helpful in cases. I usually put it in an admonition.

---

_Review comment by @AlexWaygood on `docs/reference/typing-faq.md`:8 on 2025-12-20 16:42_

From a user perspective, it would probably be nicer to also have direct links to the error codes where we know we have different behaviour to mypy/pyright -- something like:

> Rules where we have slightly different behaviour to mypy and pyright include:
> - `invalid-method-override`
> - `foo-bar-baz`
> - `spam-ham-eggs`

But the advantage of the way you've currently got it is that we won't have to remember to change the link here if we rename a rule in the future (https://github.com/astral-sh/ty/issues/2001). So: no strong opinion.

---

_@AlexWaygood approved on 2025-12-20 16:42_

thanks!

---

_Merged by @carljm on 2025-12-20 17:00_

---

_Closed by @carljm on 2025-12-20 17:00_

---

_Branch deleted on 2025-12-20 17:00_

---
