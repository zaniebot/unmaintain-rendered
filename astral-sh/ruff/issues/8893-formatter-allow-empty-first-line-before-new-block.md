```yaml
number: 8893
title: "Formatter: `allow_empty_first_line_before_new_block_or_comment` preview style"
type: issue
state: closed
author: MichaReiser
labels:
  - formatter
  - preview
assignees: []
created_at: 2023-11-29T02:30:43Z
updated_at: 2024-01-25T07:26:03Z
url: https://github.com/astral-sh/ruff/issues/8893
synced_at: 2026-01-12T15:54:48Z
```

# Formatter: `allow_empty_first_line_before_new_block_or_comment` preview style

---

_@MichaReiser_

Implement Black's [`allow_empty_first_line_before_new_block_or_comment`](https://github.com/psf/black/pull/3967) as a Ruff preview style. 

```python
def foo():
    """
    Docstring
    """

    # Here we go
    if x:

        # This is also now fine
        a = 123

    else:
        # But not necessary
        a = 123
```

Notice how black now allows empty lines before the comments. Ruff seems to support this sometimes.

Goal: Implement the new formatting behind the preview flag and import the Black tests.

---

_Label `formatter` added by @MichaReiser on 2023-11-29 02:30_

---

_Label `preview` added by @MichaReiser on 2023-11-29 02:30_

---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2023-11-29 02:30_

---

_Comment by @charliermarsh on 2023-12-20 05:11_

Heads up: it looks like this was changed to _always_ allow a blank line at the top of the block, _unless_ it's immediately followed by a docstring. See: https://github.com/psf/black/pull/4060.

---

_Comment by @charliermarsh on 2023-12-20 05:15_

Except, apparently, if it's a class immediately followed by a function: https://black.vercel.app/?version=main&state=_Td6WFoAAATm1rRGAgAhARYAAAB0L-Wj4ACbAGldAD2IimZxl1N_WlXnON2nzNKOlH52D45tYviogiBy8ZlN26ZjqUAmZNQ-muxSkGcV-iAN9HZdhppScEmlx2Al7gJjtYIOGHyFQmjMSn_t7leQm1ctcrtU7IhNuHk3t8IsBD624xhI5TmvygAAAAC8TNkRzPz_WQABhQGcAQAALUwcRrHEZ_sCAAAAAARZWg==

---

_Comment by @MichaReiser on 2023-12-22 05:30_

I see the motivation behind this change but it can lead to unintentional deviations that outweigh the benefit of the change. 

Let's say you start with

```python
def foo():
    """
    Docstring
    """

    # Here we go
    if x:
		if no_longer_needed:
			print("Some complex statements")

        # This is also now fine
        a = 123
```

And you refactor the code to remove the `no_longer_needed` branch

```python
def foo():
    """
    Docstring
    """

    # Here we go
    if x:

        # This is also now fine
        a = 123
```

and you now hit save. Without this change, the comment and `a = 123` gets moved directly under the `if x:` condition which is what I would expect and gives us consistent formatting. 


```python
def foo():
    """
    Docstring
    """

    # Here we go
    if x:
        # This is also now fine
        a = 123
```

This would no longer happen if we implement the change. Instead, I have to remove the line above the comment manually. While I see that being able to make an exception might help with readability in few cases, removing the new line is the desired behavior in most cases. 


I opened [an issue in the Black repository](https://github.com/psf/black/issues/4121) to share our feedback with them.


---

_Closed by @MichaReiser on 2023-12-22 05:30_

---

_Comment by @vaughnkoch on 2024-01-25 07:20_

Hi, thanks for building Ruff! 

Given that Black is allowing newlines after a block opening line in the 2024 preview style, based on https://github.com/psf/black/issues/4043, does Ruff plan to reopen this issue and conform to Black's intended style?

Personally, I place a high premium on code readability, and while I don't always add blank lines after a block opening line, it can often make the code a lot clearer, as demonstrated here: https://github.com/psf/black/issues/902#issuecomment-1425713025

I would love to see this reopened and addressed. Thanks for considering!

note: I prefer not to comment on closed issues, but this seems like the most appropriate place to comment. Please let me know if there's a better way to provide feedback :)

---

_Comment by @MichaReiser on 2024-01-25 07:26_

Hy @vaughnkoch 

We share your concern for readability. That's the main purpose of a code formatter, isn't it.

We have some concerns with the preview style as it is defined by black and decided not to ship it as part of Ruff's upcoming new stable style (which this issue is tracking). It doesn't mean we don't care. It's just that we aren't convinced by the heuristic and want to avoid changing styles too often. 

My personal take on Black's blank line rules is that they're too opinionated and I could see it as an option that we explore with simpler blank line rules in the future, but that requires taking a holistic look at all of Ruff's/Black's blank line rules and find ways to simplify them.

I recommend opening a new issue that we can use for future style discussions. 

---
