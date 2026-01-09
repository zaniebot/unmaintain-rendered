---
number: 8908
title: "docstring code formatter: decide whether doctests should end with a empty PS2 prompt line or not"
type: issue
state: closed
author: BurntSushi
labels:
  - docstring
  - formatter
  - needs-decision
assignees: []
created_at: 2023-11-29T13:18:19Z
updated_at: 2023-12-23T12:38:15Z
url: https://github.com/astral-sh/ruff/issues/8908
synced_at: 2026-01-07T13:12:15-06:00
---

# docstring code formatter: decide whether doctests should end with a empty PS2 prompt line or not

---

_Issue opened by @BurntSushi on 2023-11-29 13:18_

(This issue is forked off from discussion beginning at https://github.com/astral-sh/ruff/issues/7146#issuecomment-1829264623.)

Currently, the way code snippet formatting in docstrings works is by collecting all of the lines in a single code snippet, feeding them into a recursive call to the formatter and then printing the resulting lines back into the docstring. For doctests in particular, the first line in a code snippet is always a PS1 prompt (starts with `>>> `) and all subsequent lines (if any) are always PS2 prompts (starts with `... `). [For example](https://github.com/BurntSushi/polars/commit/559b9d683bce3d4fd48d7946359fbdaeea1afccc#diff-7a25a2e8cd476e7265a2fc04c76d36c7f30fc288600119595fbf1caba75618c4L1262):

```python
>>> with pl.Config(trim_decimal_zeros=False):
...     print(df)
...
```

This particular example contains a trailing empty line. This trailing empty line is collected as part of the code snippet and fed into the formatter. The formatter, as one might expect, trims this empty line. The end result is that the reformatted code snippet looks like this:


```pycon
>>> with pl.Config(trim_decimal_zeros=False):
...     print(df)
```

The point of this issue is to decide whether this is desirable behavior or not. It came up in feedback from @stinodego after I [reformatted the code snippets in polars](https://github.com/BurntSushi/polars/commit/559b9d683bce3d4fd48d7946359fbdaeea1afccc). If one looks at the diff carefully, you can see some examples where there was an empty PS2 prompt line at the end of a code snippet (and got trimmed). But there are also plenty of code snippets that do not have an empty PS2 prompt line at the end of it. It therefore seems clear that we cannot unconditionally add an empty PS2 prompt line at the end of every reformatted doctest code snippet.

For example, it seems at least clear that a code snippet with only a PS1 prompt line should not have an empty line added after it. That is, we wouldn't want a reformatted code snippet to look like this:

```pycon
>>> foo(x)
...
```

I say this because I've looked at a number of real doctests in the wild and I don't think I've ever seen one written like this.

But if the code snippet does contain at least one non-empty PS2 prompt line, then I think it's less clear whether we should add a trailing empty line or not. Namely, if you look at the [diff to polars](https://github.com/BurntSushi/polars/commit/559b9d683bce3d4fd48d7946359fbdaeea1afccc) closely, not all code snippets with non-empty PS2 prompt lines contain a trailing empty line. That is, we can't really discern a consistent pattern from existing practice from this one example.

Another data point is a [diff from applying code snippet reformatting to CPython](https://github.com/BurntSushi/cpython/commit/19d1c85db9c862172f0cffd0872a942aef26d934#diff-f67a8b5be6bdc48383d2ab907325a4c75ef998fa7cd0bba382ec2efa7e4f00b5R204-R205). I can't discern a consistent rule there either. Some code snippets with non-empty PS2 prompt lines have a trailing empty line and others don't.

-----

Speaking for me personally, I favor following the existing formatting rules and trimming the new line. But this might be something we want to collect user feedback on.

Pinging folks involved in the discussion in #7146: @stinodego @MichaReiser @ofek @pawamoy

---

_Label `docstring` added by @BurntSushi on 2023-11-29 13:18_

---

_Label `formatter` added by @BurntSushi on 2023-11-29 13:18_

---

_Label `needs-decision` added by @BurntSushi on 2023-11-29 13:18_

---

_Added to milestone `Formatter: Stable` by @BurntSushi on 2023-11-29 13:18_

---

_Referenced in [astral-sh/ruff#7146](../../astral-sh/ruff/issues/7146.md) on 2023-11-29 13:18_

---

_Comment by @ofek on 2023-11-29 13:34_

I think trimming such new lines is the correct behavior 👍 

---

_Comment by @pawamoy on 2023-11-29 14:04_

Agree with @ofek here, IMO it should trim the new line. Copying the snippet and pasting it in a REPL demands interaction anyway, so even if the user has to hit enter after paste, it's fine. The trailing PS2 empty line is just a consequence of how the REPL works, and shouldn't be replicated in snippets (even though these snippets legitimately try to mimic the input/output of the REPL). Depending on the rendering method/generator, and/or [client side customization of the selection behavior](https://mkdocstrings.github.io/recipes/#prevent-selection-of-prompts-and-output-in-python-code-blocks), the prompts might even be left out of the selected content, so the trailing new line would make even less sense.

---

_Comment by @stinodego on 2023-11-29 15:20_

> But if the code snippet does contain at least one non-empty PS2 prompt line, then I think it's less clear whether we should add a trailing empty line or not. Namely, if you look at the [diff to polars](https://github.com/BurntSushi/polars/commit/559b9d683bce3d4fd48d7946359fbdaeea1afccc) closely, not all code snippets with non-empty PS2 prompt lines contain a trailing empty line. That is, we can't really discern a consistent pattern from existing practice from this one example.

I think there is actually a consistent way to determine when there should be a trailing `...`. It occurs in the Python console when an indentation decrease happens, i.e. after defining a function or using a context manager. But not after a simple statement like `foo(x)`.

That being said, I'm completely fine with these being trimmed. Humans writing doc examples also naturally leave them out. They are currently enforced and added by `blackdoc` in our code base.

---

_Comment by @keewis on 2023-12-07 22:11_

adding a trailing `...` after the end of block statements like `if` or `with` is a code-style choice, so this is very subjective (see keewis/blackdoc#52): it makes the code example a appear little less dense. Obviously, trailing empty lines should be removed for anything that is not a block statement.

Following the argument that `doctest` is basically a copy-pasted REPL cell would actually mean that you'd have to remove the trailing empty line after a block statement (which is what `blacken-docs` does, AFAIR). However, this appears to be changing in `python=3.13` (unfortunately, I can't remember where I got that bit of information).

---

_Referenced in [networkx/networkx#7160](../../networkx/networkx/pulls/7160.md) on 2023-12-14 07:44_

---

_Comment by @MichaReiser on 2023-12-22 06:12_

@BurntSushi from what I understand is that we currently strip the empty line. I'm fine with leaving it as is because we haven't received any feedback that clearly shows that it should be the other way round. The "worst" outcome is that we have to add an option controlling whether it should be stripped or not, which I think would be fine. So I would recommend closing this for now and re-visit when it comes up as an issue

---

_Comment by @BurntSushi on 2023-12-23 12:38_

SGTM!

---

_Closed by @BurntSushi on 2023-12-23 12:38_

---
