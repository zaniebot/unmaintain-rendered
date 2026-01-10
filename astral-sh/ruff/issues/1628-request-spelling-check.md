---
number: 1628
title: "Request: Spelling check"
type: issue
state: closed
author: strickvl
labels:
  - plugin
assignees: []
created_at: 2023-01-04T12:10:17Z
updated_at: 2025-11-12T14:24:10Z
url: https://github.com/astral-sh/ruff/issues/1628
synced_at: 2026-01-10T01:22:39Z
---

# Request: Spelling check

---

_Issue opened by @strickvl on 2023-01-04 12:10_

There are seemingly two big Python code checkers: [`pyspelling`](https://facelessuser.github.io/pyspelling/) and [`codespell`](https://github.com/codespell-project/codespell). Codespell doesn't catch everything, and pyspelling is slow to run. It'd be great to have a version / implementation of the logic of these (eventually including PySpelling's exception syntax etc) in `ruff`.

---

_Comment by @colin99d on 2023-01-04 14:04_

@charliermarsh could we just port over typos, or is it too rust specific?

---

_Comment by @charliermarsh on 2023-01-04 15:50_

I think the best option would be to try and integrate typos as a library (https://docs.rs/typos/0.10.6/typos/). Not sure if the API is amenable to that.

---

_Label `plugin` added by @charliermarsh on 2023-01-04 15:50_

---

_Comment by @charliermarsh on 2023-01-04 15:50_

(That is: call the typos functions directly in Rust, rather than using the CLI, and report typos as Ruff errors.)

---

_Referenced in [astral-sh/ruff#1820](../../astral-sh/ruff/issues/1820.md) on 2023-01-12 16:27_

---

_Referenced in [astral-sh/ruff#1999](../../astral-sh/ruff/pulls/1999.md) on 2023-01-19 17:14_

---

_Comment by @colin99d on 2023-02-10 23:30_

I have been thinking about this for a while and I think we should keep spell checking separate from Ruff for the following reasons:

1. This spell checker is already blazingly fast, so users of typos have no reason to switch
2. Spell checkers do not need to parse python code, so we would not save any additional time from them being part of the library
3. Having to support multiple languages could be burdensome

---

_Comment by @charliermarsh on 2023-02-11 00:08_

Yeah that makes sense. Let's pass on this for now.

---

_Closed by @charliermarsh on 2023-02-11 00:08_

---

_Referenced in [zenml-io/zenml#1400](../../zenml-io/zenml/pulls/1400.md) on 2023-03-09 17:54_

---

_Comment by @MicaelJarniac on 2023-05-19 20:29_

> I have been thinking about this for a while and I think we should keep spell checking separate from Ruff for the following reasons:
> 
>     1. This spell checker is already blazingly fast, so users of typos have no reason to switch
> 
>     2. Spell checkers do not need to parse python code, so we would not save any additional time from them being part of the library
> 
>     3. Having to support multiple languages could be burdensome

Regarding 2, I think spell checkers can benefit greatly from parsing Python code.

For example, say I've installed a lib from PyPI that has typos on its function names. There's nothing I can do about them, they're outside my control, but I'll need to type the typos if I want to use these functions.

A normal spell checker, that treats everything as plain text, would see the typo and complain about it. But again, there's nothing I can do about them.

If the spell checker understands Python, it'll be able to tell what's my fault and what's outside my control, and so if the typo is on something from an external lib, it can ignore them, as I can't control that, but if the typo is on my own functions and variable names, it can then warn me.

---

_Referenced in [astral-sh/ruff#5014](../../astral-sh/ruff/issues/5014.md) on 2023-06-11 18:39_

---

_Referenced in [qiskit-community/ffsim#161](../../qiskit-community/ffsim/issues/161.md) on 2024-09-25 13:04_

---

_Comment by @adam-grant-hendry on 2024-11-24 00:49_

> > I have been thinking about this for a while and I think we should keep spell checking separate from Ruff for the following reasons:
> > ```
> > 1. This spell checker is already blazingly fast, so users of typos have no reason to switch
> > 
> > 2. Spell checkers do not need to parse python code, so we would not save any additional time from them being part of the library
> > 
> > 3. Having to support multiple languages could be burdensome
> > ```
> 
> Regarding 2, I think spell checkers can benefit greatly from parsing Python code.
> 
> For example, say I've installed a lib from PyPI that has typos on its function names. There's nothing I can do about them, they're outside my control, but I'll need to type the typos if I want to use these functions.
> 
> A normal spell checker, that treats everything as plain text, would see the typo and complain about it. But again, there's nothing I can do about them.
> 
> If the spell checker understands Python, it'll be able to tell what's my fault and what's outside my control, and so if the typo is on something from an external lib, it can ignore them, as I can't control that, but if the typo is on my own functions and variable names, it can then warn me.

Also regarding 2, I need to spellcheck docstrings for my documentation, so I do actually do need to run in through python files sometimes.

---

_Referenced in [ansys/pyaedt#6157](../../ansys/pyaedt/pulls/6157.md) on 2025-05-16 10:04_

---

_Comment by @ashrub-holvi on 2025-11-12 14:20_

Good to keep in mind that, if I understood it correctly, there are two approaches for spellcheck - based on dictionary of correct words and based on dictionary/rules for mistakes. Both has own pros and cons. `Typos` and `codespell` is fixing known typos, `PySpelling` uses Aspell and Hunspell, I guess it's mostly dictionaries.

---
