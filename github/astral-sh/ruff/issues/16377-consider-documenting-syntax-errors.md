---
number: 16377
title: Consider documenting syntax errors
type: issue
state: open
author: ntBre
labels:
  - documentation
assignees: []
created_at: 2025-02-25T17:28:01Z
updated_at: 2025-02-25T17:35:31Z
url: https://github.com/astral-sh/ruff/issues/16377
synced_at: 2026-01-07T13:12:16-06:00
---

# Consider documenting syntax errors

---

_Issue opened by @ntBre on 2025-02-25 17:28_

One reason why we might want to have different error codes for different syntax errors is so that we can provide more detailed documentation for some syntax errors. This isn't _much_ of a concern with the specific syntax error that @ntBre is adding here, as there's not much more to say than "you can't use the `match` statement on Python <3.10".[^1]. However, as we discussed on Brent's design proposal, it will be a concern with other syntax errors that we'll want to detect in the future -- for example, we have very nice docs for [`F404`](https://docs.astral.sh/ruff/rules/late-future-import/#late-future-import-f404) currently in Ruff, and it would be a shame to provide worse docs for red-knot users when we start detecting that syntax error in our brand new tool.

Some errors that we may want to provide per-error docs for are errors that only appear on older or newer Python versions. For example, the details around when you're able to parenthesize context managers on Python <3.9 are pretty subtle. So are the details on when `match` patterns are considered [irrefutable](https://peps.python.org/pep-0634/#irrefutable-case-blocks) (it's a syntax error to have more than one irrefutable pattern in a `match` statement on Python 3.10+).

@MichaReiser also pointed out in the comments on Brent's design doc that there are existing syntax errors the parser detects that probably might also benefit from better docs.

(To be clear, I'm not saying that we _have_ to have multiple error codes for these syntax errors. And there are obviously costs as well as benefits, which Micha outlined well. But this is one possible motivation for why it might help to have different error codes for different syntax errors: it would allow users to easily look them up in our documentation.)

[^1]: That said, I actually don't think it would _hurt_ to have a documentation page that links to the relevant PEPs that introduced the `match` statement, and/or the Python docs for the `match` statement.

_Originally posted by @AlexWaygood in https://github.com/astral-sh/ruff/pull/16090#discussion_r1969810589_
            

---

_Referenced in [astral-sh/ruff#16090](../../astral-sh/ruff/pulls/16090.md) on 2025-02-25 17:29_

---

_Label `documentation` added by @ntBre on 2025-02-25 17:29_

---

_Comment by @AlexWaygood on 2025-02-25 17:35_

Thanks for opening the tracking issue! As I said in https://github.com/astral-sh/ruff/pull/16090#discussion_r1969858221, it's pretty important for me that we figure out a way to provide good documentation for the more complex errors you'll be tackling in this project, as it can sometimes be pretty subtle when and why something is a syntax error. I don't think we should stabilise these new errors before we have solid docs for the more complicated ones.

---
