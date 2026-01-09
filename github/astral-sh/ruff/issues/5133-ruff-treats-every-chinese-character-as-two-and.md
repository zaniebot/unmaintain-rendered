---
number: 5133
title: "Ruff treats every Chinese character as two and often falsely reports the \"Line too long (E501)\" warning"
type: issue
state: closed
author: jackjyq
labels: []
assignees: []
created_at: 2023-06-16T01:53:52Z
updated_at: 2023-06-16T02:25:34Z
url: https://github.com/astral-sh/ruff/issues/5133
synced_at: 2026-01-07T13:12:15-06:00
---

# Ruff treats every Chinese character as two and often falsely reports the "Line too long (E501)" warning

---

_Issue opened by @jackjyq on 2023-06-16 01:53_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

I have noticed that [Ruff](https://marketplace.visualstudio.com/items?itemName=charliermarsh.ruff) treats every Chinese character as two, so it often falsely reports the "Line too long (E501)" warning.

```python
x = "一二三四五六七八九十一二三四五六七八九十一二三四五六七八九十一二三四五六七八九十一二三四五六七八九十一二三四五六七八九十一二三四五六七八九十一二三四五六七八九十"
```

I believe that this behavior is different than [Flake8](https://marketplace.visualstudio.com/items?itemName=ms-python.flake8) and [Black](https://marketplace.visualstudio.com/items?itemName=ms-python.black-formatter) as they treat one Chinese character as one.

![image](https://github.com/astral-sh/ruff/assets/16525413/bc0ba50f-8882-4e41-9ca5-290b0bdf4d0a)

I use Ruff (v2023.20.0) the VSCode version with all the default settings.

## Other considerations

I know that Chinese characters take more space than ASCII characters and the actual ratio depends on fonts and rendering.

On Windows 11 VSCode,

- Dejavu Sans Mono: 53 Chinese characters take 88 ASCII characters' space
- Cascadia Mono: 52 Chinese characters take 88 ASCII characters' space

As we can see, the radio is not 2.

Also, this behaviour makes Ruff not compatible with the Black formatter because more often than not, the Black does not break lines while Ruff keeps reporting the E501 warning.




---

_Comment by @charliermarsh on 2023-06-16 02:24_

There's a bit more info on this in #3902 and #3825. Ruff is using character width (which is defined as part of the [Unicode standard](https://www.unicode.org/reports/tr11/), I think, although I'm not an expert), rather than character count. Black made the same change, but it's still in preview (you _should_ see the same behavior if you run with `black --preview`). We may consider changing this, though I'm going to close this as a duplicate of the others for now. 

---

_Closed by @charliermarsh on 2023-06-16 02:24_

---

_Comment by @charliermarsh on 2023-06-16 02:25_

(For posterity: the primary issue for tracking this is #3825.)

---
