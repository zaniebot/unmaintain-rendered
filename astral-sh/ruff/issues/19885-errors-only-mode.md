```yaml
number: 19885
title: "\"errors-only\" mode"
type: issue
state: closed
author: ctcjab
labels:
  - question
  - configuration
assignees: []
created_at: 2025-08-12T20:16:11Z
updated_at: 2025-11-10T15:19:49Z
url: https://github.com/astral-sh/ruff/issues/19885
synced_at: 2026-01-12T15:54:57Z
```

# "errors-only" mode

---

_@ctcjab_

# Use case
Our company has a large monorepo of Python notebooks* with varying levels of code quality (a lot of it was written by quantitative researchers and other folks who aren't professional software engineers), where type hints and automated tests are practically nonexistent, and this code is nonetheless used in production.

We need a tool we can run in CI, ideally without having to maintain our own complex configuration, that is designed to catch only issues that will result in crashes at runtime, and obvious logic bugs, rather than code style issues (especially not flagging any issues that make no difference to the result that the code produces, which would only frustrate this type of user), without requiring users to first start adding type hints or any other major changes to the type of code they write.

(*) We use Jupytext to store notebooks as .py files, so we don't care if the tool doesn't support .ipynb files. Given you're already tracking #8800, I'm only mentioning "notebooks" here to paint a better picture of the user persona and the environment they work in.

# Prior Art
`pylint` supports an `--errors-only` option, which seems designed for exactly this use case. Initial testing on a small subset of the monorepo yielded few false positives as well as some true positives, which is exactly what we are aiming for.

But unfortunately pylint is slow, works best when installed in the same Python environment as the code it's linting (so it can follow imports), and (since it's implemented in Python and has its own Python dependencies) pulls in a lot of transitive dependency constraints, which is very undesired.

# Proposal
What if `ruff check` supported a single, simple `--errors-only` option or something like this, which changed its default behavior to only look for issues that will almost definitely cause crashes or other serious issues at runtime?

Thanks for your consideration and for the great work on ruff ❤️ 

---

_Comment by @ntBre on 2025-08-12 21:07_

Thanks for the kind words and the clear write-up!

I believe this overlaps heavily with #1774, which is our issue tracking rule recategorization. This sounds like an `error` or `correctness` category, which is how I think we'll handle this eventually.

There's also https://github.com/astral-sh/ruff/issues/11415, another related issue about default categories. This is definitely on our radar, but it's a big project and we want to be careful to get it right.

---

_Label `configuration` added by @ntBre on 2025-08-12 21:07_

---

_Label `question` added by @MichaReiser on 2025-08-13 06:41_

---

_Comment by @MichaReiser on 2025-08-13 06:43_

As @ntBre said, this is on our mind, but there's no easy way to select error-only rules right now. However, Ruff's default rules is intentionally unopinionated. That's probably a good start for the notebook case. 

But you'll still need different configuratinos to enable the stricter checking for the non notebook cases.

---

_Comment by @ctcjab on 2025-08-13 14:51_

Thanks for the info and great to hear you're thinking about this already!

> Ruff's default rules is intentionally unopinionated. That's probably a good start for the notebook case.

So far my anecdata suggests otherwise (for whatever it's worth). If anything, ruff's default rules seem too strict for the notebook use case:

I ran `ruff check` with no custom configuration on a small subset of our monorepo, and all the issues it caught would be considered annoyingly opinionated and too unimportant to prioritize by these users:

| Issue                                                                              | Occurrence Count |
|------------------------------------------------------------------------------------|------------------|
| F401 imported but unused                                                           | 38               |
| E402 Module level import not at top of file                                        | 28               |
| I001 Import block is un-sorted or un-formatted                                     | 27               |
| F811 Redefinition of unused ... from line ...                                      | 11               |
| E722 Do not use bare `except`                                                      | 6                |
| F841 Local variable ... is assigned to but never used                              | 5                |
| E711 Comparison to `None` should be `cond is None`                                 | 1                |
| E712 Avoid equality comparisons to `True`; use `x:` for truth checks               | 1                |
| E721 Use `is` and `is not` for type comparisons, or `isinstance()` for checks      | 1                |
| F403 `from ... import *` used; unable to detect undefined names                    | 1                |

By contrast, running `pylint --errors-only` with no custom config on the same code is much closer to something these users would put up with (only one false positive that I'd have to account for in custom config, and only one issue that might annoy these users):
* E0611: No name 'display' in module 'IPython.core.display' (no-name-in-module) -- false positive, if this had been caught by ruff would it eventually be addressed by #8800?
* E1111: Assigning result of a function call, where the function has no return (assignment-from-no-return) -- user assigned the result to a variable out of habit, but then never referred to it again anyway, would consider this "harmless"

Hope this provides some additional helpful context!

---

_Comment by @ntBre on 2025-08-13 15:18_

Ah thank you! I think some of these rules would be suppressed in a regular notebook, so the Jupytext issue is very relevant. In particular, E402 is modified to check "import not at top of cell," and I'm not seeing any F841 errors in a notebook. F401 still seems to be enforced, though.

---

_Comment by @ctcjab on 2025-08-13 19:03_

> E402 is modified to check "import not at top of cell," and I'm not seeing any F841 errors in a notebook. F401 still seems to be enforced, though.

Thanks for checking this, very interesting! So ruff implicitly changes rule selection and behavior based on the .ipynb vs. .py file extension? And #8800 would bring the behavior for jupytext .py notebooks in line with with the behavior for .ipynb files?

FWIW, most of these users would probably find even the weaker variants of the style-related rules undesired (e.g. the modified E402 to check "import not at top of cell" rather than "top of file"). But happily ruff already supports hierarchical configuration, so even if we had a `ruff.toml` at the monorepo root that set some new `errors-only = true` setting, any more linter-positive teams could override that with their own `ruff.toml`s in their subdirectories.

---

_Comment by @ntBre on 2025-08-13 21:35_

Sorry, I confused myself slightly with F841. I always forget that it only applies to function-scoped variables, but I was testing in a top-level notebook cell. But yes, the E402 behavior in notebooks is [documented](https://docs.astral.sh/ruff/rules/module-import-not-at-top-of-file/#notebook-behavior) at least, so some rules (at least one!) do differ for notebooks.

I understand what you mean, though. I suspected that top-of-cell wouldn't fully help anyway.

---

_Closed by @MichaReiser on 2025-11-06 13:29_

---

_Comment by @ctcjab on 2025-11-06 17:05_

Was this marked completed by accident? I checked recent release notes and linked tickets and didn't see any associated progress.

---

_Comment by @ntBre on 2025-11-10 14:26_

It might have been closed because we don't have immediate plans to work on this besides by introducing new rule categories and improving our default rule selection, but @MichaReiser may be able to confirm.

---

_Comment by @MichaReiser on 2025-11-10 15:19_

I closed it because I considere the question to be answered and the "solution" is rule categorization, which we track separately

---
