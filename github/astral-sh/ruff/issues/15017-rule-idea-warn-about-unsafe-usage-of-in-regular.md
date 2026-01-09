---
number: 15017
title: "Rule idea: warn about unsafe usage of `$` in regular expressions"
type: issue
state: open
author: bustbr
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2024-12-16T10:09:09Z
updated_at: 2024-12-17T10:13:10Z
url: https://github.com/astral-sh/ruff/issues/15017
synced_at: 2026-01-07T13:12:16-06:00
---

# Rule idea: warn about unsafe usage of `$` in regular expressions

---

_Issue opened by @bustbr on 2024-12-16 10:09_

In Python's implementation of `re` the `$` does not only match the end of the line, but also before any line break, even without multiline mode.
There's [an article by the OpenSSF about this issue](https://best.openssf.org/Correctly-Using-Regular-Expressions) suggesting to use `\Z` instead, or prefer the `fullmatch` function.

An example of the issue:
```
>>> re.search(r'cat$', 'cat\n')
<re.Match object; span=(0, 3), match='cat'>
```

Using `\Z` fixes this:
```
>>> re.search(r'cat\Z', 'cat\n')  # no match
>>> re.search(r'cat\Z', 'cat')
<re.Match object; span=(0, 3), match='cat'>
```

Or using `fullmatch`:
```
>>> re.fullmatch('cat', 'cat\n')  # no match
>>> re.fullmatch('cat', 'cat')
<re.Match object; span=(0, 3), match='cat'>
```

Should Ruff warn about using `$` with `search`?

---

_Label `rule` added by @AlexWaygood on 2024-12-16 14:45_

---

_Label `needs-decision` added by @AlexWaygood on 2024-12-16 14:47_

---

_Referenced in [astral-sh/ruff#14738](../../astral-sh/ruff/issues/14738.md) on 2024-12-16 21:14_

---

_Comment by @InSyncWithFoo on 2024-12-17 09:35_

> In Python's implementation of `re` the `$` does not only match the end of the line, but also before any line break, even without multiline mode.

This is incorrect. In non-multiline mode, `$` matches either the end of the entire input or the position right before the trailing newline (only `\n`, not `\r\n`). It is similar to PCRE's `\Z`, whereas Python's `\Z` resembles PCRE's `\z`:

<pre><code><a href="https://www.pcre.org/current/doc/html/pcre2syntax.html#TOC1">ANCHORS AND SIMPLE ASSERTIONS</a>

  \b          word boundary
  \B          not a word boundary
  ^           start of subject
                also after an internal newline in multiline mode
                (after any newline if PCRE2_ALT_CIRCUMFLEX is set)
  \A          start of subject
  $           end of subject
                also before newline at end of subject
                also before internal newline in multiline mode
  \Z          end of subject
                also before newline at end of subject
  \z          end of subject
  \G          first matching position in subject
</code></pre>

```pycon
>>> import re
>>> [*re.finditer(r'$', 'foo\nbar\r\n')]
[<re.Match object; span=(8, 8), match=''>, <re.Match object; span=(9, 9), match=''>]
>>> [*re.finditer(r'\Z', 'foo\nbar\r\n')]
[<re.Match object; span=(9, 9), match=''>]
```

---

_Comment by @bustbr on 2024-12-17 10:13_

> > In Python's implementation of `re` the `$` does not only match the end of the line, but also before any line break, even without multiline mode.
> 
> This is incorrect. In non-multiline mode, `$` matches either the end of the entire input or the position right before the trailing newline (only `\n`, not `\r\n`). It is similar to PCRE's `\Z`, whereas Python's `\Z` resembles PCRE's `\z`:

Thank you for the clarification.
Based on your example here's a series of calls to highlight the specific behavior of `$`:

```python-console
>>> re.search(r'$', 'foo\nbar')
<re.Match object; span=(7, 7), match=''>
>>> re.search(r'$', 'foo\nbar\n')
<re.Match object; span=(7, 7), match=''>    # still matches at pos 7, before the final new line
>>> re.search(r'$', 'foo\nbar\n\n')
<re.Match object; span=(8, 8), match=''>    # matches at pos 8, before the final new line

>>> re.search(r'\Z', 'foo\nbar')
<re.Match object; span=(7, 7), match=''>
>>> re.search(r'\Z', 'foo\nbar\n')
<re.Match object; span=(8, 8), match=''>
>>> re.search(r'\Z', 'foo\nbar\n\n')
<re.Match object; span=(9, 9), match=''>
```


---
