```yaml
number: 13897
title: Document unsupported and extra rules
type: issue
state: open
author: LordAro
labels:
  - documentation
assignees: []
created_at: 2024-10-23T17:18:37Z
updated_at: 2025-04-22T07:54:41Z
url: https://github.com/astral-sh/ruff/issues/13897
synced_at: 2026-01-10T11:09:55Z
```

# Document unsupported and extra rules

---

_Issue opened by @LordAro on 2024-10-23 17:18_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

One of the bits of feedback I've had from colleagues about using ruff is the lack of visibility over which rules are implemented and which aren't:

>No I wouldn't have expected it, but one of my concerns about Ruff was potentially poor pycodestyle support, which is the main thing we wanted out of flake8. [I've set up an example in Ruff's playground](https://play.ruff.rs/df57c7dc-8ff2-4014-bd97-fb91bd83d846) and it doesn't catch any issues, which from further inspection [its pycodestyle support](https://docs.astral.sh/ruff/rules/#error-e) doesn't claim it supports these rules at all. I'd have assumed the table would be complete with gaps for no support instead of just missing rows for no support 😠
>It looks from diffing with [pycodestyle's rules](https://github.com/PyCQA/pycodestyle/blob/main/docs/intro.rst#error-codes) that Ruff is missing E121-E133, of which only 4/11 rules are disabled by default in pycodestyle. I didn't actually know that pycodestyle disabled anything by default, and that would explain a lot of the changes that [snip] had to make when converting to Ruff. [Ruff do have an issue for supporting the rest of the rules](https://github.com/astral-sh/ruff/issues/2402) but it looks like it's not been thought about for a little while...

This is obviously specific to pycodestyle but it would apply to all linter types.

It'd be nice if the rules table listed everything unsupported or not. Would cover both "not yet supported" (like those mentioned above) and "never supported" (e.g. B001)

ruff 0.7.0


---

_Label `documentation` added by @AlexWaygood on 2024-10-23 17:44_

---

_Comment by @MichaReiser on 2024-10-30 08:14_

Thanks for sharing this feedback. I understand that it can be confusing that Ruff lists linter categories without supporting all linters from upstream. 

Our long-term solution to this is to re-categorize our rules (see #1774) to clarify that Ruff is not an exact reimplementation of upstream linters. We could then provide a migration tool that migrates your existing linter configuration and warns you about unsupported lint codes. 

I don't know what a good short-term solution looks like. Adding all rules, including the unsupported rules (some of which we intentionally do not support), would bloat Ruff's documentation a lot. That's why I'm unsure if it is the right solution to communicate missing rules. We would also have to automate the documentation generation to ensure we remain up-to-date when upstream linters add new rules(we're trying to hit a moving target). Considering that this is possibly wasted work long term, it might be better to start with a migration tool right away. 




---

_Comment by @MichaReiser on 2025-04-22 07:54_

https://github.com/astral-sh/ruff/issues/17481 requests to also document rules that Ruff implements but that don't exist in the upstream linters.

---

_Renamed from "Document unsupported rules" to "Document unsupported and extra rules" by @MichaReiser on 2025-04-22 07:54_

---
