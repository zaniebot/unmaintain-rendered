```yaml
number: 8686
title: WIP Polars
type: pull_request
state: closed
author: MarcoGorelli
labels:
  - plugin
assignees: []
draft: true
base: main
head: polars
created_at: 2023-11-14T22:02:29Z
updated_at: 2023-12-17T15:34:16Z
url: https://github.com/astral-sh/ruff/pull/8686
synced_at: 2026-01-10T23:31:11Z
```

# WIP Polars

---

_Pull request opened by @MarcoGorelli on 2023-11-14 22:02_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

closes #7721 

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

work in progress, but I really like the Numpy rules, so I'm trying to do something in a similar vein

## Test Plan

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2023-11-15 14:33_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @charliermarsh on 2023-11-15 16:42_

Cool, this is great!

---

_Label `plugin` added by @charliermarsh on 2023-11-15 16:42_

---

_Comment by @MarcoGorelli on 2023-12-09 15:07_

Thanks!

I'm struggling a bit to figure out what might be in-scope, and what not

For the respective numpy rules, it looks like the changes are backwards compatible - `np.round_` gets autofixed to `np.round`, but both have been around for years, whereas the Polars deprecations here were only introduced quite recently and the deprecated names will be removed relatively soon - so, these fixes would only be useful for a brief amount of time

Still, it's been useful to learn a bit about Ruff

I think I'll pivot back towards the kinds of rules discussed in https://github.com/astral-sh/ruff/issues/7721, as those will likely be useful for the foreseeable future

Will try to update soon-ish!

---

_Comment by @MarcoGorelli on 2023-12-17 15:34_

closing this for now, but I'm giving a go at some polars rewrites in polars-upgrade, once that's a bit more upstream / battle-tested I'll what if might make sense to contribute here

---

_Closed by @MarcoGorelli on 2023-12-17 15:34_

---
