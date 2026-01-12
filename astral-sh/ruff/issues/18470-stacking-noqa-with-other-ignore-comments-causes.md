```yaml
number: 18470
title: "Stacking `# noqa` with other ignore comments causes an E501 (line too long) violation"
type: issue
state: open
author: user27182
labels:
  - bug
  - help wanted
assignees: []
created_at: 2025-06-04T19:59:08Z
updated_at: 2025-07-04T04:59:04Z
url: https://github.com/astral-sh/ruff/issues/18470
synced_at: 2026-01-12T15:54:56Z
```

# Stacking `# noqa` with other ignore comments causes an E501 (line too long) violation

---

_@user27182_

### Summary

The docs for [E501](https://docs.astral.sh/ruff/rules/line-too-long/#line-too-long-e501) states that  "a line will not be flagged as overlong if a pragma comment causes it to exceed the line length."

This works if the comment is `# noqa`, but seems to fail if there's any other preceding `#` (with some exceptions for  `# type: ignore`, see below). 

Reproducible with source:
``` python
def foo(arg=True):... ## noqa: FBT002
```
and settings:
``` json
{
  "line-length": 35,
  "lint": {
    "select": [
      "E501",
      "FBT"
    ]
  },
  "format": {}
}
```

The practical real-world use-case for this is when multiple linting tools and ignore comments are used, e.g. adding `# noqa` here where the line was previously not too long will cause a violation (but should not):
```
# numpydoc ignore=PR01  # noqa: FBT002
```
Interestingly, the `# type: ignore` case is handled properly, and does _not_ cause a violation:
```
# type: ignore  # noqa: FBT002
```

### Version

ruff v0.11.12 (using the ruff playground)

EDIT: For clarity:

This line is not too long:

``` python
def foo(arg=True):... #
```

But this line is:

``` python
def foo(arg=True):... ## noqa: FBT002
```

But, simply adding `# noqa: FBT002` should not cause the line to be too long.

---

_Comment by @ntBre on 2025-06-04 21:06_

It looks like we use this `is_pragma_comment` function, which doesn't recognize `numpydoc` but does recognize `type`.

 https://github.com/astral-sh/ruff/blob/a3ee6bb3b5f4f375e606d3bc12094549bb46a98b/crates/ruff_python_trivia/src/pragmas.rs#L13

There's an [open issue](https://github.com/astral-sh/ruff/issues/11941) for the formatter to allow specifying custom pragmas, but I don't see one for the linter.

(Although this is still arguably a duplicate since we'd likely use the same option)

---

_Label `configuration` added by @ntBre on 2025-06-04 21:06_

---

_Comment by @user27182 on 2025-06-04 21:21_

> It looks like we use this `is_pragma_comment` function, which doesn't recognize `numpydoc` but does recognize `type`.
> 
> [ruff/crates/ruff_python_trivia/src/pragmas.rs](https://github.com/astral-sh/ruff/blob/a3ee6bb3b5f4f375e606d3bc12094549bb46a98b/crates/ruff_python_trivia/src/pragmas.rs#L13)
> 
> Line 13 in [a3ee6bb](/astral-sh/ruff/commit/a3ee6bb3b5f4f375e606d3bc12094549bb46a98b)
> 
>  pub fn is_pragma_comment(comment: &str) -> bool { 
> There's an [open issue](https://github.com/astral-sh/ruff/issues/11941) for the formatter to allow specifying custom pragmas, but I don't see one for the linter.
> 
> (Although this is still arguably a duplicate since we'd likely use the same option)

Perhaps this issue is more general than allowing custom pragma ignores? I would have expected _any_ comment to be allowed before `# noqa`, without requiring any special config. E.g. this should also work

``` python
def foo(arg=True):... # a comment  # noqa: FBT002
```

Basically, if a line isn't too long before adding `# noqa`, it should _still_ be not too long after adding `# noqa`, regardless of whatever precedes it. This is my interpretation/understanding of the statement "a line will not be flagged as overlong if a pragma comment causes it to exceed the line length."


---

_Comment by @user27182 on 2025-06-04 21:23_

> > There's an [open issue](https://github.com/astral-sh/ruff/issues/11941) for the formatter to allow specifying custom pragmas, but I don't see one for the linter.
> > (Although this is still arguably a duplicate since we'd likely use the same option)
> 
> Perhaps this issue is more general than allowing custom pragma ignores? I would have expected _any_ comment to be allowed before `# noqa`, without requiring any special config. E.g. this should also work
> 
> def foo(arg=True):... # a comment  # noqa: FBT002
> Basically, if a line isn't too long before adding `# noqa`, it should _still_ be not too long after adding `# noqa`, regardless of whatever precedes it. This is my interpretation/understanding of the statement "a line will not be flagged as overlong if a pragma comment causes it to exceed the line length."

Of course, adding special pragmas would also be nice to ensure that a comment like `# numpydoc ignore=PR01` would not cause a E501 violation by itself

---

_Comment by @ntBre on 2025-06-04 22:31_

> Basically, if a line isn't too long before adding `# noqa`, it should _still_ be not too long after adding `# noqa`, regardless of whatever precedes it. This is my interpretation/understanding of the statement "a line will not be flagged as overlong if a pragma comment causes it to exceed the line length."

Oh I see what you mean. Looking at the implementation again, it does sound like that's the intention. Maybe there's a more subtle bug here than I thought.

---

_Label `configuration` removed by @ntBre on 2025-06-04 22:32_

---

_Label `bug` added by @ntBre on 2025-06-04 22:32_

---

_Comment by @MichaReiser on 2025-06-05 06:14_

I think the problem is that [`is_pragma_comment`](https://github.com/astral-sh/ruff/blob/ff6f0b6ab8fef4df5a14651cf9ef86f016ec0367/crates/ruff_python_trivia/src/pragmas.rs#L13-L30) doesn't handle nested comments like `# test # noqa: RUF100`. It only matches on `test`, which isn't a pragma comment and bails. Instead, it should split comments by `#` and test every element if it is a pragma comment.

---

_Label `help wanted` added by @MichaReiser on 2025-06-05 06:14_

---

_Comment by @CodeMan62 on 2025-06-05 10:58_

will try to fix it 

---

_Assigned to @CodeMan62 by @ntBre on 2025-06-05 12:31_

---

_Comment by @CodeMan62 on 2025-07-04 04:59_

i will try again and try to purpose a better fix this time 

---
