```yaml
number: 12283
title: "Rule idea: Unnecessary use of `re`"
type: issue
state: closed
author: JelleZijlstra
labels:
  - rule
  - help wanted
  - accepted
assignees: []
created_at: 2024-07-10T21:22:04Z
updated_at: 2024-12-02T17:29:49Z
url: https://github.com/astral-sh/ruff/issues/12283
synced_at: 2026-01-10T11:09:54Z
```

# Rule idea: Unnecessary use of `re`

---

_Issue opened by @JelleZijlstra on 2024-07-10 21:22_

I recently saw someone write `re.sub('\\\$', '', s)` when they could have just written `s.replace('$', '')`. Similarly, `re.match("abc", s)` could be replaced with `s.startswith("abc")`, `re.search("abc", s)` could be replaced with `"abc" in s`, `re.fullmatch("abc", s)` could be replaced with `s == "abc"`, `re.split("abc", s)` could be replaced with `s.split("abc")`.

Why is it better to use the str methods directly? It makes the code simpler and therefore easier to understand, may require less escaping, and it's probably usually faster.

Some thoughts on what the implementation could look like: We'd have to inspect the pattern string and ensure it doesn't contain any characters that are special for the regex engine, other than `\` (for escaping) and in some cases `^` and `$` for the beginning and end of the string. The implementation would also need to check whether any other flags are passed to the regex function (e.g., `re.IGNORECASE` would mean that a regex that contains alphabetical characters is no longer replaceable). There are other subtleties that I haven't fully thought through; for example, `$` also matches right before a newline at the end of a string. An initial implementation could limit itself to simpler cases, and leave possible more complex cases for later.

---

_Label `rule` added by @MichaReiser on 2024-07-10 21:31_

---

_Label `help wanted` added by @AlexWaygood on 2024-11-24 22:49_

---

_Label `accepted` added by @AlexWaygood on 2024-11-24 22:49_

---

_Comment by @tdulcet on 2024-12-01 12:50_

Here are some slightly more complex examples not supported by the initial implementation in #14659:
```py
re.search(r"abc", s) is None       # ⇒ "abc" not in s
re.search(r"abc", s) is not None   # ⇒ "abc" in s
re.sub(r"abc", "", s, count=1)     # ⇒ s.replace("abc", "", count=1)
re.split(r"abc", s, maxsplit=1)    # ⇒ s.split("abc", maxsplit=1)
re.search(r"^abc", s)              # ⇒ s.startswith("abc")
re.search(r"abc$", s)              # ⇒ s.endswith("abc")
re.search(r"^abc$", s)             # ⇒ s == "abc"
re.match(r"abc|xyz|123", s)        # ⇒ s.startswith(("abc", "xyz", "123"))
re.search(r"^abc|^xyz|^123", s)
re.search(r"^(?:abc|xyz|123)", s)
re.search(r"abc$|xyz$|123$", s)    # ⇒ s.endswith(("abc", "xyz", "123"))
re.search(r"(?:abc|xyz|123)$", s)
re.fullmatch(r"abc|xyz|123", s)    # ⇒ s in {"abc", "xyz", "123"}
re.search(r"^(?:abc|xyz|123)$", s)
re.match(r"[abc]", s)              # ⇒ s.startswith(("a", "b", "c"))
re.search(r"[abc]$", s)            # ⇒ s.endswith(("a", "b", "c"))
re.fullmatch(r"[abc]", s)          # ⇒ s in {"a", "b", "c"}
# Handle the ignore case flag
re.match(r"AbC", s, re.I)          # ⇒ s.casefold().startswith("abc")
re.search(r"AbC", s, re.I)         # ⇒ "abc" in s.casefold()
re.fullmatch(r"AbC", s, re.I)      # ⇒ "abc" == s.casefold()
# Handle any escaped special characters
re.fullmatch(r"""!"\#\$%\&'\(\)\*\+,\-\./:;<=>\?@\[\\\]\^_`\{\|\}\~""", s) # ⇒ s == """!"#$%&'()*+,-./:;<=>?@[\\]^_`{|}~"""
```
Edit: The `.endswith()` examples would be unsafe, per https://github.com/astral-sh/ruff/issues/12283#issuecomment-2509831070.

---

_Comment by @JelleZijlstra on 2024-12-01 15:42_

Great to see that the rule was implemented!

> `re.search(r"abc$", s)              # ⇒ s.endswith("abc")`

This is not equivalent:

```python
>>> s = "abc\n"
>>> re.search(r"abc$", s)
<re.Match object; span=(0, 3), match='abc'>
>>> s.endswith("abc")
False
```

I'm also not sure about the `re.I` ones; are those transformations correct in the presence of exotic casing conventions like in Turkish?

And I'm not sure the replacement is worth it in cases where the replacement is a little more complex (e.g., passing a tuple to startswith). The goal of the rule should be to make code simpler; with those transformations, that may not be the case any more.

---

_Comment by @tdulcet on 2024-12-01 16:48_

> This is not equivalent

Thanks. I edited my comment to note that they would be unsafe. If necessary, assuming Python 3.9+, one could always do something like `s.removesuffix("\n").endswith("abc")`.

> I'm also not sure about the `re.I` ones; are those transformations correct in the presence of exotic casing conventions like in Turkish?

I believe these would be safe as long as the user is not using the `re.L/re.LOCALE` flag, as otherwise both the re library and [`str.lower()`](https://docs.python.org/3/library/stdtypes.html#str.lower) should be using the same standard Unicode conventions. However, I changed my examples to use [`str.casefold()`](https://docs.python.org/3/library/stdtypes.html#str.casefold) for good measure.

---

_Comment by @MichaReiser on 2024-12-02 17:29_

I created a new issue to track the scope extension. I'll close this issue because the rule itself and what's been asked in the original issue has been implemented as RUF055

---

_Closed by @MichaReiser on 2024-12-02 17:29_

---
