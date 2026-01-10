```yaml
number: 19236
title: "[`docs`] Clarify inheritance/overridding rules of Ruff settings (#18641)"
type: pull_request
state: open
author: pakal
labels:
  - documentation
assignees: []
base: main
head: update-configuration-docs
created_at: 2025-07-09T15:18:36Z
updated_at: 2025-10-01T15:11:49Z
url: https://github.com/astral-sh/ruff/pull/19236
synced_at: 2026-01-10T17:40:28Z
```

# [`docs`] Clarify inheritance/overridding rules of Ruff settings (#18641)

---

_Pull request opened by @pakal on 2025-07-09 15:18_

## Summary
As per https://github.com/astral-sh/ruff/issues/18641 , the goal is to clarify that default settings are not "replaced" at any moment by custom config files, they are always used as fallbacks ; only custom config files exclude each other (unless "extend" is used).


---

_Label `documentation` added by @ntBre on 2025-07-09 19:48_

---

_@MichaReiser reviewed on 2025-07-10 08:58_

---

_Review comment by @MichaReiser on `docs/configuration.md`:274 on 2025-07-10 08:58_

I find the new text slightly less clear because it omits what happens if no configuration file is found at all (including the user configuration)

---

_@pakal reviewed on 2025-07-15 10:13_

---

_Review comment by @pakal on `docs/configuration.md`:274 on 2025-07-15 10:13_

I found the "Ruff's default settings are equivalent to:" part sufficient on this, but indeed it can be good to restate it here, I'll tweak it asap

---

_Converted to draft by @MichaReiser on 2025-07-16 09:28_

---

_Comment by @pakal on 2025-07-21 09:09_

Added a comment about select/extend-select inheritance too, sence it can be confusing

---

_Marked ready for review by @pakal on 2025-07-21 09:09_

---

_@MichaReiser reviewed on 2025-07-28 12:34_

---

_Review comment by @MichaReiser on `docs/configuration.md`:307 on 2025-07-28 12:34_

sorry for my late response. I like the changes above but I think this part is a bit misleading. Whether `extend-select` and `select` are merged depends on how they're used:

* `ruff.toml`: Uses `select`
* `pyproject.toml` that extends `ruff.toml`: Uses `extend-select`. In this case, the settings are merged

But the settings aren't merged for
* `ruff.toml`: Uses `extend-select`
* `pyproject.toml` that extends `ruff.toml`: Uses `select`. 

Because `select` always overrides `extend-select`


---

_@pakal reviewed on 2025-07-28 15:25_

---

_Review comment by @pakal on `docs/configuration.md`:307 on 2025-07-28 15:25_

Ah, it's not what I was trying to say ; what I meant is that in a SAME CONFIG FILE, `select` and `extend-select` behave as if they were merged (into a full `select` setting) **BEFORE** they are inherited by the more specific config file (so the `extend-select` settings of ancestor config files disappear in the process).

Thus : 
- if the "(child) pyproject.toml that extends the (parent) ruff.toml" doesn't define these settings at alls, it behave as if it inherited the addition of both parent's settings ;
- if it defines only `select`, or both `select` and `extend-select`, this RESETS,  OVERRIDES the inherited full `select` and `extend-select` settings ;
- if it defines only `extend-select`, the final list of rules is the addition of add (parent's select + parent's extend-select + child's extend-select) ;

This is the behaviour I've observed, do we agree that it's how it was designed ?

If so, I have to find a clear way of expressing how the `extend-select` of each parent configuration is pre-merged into the total config, BEFORE the child config can continue to 'extend-select' (or, on the opposite, entirely override with a 'select') this rule selection.   

---

_@pakal reviewed on 2025-08-06 14:02_

---

_Review comment by @pakal on `docs/configuration.md`:307 on 2025-08-06 14:02_

I attempted a more detailed explanation of how select/extend-select behave when inherited from a parent config file, I hope it's clearer and not too verbose  :?

---

_Review comment by @MichaReiser on `docs/configuration.md`:310 on 2025-08-08 10:03_

Thanks for trying to clarify this. I'm afraid, I think it's more complicated than this.

The logic is here

https://github.com/astral-sh/ruff/blob/e917d309f1d37887a9f5ea11698389caa6c76b11/crates/ruff_workspace/src/configuration.rs#L907-L932

What that means is that Ruff merges `extend-selects` across file whereas `select` overrides any previous selection. 

* a: `select` -> Easy, the rule selection is what's in `select`
* b: extends `a`, `extend-select` -> Enables all rules in `select` and `extend-select`
* c: extends `b` select -> Only enables the rules in `c`'s `select`
* d: extends `c`: `extend-select` -> Enables the rules from `c`'s `select` and `d`'s extend select
* e: extends `d`: `extend-select`, Enables the rules form `c`'s `select` and `d`'s and `e`'s `extend-select`.


---

_@MichaReiser reviewed on 2025-08-08 10:03_

---

_@pakal reviewed on 2025-08-22 07:14_

---

_Review comment by @pakal on `docs/configuration.md`:310 on 2025-08-22 07:14_

Alright thanks for the explanation, I shall asap update the PR with this new description !

---

_@pakal reviewed on 2025-08-22 09:09_

---

_Review comment by @pakal on `docs/configuration.md`:310 on 2025-08-22 09:09_

PS: is this behaviour the same for all "setting <-> extend-setting" pairs ?
For the file selection, the rule selection, etc. ? Or are there different subtleties for other pairs of similar settings?

---

_@pakal reviewed on 2025-09-17 08:21_

---

_Review comment by @pakal on `docs/configuration.md`:310 on 2025-09-17 08:21_

I've done some testing, I now see that the "exclude" and "extend-include" do not work that way : the inherited "extend-include" settings are always aggregated to the mix, there is no reset of this chain of settings via a child config's "exclude". Is this difference of behavior between file-selection and rule-selection wanted ?

With the "include" and "extend-include" settings, it becomes even more complicated, for now I don't understand how they combine with exclusion settings along inheritance chain.... it'd really be worth a doc section if somebody has it all in mind and can explain it to me   ^^'

---

_@pakal reviewed on 2025-09-17 08:22_

---

_Review comment by @pakal on `docs/configuration.md`:310 on 2025-09-17 08:22_

Maybe I shall remove this aspect from the PR so that we can already merge what is sure and clear ?

---

_@pakal reviewed on 2025-10-01 14:56_

---

_Review comment by @pakal on `docs/configuration.md`:307 on 2025-10-01 14:56_

I de-scope this part of the documentation for a subsequent PR

---

_@pakal reviewed on 2025-10-01 14:57_

---

_Review comment by @pakal on `docs/configuration.md`:310 on 2025-10-01 14:57_

I de-scope this part of the documentation for a subsequent PR, the behaviours of these "extend" settings are not all the same so it'll need more testing and explanation in the docs, later  ^^

---

_Comment by @pakal on 2025-10-01 15:11_

I simplified the PR to only included "simple" clarifications ; select/extend-select and similar will be treated in another PR later    ^^

---
