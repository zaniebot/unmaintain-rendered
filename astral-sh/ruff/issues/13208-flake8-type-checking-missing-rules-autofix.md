```yaml
number: 13208
title: "[flake8-type-checking] Missing rules, autofix strategies, overlapping rules and more"
type: issue
state: open
author: Daverball
labels:
  - plugin
assignees: []
created_at: 2024-09-02T09:26:25Z
updated_at: 2024-09-25T00:36:21Z
url: https://github.com/astral-sh/ruff/issues/13208
synced_at: 2026-01-12T15:54:52Z
```

# [flake8-type-checking] Missing rules, autofix strategies, overlapping rules and more

---

_@Daverball_

Hey, I'm one of the maintainers for flake8-type-checking and I really like some of the decisions ruff has made when implementing these rules, especially at a time where the implementation of the flake8 plugin was still a lot more naïve and reported tons of false positives. Since then however, the flake8 plugin underwent a lot of changes and now has its own semantic analysis to bring false positives down to a negligible percentage, in fact in all of my code bases with hundreds of thousands of lines of code I do not have a single `noqa` comment that silences a `flake8-type-checking` violation.

Ruff's implementation is in a pretty good spot, however I think we can still do better and get the missing rules added including reliable (albeit not always safe, depending on the configuration) autofixes for all or most of them. #12927 is my first PR that aims to add two of the missing rules without making any of the more controversial changes, that would require further discussion.

So this is a meta issue to track some of the things I would like to improve and/or change.

First off, I love the idea of having strategies for prioritizing the kind of fixes ruff should attempt. However one of them is notably missing when compared to the design of the flake8 plugin: "Add future import", this could be applied in the same cases as "Quote annotations", except it is more reliable, since the autofix is a lot simpler and it may be what a particular codebase prefers to do anyways. This would mean however that the boolean setting for quoting usages would need to be deprecated and replaced with an enum setting that selects one of the three strategies.

One fundamental problem with the strategies other than moving the import however, is that the violation and the fix are in completely different places, the flake8 plugin has violations (TC1XX/TC2XX) that would match these strategies, however these violations are currently absent from ruff. I think it would make sense that depending on the selected strategy and what autofixes we can emit, we should emit different violations (but the autofix should still be shared, like it currently is). I realize that this will make the logic more complex and most of the code for emitting violations would need to be moved into a common analysis function, rather than neatly split into different functions for different violations, in order to avoid duplicating the work, but the improvement in user experience I think would be significant enough for this to be worth it.

TC009 is another big rule that's currently missing, which is an expansion of TC004, where rather than only looking at imports, we look at all the bindings created within type checking blocks and mark the ones that need to be moved outside. An autofix will likely be very difficult in this case, since it is very likely that moving the declaration will cause new violations.

Then there are the rules for unnecessary quotes, there's technically already a rule in ruff from pyupgrade, but I think it is currently not careful enough to be considered an alias of the corresponding flake8-type-checking rules. So we could either try to make the pyupgrade rule more conservative and reuse it for flake8-type-checking or just accept that we'll end up with two very similar rules, but different degrees of aggressiveness, although we could also consider making it a setting.

To be more concrete: I currently treat any string annotation that contains either a subscript or attribute access (i.e. `[` or `.`) as no violation, regardless of whether or not it contains only references to runtime bindings. This is because there's many packages with third party stubs where generics are only generics at type checking time, but not at runtime. Similarly there will be module attributes that are only used for type checking and are not available at runtime.

TC006 is currently missing as well, and would be very easy to add. However there is still a redirection from TCH006 to TCH010 currently. So it might be worth to do #9573 first, so we can leave an alias from TCH006 to TC010, but TC006 would remain its own distinct violation.

We can start splitting some of these topics into separate issues, but I wanted to provide a starting point for a discussion to emerge. I am willing to implement these changes myself, but I am of course happy for any help and guidance, since I'm still new to Rust.

---

_Label `plugin` added by @charliermarsh on 2024-09-03 00:50_

---

_Comment by @jonyscathe on 2024-09-25 00:36_

Agree that TC006 and TC009 would be great to have

---
