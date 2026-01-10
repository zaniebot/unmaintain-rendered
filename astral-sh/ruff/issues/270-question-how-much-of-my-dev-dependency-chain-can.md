---
number: 270
title: "[Question] How much of my dev dependency chain can ruff remove?"
type: issue
state: closed
author: anthonycorletti
labels:
  - question
assignees: []
created_at: 2022-09-27T17:25:38Z
updated_at: 2022-09-29T20:38:21Z
url: https://github.com/astral-sh/ruff/issues/270
synced_at: 2026-01-10T01:22:37Z
---

# [Question] How much of my dev dependency chain can ruff remove?

---

_Issue opened by @anthonycorletti on 2022-09-27 17:25_

Hi @charliermarsh! Thanks for the work you're doing with ruff. 

I'm curious about where ruff fits into the formatting, linting, and type checking ecosystem. I have to install quite a few packages to handle formatting, linting, type checking, et cetera ([see here](https://github.com/anthonycorletti/python-project-template/blob/main/pyproject.toml#L20-L25)) for python projects and would really like to trim that list down. Is this something ruff can help with? If so, what areas would it cover/ packages would ruff replace?

Thanks so much!

---

_Label `question` added by @charliermarsh on 2022-09-28 17:34_

---

_Comment by @charliermarsh on 2022-09-28 17:46_

This is a great question! Thank you for asking it. I've used identical setups in the past and so am very familiar with the workflow you're describing. (Apologies in advance for the long answer.)

_Today_, ruff can be used as a drop-in replacement for `flake8`, when used alongside Black, and without plugins. Your setup uses `flake8-docstrings`, so the rules you'd be losing would be those focused on docstring enforcement.

The plugin thing is important: ruff doesn't yet support any kind of plugin architecture, though I am interested in building-in top 2-3 `flake8` plugins into ruff directly to enable wider adoption. (For example, we could just add the `flake8-docstrings` logic to ruff; then you could replace `flake8` and `flake8-docstrings`.)

In the short-term roadmap, I'd like ruff to support all of the autofixing behaviors that you get from `autoflake`. At that point, you could replace `flake8`, `flake8-docstrings`, and `autoflake` with ruff. We're not there yet, but the work is clear to me and very tractable. (ruff currently supports autofixing for a small number of rules, but not yet those enforced by `autoflake`.)

In the medium-term roadmap, I'd like ruff to support auto-formatting too. You could then imagine using a single, unified tool to perform linting, autofixing, and autoformatting in lieu of `flake8`, `flake8-docstrings`, `autoflake`, `black`, and even `isort`. Combining a linter with an autoformatter makes for a very powerful combination, and something I'm very interested in. This work isn't scoped yet, and doesn't have a clear timeline, but it does fit with the broader vision behind ruff.

In the longer term, I have an even bigger vision that touches on some of the other tools in the toolchain... But I think that's enough for now :)


---

_Comment by @charliermarsh on 2022-09-28 17:49_

One follow-up: it's important to me that even if ruff grows in scope, it can be used and adopted incrementally. For example, even if ruff grows to support autoformatting, I want it to be just as useable as a standalone linter alongside Black.


---

_Comment by @anthonycorletti on 2022-09-29 20:27_

Sounds good @charliermarsh! Closing this out because my question was answered. Cheers!

---

_Closed by @anthonycorletti on 2022-09-29 20:27_

---

_Comment by @charliermarsh on 2022-09-29 20:38_

👍 Stay tuned!

---
