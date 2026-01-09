---
number: 1123
title: E501 error with strings that contains unicode
type: issue
state: closed
author: keul
labels:
  - question
assignees: []
created_at: 2022-12-07T15:14:07Z
updated_at: 2022-12-16T07:43:57Z
url: https://github.com/astral-sh/ruff/issues/1123
synced_at: 2026-01-07T13:12:14-06:00
---

# E501 error with strings that contains unicode

---

_Issue opened by @keul on 2022-12-07 15:14_

Non sure if this an issue or a "by design" behaviour: I get a

```
226:1: E501 Line too long (105 > 100 characters)
```

The line itself is less than 100 chars long (my custom `line-length` in .toml), but I have a string with emoji chars there:

```python
                f"⚠️ ⚠️ ⚠️ - There are…"
```


---

_Comment by @charliermarsh on 2022-12-07 15:15_

Would you mind posting the full line?

---

_Label `question` added by @charliermarsh on 2022-12-07 15:19_

---

_Comment by @keul on 2022-12-07 15:37_

This is another example, but I post these just for reference. Looking at this gain: it seems is not a ruff issue. I didn't noticed that also VsCode count these chars a 2.

For reference:

```python
        click.echo(
            Back.YELLOW
            + Fore.BLACK
            + (
                f"⚠️ ⚠️ ⚠️ - There are {len(warn_lines)} lines with warnings. Please check them. ⚠️ ⚠️ ⚠️ "
            )
            + Style.RESET_ALL
        )
```

I get:

```
E501 Line too long (107 > 100 characters)
```

Sorry for the noise!


---

_Closed by @keul on 2022-12-07 15:37_

---

_Comment by @charliermarsh on 2022-12-07 15:39_

No prob! Yeah Unicode characters are tricky. We do count "characters" and not bytes explicitly (via the Rust standard library), but it's not always intuitive, I think...

---

_Comment by @lengau on 2022-12-16 00:29_

Sorry to necro a week-old thread but I'm a big unicode nerd who happened upon this thread during an unrelated search about E501. I'm adding this purely for the sake of nerdy interest.

`⚠️` is actually two unicode codepoints. Specifically, it's code point [26A0](https://util.unicode.org/UnicodeJsps/character.jsp?a=26A0) for the warning sign and code point [FE0F](https://util.unicode.org/UnicodeJsps/character.jsp?a=FE0F), variation selector. 

Codepoint 26A0 on its own is `⚠`, which looks the same but without the pretty colours of an emoji that the variation selector provides. Or, from a Python terminal:

```
>>> len('⚠️')
2
>>> len('⚠')
1
```

Just to add to the confusion, in UTF-8, ⚠️ is actually 6 bytes long. (Each codepoint is 3 bytes).

I suppose one *could* argue that what we care about here is the number of [graphemes](https://en.wikipedia.org/wiki/Grapheme) rather than bytes or code points, and the [unicode-segmentation](https://crates.io/crates/unicode-segmentation) crate can count graphemes, but that's not without its own costs (mostly performance, but also potentially its own "huh?" moments since the way it's currently counted aligns with Python's `len`).

---

_Comment by @charliermarsh on 2022-12-16 00:32_

Always appreciate a clarification from someone who knows way more than me about a topic like this :)

---

_Comment by @keul on 2022-12-16 07:43_

@lengau thanks for the spelunking! This is probably why my terminal has some issues rendering the ⚠️ (I've to separate multiple ones with a blank space) while ⚠ is OK! 

---
