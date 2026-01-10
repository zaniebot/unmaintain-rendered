---
number: 6284
title: "Formatter: Add coverage for comments in lambda star parameters"
type: issue
state: closed
author: zanieb
labels:
  - formatter
  - help wanted
assignees: []
created_at: 2023-08-02T19:29:13Z
updated_at: 2023-09-08T09:41:01Z
url: https://github.com/astral-sh/ruff/issues/6284
synced_at: 2026-01-10T01:22:45Z
---

# Formatter: Add coverage for comments in lambda star parameters

---

_Issue opened by @zanieb on 2023-08-02 19:29_

You can insert a comment between the star and the parameter. We'll probably want test coverage for cases like this:

```python
(
    lambda
    *
    # test
    x: print(x)
)
```

_Originally posted by @zanieb in https://github.com/astral-sh/ruff/pull/6257#discussion_r1281363287_
            

---

_Label `formatter` added by @zanieb on 2023-08-02 19:29_

---

_Comment by @charliermarsh on 2023-08-02 19:42_

FWIW Black doesn't even seem to parse this: https://black.vercel.app/?version=main&state=_Td6WFoAAATm1rRGAgAhARYAAAB0L-Wj4ACbAGFdAD2IimZxl1N_WhQxSjX4pwCOC0bXqrOAPC2j0OSxmS2NWKjL0LiLowfwOGkTU-08OZBXl1DPvepYNHPFHLbR7GY_K3EzsmF5YZkQnDBTvd0fNZ4DD5c_mNDl7STR-bXwiQAAAAAA872hMcs31FsAAX2cAQAAALZno9ixxGf7AgAAAAAEWVo=

---

_Comment by @zanieb on 2023-08-02 20:24_

It executes with CPython though! I'd say this is low priority.

---

_Comment by @charliermarsh on 2023-08-02 20:25_

Oh yeah I agree that it's valid! :)

---

_Label `help wanted` added by @MichaReiser on 2023-08-03 06:40_

---

_Comment by @qdegraaf on 2023-08-03 11:33_

Adding the example, current formatter implementation makes:

```python
(
    lambda # comment
    *x: print(x)
)
```

of it. Played around with placement of comment around the expression and it appears to fairly consistently format to that. It looks alright to me. Considering Black does not parse this and therefore we can't copy their result, are we happy with this formatting? If so, I can open a small PR with some added fixtures and leave actual formatting untouched.

---

_Comment by @zanieb on 2023-08-03 14:55_

@qdegraaf that looks good to me — is it consistent with what we do for function definitions?

---

_Comment by @qdegraaf on 2023-08-03 20:46_

@zanieb looking at a somewhat similar function definition:

```python
def varg_with_leading_comments(
    a,
    b,
    # comment
    *args,
):
    ...
```

which becomes

```python
def varg_with_leading_comments(
    a, b,
    # comment
    *args
): ...
```
and looking at a couple more examples in the function_def snapshot it looks like they are comparable.

They are consistent in ensuring a line break after the comma, but a little different with `lambda` allowing a leading comment to the `*foo` to be on the same line and the function def only allowing this when there's a comma between a given arg and the comment. (Judging from a quick scan of the snapshot e.g.:

```python
def f35(
    # keyword only comment, leading
    *,  # keyword only comment, trailing
    c,
):
    pass
```

I can open a PR with the test cases and if any tweaks are preferred they can be made and discussed there.

---

_Referenced in [astral-sh/ruff#6318](../../astral-sh/ruff/pulls/6318.md) on 2023-08-03 20:55_

---

_Comment by @qdegraaf on 2023-08-03 20:59_

@zanieb PR is open. Found an interesting case that does not pass current tests yet:

```python
(
    lambda
    # comment 1
    *
    # comment 2
    x:
    # comment 3
    x
)
```

When formatted once becomes:

```python
(
     lambda # comment 1
     # comment 2
     *x: # comment 3
     x
)
```

And when formatted again moves the last comment again:
```python
 (
     lambda # comment 1
     # comment 2
     *x: x  # comment 3
 )
```

I can have a look tomorrow if I can figure out why and fix it. In the meantime, additional cases are in the PR but if someone sees a quick fix feel free to open another PR with the required changes.

---

_Comment by @zanieb on 2023-08-04 00:16_

For what it's worth, Black will not move this comment


```python
def foo(
    *
    # comment
    args,
):
    pass
```

We format it as

```python
def foo(
    # comment
    *args,
):
    pass
```

We'll need to decide if we want to deviate from Black intentionally here. I think lambdas should probably be treated the same as functions.

---

_Comment by @MichaReiser on 2023-08-04 05:55_

The deviation looks reasonable to me. I'm not aware of a good reason why you should have a comment between the `*` and the argument name. 

We should feel confident to move comments if we think they improve formatting. Ultimately, it is the job of a formatter to make your code look good, as opposed to making the fewest possible changes to your code (our formatter would work very differently if that's the goal). I believe the reason why Black is more conservative around moving comments is because they don't have an infrastructure that supports this well (but their approach has the benefit that they can keep comments closer to where they were initially placed)

---

_Comment by @qdegraaf on 2023-08-07 14:32_

It actually looks like Black does parse this sometimes, and when it does, moves all the comments out of the body and param: 

https://black.vercel.app/?version=main&state=_Td6WFoAAATm1rRGAgAhARYAAAB0L-Wj4ACvAGNdAD2IimZxl1N_WhQxSjX4pwCOC0bXpy9CkwkC7iHczyWxADRF-q4ga7QXfhtJq4VEU5ZvaS_iV0a7TnPwpkbVOmHyXDXSLm91HFafcjSRH6yxz3IR-zB59GRNdryW7smhGh85AAAA9R0yDc1uYQ0AAX-wAQAAALgEWpGxxGf7AgAAAAAEWVo=

Do we want to copy this behaviour for the cases Black does not parse? 

---

_Comment by @zanieb on 2023-08-07 17:19_

I think we can do better than Black in this case. The comments lose all context if moved outside of the expression. @MichaReiser is more of an authority here, however.

---

_Comment by @MichaReiser on 2023-08-07 17:25_

> I think we can do better than Black in this case. The comments lose all context if moved outside of the expression. @MichaReiser is more of an authority here, however.

Do you have an example on what you mean by moving the comments out?

---

_Comment by @qdegraaf on 2023-08-07 19:46_

The Black playground turns

```python
(
    lambda # comment 1
    * # comment 2
    x: # comment 3
    x
)
```
into
```python
(lambda *x: x)  # comment 1  # comment 2  # comment 3
```

---

_Comment by @MichaReiser on 2023-08-08 08:42_

Thanks. I agree, moving the comments out removes the context from them and can be problematic if one of the comments is a noqa comment.

---

_Added to milestone `Formatter: Alpha` by @MichaReiser on 2023-08-16 07:09_

---

_Removed from milestone `Formatter: Alpha` by @MichaReiser on 2023-08-22 14:13_

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-08-22 14:13_

---

_Closed by @MichaReiser on 2023-09-08 09:41_

---
