```yaml
number: 827
title: Implement pyupgrade
type: issue
state: closed
author: JonathanPlasse
labels:
  - plugin
assignees: []
created_at: 2022-11-20T11:15:37Z
updated_at: 2023-05-14T07:42:01Z
url: https://github.com/astral-sh/ruff/issues/827
synced_at: 2026-01-10T11:09:42Z
```

# Implement pyupgrade

---

_Issue opened by @JonathanPlasse on 2022-11-20 11:15_

- [x] Set literals
- [x] Dictionary comprehensions
- [x] use `datetime.UTC` alias
- [x] Format Specifiers
- [x] printf-style string formatting
- [x] Unicode literals
- [x] Invalid escape sequences
- [x] `is` / `is not` comparison to constant literals
- [x] `.encode()` to bytes literals
- [x] extraneous parens in `print(...)`
- [x] unittest deprecated aliases
- [x] `super()` calls
- [x] "new style" classes
  - [x] rewrites class declaration
  - [x] removes `__metaclass__ = type` declaration
- [x] forced `str("native")` literals
- [x] `.encode("utf-8")`
- [x] `# coding: ...` comment
- [x] `__future__` import removal
- [x] Remove unnecessary py3-compat imports
- [x] import replacements
- [x] rewrite `mock` imports
- [x] `yield` => `yield from`
- [x] Python2 and old Python3.x blocks
- [x] remove `six` compatibility code
- [x] `open` alias
- [x] redundant `open` modes
- [x] `OSError` aliases
- [x] `typing.Text` str alias
- [x] Unpacking list comprehensions
- [x] Rewrite `xml.etree.cElementTree` to `xml.etree.ElementTree`
- [x] Rewrite `type` of primitive
- [x] `typing.NamedTuple` / `typing.TypedDict` py36+ syntax
  - [x] `typing.NamedTuple`
  - [x] `typing.TypedDict`
- [x] f-strings
- [x] `subprocess.run`: replace `universal_newlines` with `text`
- [x] `subprocess.run`: replace `stdout=subprocess.PIPE, stderr=subprocess.PIPE` with `capture_output=True`
- [x] remove parentheses from `@functools.lru_cache()`
- [x] replace `@functools.lru_cache(maxsize=None)` with shorthand
- [x] pep 585 typing rewrites
- [x] pep 604 typing rewrites
- [x] remove quoted annotations

Please add the tests from pyupgrade to assure equivalent behavior.

---

_Renamed from "pyupgrade implemented list" to "Implement pyupgrade" by @JonathanPlasse on 2022-11-20 14:54_

---

_Label `rule` added by @charliermarsh on 2022-11-20 15:05_

---

_Comment by @martinlehoux on 2022-11-22 08:04_

I guess I can just pick some rules in here for implementation?

---

_Comment by @JonathanPlasse on 2022-11-22 08:13_

> I guess I can just pick some rules in here for implementation?

Yes, can you comment on the one you choose so there is no conflict?

---

_Comment by @martinlehoux on 2022-11-22 22:26_

I'll get started on `six` code

---

_Comment by @squiddy on 2022-11-26 13:28_

I'll look at `yield -> yield from`.

---

_Comment by @charliermarsh on 2022-12-02 22:12_

I'd like to knock out a few more of these. Now that the LSP supports autofix, they're even more valuable.

---

_Comment by @charliermarsh on 2022-12-04 04:49_

I can do `Remove unnecessary py3-compat imports` and `import replacements`.

---

_Comment by @colin99d on 2022-12-26 16:30_

I can dig into typing.Text str alias.

---

_Comment by @colin99d on 2022-12-26 23:36_

I am going to take these two next:

- subprocess.run: replace universal_newlines with text (UP021)
- subprocess.run: replace stdout=subprocess.PIPE, stderr=subprocess.PIPE with capture_output=True (UP022)

---

_Comment by @colin99d on 2022-12-28 13:04_

Today I will work on:
- Rewrite xml.etree.cElementTree to xml.etree.ElementTree (UP023)
- OSError aliases (UP024)
- Unicode literals (UP025)

---

_Comment by @colin99d on 2022-12-29 20:32_

Going to start on rewrite mock imports (UP026)

---

_Label `rule` removed by @charliermarsh on 2022-12-31 18:18_

---

_Label `plugin` added by @charliermarsh on 2022-12-31 18:18_

---

_Comment by @colin99d on 2023-01-01 13:45_

I will take .encode() to bytes literals (UP027)

Edit, looks like someone already took this (UP012)

---

_Comment by @colin99d on 2023-01-01 17:13_

I will take: Unpacking list comprehensions (UP027)

---

_Comment by @colin99d on 2023-01-01 19:37_

