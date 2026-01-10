```yaml
number: 12155
title: "Consider extending `PLR2004` to cover iterables with magic values"
type: issue
state: open
author: kgleba
labels:
  - rule
assignees: []
created_at: 2024-07-02T18:29:19Z
updated_at: 2024-07-03T06:32:18Z
url: https://github.com/astral-sh/ruff/issues/12155
synced_at: 2026-01-10T11:09:54Z
```

# Consider extending `PLR2004` to cover iterables with magic values

---

_Issue opened by @kgleba on 2024-07-02 18:29_

Keywords: `PLR2004`, `magic-value-comparison`

Here's a suggestion regarding `PLR2004`, although I don't quite understand the final optimal form it should take (I've found a [comment](https://github.com/astral-sh/ruff/issues/1949#issuecomment-1387004942) that Ruff tries to be a "faithful implementation of the Pylint rule" in this case, so I do not insist on changing the behavior of the original rule).

Imagine that a developer wants to check if the `status_code` of some request is in a redirect group (e.g., `301`, `302`).

A naive approach is to simply check for equality:

```python
if status_code == 301 or status_code == 302:
    ...
```

The developer immediately receives 3 errors: `PLR1714` (`repeated-equality-comparison`) and 2 instances of `PLR2004`.

More advanced Python developers will skip the step above and just write:

```python
if status_code in (301, 302):
    ...
```

And all 3 errors will disappear, even though the problem with magic value comparison still remains.

The code above is also the default suggestion from `PLR1714`, and copy-pasting the solution (which mysteriously removed 2 more warnings) can be misleading for inexperienced developers.

As I mentioned above, I'm not sure how to properly address this issue and I'm obviously unaware of some implementational pitfalls, but I believe that forcing this check even for homogeneous iterables (such as `list[int]` or `tuple[int]`) would be a pretty good step in the right direction.

Hoping to get some feedback! :)

---

_Label `rule` added by @MichaReiser on 2024-07-03 06:32_

---
