```yaml
number: 8404
title: "Usage of extend-unsafe-fixes = [\"ALL\"] option"
type: issue
state: closed
author: lukaspiatkowski
labels:
  - configuration
assignees: []
created_at: 2023-11-01T14:01:33Z
updated_at: 2023-11-07T16:33:42Z
url: https://github.com/astral-sh/ruff/issues/8404
synced_at: 2026-01-12T15:54:48Z
```

# Usage of extend-unsafe-fixes = ["ALL"] option

---

_@lukaspiatkowski_

This is more of a feature request than a bug. I would like to configure Ruff in a manner that I can apply different set of fixes depending on the context when it is invoked.
Some fixes that ruff has are considered to be "safe" since they don't change the semantic of the code, but they might indicate a typo or a mistake done by the author of code, e.g. the `B033 - Sets should not contain duplicate item {value}` might just mean that the author mistyped the value they wanted to the set. For those set of rules I would like them to require a manual call to `ruff`. Lets call them "manual" fixes for now.
Other are quite straightforward (like `I001 - Import block is un-sorted or un-formatted`), those I would like to be applied by a pre-commit hook automatically even without code authors knowledge. Lets call them "automatic" fixes for now.
An additional feature would be for the Ruff integration with IDE work as expected (at least the VSCode one) - when format-on-save action is invoked the "automatic" rules are applied, but not the "manual" ones. It would be nice though to be still able to fix the "manual" ones with a Quick Fix action.

There are two solutions I was thinking of how I could achieve this:
* Use `fixable/extend-fixable` configs to set it to include only the "automatic" fixes. In order to call the "manual" fixes `ruff` will have to be called with `--fixable ALL`. The disadvantage of this is that IDE doesn't pick up the Quick Fix action for "manual" fixes as they are unfixable.
* Use `extend-safe-fixes/extend-unsafe-fixes` to mark all "automatic" fixes as safe and "manual" fixes as unsafe. In order to call the "manual" fixes `ruff` will have to be called with `--unsafe-fixes` and the IDE integration works as expected. The problem with this approach is that it isn't easy to maintain the config. It would be ideal to write:
```
extend-unsafe-fixes = ["ALL"]
extend-safe-fixes = [
  "I001", # isort
  "F401", # remove unused imports
  "UP",   # pyupgrade
]
```
But this doesn't work on 0.1.3 - all fixes are marked as unsafe and the `extend-safe-fixes` is essentially ignored.

Could you please either reverse the precedence of those configs or make it smarter? E.g. more specialized "UP" overrides "ALL" and "F401" overrides "F"? It is unclear to me though how this would work with the `extend = "<other config>"` option that we are also a heavy user of.

---

_Comment by @lukaspiatkowski on 2023-11-01 14:20_

Another suggestion would be to add a `safe-fixes` config that when unset would default to the current behavior, but if set to `safe-fixes = ["ALL"]` or `safe-fixes = [ ]` would change the base to be all fixes are safe or no fixes are safe respectively. Then apply the `extend-*` configs on top of that. This matches the `select` and `extend-select` behavior.

---

_Comment by @zanieb on 2023-11-01 16:36_

Hm.. `extend-unsafe-fixes` takes precedence over `extend-safe-fixes` because we would rather err on the side of caution if someone uses both. We can update that to take into account the _specificity_ of the selectors you've provided though, that's how other configuration options work.

Are you interested in contributing? Otherwise I can take a look at it.

re `safe-fixes` I'd prefer not to add another configuration option yet.

---

_Label `configuration` added by @zanieb on 2023-11-01 16:36_

---

_Assigned to @zanieb by @zanieb on 2023-11-01 16:36_

---

_Comment by @lukaspiatkowski on 2023-11-02 10:31_

Sorry for not giving you an answer straight away @zanieb, I wasn't sure if I will have time to write the PR, but in the end I did.

---

_Closed by @zanieb on 2023-11-07 16:33_

---
