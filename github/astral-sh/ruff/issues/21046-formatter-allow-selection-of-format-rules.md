---
number: 21046
title: "Formatter: allow selection of format-\"rules\""
type: issue
state: closed
author: spaceone
labels:
  - question
assignees: []
created_at: 2025-10-23T13:40:49Z
updated_at: 2025-10-23T13:51:50Z
url: https://github.com/astral-sh/ruff/issues/21046
synced_at: 2026-01-07T13:12:16-06:00
---

# Formatter: allow selection of format-"rules"

---

_Issue opened by @spaceone on 2025-10-23 13:40_

### Question

Dear ruff format,

many users are unsatisfied with the strong opinions of black/ruff format. While I agree with 90% of the opinions, I have some, which i would like to avoid.
So my imagination would be that I could select specific format "rules" of the existing ones.
I saw that they are even already (partly?) named:
https://github.com/psf/black/blob/main/docs/the_black_code_style/future_style.md
e.g. `wrap_comprehension_in`.

So maybe you could expose a `--select` option for (at least some of) the names, (which would be easy to implement) that the AST normalizes only according to these rules?

It's nice that I can already prevent string-normalization, which helps me to separate these into single git-commits. (via `ruff format --config 'format.quote-style = "preserve"' .`).

### Version

ruff 0.14.1

---

_Label `question` added by @spaceone on 2025-10-23 13:40_

---

_Comment by @MichaReiser on 2025-10-23 13:51_

Thanks for opening this issue. 

We understand that some users want more flexibility in formatting. We also hear that other users deliberately don't want more options because they result in discussions on the team how those options should be configured. 

I think Ruff ideally finds itself somewhere in between, allowing some configurability but intentionally keeping the options minimal (also just because we need to test every possible combination of options). 

I'm sympathetic to introducing e.g. an option that changes how Ruff picks nested quotes in f-strings (there's an issue for this somewhere). I don't want to introduce a new option for every style change as that wouldn't take into account the actual demand from users in comparison to the maintenance burden it creates. 

To get a specific option added, I recommend opening an issue that asks for an option that changes a specific formatting behavior and we can see if there's interest from other users.

---

_Closed by @MichaReiser on 2025-10-23 13:51_

---
