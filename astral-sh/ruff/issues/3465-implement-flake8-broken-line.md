```yaml
number: 3465
title: Implement flake8-broken-line
type: issue
state: open
author: Cielquan
labels:
  - plugin
  - needs-decision
assignees: []
created_at: 2023-03-12T17:21:40Z
updated_at: 2025-12-13T14:04:14Z
url: https://github.com/astral-sh/ruff/issues/3465
synced_at: 2026-01-12T15:54:43Z
```

# Implement flake8-broken-line

---

_@Cielquan_

# [flake8-broken-line](https://pypi.org/project/flake8-broken-line/)

Forbids broken lines with a `\`.

### Error Codes
* [ ] `N400` Found backslash that is used for line breaking

---

_Label `plugin` added by @charliermarsh on 2023-03-12 17:47_

---

_Comment by @Cielquan on 2023-03-17 16:31_

Because of compatibility with `black` this should be taken into account: https://black.readthedocs.io/en/stable/the_black_code_style/future_style.html#using-backslashes-for-with-statements

---

_Comment by @conformist-mw on 2023-03-17 16:56_

As of python 3.10 with statements can be [parenthesized](https://docs.python.org/3/whatsnew/3.10.html#parenthesized-context-managers).

---

_Comment by @Cielquan on 2023-03-17 21:50_

You are right.

Therefore the check should probably either run differently depending on the version of python that is targeted (pre vs. post 3.10) or have an option to allow backslashes in with statements.

On the other hand the special behavior is an edge case and becomes kind of obsolete in 2.5 years with 3.9 eol. So I can fully understand when the special behavior is not implemented. I do not know how much work that would be to implement. Unfortunately I currently have no time at hand to dig in to this.

---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:26_

---

_Comment by @Avasam on 2023-12-08 19:25_

For searchability, this is also: "Implement flake8-continuation"

---

_Comment by @johnthagen on 2025-03-24 20:47_

@charliermarsh With Python 3.9 going end-of-life this year, perhaps we can simply move forward? I feel like practically speaking, anyone who opts into this should be able to reasonably parenthesize their `with` blocks in Python 3.10+ or else simply not enable this lint until 3.9 goes end of life soon. 

---

_Comment by @MichaReiser on 2025-03-24 20:53_

There's the `local` and `globals` statement that both don't support parenthesizing and Ruff's formatter makes use of `\` in those cases.

---

_Comment by @Daverball on 2025-05-13 07:59_

I'm not really sure why this rule is at all controversial. It should be easy to only enable it in statements that can be parenthesized in the current target version. Writing a simple per statement fix should be no problem either.

Compared to other rules that have overlap with the formatter we shouldn't run into circularity issues with newer versions of Python, since at worst a statement that could previously not be parenthesized can now be parenthesized, the opposite case shouldn't really occur. So at worst we introduce a false negative for this rule, it should never be able to cause issues for people that use the formatter.

If there are no longer any objections to implementing this rule, I'd be happy to contribute an implementation. I personally don't use formatters in my projects, but I would like to use this rule without having to rely on flake8.

---

_Comment by @claydugo on 2025-11-18 18:39_

We ran into this today while reviewing a new developer’s pull request.

It seems like the Ruff team isn’t particularly interested in supporting certain linting rules that are handled by ruff format. 
We currently use ruff check without requiring developers to run ruff format, so the gaps between the two create real pain points for us.

I’d prefer to avoid adding extra dependencies just to cover these edge cases, if possible.

---
