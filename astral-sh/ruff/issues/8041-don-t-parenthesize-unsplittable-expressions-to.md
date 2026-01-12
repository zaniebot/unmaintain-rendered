```yaml
number: 8041
title: "Don't parenthesize unsplittable expressions to make comments fit"
type: issue
state: closed
author: konstin
labels:
  - formatter
  - needs-decision
assignees: []
created_at: 2023-10-18T10:04:15Z
updated_at: 2023-11-03T05:13:01Z
url: https://github.com/astral-sh/ruff/issues/8041
synced_at: 2026-01-12T15:54:47Z
```

# Don't parenthesize unsplittable expressions to make comments fit

---

_@konstin_

Black:
```python
__ = excel_safe  # just so the compatibility import above is "used" and doesn't get removed by linter
```
Ours:
```python
__ = (
    excel_safe
)  # just so the compatibility import above is "used" and doesn't get removed by linter
```
We make the comment fit, but the style is worse.


This also applies to:

* Other assignments
* Return
* Await
* Yield

It does not apply to:

* Clause headers (because followed by a `:`)
* Assert (even if it only has one expression)
* Black's new preview style

Note:

Black moves the comment for unsplittable nodes into the parentheses (keeps them with the expression) rather than moving them at the end of the assignment (as it does with other expressions)


```python
return (
    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa  # com
)
```

Edited by @MichaReiser 

---

_Label `bug` added by @konstin on 2023-10-18 10:04_

---

_Label `formatter` added by @konstin on 2023-10-18 10:04_

---

_Comment by @charliermarsh on 2023-10-18 13:05_

For what it’s worth, this is already documented as an intentional deviation (though we could revisit it): https://github.com/astral-sh/ruff/blob/main/crates/ruff_python_formatter/README.md#expressions-with-non-pragma-trailing-comments-are-split-more-often--7823

---

_Comment by @konstin on 2023-10-18 13:10_

Sorry, i missed that. I like the behavior without comments, with comments i find it looking really strange

---

_Label `bug` removed by @konstin on 2023-10-18 13:10_

---

_Label `needs-decision` added by @konstin on 2023-10-18 13:10_

---

_Comment by @MichaReiser on 2023-10-18 22:00_

I expect that we have the same behavior for literals and I'm not sure how you would implement this because we would need to lift comments of the "best fitting" content. I further think that what we have is more consistent with Black's approach of considering comments as part of the content (vs Prettier that never includes comments in the measured width)

Edit: The problem is slightly different than I thought. The comment is a trailing comment on the assignment statement. So the expression itself doesn't know about the comment. The issue is that the name should be parenthesized if it makes it fit (including any content, including comments, after it). This would mean we need to exclude the comment from the width for this specific case which seems odd.

---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2023-10-20 07:09_

---

_Comment by @Avasam on 2023-10-24 21:47_

Arguably long same-line comments aren't great to begin with and this will fit both:
```python
# just so the compatibility import above is "used" and doesn't get removed by linter
__ = excel_safe
```

But I agree for Black-to-Ruff transition PRs it's a bit annoying as I'd have to first include a commit that moves all these comments, and then the "*autoformat, no manual changes*" commit that shows minimal differences.

---

_Comment by @charliermarsh on 2023-10-25 23:23_

FWIW, I got more feedback wondering about this deviation.

---

_Comment by @MichaReiser on 2023-10-27 10:42_

What about: 

```python
await (
      self._shade.refresh()
)  # force update data to ensure new info is in coordinator
```

Black doesn't parenthesis, Ruff does.

Edit: this is related to https://github.com/astral-sh/ruff/issues/8182

---

_Assigned to @MichaReiser by @MichaReiser on 2023-10-30 00:48_

---

_Comment by @MichaReiser on 2023-10-30 09:39_

To make this slightly more complicated, the comment width influences whether the target should split or not. Blacked code

```python
a[
    "bbbbb"
] = excel_safe  # just so the compatibility import above is "used" and doesn't get removed by linter
a[
    "bbbbb"
] = excel_saaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaafe  # just so the compatibility import above is "used" and doesn't get removed by linter
```

That means it's somehow required to only exclude the line-suffix lengths when deciding if `best_fit_parentheses` should expand or not. Such conditional inclusion/excludion of widths can be problematic because it breaks the `measured_group_fits` optimization in the `Printer` (the `excel_safe` group fits, the printer avoids testing if other groups before the next line break fit too, but that's incorrect when ignoring the width only for `excel_safe`).

---

_Comment by @MichaReiser on 2023-10-30 09:51_

I want to try the following tomorrow:

* Change the `LineSuffix` width to be measured lazily when encountering the end of the file, a line break, or a line suffix. 
* Change the semantics of `BestFitsParenthesize` to only measure its own width to make the decision whether it should be parenthesized

Issue, I need to double check if this would play nice with preview style where the right should break first. I fear it does not. 

Interestingly, black's preview style formatting formats this as:

```python
a["bbbbb"] = (
    excel_safe  # just so the compatibility import above is "used" and doesn't get removed by linter
)
a["bbbbb"] = (
    excel_saaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaafe  # just so the compatibility import above is "used" and doesn't get removed by linter
)
```

Edit: but only if the left side can be split. It still doesn't parenthesize 

```python
a = excel_saaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaafe  # just so the compatibility import above is "used" and doesn't get removed by linter
```

---

_Renamed from "Don't parenthesize names to make comments fit" to "Don't parenthesize unsplittable expressions to make comments fit" by @MichaReiser on 2023-11-01 03:01_

---

_Comment by @MichaReiser on 2023-11-01 07:45_

Okay this is embarrassing that it took me so long to realise this, but it could have been because I believe there's an off by one bug in black. 

Black respects the comment width in all cases, but it "just works" for them because they place the trailing comment differently than Ruff:

```python
## ident, 83 columns, comment 89 columns
____aaa = aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaabbbbbbbbbbbbbbbbbbbbbbbbbbbvvvvvvvvvvv  # ccc

## ident, 83 columns, comment 88 columns
____aaa = aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaabbbbbbbbbbbbbbbbbbbbbbbbbbbvvvvvvvvvvv  # cc

## ident, 83 columns, comment 87 columns
____aaa = (
    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaabbbbbbbbbbbbbbbbbbbbbbbbbbbvvvvvvvvvvv  # c
)

# 88 column
a = aaaaaaaaaaaaaaaaaaaaaaaaaaaaaa and bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb
# 89
a = (
    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
    and bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb
)
```

Black attaches the trailing end-of-line comment to the parenthesized expression and only parenthesizes it if the expression and comment fit. It otherwise omits the parentheses and formats the comments after the expression. 

What's unclear to me is why Black only keeps the parentheses up to a length of 87, and not 88 as configured. I suspect that this is an off-by-one error. 


Now, we could implement the same comment design in Ruff but there are two challenges:

1. Assigning the comment to the expression is not in line with our general guiding principle that comments should always split the parent. This is relevant here because Ruff needs to "undo" the parenthesizing if the line suddenly fits (or no longer fits). 
2. Attaching the comments to the value sounds trivial, but it isn't when the value is parenthesized because the enclosing node then is the Suite (module) and not the `AssignStmt`. Meaning, we now need to handle the comments for all suites.

We have two options:

1. Keep our existing layout and never parenthesize unsplittable values in simple assignments (simple value, single, unspittable target). May result in Ruff not splitting an expression if parenthesizing could make it fit
2. Use Black's layout at the cost of a more complex comment placement 

I'm leaning towards two

---

_Closed by @MichaReiser on 2023-11-03 05:13_

---
