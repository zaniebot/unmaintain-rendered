```yaml
number: 11748
title: Fix formatter instability for lines only consisting of zero-width characters
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - formatter
assignees: []
merged: true
base: main
head: fix-docstring-non-visible-whitespace-content
created_at: 2024-06-05T07:52:50Z
updated_at: 2024-06-05T15:55:15Z
url: https://github.com/astral-sh/ruff/pull/11748
synced_at: 2026-01-12T15:55:38Z
```

# Fix formatter instability for lines only consisting of zero-width characters

---

_@MichaReiser_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/11724#issuecomment-2148183543

Code from the linked issue that produces instable formatting:

```python

'''


'''
```

This turned out to be something entirely different than I expected. 
I initially thought that it was an issue with the docstring rendering, which uses `trim` instead of `PythonWhitespace::trim`. 
But I learned the better when I compared the generated IR for the above snippet and the same snippet but with `\u{15}` (the non visible character in the snippet above) with `a`. 
Both snippets generate the same IR! That means this is a bug in the `Printer` and not in the Python formatter. 

It turns out that the fix is relatively simple. 

The `Printer` omits `hard_line_break` IR elements or only renders a single `\n` for a `empty_line` IR elements when the 
current line is all empty. The problem is that the `Printer` uses the line width to determine if the line is empty. 
However, using the line width here isn't correct because a line can be non-empty but has a zero width if it only consists of zero-width characters, which is exactly what we have in the reported instability. 

I considered removing this behavior but we rely heavily on it in `suite.rs` and I'm not going to touch this to fix this bug! 
So what I did instead is to change the `Printer` to keep track of the start of the current line and test if the current formatted line is empty. 

I don't suspect that this changes performance much because it only adds a single write after a newline character. Testing whether the line gets a bit more expensive
because it now needs to dereference into the buffer, but I think in total, this is just noise.

And I was wrong lol. THis has a huge perf impact. Let's see if I can do better


## Backwards compatibility

It's hard to be a 100% sure, but I'm confident that it doesn't have widespread impact because zero-width characters are rare, even more so zero-width lines only consisting of zero-width characters. 

The only zero-width character that Python allows outside strings and comments is the form-feed character. Ruff does not preserve line-feed characters except in suppressed ranges. The existing implementation used a work-around to avoid this very specific printer behavior. This PR allows to remove the work around and the tests keep passing.


For strings, we write the entire string content as a single text, preserving line breaks as is.

For comments: That's the issue discovered here. Existing code would already have the trailing line stripped and reformatting the same code wouldn't insert the trailing line after the zero-width characters line. 

So we should be good 

## Test Plan

Added test. All existing tests keep passing. 


## Thanks

Thanks to @jasikpark for finding the instability and helping me to reproduce the issue. 


---

_Label `bug` added by @MichaReiser on 2024-06-05 07:54_

---

_Label `formatter` added by @MichaReiser on 2024-06-05 07:54_

---

_Comment by @codspeed-hq[bot] on 2024-06-05 07:57_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/fix-docstring-non-visible-whitespace-content)

### Merging #11748 will **degrade performances by 12.18%**

<sub>Comparing <code>fix-docstring-non-visible-whitespace-content</code> (945b033) with <code>main</code> (b021b5b)</sub>



### Summary

`❌ 1 (👁 1)` regressions
`✅ 29` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `fix-docstring-non-visible-whitespace-content` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 👁 | `linter/all-with-preview-rules[numpy/globals.py]` | 802.8 µs | 914.1 µs | -12.18% |


---

_Converted to draft by @MichaReiser on 2024-06-05 08:14_

---

_Marked ready for review by @MichaReiser on 2024-06-05 08:27_

---

_Review requested from @charliermarsh by @MichaReiser on 2024-06-05 08:27_

---

_Comment by @github-actions[bot] on 2024-06-05 08:41_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@charliermarsh approved on 2024-06-05 15:39_

---

_Comment by @charliermarsh on 2024-06-05 15:40_

Nice write-up.

---

_Merged by @MichaReiser on 2024-06-05 15:55_

---

_Closed by @MichaReiser on 2024-06-05 15:55_

---

_Branch deleted on 2024-06-05 15:55_

---
