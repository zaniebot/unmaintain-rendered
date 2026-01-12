```yaml
number: 10461
title: "[`flake8-pytest-style`] Add automatic fix for `pytest-parametrize-values-wrong-type` (`PT007`)"
type: pull_request
state: merged
author: autinerd
labels:
  - fixes
assignees: []
merged: true
base: main
head: ruff-pt007-fixes
created_at: 2024-03-18T17:58:12Z
updated_at: 2024-03-18T20:35:23Z
url: https://github.com/astral-sh/ruff/pull/10461
synced_at: 2026-01-12T15:55:32Z
```

# [`flake8-pytest-style`] Add automatic fix for `pytest-parametrize-values-wrong-type` (`PT007`)

---

_@autinerd_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This adds automatic fixes for the `PT007` rule.

I am currently reviewing and adding Ruff rules to Home Assistant. One rule is PT007, which has multiple hundred occurrences in the codebase, but no automatic fix, and this is not fun to do manually, especially because using Regexes are not really possible with this.

My knowledge of the Ruff codebase and Rust in general is not good and this is my first PR here, so I hope it is not too bad.

One thing where I need help is: How can I have the transformed code to be formatted automatically, instead of it being minimized as it does it now?

## Test Plan

Using the existing fixtures and updated snapshots.


---

_Review requested from @charliermarsh by @charliermarsh on 2024-03-18 18:00_

---

_Label `fixes` added by @charliermarsh on 2024-03-18 18:01_

---

_Comment by @charliermarsh on 2024-03-18 18:08_

Thanks for this!

> One thing where I need help is: How can I have the transformed code to be formatted automatically, instead of it being minimized as it does it now?

So, if you want to do this, you can't use `generator`. The generator is just a basic printer that takes an AST and prints it without formatting. If you want to preserve the existing formatting (including comments), you need to implement a more manual fix (e.g., change the `[` to `(` and the `]` to `)`, or similar). Check out `unnecessary_generator_set` for an example of what that might look like.

I'm fine to merge with the generator-based fix, though a locator-based fix tends to preserve more of this kind of trivia. Are you interested in trying it?

---

_Comment by @autinerd on 2024-03-18 18:15_

Oh yes, I see it now that this approach deletes all comments. Thanks for pointing me to a place to check, I will try if I can do this.

---

_Comment by @charliermarsh on 2024-03-18 18:15_

Yeah it's kind of a tradeoff. The generator is much easier (to program, and to get right), but much lower-fidelity.

---

_Comment by @github-actions[bot] on 2024-03-18 18:16_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @autinerd on 2024-03-18 18:59_

So, I have it now as a locator-based fix based of `unnecessary_generator_set`, thanks!

It also checks if a list has only 1 element and transforms `["test"]` into `("test",)` to make it a valid tuple.

The only thing is that `("test",)` will be converted into `["test",]`, which is valid from the syntax, but of course not the most beautiful.

---

_Comment by @autinerd on 2024-03-18 19:06_

And I have it now as a unsafe fix, but I think it should be safe to use in this approach.

---

_Comment by @autinerd on 2024-03-18 19:12_

Oh, ruff-format seems to transform `["test",]` unconditionally into
```
[
    "test",
]
```

Then maybe I need to remove the comma, because this disrupts the source formatting.

---

_Comment by @charliermarsh on 2024-03-18 19:13_

You could, e.g., _only_ remove the trailing comma if it immediately precedes the closing brace.

---

_Comment by @autinerd on 2024-03-18 19:29_

~I'm sorry, but can you maybe hint me on how to access the text of the `Expr` to that I can check on whether it ends with `,)`?~

Ah, I'm blind, I see it now.

---

_Comment by @autinerd on 2024-03-18 20:06_

It doesn't look very elegant in my opinion, but it works.

---

_Comment by @charliermarsh on 2024-03-18 20:07_

Thanks! I'll take a look now. I may tweak the locator usage a bit but it's generally what I was suggesting.

---

_@charliermarsh approved on 2024-03-18 20:21_

---

_@charliermarsh reviewed on 2024-03-18 20:21_

---

_Review comment by @charliermarsh on `crates/ruff_linter/resources/test/fixtures/flake8_pytest_style/PT007.py`:82 on 2024-03-18 20:21_

Just one bugfix: the PR was adding a trailing comma here, which led to `,,`

---

_Renamed from "Add automatic fixes for PT007" to "[`flake8-pytest-style`] Add automatic fix for `pytest-parametrize-values-wrong-type` (`PT007`)" by @charliermarsh on 2024-03-18 20:22_

---

_Merged by @charliermarsh on 2024-03-18 20:28_

---

_Closed by @charliermarsh on 2024-03-18 20:28_

---
