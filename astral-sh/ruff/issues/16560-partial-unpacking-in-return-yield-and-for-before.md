```yaml
number: 16560
title: "Partial unpacking in `return`, `yield`, and `for` before Python 3.9"
type: issue
state: open
author: ntBre
labels:
  - rule
  - parser
assignees: []
created_at: 2025-03-07T20:54:36Z
updated_at: 2025-03-10T23:02:44Z
url: https://github.com/astral-sh/ruff/issues/16560
synced_at: 2026-01-12T15:54:55Z
```

# Partial unpacking in `return`, `yield`, and `for` before Python 3.9

---

_@ntBre_

I made this a sub-issue of #6591 for easier discussion, but I ran into the fact that syntax like this:

```python
def f(): return 1, (*rest)
def g(): yield 1, (*rest)
for _ in  1, (*rest): ...
```

is allowed on Python 3.7 and 3.8 but not on 3.9 while working on #16485 and then again on #16558.

We actually already emit a normal parse error for this, so the change I guess we'd need is more like skipping the existing `ParseError` based on the version rather than adding an `UnsupportedSyntaxError`.

On the other hand, this only affects Python versions before 3.9, which are no longer supported. I'm leaning toward just removing this from #6591 and considering it finished without this check, but I wanted to get some other thoughts on this too.

That approach also raises the question of whether we should have taken the same approach with the parenthesized keyword argument names in #16482. That's the only other example of removed syntax in #6591, unless I'm missing another case in my unmerged PRs. We could just emit a `ParseError` for that and not worry about removed syntax for versions before 3.9.

For the partial unpacking case, specifically, I also didn't find any documentation about that change, so I think it just fell out of the PEG parser change in 3.9.

---

_Renamed from "`def i(): return 1, (*rest)` is no longer allowed (something with `*rest`)" to "Partial unpacking in `return`, `yield`, and `for` before Python 3.9" by @ntBre on 2025-03-07 20:55_

---

_Label `rule` added by @ntBre on 2025-03-07 21:06_

---

_Label `parser` added by @ntBre on 2025-03-07 21:06_

---

_Comment by @dhruvmanila on 2025-03-10 11:33_

Starred expressions are a bit tricky because they depend on the context in which it's being used. I see that it seems to also need to consider the target version.

> On the other hand, this only affects Python versions before 3.9, which are no longer supported. I'm leaning toward just removing this from [#6591](https://github.com/astral-sh/ruff/issues/6591) and considering it finished without this check, but I wanted to get some other thoughts on this too.

I'd be fine to proceed with this.

> That approach also raises the question of whether we should have taken the same approach with the parenthesized keyword argument names in [#16482](https://github.com/astral-sh/ruff/pull/16482). That's the only other example of removed syntax in [#6591](https://github.com/astral-sh/ruff/issues/6591), unless I'm missing another case in my unmerged PRs. We could just emit a `ParseError` for that and not worry about removed syntax for versions before 3.9.

Hmm, that's a good point. I wouldn't consider this a high priority but it might make sense to use `ParseError` for this case and just avoid raising `ParseError` if the target version allows it.

So, that prompted me to look at why the current implementation doesn't already raise a `ParseError` for this and I realize that I didn't special case this. In certain cases we do try to parse it using a more relaxed grammar but then later raise `ParseError` if we encounter invalid expression in that position ([example](https://github.com/astral-sh/ruff/blob/7d8fdb04c240d5c4f9f72a0e6aae42d0739fc099/crates/ruff_python_parser/src/parser/expression.rs#L384-L390)).

This also make me think as to whether there's any need for `Change::Removed` especially as the current list only includes the removed syntax in end-of-life Python versions.

---

_Comment by @ntBre on 2025-03-10 23:02_

Thanks @dhruvmanila! It sounds like we're leaning in the same direction. I think you're right that we could get rid of `Change::Removed` and the `Change` enum if we just made these `ParseError`s.

On the other hand, I mentioned this to Alex in our 1:1 today, and he said that a significant number of people are still using older, unsupported Python versions, so it may still be worth making these `UnsupportedSyntaxError`s.

I agree that it's probably not too high a priority either way, especially this week.

---
