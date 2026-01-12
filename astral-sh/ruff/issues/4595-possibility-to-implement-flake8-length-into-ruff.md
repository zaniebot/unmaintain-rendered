```yaml
number: 4595
title: Possibility to implement flake8-length into ruff
type: issue
state: open
author: jsh9
labels:
  - plugin
  - needs-decision
assignees: []
created_at: 2023-05-23T07:48:44Z
updated_at: 2025-08-04T14:24:59Z
url: https://github.com/astral-sh/ruff/issues/4595
synced_at: 2026-01-12T15:54:44Z
```

# Possibility to implement flake8-length into ruff

---

_@jsh9_

Hi,

I've found that [flake8-length](https://github.com/orsinium-labs/flake8-length) written by @orsinium to be one of my favorite flake8 plugins.  I searched the issue pool of ruff and didn't find anyone mentioning it.

Thank you!

---

_Comment by @orsinium on 2023-05-23 08:51_

![image](https://github.com/charliermarsh/ruff/assets/9638362/5677ad29-0c59-4652-b849-6ed4a8284747)


---

_Label `plugin` added by @charliermarsh on 2023-05-24 01:19_

---

_Comment by @charliermarsh on 2023-05-24 01:24_

I think my preference would be to modify our existing rule rather than add multiple line-length rules within Ruff itself. Same goes for flake8-bugbear's variant that allows a 10% buffer. E.g., we do allow long lines that end in URLs, as long as the URL _starts_ before the length limit (which differs from pycodestyle). We also allow lines that consistent of a single token without whitespace to break it up.

What other behaviors or exceptions might you find helpful in our check?

---

_Label `question` added by @charliermarsh on 2023-05-24 01:24_

---

_Comment by @jsh9 on 2023-05-24 02:43_

Hi Charlie,

Ideally for me, an "intelligent" line length checker should ignore these situations:

- Long string literals (if the string starts before the length limit)
- Comments (if the non-comment part of the line ends before the length limit)

flake8-length can do both, which is why I like it.  Also, the violation code of flake8-length is `LN`, which is intuitive (better than `E501`).

---

_Comment by @orsinium on 2023-05-24 06:16_

> What other behaviors or exceptions might you find helpful in our check?

Flake8-length allows:

+ Long string literals.
+ Long URLs in strings and comments.
+ When the last word in a text (comment, string, or docstring) doesn't fit a bit.
+ Long shebangs.
+ `noqa:`, `pragma:` and similar comments that should appear on the same line as their violation and so cannot be easily moved or split.

The motivation is described in the README, but in short, these are things you want to keep unbroken for the sake of grep'ability. 

It also allows [certain symbols and tokens](https://github.com/orsinium-labs/flake8-length/blob/master/flake8_length/_parser.py#L15-L36) to appear after the string. So, `a: f('very long text'),` won't be reported (`)` and `,` are allowed to go after) but `f('very long text').something_else()` will be reported because the `something_else` part goes beyond the screen but it's significant for the reader to see.

There is also a specific exception for raw SQL queries. SQL allows newlines between tokens, so they can be safely reformatted into a multiline string literal.

I guess that's it. The parser's source code is just about 100 lines.

---

_Label `question` removed by @charliermarsh on 2023-07-10 01:13_

---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:13_

---

_Comment by @stinodego on 2023-08-09 19:03_

I have tried `flake8-length` before, but I found that I get pretty much the same behavior by ignoring `E501` (line too long) and enabling pycodestyle's `max-doc-length`:

```

[tool.ruff]
line-length = 88

select = [
  "E", # pycodestyle
  "W", # pycodestyle
]
ignore = [
  "E501",  # Line length regulated by black
]

[tool.ruff.pycodestyle]
max-doc-length = 88

---

_Comment by @orsinium on 2023-08-10 06:16_

> but I found that I get pretty much the same behavior by ignoring E501 (line too long) <...>

The point of flake8-length is to not remove the line length limit for the code. From the README:

> If you're about having as strict limits as possible, flake8-length is on your side. It's better to set 90 chars limit with a few reasonable exceptions rather than have 120 or more chars limit for everything.

---

_Comment by @stinodego on 2023-08-10 08:21_

Yes, the point being that `black` already takes care of that!


It will wrap anything except for very long strings and comments. And the `max-doc-length` setting will then warn you about certain long strings / comments, but has reasonable exceptions built-in. So it works out to be approximately the same (in my experience) as what the `flake8-length` plugin is trying to achieve.

If you take `black` out of the picture, then it becomes a whole different conversation.

---

_Comment by @lev-blit on 2023-08-22 12:47_

@stinodego I agree that black takes care of that, but it adds a large overhead. There's a project I worked on which had about 200~ files, with some of them being very large, and running ruff was shorter than a blink of an eye but running black against all those files felt like an eternity compared to that (and it was quite long without comparison too - about 8 seconds). That's just because ruff doesn't ignore comments and other examples given above regarding `flake8-length`.

It would be really useful for ruff to be able to support those features, then I would be able to use black only as a formatting tool and not as a linting tool just for E501.

---

_Comment by @RosanneZe on 2024-02-16 08:58_

We're looking to switch from flake8 to ruff, but we would like to keep the behaviour that flake8-length gives. So I'd like to add my vote to adding it as a plugin.

---

_Comment by @SmartManoj on 2025-06-26 02:58_

Is this not solved with [this](https://docs.astral.sh/ruff/settings/#line-length)?

---

_Comment by @lev-blit on 2025-07-04 22:59_

@SmartManoj no, it doesn't ignore comments / docstrings

---

_Comment by @MarthinusBosman on 2025-08-04 14:24_

Hoping to add some attention to this. At least some solution where I can either autowrap comments/docstrings or ignore them to some degree.

It's very boring to have to go and manually fix this for every instance when trying to switch existing projects to ruff.

---