I will take: yield => yield from (UP028)

---

_Comment by @colin99d on 2023-01-02 17:58_

I will take: Format Specifiers (UP029) next

---

_Comment by @colin99d on 2023-01-11 01:52_

I will take:  printf-style string formatting (UP031) next. I will have this done by Sunday.

---

_Comment by @colin99d on 2023-01-15 21:13_

I will take: f-strings (UP032)

---

_Comment by @colin99d on 2023-01-16 21:08_

I will take: extraneous parens in print(...) (UP033)

---

_Comment by @colin99d on 2023-01-19 22:30_

I will take:  import replacements (UP035)

---

_Comment by @charliermarsh on 2023-01-19 22:32_

Import replacements might requiring auditing some of what we already do. Like, we do _some_ of the replacements as part of the other Pyupgrade rules. I'm not totally sure what we're missing. (I'm also worried that it'll be a really tricky one to implement :joy:)

---

_Comment by @colin99d on 2023-01-20 03:02_

I am probably going to regret saying this, but this seems like its not too challenging. The process I was thinking is:
1. Search [this](https://github.com/asottile/pyupgrade/blob/main/pyupgrade/_plugins/imports.py#L43) dict for matches (converted to a rust hashmap)
2. Replace any specific items that match. If its a muli-line then we can move the replaced items to a new line. I would use libcst imports here
3. Return the new strings 

---

_Comment by @colin99d on 2023-01-23 00:04_

I will take: Python2 and old Python3.x blocks (UP037)

---

_Comment by @colin99d on 2023-01-29 23:16_

I will take:  remove quoted annotations (UP038) AKA the LAST one 🎉🎉🎉.

---

_Comment by @charliermarsh on 2023-01-30 00:06_

@colin99d - Awesome. Like `pyupgrade` (IIUC), let's only enforce that when `from __future__ import annotations` is present.

---

_Comment by @Bobronium on 2023-02-02 20:10_

I'm not sure where is a good place to drop this, but it would be great to have pydowngrade (AFAIK it's not a thing yet) along with pyupgrade.

This will allow to develop a project with newest set of features, and maintain compatibility with previous python version by downgrading certain things when building a wheel for specific version.


Hope it's not completely out of place 😄 

---

_Comment by @colin99d on 2023-02-02 20:59_

Hello Bobronium,

I love the idea! As a developer on a python library myself, it is annoying not being able to add the newest features in my code, because some people are still on python 3.7. However, I see two issues with this idea:
- This would take a while to code (probably just as long as coding all the pyupgrade features), but with a much smaller potential use case.
- Some of the lints would not be able to be reverted, for example, if you ran the lint that removes "u" before a string, then ruff would not know which strings to add it back to.

I think a much better implementation for this would be to have your CI that uploads pip take the version that works for your oldest members, and then upgrade it to the correct version for each user group downloading it. While this doesn't help the developers, it would mean all users of the code get the newest features.

---

_Closed by @charliermarsh on 2023-02-05 14:43_

---

_Comment by @sjdemartini on 2023-02-22 21:51_

