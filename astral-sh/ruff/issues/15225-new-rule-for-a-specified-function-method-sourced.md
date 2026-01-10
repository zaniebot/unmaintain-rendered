---
number: 15225
title: "New rule: For a specified function/method sourced from a specified module, make a specified non-required argument mandatory as a keyword argument."
type: issue
state: open
author: andyscho
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-01-02T18:12:00Z
updated_at: 2025-02-16T08:12:34Z
url: https://github.com/astral-sh/ruff/issues/15225
synced_at: 2026-01-10T01:22:56Z
---

# New rule: For a specified function/method sourced from a specified module, make a specified non-required argument mandatory as a keyword argument.

---

_Issue opened by @andyscho on 2025-01-02 18:12_

# Rule

For a user-configured (module, function, argument name), enforce that a non-required argument gets supplied. For instance, if a user configures the `validate` argument in `pandas.merge` to always be mandatory, the rule would flag if someone passes `pandas.merge(df1, df2)` without the `validate` argument.

# Example Use Case / Motivation

Making the `validate` argument for [pandas.merge](https://pandas.pydata.org/docs/reference/api/pandas.merge.html) functions/methods mandatory.

Some reasoning from [this post](https://medium.com/@akarabaev96/pandas-merge-best-practices-stop-wasting-your-time-on-debugging-the-merge-operations-72778845d9b7):

> Of course, many of you can say that the source problem is the duplication in the merge keys itself and it is better simply to check for duplication before performing any merge. However, this is something that we can forget to do and it is better to ALWAYS add parameter validateto the pandas.merge , since it is just 2 words to add to your code. Of course, you need to specify the validation type (“one_to_one”, “one_to_many”, etc), however, I always go with “one_to_one” as a default option, and when it raises an exception, only then I start thinking that maybe I need to change validation type

This particular use case could be hardcoded as a new rule in pandas-vet, but it would be nice to do something more general since the core functionality seems clearly useful when dealing with various third-party libraries. Having it as a ruff rule configuration entry seems nicer than wrapping third-party method/function calls to make the arguments mandatory whenever you'd want to do this.

# Configuration

I am envisioning something like the [banned-api configuration](https://docs.astral.sh/ruff/settings/#lint_flake8-tidy-imports_banned-api), allowing for specifying (1) a module qualified name base import path, (2) the name of a function, and (3) arguments to be enforced to be present.

# Implementation

I am envisioning something like [pandas-use-of-inplace-argument](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pandas_vet/rules/inplace_argument.rs), but using the configured module names, function names, and argument names instead of `"pandas"`, `"drop"`, and `"inplace"`, and then just enforcing that the argument is present in the keywords.

---

_Label `rule` added by @dhruvmanila on 2025-01-03 02:50_

---

_Label `needs-decision` added by @dhruvmanila on 2025-01-03 02:50_

---

_Referenced in [astral-sh/ruff#16179](../../astral-sh/ruff/pulls/16179.md) on 2025-02-16 08:02_

---

_Comment by @andyscho on 2025-02-16 08:05_

Although I made a quick and very rough/not correct draft that handles a few scenarios in #16179, this issue should likely have the `type-inference` label. It should also plausibly wait for #1774.

---
