```yaml
number: 8631
title: Indicated Successful Check
type: pull_request
state: merged
author: vinhocent
labels:
  - cli
assignees: []
merged: true
base: main
head: tri/8553/successfulcheck
created_at: 2023-11-12T18:00:18Z
updated_at: 2024-03-27T13:57:49Z
url: https://github.com/astral-sh/ruff/pull/8631
synced_at: 2026-01-12T15:55:26Z
```

# Indicated Successful Check

---

_@vinhocent_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Adds a successful check message after no errors were found 
Implements #8553 

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

Ran a check on a test file with `cargo run -p ruff_cli -- check test.py --no-cache` and outputted as expected.

Ran the same check with `cargo run -p ruff_cli -- check test.py --no-cache --silent` and the command was gone as expected.

<!-- How was it tested? -->


---

_Comment by @vinhocent on 2023-11-12 18:14_

I'm wondering how to add the counting for the number of files checked - I see that it lives in `check.rs` here but this stat isn't available in diagnostics, is it?

![image](https://github.com/astral-sh/ruff/assets/17696462/4efa256b-79e1-4770-9d8d-4814a687910e)


---

_Comment by @vinhocent on 2023-11-12 18:29_

Also in the case of tests like this:
![image](https://github.com/astral-sh/ruff/assets/17696462/26aae658-b3b9-4b53-9cb9-51383f52c316)


Do we even want to output a successful check?

---

_Comment by @konstin on 2023-11-13 11:09_

No, we should fail there but currently don't 

---

_Review requested from @zanieb by @konstin on 2023-11-13 11:09_

---

_Comment by @nunokaeru on 2023-12-17 11:22_

This might cause issues with `pre-commit` since that framework uses "any message at the output" as error. Or bash pre-commit hooks.

---

_Comment by @hughhan1 on 2024-01-13 16:51_

> No, we should fail there but currently don't

@konstin, by "fail", do you specifically mean that we should be exiting with a non-zero exit code? And perhaps continuing to have nothing dumped to `stdout`?

---

_Comment by @konstin on 2024-01-18 09:55_

> by "fail", do you specifically mean that we should be exiting with a non-zero exit code? 

yes

---

_Assigned to @zanieb by @zanieb on 2024-03-02 17:39_

---

_@zanieb approved on 2024-03-11 18:56_

@konstin this seems very simple, my only thought is maybe we should have a different message that's in a different format so it's obviously distinct e.g. "No errors found" (I don't love exact message that since it sounds like an error)

@charliermarsh any ideas?

I'd also like to say how many files we scanned e.g. "Found 0 errors in 125 files" would be helpful to know it actually scanned something. We can track that separately.

---

_Comment by @konstin on 2024-03-11 20:02_

Slight preference for something like "All checks passed" over something that contains the word error, otherwise i'm on board.

---

_Comment by @zanieb on 2024-03-11 21:28_

I like "All checks passed" although it's a little outside our normal terminology

---

_Label `cli` added by @zanieb on 2024-03-13 18:59_

---

_Merged by @zanieb on 2024-03-13 19:07_

---

_Closed by @zanieb on 2024-03-13 19:07_

---

_Comment by @github-actions[bot] on 2024-03-13 19:12_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @zanieb on 2024-03-21 18:23_

I'm not sure what to say here. A configuration option for this seems excessive and it is significantly friendlier for Ruff to indicate that it _did_ something. If you want to open an issue requesting a configuration option, we can consider it. I'd want to see significant community feedback that this is important first though.

---

_Comment by @silverwind on 2024-03-21 18:26_

Sorry, I figured out that `-q` actually works as expected and suppresses this logging just fine, so I deleted my rant. So nothing to do and sorry for the noise :)

---

_Comment by @zanieb on 2024-03-21 18:51_

Oh great! :)

---

_Comment by @alexeagle on 2024-03-27 13:18_

> that framework uses "any message at the output" as error

https://github.com/aspect-build/rules_lint/blob/main/docs/lint_test.md is one such tool. It is used by Bazel users to assert "no warnings were produced by the tool". So this change broke our tests. From what I can tell, ruff is the only linter we integrate that prints on success.

> issue requesting a configuration option, we can consider it. I'd want to see significant community feedback that this is important

Here's some other linter decisions on this:
- https://github.com/golangci/golangci-lint/issues/121 - always be silent following https://www.linfo.org/rule_of_silence.html
- https://github.com/eslint/eslint/issues/3495#issuecomment-133820141 - eslint is incredibly widely adopted and the person commenting here is one of the most prolific and respected developers in the Node.js ecosystem.
- From the (still open?) issue requesting this change, the author observes
  "On the other hand, several linters are silent if they find nothing (pycodestyle, flake8, isort, etc.)."

So I think rather than add an option, ruff should go back to the old silent-on-success behavior.


---

_Comment by @MichaReiser on 2024-03-27 13:20_

@alexeagle would you mind creating a new issue for this to avoid having discussions on merged PRs. Thank you

I wonder if it would be sufficient to test if Ruff's in an interactive terminal and only then print the message.

---

_Comment by @alexeagle on 2024-03-27 13:28_

@MichaReiser thanks for the quick reply, since the issue requesting this change is still open, I've just commented there: https://github.com/astral-sh/ruff/issues/8553#issuecomment-2022771565

---

_Comment by @zanieb on 2024-03-27 13:57_

Thanks for moving the discussion!

---
