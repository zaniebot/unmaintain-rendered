```yaml
number: 12607
title: "[`DJ`] Add `django-model-with-dunder-unicode`"
type: pull_request
state: closed
author: Amar1729
labels:
  - rule
  - needs-decision
assignees: []
base: main
head: rule/django-dunder-unicode
created_at: 2024-08-01T05:50:47Z
updated_at: 2024-08-14T13:51:43Z
url: https://github.com/astral-sh/ruff/pull/12607
synced_at: 2026-01-12T15:55:41Z
```

# [`DJ`] Add `django-model-with-dunder-unicode`

---

_@Amar1729_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Derived from a [datadog rule](https://docs.datadoghq.com/code_analysis/static_analysis_rules/python-django/no-unicode-on-models/), I've implemented a check for a Django model implementing a `__unicode__` method. This was a python2-ism that is not used anymore (as of Python 3.0).

- [Link to Django docs](https://docs.djangoproject.com/en/1.11/topics/python3/#str-and-unicode-methods) from 1.11 explaining why it's no longer necessary

<!-- What's the purpose of the change? What does it do, and why? -->

This method is probably quite uncommon by now, but i recently ran into a legacy codebase where `__unicode__()` was still defined and figured it might be helpful to write a simple rule for it. I believe this method was common in Django projects at the time, but it's entirely possibly to have defined a `__unicode__` method outside of using Django; as such, I'm not sure if it makes sense to put this rule somewhere else (e.g. PYUP?) and make it more general, to any kind of class. Either way, it's not directly derived from either of those checkers, and so I'm not sure either if `DJ014` is necessarily the best number for it. Thoughts?

## Test Plan

<!-- How was it tested? -->

- `cargo run -p ruff -- check crates/ruff_linter/resources/test/fixtures/flake8_django/DJ014.py --no-cache --preview --select DJ014`
- `cargo test`


---

_Comment by @github-actions[bot] on 2024-08-01 06:16_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `rule` added by @charliermarsh on 2024-08-09 02:07_

---

_Label `needs-decision` added by @charliermarsh on 2024-08-09 02:07_

---

_Comment by @charliermarsh on 2024-08-09 02:08_

This looks reasonable to me, but would be helpful to have a second opinion -- maybe @AlexWaygood or @carljm with Django expertise?

---

_Comment by @carljm on 2024-08-09 06:18_

I think this rule makes sense, but I don't think there should be anything Django specific about it. The `__unicode__` method was part of the Python 2 data model and is not part of the Python 3 data model, so it makes sense to lint against its definition anywhere in a Python 3 only code base, as it isn't doing what the author likely thought it might do. 

---

_Comment by @AlexWaygood on 2024-08-09 09:12_

Would this already be caught by PLW3201? If not, should it be?

---

_Comment by @MichaReiser on 2024-08-09 15:57_

> Would this already be caught by PLW3201?

Nope, this doesn't seem to be the case today [playground](https://play.ruff.rs/af09fdd6-7a62-4a67-b224-39b40970a2a3)

Including it in `PLW3201` does make sense to me if the method was removed entirely. Although it could be nice to have a more specific error message.

---

_Comment by @AlexWaygood on 2024-08-09 16:01_

> Including it in `PLW3201` does make sense to me if the method was removed entirely.

The special handling of the method has been removed from Python's data model. I wouldn't say that the _method_ was removed, since you can still define it on any class you like, and you won't get an error. It's just that it now behaves as a normal method rather than one that has any special semantics. But anybody defining it on any class is almost certainly doing so erroneously, because they think that it will have special semantics when it won't.

---

_Comment by @Amar1729 on 2024-08-10 14:27_

Hm, is there a straightforward way within the PLW3201 ruff code to output an extra message for a specific case? or would it make more sense to introduce a new code eg PLW3202 deprecated-dunder? (are there even any other deprecated dunder methods?*)

*edit: a [quick search of the docs](https://stackoverflow.com/questions/70316851/do-python-2-and-3-share-the-same-set-of-magic-methods) indicates some magic methods in py2 documentation that aren't mentioned in docs for py3. i don't know about some of them but `__cmp__` for example is also no longer used. 

---

_Comment by @MichaReiser on 2024-08-13 15:49_

@carljm do you know if there are any other dunder methods that were deprecated as part of Python 3?

@Amar1729 here's an example of a rule that uses different messages. 

https://github.com/astral-sh/ruff/blob/18452cf477390e849979df46dbceb88a8c3dfbe1/crates/ruff_linter/src/rules/pylint/rules/assert_on_string_literal.rs#L28-L43

---

_Comment by @AlexWaygood on 2024-08-13 15:58_

> @carljm do you know if there are any other dunder methods that were deprecated as part of Python 3?

Four that I know off, offhand, are `__div__`, `__rdiv__`, `__idiv__` and `__nonzero__`. `__metaclass__` also has no special meaning in Python 3, though it was never a special method, just a special attribute.

---

_Comment by @carljm on 2024-08-13 16:16_

In addition to the ones Alex mentioned, these are also all gone in Python 3: `__cmp__`, `__getslice__`, `__setslice__`, `__delslice__`, `__oct__`, `__hex__`, `__members__`, `__methods__`, `__nonzero__`, `__rdiv__`, `__idiv__`. There may be others, that's just what I found in https://docs.python.org/3/whatsnew/3.0.html plus some I already knew of (it doesn't seem like they are all listed in that doc.) 

---

_Comment by @dscorbett on 2024-08-13 16:55_

Other removed dunder methods are `__coerce__`, `__long__`, and `__rcmp__`.

---

_Comment by @MichaReiser on 2024-08-14 07:14_

Thanks @Amar1729 for creating this PR and triggering this discussion. Considering that `PLW3201` already covers this (in preview mode), I suggest extending the rule to give a custom error message if it detects any Python 2  dunder methods. Would you be interested on PRing this change?

---

_Closed by @MichaReiser on 2024-08-14 07:14_

---

_Comment by @MichaReiser on 2024-08-14 07:19_

I created an issue for the improvement

---

_Branch deleted on 2024-08-14 13:50_

---

_Comment by @Amar1729 on 2024-08-14 13:51_

no problem! a new PR sounds good, I'll see what I can do. 

---
