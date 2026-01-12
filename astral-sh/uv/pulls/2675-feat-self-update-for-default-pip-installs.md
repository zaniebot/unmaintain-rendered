```yaml
number: 2675
title: "feat: self-update for default pip installs"
type: pull_request
state: closed
author: bschoenmaeckers
labels: []
assignees: []
base: main
head: self-update-pip
created_at: 2024-03-26T20:03:41Z
updated_at: 2024-04-04T16:27:52Z
url: https://github.com/astral-sh/uv/pull/2675
synced_at: 2026-01-12T16:05:10Z
```

# feat: self-update for default pip installs

---

_@bschoenmaeckers_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This is a proof of concept for updating uv installations that were installed using the system default pip. e.g. `pip install uv`.

Code is a little rough right now and is not implemented for pipx installations. I wanted to first discuses this implementation before going forward. So thorough review is appreciated!

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

I was unsure how to make a unit test for this but currently tested using the following commands:

```shell
pip install .
uv self update
```

<!-- How was it tested? -->


---

_Comment by @bschoenmaeckers on 2024-03-27 09:05_

Possible related to #2338 and #2386

---

_Marked ready for review by @bschoenmaeckers on 2024-04-04 15:41_

---

_Comment by @konstin on 2024-04-04 16:27_

Hi, and sorry for not looking at this earlier! In general, we don't want to self-modify uv in an environment that is not managed by us. If you install uv with e.g. poetry, poetry has a lockfile with the version of uv to be installed and we should not self-update and create a mismatch with the lockfile. For e.g. `pipx install uv`, pipx has it's own upgrade mechanism and we shouldn't interfere with that. Thanks for contributing, but i'm afraid it's not something we can merge.

---

_Closed by @konstin on 2024-04-04 16:27_

---
