```yaml
number: 9023
title: "Fix typo, explictly -> explicitly"
type: pull_request
state: closed
author: kianmeng
labels: []
assignees: []
base: main
head: fix-typo
created_at: 2024-11-11T18:19:36Z
updated_at: 2024-11-11T20:40:32Z
url: https://github.com/astral-sh/uv/pull/9023
synced_at: 2026-01-10T12:00:00Z
```

# Fix typo, explictly -> explicitly

---

_Pull request opened by @kianmeng on 2024-11-11 18:19_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Found via `codespell -L
crate,ser,astroid,borken,connexion,uptodate,formattings,hass,incomfort,nam`

## Test Plan

<!-- How was it tested? -->


---

_@zanieb approved on 2024-11-11 18:42_

I'm a little so-so on this though, this is a file copied from another repository for testing.

cc @BurntSushi 

---

_Comment by @BurntSushi on 2024-11-11 19:05_

Yeah I'd rather we whitelist these files, but I don't feel strongly.

---

_Comment by @zanieb on 2024-11-11 20:40_

I think that we shouldn't apply this then.

Thanks for taking the time to contribute though!

---

_Closed by @zanieb on 2024-11-11 20:40_

---
