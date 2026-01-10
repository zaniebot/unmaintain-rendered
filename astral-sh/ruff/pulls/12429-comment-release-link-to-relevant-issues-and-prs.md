```yaml
number: 12429
title: Comment release link to relevant issues and PRs
type: pull_request
state: closed
author: ddelange
labels: []
assignees: []
base: main
head: patch-1
created_at: 2024-07-21T13:23:22Z
updated_at: 2024-07-22T13:25:47Z
url: https://github.com/astral-sh/ruff/pull/12429
synced_at: 2026-01-10T21:47:02Z
```

# Comment release link to relevant issues and PRs

---

_Pull request opened by @ddelange on 2024-07-21 13:23_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Comment release link to relevant issues and PRs when a new Github (pre)release is created.

The action step sends out comments like [this](https://github.com/ddelange/mapply/pull/67#issuecomment-1976854659) on PRs and on the issues they `fix`
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

CI-only change
<!-- How was it tested? -->


---

_Comment by @AlexWaygood on 2024-07-22 13:25_

Hey, thanks for the PR! This looks like it could be really useful for some projects.

For us, I think it might be less useful than some other projects, however, so we'll decline this PR:
- We have a pretty frequent release schedule, so contributors won't usually have to wait more than a few days before their feature that's been merged into `main` is out -- keeping track of whether a feature has been released or not isn't usually too difficult
- We use GitHub releases, so it's fairly easy to track when we publish releases
- As maintainers, we already get lots of notifications from this repo, and we're worried that this make our inboxes even harder to deal with :-)

Thanks again for the PR, though, we really appreciate the contribution!

---

_Closed by @AlexWaygood on 2024-07-22 13:25_

---