In testing this out using Ruff 0.0.251 and enabling "UP" rules, the first pyupgrade feature mentioned in the original checklist ("Set literals", https://github.com/asottile/pyupgrade#set-literals) does not appear to be handled and is not listed under the rules here https://beta.ruff.rs/docs/rules/#pyupgrade-up, despite being checked off above. Perhaps it was actually never implemented? Should I file a new issue for this?

---

_Comment by @charliermarsh on 2023-02-22 21:53_

@sjdemartini - I think those rules are handled instead by [`flake8-comprehensions` rules](https://beta.ruff.rs/docs/rules/#flake8-comprehensions-c4), like `C405`.

---

_Comment by @sjdemartini on 2023-02-22 21:55_

Aha, indeed, thank you Charlie!

---

_Comment by @charliermarsh on 2023-02-22 21:57_

No prob! Same is probably true of dictionary comprehensions.

---

_Comment by @aureliojargas on 2023-05-13 23:54_

First of all, thanks for the excellent tool! Loving it 😍

After reading those last comments here, I got curious on exactly which extra rules besides `UP` I should use to get the full pyupgrade functionality in ruff.

I haven't found that information in the docs, so [I did some testing](https://gist.github.com/aureliojargas/70bd3667afd18f2a4f4e23d7ae5a2915) and the closer I got was:

```bash
ruff --target-version py311 --select UP,C401,C402,C403,C404,C405,F632,W605 --fix foo.py
```

Description of those rules:

```toml
# Activate all the rules that are pyupgrade-related
select = [
    "UP",    # pyupgrade
    "C401",  # flake8-comprehensions: unnecessary-generator-set
    "C402",  # flake8-comprehensions: unnecessary-generator-dict
    "C403",  # flake8-comprehensions: unnecessary-list-comprehension-set
    "C404",  # flake8-comprehensions: unnecessary-list-comprehension-dict
    "C405",  # flake8-comprehensions: unnecessary-literal-set
    "F632",  # pyflakes: is-literal
    "W605",  # pycodestyle: invalid-escape-sequence
]
```

The only pyupgrade feature not supported in ruff is [remove `six` compatibility code](https://github.com/asottile/pyupgrade#remove-six-compatibility-code).

The only ruff `UP` rule not supported in pyupgrade is [UP038: non-pep604-isinstance: Use `X | Y` in `{}` call instead of `(X, Y)`](https://beta.ruff.rs/docs/rules/#pyupgrade-up).

On a side note, since this PR is highly ranked in the "ruff pyupgrade" search, maybe we could add the ruff rules IDs to the items in this PR description, exactly as being done in "Implement Pylint" (#970)? This way people can better correlate the pyupgrade feature name with the ruff rule. [This may help](https://gist.github.com/aureliojargas/70bd3667afd18f2a4f4e23d7ae5a2915#ruff-rules-for-every-pyupgrade-fix).

---

_Comment by @charliermarsh on 2023-05-14 02:58_

Thanks, this is a great comment! I'd love to add the relevant annotations to the issue. If anyone is interested in writing a comment that does that, I'm happy to copy it into the description.

Edit: oh wait, that's basically exactly what you did in your [linked issue](https://gist.github.com/aureliojargas/70bd3667afd18f2a4f4e23d7ae5a2915#ruff-rules-for-every-pyupgrade-fix), thanks @aureliojargas!

---

_Comment by @aureliojargas on 2023-05-14 07:42_

Thanks @charliermarsh, I'm happy to provide it:

```markdown
- [X] Set literals / `C401` `C403` `C405`
- [X] Dictionary comprehensions / `C402` `C404`
- [X] Format Specifiers / `UP030`
- [X] printf-style string formatting / `UP031`
- [X] Unicode literals / `UP025`
- [X] Invalid escape sequences / `W605`
- [X] `is` / `is not` comparison to constant literals / `F632`
- [X] `.encode()` to bytes literals / `UP012`
- [X] extraneous parens in `print(...)` / `UP034`
- [X] unittest deprecated aliases / `UP005`
- [X] `super()` calls / `UP008`
- [X] "new style" classes
    - [X] rewrites class declaration / `UP004`
    - [X] removes `__metaclass__ = type` declaration / `UP001`
- [X] forced `str("native")` literals / `UP018`
- [X] `.encode("utf-8")` / `UP012`
- [X] `# coding: ...` comment / `UP009`
- [X] `__future__` import removal / `UP010`
- [X] Remove unnecessary py3-compat imports / `UP029`
- [X] import replacements / `UP035`
- [X] rewrite `mock` imports / `UP026`
- [X] `yield` => `yield from` / `UP028`
- [X] Python2 and old Python3.x blocks / `UP036`
- [X] remove `six` compatibility code / not ported to ruff
- [X] `open` alias / `UP020`
- [X] redundant `open` modes / `UP015`
- [X] `OSError` aliases / `UP024`
- [X] `typing.Text` str alias / `UP019`
- [X] Unpacking list comprehensions / `UP027`
- [X] Rewrite `xml.etree.cElementTree` to `xml.etree.ElementTree` / `UP023`
- [X] Rewrite `type` of primitive / `UP003`
- [X] `typing.NamedTuple` / `typing.TypedDict` py36+ syntax
    - [X] `typing.NamedTuple`  / `UP014`
    - [X] `typing.TypedDict` / `UP013`
- [X] f-strings / `UP032`
- [X] `subprocess.run`: replace `universal_newlines` with `text` / `UP021`
- [X] `subprocess.run`: replace `stdout=subprocess.PIPE, stderr=subprocess.PIPE` with `capture_output=True` / `UP022`
- [X] remove parentheses from `@functools.lru_cache()` / `UP011`
- [X] replace `@functools.lru_cache(maxsize=None)` with shorthand / `UP033`
- [X] pep 585 typing rewrites / `UP006`
- [X] pep 604 typing rewrites / `UP007`
- [X] remove quoted annotations / `UP037`
- [X] use `datetime.UTC` alias / `UP017`

Please add the tests from pyupgrade to assure equivalent behavior.
```

BTW, the only change I did was moving the "use `datetime.UTC` alias" item to the end, because it was the only one not following the order from the [pyupgrade README](https://github.com/asottile/pyupgrade/blob/main/README.md).

---
