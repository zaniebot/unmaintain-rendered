---
number: 1040
title: RUF001 and RIGHT SINGLE QUOTATION MARK
type: issue
state: closed
author: lovetox
labels:
  - configuration
assignees: []
created_at: 2022-12-04T18:07:46Z
updated_at: 2023-11-12T16:39:53Z
url: https://github.com/astral-sh/ruff/issues/1040
synced_at: 2026-01-10T01:22:38Z
---

# RUF001 and RIGHT SINGLE QUOTATION MARK

---

_Issue opened by @lovetox on 2022-12-04 18:07_

Hi,

I like the RUF001 check, but what was the rational to consider U+2019 an ambiguous character?

In english it seems there is lots of controversy [1] about what to use as correct apostrophe, i read a little bit about it, but i think we are far away from considering U+2019 as an error.

because U+2019 is very often used in strings, i would argue the RUF001 check is hard to use, because one would need to ignore a lot of strings in the codebase.

[1] https://www.quora.com/Punctuation-Why-is-the-right-single-quote-U+2019-and-not-the-semantically-distinct-apostrophe-U+0027-the-preferred-apostrophe-character-in-Unicode

---

_Comment by @charliermarsh on 2022-12-04 18:11_

The list was actually taken directly from VS Code, which defines a list of "confusing" unicode characters. I'd like to continue mirroring that list, but perhaps we could allow some characters to be marked as acceptable.

---

_Label `configuration` added by @charliermarsh on 2022-12-04 18:11_

---

_Comment by @lovetox on 2022-12-04 18:20_

I think this would be useful, and probably spares you a lot of discussion over the years.

---

_Comment by @JonathanPlasse on 2022-12-04 18:47_

I would be open to implementing it.
How should the setting be named?
Should it be the confusable or its utf-8 code?
```toml
[tool.ruff.ruff]
allowed-confusables = ["…", "−"]
```

---

_Comment by @charliermarsh on 2022-12-04 18:58_

I think it can be:

```toml
[tool.ruff]
allow-confusables = ["…"]
```


---

_Comment by @JonathanPlasse on 2022-12-04 19:02_

Should it be added to the cli?

---

_Comment by @charliermarsh on 2022-12-04 19:03_

I don't think so, no.

---

_Referenced in [astral-sh/ruff#1059](../../astral-sh/ruff/pulls/1059.md) on 2022-12-05 13:47_

---

_Comment by @charliermarsh on 2022-12-05 17:54_

Closed by https://github.com/charliermarsh/ruff/pull/1059.

---

_Closed by @charliermarsh on 2022-12-05 17:54_

---

_Comment by @reagle on 2023-02-15 21:17_

There's no discussion forum, so sorry to crash a closed issue, but what does this even mean? I don't use vscode, and I don't know why the first chunk is problematic, and why it is "fixed" into the second:

Here, in the actual code, I want to replace curly quotes with ascii quotes.

```
        ("‘", "'"),
        ("’", "'"),
```

But ruff converts the characters I'm looking for into apostrophes...?

```
        ("`", "'"),
        ("`", "'"),
```


---

_Comment by @bodograumann on 2023-11-12 13:06_

This is an upstream bug, @reagle : https://github.com/hediet/vscode-unicode-data/pull/1
Needs to fixed [in ruff](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/ruff/rules/confusables.rs) and [VSCode](https://github.com/microsoft/vscode/blob/main/src/vs/base/common/strings.ts#L1074).

---

_Comment by @charliermarsh on 2023-11-12 16:39_

Thanks @bodograumann!

---
