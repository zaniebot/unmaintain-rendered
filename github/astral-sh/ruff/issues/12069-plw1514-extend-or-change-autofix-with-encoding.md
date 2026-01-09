---
number: 12069
title: "PLW1514: extend or change autofix with `encoding=\"utf-8\"`"
type: issue
state: closed
author: xmo-odoo
labels:
  - good first issue
  - rule
  - fixes
  - help wanted
assignees: []
created_at: 2024-06-27T12:11:12Z
updated_at: 2024-07-17T16:57:28Z
url: https://github.com/astral-sh/ruff/issues/12069
synced_at: 2026-01-07T13:12:15-06:00
---

# PLW1514: extend or change autofix with `encoding="utf-8"`

---

_Issue opened by @xmo-odoo on 2024-06-27 12:11_

#8883 requested that the `open`/`read` calls without an explicit encoding be autofixed to use the locale, however *the use of the locale is exactly why PEP 597 / EncodingWarning was implemented*: in this day and age, and especially when interacting with internal data or software-created files, following the possibly wonky and misconfigured locale of the host machine is actively detrimental. Even more so when accounting for Windows where the locale is essentially always useless (it generally defaults to a single-byte windows codepage, changing the locale to UTF8 -- codepage 65001 -- is possible but uncommon).

At https://docs.astral.sh/ruff/rules/unspecified-encoding/ it is claimed that this is the PEP 597 recommendation, but it is the opposite of reality:

> [How to Teach This](https://peps.python.org/pep-0597/#how-to-teach-this)
>
> [For new users](https://peps.python.org/pep-0597/#for-new-users)
>
> Since EncodingWarning is used to write cross-platform code, there is no need to teach it to new users.
>
> We can just recommend using UTF-8 for text files and using encoding="utf-8" when opening them.

Using `encoding=locale.getpreferredencoding(False)` is a *forward-compatibility* measure, and the new `encoding="locale"` is a shorter and less error prone to specify that, but *using the locale is not the expected default*.

As a result, the autofixer for PLW1514 should either:

- autofix to `encoding-"utf-8"` by default, that is more likely to be correct, expected, and desired than not
- if supported, provide a second(ary) autofix to use the locale instead

---

_Label `rule` added by @AlexWaygood on 2024-06-27 17:12_

---

_Label `fixes` added by @AlexWaygood on 2024-06-27 17:12_

---

_Comment by @Avasam on 2024-07-02 03:12_

I thought I raised something about this already somewhere, but I can't find it anymore.
Even though I originally suggested the autofix for this rule (https://github.com/astral-sh/ruff/issues/8883) based on official doc. After trying it out in setuptools, I noticed that this autofix doesn't help, because `locale.getpreferredencoding(False)` will itself lead to a warning !

A better autofix imo is `encoding="locale" if sys.version_info >= (3, 10) else None` which does successfully keep the same behaviour, removes the runtime warnings *and* is easy to search7replace to "utf-8" in a manual pass.

As bonus, the preferred encoding could be configurable, so that "locale", "utf-8", or a project's custom needs is used for autofixing.

As a sidenote, whilst I agree a move to utf-8 is preferred, that would be an unsafe fix atm as it would change the behaviour and *can definitely* break code in real-world scenario.

---

_Comment by @xmo-odoo on 2024-07-02 06:03_

> A better autofix imo is encoding="locale" if sys.version_info >= (3, 10) else None which does successfully keep the same behaviour

The *entire point* of PEP 597 is that "the same behaviour" is garbage.

> As a sidenote, whilst I agree a move to utf-8 is preferred, that would be an unsafe fix atm as it would change the behaviour and can definitely break code in real-world scenario.

The problem is that using the locale is generally broken and only sometimes correct, but can be very hard to discover (for western devs anyway), as developers tend to have non-misconfigured system, limit themselves to ASCII data, and have extended-ASCII-codepage systems (e.g. CP437).

So while the current fixer "preserves the current behaviour" that doesn't mean it's not broken, the developer might just have not thought about the issue, ran the fixer, and now the broken behaviour gets embedded into the source as if it had been an actual decision rather than something that was not considered. That is exactly what happened to a colleague and why I opened this issue in the first place.

As things are, I consider the fixer to be actively detrimental because the only thing it fixes is the warning, not the underlying issue. If "unsafe fixers" are not allowed I do consider this one to be that in both cases, and would vote to see it removed entirely. IO encoding is very much in the realm of developer decision, and the tool pretending it can know that will always fail in some way.

---

_Comment by @charliermarsh on 2024-07-17 02:17_

Unsafe fixes are allowed -- we have a separate opt-in level for them. It seems reasonable to change the fix to `encoding-"utf-8"` as you suggested above.

---

_Comment by @Avasam on 2024-07-17 02:24_

This rule is worth requiring human intervention anyway, fixing to `encoding-"utf-8"` would end up simplifying the fixer too (no version check, conditionals, or imports)

---

_Label `good first issue` added by @charliermarsh on 2024-07-17 02:25_

---

_Label `help wanted` added by @charliermarsh on 2024-07-17 02:25_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-17 16:35_

---

_Referenced in [astral-sh/ruff#12370](../../astral-sh/ruff/pulls/12370.md) on 2024-07-17 16:47_

---

_Closed by @charliermarsh on 2024-07-17 16:57_

---
