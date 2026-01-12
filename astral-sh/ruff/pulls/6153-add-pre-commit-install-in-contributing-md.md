```yaml
number: 6153
title: "Add `pre-commit install` in `CONTRIBUTING.md`"
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: add-pre-committ-install
created_at: 2023-07-28T14:08:21Z
updated_at: 2023-08-01T14:28:51Z
url: https://github.com/astral-sh/ruff/pull/6153
synced_at: 2026-01-12T02:58:30Z
```

# Add `pre-commit install` in `CONTRIBUTING.md`

---

_Pull request opened by @harupy on 2023-07-28 14:08_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

To enable the git hooks, `pre-commit install` needs to be executed.

## Test Plan

<!-- How was it tested? -->

N/A


---

_@MichaReiser reviewed on 2023-07-28 16:11_

---

_Review comment by @MichaReiser on `CONTRIBUTING.md`:76 on 2023-07-28 16:11_

I believe the reason why it is not mentioned is because using pre-commit depends on personal preference. I prefer *not* to use pre-commit because I want to be able to commit intermediate states, even if they violate some pre-commit checks. I also find it important that commits are instant, I don't want to wait for slow checks because it interrupts my development workflow. 

However, I find it useful to point out the possibility of using and installing pre-commit in the contributing.md as you did. It may be worth to add a comment making it clear that this step is optional and only necessary if you want to run the checks on every commit. 

---

_@harupy reviewed on 2023-07-28 16:35_

---

_Review comment by @harupy on `CONTRIBUTING.md`:76 on 2023-07-28 16:35_

Thanks for the comment :) Adding a comment sounds good. 

---

_@JonathanPlasse reviewed on 2023-07-28 16:56_

---

_Review comment by @JonathanPlasse on `CONTRIBUTING.md`:76 on 2023-07-28 16:56_

Maybe, suggest using `pre-push` instead of `pre-commit`. Like this, commit are instant, only when you want to push will it run pre-commit and avoid running CI for invalid code.

---

_@zanieb approved on 2023-07-28 22:31_

---

_@konstin reviewed on 2023-07-31 14:14_

---

_Review comment by @konstin on `CONTRIBUTING.md`:76 on 2023-07-31 14:14_

You kinda do need `pipx install pre-commit`, e.g. for `pre-commit run --all-files`, so i'd split this blog into two sections: One for `pipx install pre-commit` and one for the optional `pre-commit install`

---

_@konstin approved on 2023-07-31 16:34_

---

_Merged by @zanieb on 2023-08-01 14:28_

---

_Closed by @zanieb on 2023-08-01 14:28_

---
