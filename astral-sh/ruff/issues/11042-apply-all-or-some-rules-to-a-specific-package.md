```yaml
number: 11042
title: Apply all, or some rules, to a specific package/folder
type: issue
state: closed
author: ggravlingen
labels:
  - needs-info
assignees: []
created_at: 2024-04-19T13:56:32Z
updated_at: 2024-04-24T12:49:40Z
url: https://github.com/astral-sh/ruff/issues/11042
synced_at: 2026-01-12T15:54:50Z
```

# Apply all, or some rules, to a specific package/folder

---

_@ggravlingen_

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

I'm eager on implementing all ruff rules over an entire codebase. Setting `lint.select=[ALL]` renders a lot of error messages. My idea was then to force all ruff rules on any new folder that I create in the codebase. I've used a similar strategy when rolling out mypy.

This is a snippet of my `pyproject.toml`:

```
target-version = "py311"

lint.select = [
    "D",  # docstrings
]

[tool.ruff.lint.per-file-ignores]
# Enforce this rule for tests
"!src/**/tests/*.py" = [
    "ANN201"  # Missing return type annotation for public function
]
# Enforce all rules for the analytics folder
"!src/analytics/**/*.py" = [
    "ALL"
]
```

I've added a line `# TODO:fix` in `src/analytics/chart/base.py, thinking that would violate `TD007`. Running `ruff check src`, however, yields `All checks passed!`

Is it possible to use `rule` in the way outlined above?

--

Edit after the comment from @carljm below: added the `!`before `src/analytics/**/*.py`.

---

_Comment by @carljm on 2024-04-19 17:08_

The config as you've written it says that you want to ignore all rules in `src/analytics/**/*.py`, so it's not surprising that you don't see lints in those files!

It looks like the feature you're wanting here is `per-file-selects`, which we don't currently have.

---

_Comment by @ggravlingen on 2024-04-19 17:15_

@carljm do'h, I see that I copied the config from when I was doing some experiments. The config in the initial post is now reflecting what I have.

It works to apply single rules as in `!src/**/tests/*.py`. How come I currently can't apply all?

---

_Comment by @carljm on 2024-04-19 17:39_

> It works to apply single rules as in !src/**/tests/*.py

It shouldn't, and in my local testing it doesn't. So I'm still not sure that the shown config here matches your actual config: if you are seeing `ANN201` fire, you must have it included in a `select` or `extend-select` somewhere.

`per-file-ignores` can only ignore rules, it can never select rules. This is equally true for both negative patterns (those starting with `!`) and positive patterns.

So if you only `lint.select` the `"D"` rules, then no matter what you put in `per-file-ignores` you will never see any rules fire that are not `"D"` rules.

If this is not the behavior you're seeing, it would be great if you could provide a complete minimal example!

Your `lint.select` should select all the rules that you want enabled anywhere, and then you can use `per-file-ignores` to pare that down. But unfortunately this is difficult to do with such a broad brush as `ALL`; if you ignore `ALL` outside of `src/analytics/`, then that really means ignoring all, there's no way to selectively enable certain rules outside of `src/analytics`. Unless your ignore pattern exhaustively lists only the rules you actually want to ignore everywhere outside `src/analytics`, and not the ones you do want elsewhere.

To make this pattern less cumbersome, we would need `per-file-selects`.

---

_Comment by @ggravlingen on 2024-04-19 17:52_

@carljm thank you for the detailed response, which leads me to believe that I have misunderstood how that negative pattern works. Please let me check config file when I'm back at work again on Monday so that I can try to reproduce a working example, or realize that the config is incorrect.

---

_Label `needs-info` added by @dhruvmanila on 2024-04-22 09:49_

---

_Comment by @ggravlingen on 2024-04-24 12:30_

I have tried to reproduce what I believe I did when adding my rule for tests only but, perhaps not surprising, without any success. Closing this issue.

Apologies for the noise!

---

_Closed by @ggravlingen on 2024-04-24 12:30_

---

_Comment by @carljm on 2024-04-24 12:49_

No worries! Thank you for reporting issues. 

---
