```yaml
number: 17269
title: "[formatter] Add pragma-tags and pragma-tags-case-insensitive settings"
type: pull_request
state: open
author: lukaspiatkowski
labels:
  - formatter
assignees: []
base: main
head: fix-11941
created_at: 2025-04-07T11:04:03Z
updated_at: 2025-04-11T09:16:03Z
url: https://github.com/astral-sh/ruff/pull/17269
synced_at: 2026-01-10T19:40:37Z
```

# [formatter] Add pragma-tags and pragma-tags-case-insensitive settings

---

_Pull request opened by @lukaspiatkowski on 2025-04-07 11:04_

## Summary

Use `pragma-tags` and `pragma-tags-case-insensitive` options instead of hardcoding pragma tags detection logic.
Include the colon as part of the pragma tag instead of special-casing pragmas that require a trailing colon.

Fixes #11941

## Test Plan

Added `pragma_tags_*` tests to `ruff/tests/format.rs` to verify the default behaviour is unchanged and that customizing pragma tags works.


---

_Review requested from @MichaReiser by @lukaspiatkowski on 2025-04-07 11:04_

---

_Label `formatter` added by @AlexWaygood on 2025-04-07 11:10_

---

_Comment by @github-actions[bot] on 2025-04-07 11:44_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @MichaReiser on 2025-04-07 11:48_

Thanks. I've to think about this a bit more because I don't think we want two options and the other thing we have to think about is how this option works in combination with inherited configurations (how can a pragma be removed)

---

_Comment by @lukaspiatkowski on 2025-04-07 16:46_

The [task-tags](https://docs.astral.sh/ruff/settings/#lint_task-tags) config for the linter doesn't support inheritance, but if you feel strongly about it I could look into adding `extend-pragma-tags`. An `ignore-pragma-tags` that would mimic the (extend-)select/ignore configuration feels like an overkill given how little sense it makes to remove a pragma from the list.

As for reducing the 2 options into 1 I can think of the following solutions: 
* Make `pragma-tags` case insensitive (I don't see much harm in this being over-permissive)
* Enumerate all the nNoOqQaA permutations - seems wrong, especially if people want to add their own case insensitive tToOdDoO etc 
* Put both configurations in one config, something like
```python
["pyright:", ("i", "noqa")]
or
["pyright:", "noqa/i"] # /i like in regexes
```
not sure if this is possible in toml though

---

_Comment by @MichaReiser on 2025-04-11 08:08_

> Make pragma-tags case insensitive (I don't see much harm in this being over-permissive)

The main downside is that case-insensitive comparison is more expensive (comparing bytes isn't sufficient anymore). Do you have an example of pragma comments where it is common to use different casing?

> Put both configurations in one config, something like

This is too fancy. I rather prefer using a regex in that case. But that's also not free. 

>  but if you feel strongly about it I could look into adding extend-pragma-tags

We're moving away from the `extend` pattern. because it means it always requires 2 settings: One with extend and one with override behavior. I think I'd prefer to instead allow `!noqa` to remove elements from the pragma again. Or we make it a single regex, that removes the entire override problematic. Although I'm not a huge fan of users having to write regexes.


The last design question is if it should be possible for users to remove the standard pragma comments. I think it should.



---

_Comment by @lukaspiatkowski on 2025-04-11 09:16_

> Do you have an example of pragma comments where it is common to use different casing?

I am aware only of `noqa`, but then maybe even in that case a full case insensitive match is not really required? If you are willing to break backwards compatibility then by looking at GitHub I see that there are:
* `/(?-i)# ?noqa/` - 2.3M files
* Cases with exactly one lower letter:
  * `/(?-i)# ?NoQA/` - 1.7k files
  * `/(?-i)# ?nOQA/` & `/(?-i)# ?NOqA/` & `/(?-i)# ?NOQa/` - 0 files
* Cases with exactly one upper letter:
  * `/(?-i)# ?Noqa/` - 59 files
  * `/(?-i)# ?noQa/` - 12 files
  * `/(?-i)# ?noqA/` & `/(?-i)# ?nOqa/` - 0 files
* Cases when `O` is upper, rest is whatever:
  * `/(?-i)# ?NOQA/` - 116k files
  * `/(?-i)# ?nO[qQ][aA]/` & `/(?-i)# ?[nN]Oq[aA]/` & `/(?-i)# ?[nN]O[qQ]a/` - 3 files (basically upper `O` and at least one lower other number, sadly GitHub doesn't support negative lookahead so I had to try all the cases)
* Cases not covered before (`o` lower case, exactly 2 upper):
  * `/(?-i)# ?NoQa/` - 13 files
  * `/(?-i)# ?NoqA/` - 0 files
  * `/(?-i)# ?noQA/` - 79 files

So given the numbers it should be fine to support just `noqa`, `NOQA` and `NoQA`? My example with `ToDo` is probably similar, you don't care about `tODo` just a few reasonable values.

Or `noqa` can be special cased, so that if it is on the list of pragmas we check it in case-insensitive way as the only tag.

> The last design question is if it should be possible for users to remove the standard pragma comments. I think it should.

Assuming yes, do you prefer to do this via mechanism of "removal" rather than just allow the users to override the list entirely? Do you want to avoid users having to list the default pragmas when they just want to extend the list? I like the `!noqa` idea, but that brings a bit of complexity to the code - you have to support config inheritance properly and probably allow escaping the `!` (maybe `!!noqa`?). I don't mind listing the default pragmas every time I want to extend their list, especially because that list is relatively short right now, but also because it makes the whole configuration less magical. If I am a dev on a large project it would make it easier for me to understand why the formatting works like that if the pragmas are listed explicitly in the config.

---
