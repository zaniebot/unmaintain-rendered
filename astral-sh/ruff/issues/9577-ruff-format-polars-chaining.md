---
number: 9577
title: Ruff format - polars chaining
type: issue
state: closed
author: mlarcane
labels:
  - formatter
  - style
assignees: []
created_at: 2024-01-19T08:58:24Z
updated_at: 2024-01-25T14:34:35Z
url: https://github.com/astral-sh/ruff/issues/9577
synced_at: 2026-01-10T01:22:49Z
---

# Ruff format - polars chaining

---

_Issue opened by @mlarcane on 2024-01-19 08:58_

First of all; thx for providing this neat framework! In our team, we write alot of polars code and we like to chain such that each new polars method "has it's own line" so to speak. We can't seem to find any configurations that allow for this. This is quite a big deal in data science in general, that people like to chain pandas and polars in this way.

Is there anything you can do on this end so it can be configured?

![ruff_format_polars](https://github.com/astral-sh/ruff/assets/154270233/c88272bf-b6cc-4420-986c-d370f509a515)


---

_Comment by @MichaReiser on 2024-01-19 09:30_

Hy @mlarcane 

No, there's currently no way to configure method chaining but I know that method chaining has come up before (I also find Ruff's/Black's formatting hard to read). 

The main challenge with method chaining is that Black/Ruff try very hard to avoid (introducing) parentheses. But method chaining would require parentheses because of Python (a problem other languages don't have). 

I don't expect us to work on this anytime soon but I'll leave it open and track it as a style discussion.

---

_Label `formatter` added by @MichaReiser on 2024-01-19 09:30_

---

_Label `style` added by @MichaReiser on 2024-01-19 09:30_

---

_Comment by @tmke8 on 2024-01-19 10:50_

There could be magic parentheses (analogous to magic commas) which, if present, would tell the formatter to apply the "call chain style".

---

_Comment by @mlarcane on 2024-01-19 10:56_

@tmke8 That would be awesome! Really dig the magic commas (for consistency).

---

_Comment by @MartinBernstorff on 2024-01-25 13:26_

Related to #8598

---

_Comment by @MichaReiser on 2024-01-25 14:34_

Thanks, @MartinBernstorff, for linking the issue. I did search for it before replying on the issue but failed to find it. 

I'll close this issue in favour of #8598 

---

_Closed by @MichaReiser on 2024-01-25 14:34_

---

_Referenced in [astral-sh/ruff#8598](../../astral-sh/ruff/issues/8598.md) on 2024-05-10 18:41_

---
