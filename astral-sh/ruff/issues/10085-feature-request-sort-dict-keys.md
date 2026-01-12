```yaml
number: 10085
title: "[Feature Request] Sort dict keys"
type: issue
state: open
author: Tatsh
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2024-02-22T21:53:31Z
updated_at: 2025-07-09T10:15:59Z
url: https://github.com/astral-sh/ruff/issues/10085
synced_at: 2026-01-12T15:54:49Z
```

# [Feature Request] Sort dict keys

---

_@Tatsh_

Would be nice and I did not find this feature already being requested.

ESLint has this feature: [sort-keys](https://eslint.org/docs/latest/rules/sort-keys). Should have options like case-sensitivity, minimum keys, natural, asc/desc. `# noqa: ...` should disable the requirement and stop the auto-fixer.

---

_Label `rule` added by @MichaReiser on 2024-02-23 07:48_

---

_Comment by @AlexWaygood on 2024-02-26 11:45_

Thanks for the feature request! For reference, this is similar to a few issues that have been opened before:
- https://github.com/astral-sh/ruff/issues/6175
- https://github.com/astral-sh/ruff/issues/7929
-  #7220

However, I think we should keep this open. The first two of those issues were closed in favour of #1198, which was closed out without implementing this specific feature. The third is different enough to this that it could be considered separate.

I'm not sure I'd want to turn on this rule in my Python projects, since Python dictionaries maintain insertion order when you iterate through them (they have been guaranteed to do so by the language spec since Python 3.7), and I've written code in the past that's relied on that useful property. That means that the autofix proposed here has the potential to break working code. The only Python builtin collections where it would be safe to autosort the collection are sets and frozensets, since those are the only two that do not maintain insertion order.

Nonetheless, it does seem like there's a fair amount of user demand for this feature, and I gather the ESLint rule also has the potential to change the iteration order of a JavaScript object. So I think we should consider having a rule like this. If we're sorting dictionary keys, though, we may as well also sort list, tuples and sets, since we'd already be doing something pretty unsafe by sorting dictionary keys.

One open question is whether enabling this rule should sort _all_ collections? `isort` has a feature where collections that are marked with an `# isort: unique-list` comment are deduplicated and sorted -- we could possibly consider doing something similar.
- The advantage is that it neatly works around the safety issues (we'd only be sorting collections where the user has explicitly asked us to do so).
- The disadvantages are:
  - It's another pragma comment for users to have to remember/more configuration is really confusing
  - It's more verbose to have to add a comment next to every collection you want to be sorted like this.

---

_Label `needs-decision` added by @AlexWaygood on 2024-02-26 11:46_

---

_Comment by @MichaReiser on 2024-02-26 12:39_

> we may as well also sort list, tuples and sets, since we'd already be doing something pretty unsafe by sorting dictionary keys.

It would be worth having different rules, at least for lists. I've used object sorting in very large code bases in JS with very little need to suppress the rules because it's uncommon that the ordering matters (at least in the projects I worked). However, that's different for lists where ordering is more likely to be semantically meaningful (or I would have used a set). 

---

_Comment by @JelleZijlstra on 2024-02-29 06:20_

I would like this specifically for set (I realize that's not what the issue is about). Sets don't have insertion ordering, so unlike with dicts or lists, there isn't as much of a concern about changing behavior. For example, it would be nice if Ruff sorted [this set](https://github.com/JelleZijlstra/taxonomy/blob/19b4c707e961a6a800d6b2f4bfaeb41d608f2140/taxonomy/parsing.py#L88) so it's easier to keep track of which letters are in it.

---

_Comment by @AlexWaygood on 2024-02-29 07:58_

> I would like this specifically for set (I realize that's not what the issue is about). Sets don't have insertion ordering, so unlike with dicts or lists, there isn't as much of a concern about changing behavior. For example, it would be nice if Ruff sorted [this set](https://github.com/JelleZijlstra/taxonomy/blob/19b4c707e961a6a800d6b2f4bfaeb41d608f2140/taxonomy/parsing.py#L88) so it's easier to keep track of which letters are in it.

Yeah, I agree that it makes much more sense for sets than any other data structure. I wonder if this should be multiple rules — or one, configurable rule.

---

_Comment by @akx on 2025-07-09 10:15_

I think a per-expression `# fmt: sort` directive could be useful instead of/in addition to a blanket rule.

---
