---
number: 18795
title: Only emit RUF039 for strings that actually require escaping
type: issue
state: open
author: mxmlnkn
labels:
  - rule
  - preview
assignees: []
created_at: 2025-06-19T15:00:46Z
updated_at: 2025-06-25T07:34:53Z
url: https://github.com/astral-sh/ruff/issues/18795
synced_at: 2026-01-07T13:12:16-06:00
---

# Only emit RUF039 for strings that actually require escaping

---

_Issue opened by @mxmlnkn on 2025-06-19 15:00_

### Summary

I have tried the preview rules (thanks for stabilizing RUF028, that seems useful to have, as my last opened [issue](https://github.com/astral-sh/ruff/issues/18674) shows, and got a dozen of these:

```python3
benchmarkSqlite.py:543:30: RUF039 [*] First argument to `re.match()` is not raw string
    |
541 |     ax.set_title('INSERT')
542 |
543 |     labelMatches = [re.match('(.*), (unique|duplicate) rows$', label) for label, _ in labelAndPath]
    |                              ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ RUF039
```

In general, this seems like a good rule, but only if double-escaping is actually necessary, which in the above case it is not. If not, it feels more like an option that should be part of the formatter, not of the linter, because it makes no difference. (The same applies btw to ["E201", "E202", "E203", "E211", "E221", "E226", "E251", "E265", "E266", "E271"](https://github.com/astral-sh/ruff/issues/2402)) Making it a raw string literal when it is not necessary is more verbose (only one character) and may raise questions such as "Why is it a raw string? Should there be a backslash in this string? Is it a bug?" when reading the code.

---

_Comment by @ntBre on 2025-06-20 12:59_

Thanks for the feedback! Glad you're getting use out of RUF028 :)

This was discussed a bit in the [PR](https://github.com/astral-sh/ruff/pull/14446#pullrequestreview-2444468166) adding the rule. It sounds like the main justification for always emitting the diagnostic is that it's easy to forget to add the `r` later if you modify the regex, but I guess the rule could just fire at that point too.

I don't have super strong feelings on this myself, but I'd love to hear others' thoughts on it, especially before we consider stabilizing the rule.

---

_Label `rule` added by @ntBre on 2025-06-20 13:00_

---

_Label `preview` added by @ntBre on 2025-06-20 13:00_

---

_Comment by @MichaReiser on 2025-06-25 07:34_

> it feels more like an option that should be part of the formatter, not of the linter, because it makes no difference. 

I don't think this is a good fit for the formatter. The fact that you disagree with it shows that this is something that users want to opt in (which is something we intentionally don't support in formatting), and it's not always clear what the right fix is Should `"\."` be fixed to `r"\."` or `r"\\."`?

> "Why is it a raw string? Should there be a backslash in this string? Is it a bug?" when reading the code.

I don't think only raising this rule on regex patterns where double escaping may have been intended would help with this. It only means that this comes up less often, but that might add to the confusion rather than reducing it. I find it easier to know: Always use raw strings for regex patterns, and I don't even need to remember the why.


I think there's an argument here that the rule falls into a different category depending on how strictly we lint:

* Flagging all non-raw regex patterns: While I think this is a good best practice because it reduces the cognitive overhead of knowing when to use a raw string and when it's fine not to, and it makes it easier to recognize which part of the pattern is being used. However, a rule enforcing raw strings for every regex pattern is mainly a stylistic rule.
* Flagging suspicious patterns: As the name suggests, patterns using escapes `\.` in a regular string is potentially a mistake because a user might have wanted to escape the `\` and not the `.` (wildcard character). This is a correctness rule.



---
