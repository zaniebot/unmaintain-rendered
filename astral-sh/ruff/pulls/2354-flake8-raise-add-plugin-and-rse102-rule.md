```yaml
number: 2354
title: "[`flake8-raise`] Add Plugin and `RSE102` Rule"
type: pull_request
state: merged
author: saadmk11
labels:
  - rule
  - plugin
assignees: []
merged: true
base: main
head: flake8-raise
created_at: 2023-01-30T14:15:49Z
updated_at: 2023-02-01T03:31:54Z
url: https://github.com/astral-sh/ruff/pull/2354
synced_at: 2026-01-12T04:52:00Z
```

# [`flake8-raise`] Add Plugin and `RSE102` Rule

---

_Pull request opened by @saadmk11 on 2023-01-30 14:15_

closes https://github.com/charliermarsh/ruff/issues/2347

---

_Comment by @charliermarsh on 2023-01-30 15:08_

I'm tempted to put this in `tryceratops` rather than introduce a new category. But we don't really have any precedent for doing that. I need to think on it.

---

_Comment by @ngnpope on 2023-01-30 16:10_

@charliermarsh I thought the same... It's a tricky one.

I guess in the fullness of time it might make sense to not group rules in `ruff` by the `flake8` plugin they came from, but have a "lookup" for each plugin that states what is implemented and what rule it maps to in `ruff`, e.g.

```
flake8-raise:R100 → ruff:reraise-no-cause
flake8-raise:R101 → ruff:verbose-raise
flake8-raise:R102 → ruff:unnecessary-paren-on-raise-exception
tryceratops:TC200 → ruff:reraise-no-cause
tryceratops:TC201 → ruff:verbose-raise
```

And any rules that are intentionally not implemented, and the reason for their omission, can also be explained.

In a sense, the name of the plugin only feels relevant while transitioning to `ruff`. The main benefit is to have a comprehensive compatibility mapping[^1] to help people migrate and understand what has become of the rules they've been using. I guess that ties in somewhat with the discussions in #1773. (It would also be good to review those friendly names with respect to each other to ensure they make sense where they've been implemented in little islands before, e.g. `reraise-no-cause` might be better as `reraise-without-from`, `verbose-raise` might be better as `reraise-redundant-argument`, and `unnecessary-paren-on-raise-exception` could be `raise-with-empty-parentheses`?

[^1]: Might see if I can get around to this by dredging the issue tracker...

---

_Comment by @ngnpope on 2023-01-30 16:11_

> But we don't really have any precedent for doing that.

I think this furthers my point about:

> In a sense, the name of the plugin only feels relevant while transitioning to ruff.

---

_Comment by @charliermarsh on 2023-01-30 17:20_

@ngnpope - Yeah this all makes sense. I've written about it a bit before, mostly [here](https://github.com/charliermarsh/ruff/issues/1992#issuecomment-1405497643), but we'll eventually move towards a Ruff-first API that looks less like a Flake8 compatibility layer (while still retaining that layer). In that world, all of those rules can be categorized under the same `exceptions` category (or whatever's appropriate) rather than being split up by Flake8 plugins.

Soon, we're also going to enable a many-to-many mapping between these Flake8-like codes and the actual Ruff rules. So we'll be able to point both `TRY200` and `R100` to the same underlying rule, which would help too.


---

_Comment by @charliermarsh on 2023-01-30 17:20_

I think the right thing to do here, for consistency, is to add this as `flake8-raise`, and then we can alias the `R100` and `R101` rules once aliasing is up and running.

However -- we should use a different prefix, as `R` is too generic. `RSE` or `RAI` would be reasonable to me?


---

_Comment by @ngnpope on 2023-01-30 17:22_

Agreed on switching the prefix. Either would work, but I guess `RSE` is potentially less likely to conflict with something else?

---

_Renamed from "[`flake8-raise`] Add Plugin and `R102` Rule" to "[`flake8-raise`] Add Plugin and `RSE102` Rule" by @saadmk11 on 2023-01-31 11:21_

---

_Comment by @saadmk11 on 2023-01-31 11:22_

> However -- we should use a different prefix, as `R` is too generic. `RSE` or `RAI` would be reasonable to me?

`RSE` sounds good to me. Updated the PR with it. :)



---

_Label `rule` added by @charliermarsh on 2023-01-31 23:09_

---

_Label `plugin` added by @charliermarsh on 2023-01-31 23:09_

---

_Merged by @charliermarsh on 2023-01-31 23:09_

---

_Closed by @charliermarsh on 2023-01-31 23:09_

---

_Branch deleted on 2023-02-01 03:31_

---
