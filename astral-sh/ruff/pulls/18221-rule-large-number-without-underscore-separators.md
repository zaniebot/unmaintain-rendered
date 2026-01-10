```yaml
number: 18221
title: "Rule: large number without underscore separators (PEP 515)"
type: pull_request
state: open
author: guillp
labels:
  - rule
  - needs-decision
assignees: []
base: main
head: rule/large_number_without_separators
created_at: 2025-05-20T15:05:35Z
updated_at: 2025-08-07T15:35:34Z
url: https://github.com/astral-sh/ruff/pull/18221
synced_at: 2026-01-10T17:52:17Z
```

# Rule: large number without underscore separators (PEP 515)

---

_Pull request opened by @guillp on 2025-05-20 15:05_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

This adds a rule (with code RUF062) that automatically formats large numbers with underscore separators to make them more readable. This is as described in [PEP515](https://peps.python.org/pep-0515/), and discussed in #12787 which I opened a while back.

E.g: 
`123456` becomes `123_456`
`.12345` becomes `.123_45`
`0xDEADBEEF` becomes `0xDEAD_BEEF`
(see [test snapshot](https://github.com/astral-sh/ruff/pull/18221/files#diff-fc9994b8c3092dd604dedd0c9d6a3920f3a5699a69c31915be9ab028c166a6e8) for more examples)

This rule works for:
- integers: if they are 5 digits or more (configurable), the rule will require underscores as thousands, millions, billions, etc. separators. This threshold avoid formatting relatively small integers (`9999` or less)  since they are already readable enough. `123456789` gives `123_456_789`
- floats: same rules as integers, and the float part is also formatted with those same rules, but the groups are formed left to right (thousandths, millionths, etc.)
`123456789.123456789` gives `123_456_789.123_456_789`
- hexadecimal notation (`0xABCD`): add underscores to form groups of 4 digits by default (configurable)
`0x1234567890ABCD` gives `0x12_3456_7890_ABCD`
- octal notation (`0o1234`): add underscores to form groups of 4 digits by default (configurable)
`0o1234567` gives `0o123_4567`
- binary notation (`0b1010101`): add underscores to from groups of 8 bits (octets) by default (configurable)
`0o1010111100100101` gives `0o10101111_00100101`
- scientific notation (`123e10`): the leading part is formatted with the same rules as integers. The exponent part is untouched as it should never be more than 3 chars anyway.
- positive or negative literals: the leading `+` or `-` sign is not part of `Expr::NumberLiteral` instances once parsed by ruff, so this rule does not modify them in any way, they just stay in place.
- any kind of number already containing separators but in the wrong places: number will be reformatted with the defined configuration.

### Support for indian-style number formatting: 
According to https://randombits.dev/articles/number-localization/formatting , most of the world groups decimal digits 3 by 3, excepted for India who uses groups of 2 after the first group of 3 (so thousands, hundred of thousands, hundreds of hundreds of thousands, etc.). A configuration option allows enabling this kind of grouping.
I am however not sure about what is the practice for formatting the float part in India. I implemented a "reversed" logic, with separators on thousandth, then hundredth of thousandth, hundredth of hundredth of thousandth, etc. (not sure if anyone ever needed such a float precision ^^'). This may need to be adjusted.

So `123456789.123456789` formatted with indian mode will give `12_34_56_789.123_45_67_89`

## Test Plan

A new test file `RUF062.py` is part of the PR and is executed on `cargo test`.


## TODO / to discuss
- rule code: I took RUF062 as it is the next available in the RUFF group, and I selected this group because I did not see such a rule implemented in any other formatter/linter.
- naming of configuration options should be reviewed and probably improved
- default config values to be discussed
- more tests?

---

_Renamed from "Rule/large number without separators" to "Rule: large number without underscore separators (PEP 515)" by @guillp on 2025-05-20 15:06_

---

_Label `rule` added by @MichaReiser on 2025-05-20 15:07_

---

_Label `needs-decision` added by @MichaReiser on 2025-05-20 15:07_

---

_Review requested from @carljm by @guillp on 2025-07-01 14:07_

---

_Review requested from @AlexWaygood by @guillp on 2025-07-01 14:07_

---

_Review requested from @sharkdp by @guillp on 2025-07-01 14:07_

---

_Review requested from @dcreager by @guillp on 2025-07-01 14:07_

---

_Review requested from @MichaReiser by @guillp on 2025-07-01 14:07_

---

_Review requested from @BurntSushi by @guillp on 2025-07-01 14:07_

---

_Review requested from @dhruvmanila by @guillp on 2025-07-01 14:07_

---

_Review request for @dcreager removed by @AlexWaygood on 2025-07-01 14:08_

---

_Review request for @carljm removed by @AlexWaygood on 2025-07-01 14:08_

---

_Review request for @BurntSushi removed by @AlexWaygood on 2025-07-01 14:08_

---

_Review request for @sharkdp removed by @AlexWaygood on 2025-07-01 14:08_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-07-01 14:08_

---

_Review request for @MichaReiser removed by @AlexWaygood on 2025-07-01 14:08_

---

_Review request for @dhruvmanila removed by @AlexWaygood on 2025-07-01 14:08_

---

_Comment by @guillp on 2025-07-01 14:11_

I rebased on current main and changed the code to RUF062 since 061 is now taken by another rule.

---

_Comment by @ntBre on 2025-07-01 14:13_

I think something went wrong with the rebase, as GitHub is now showing more than 500 commits and over 100,000 lines changed, which would make it pretty difficult to review!

---

_Comment by @guillp on 2025-07-03 07:13_

Not sure what I did wrong. But I made some clean-up so there is a single commit on top of current main with all the changes now.
And also I added configuration options for digit group sizes, and also added support for indian-style number formatting.
Edited the PR description to reflect this. Thanks for reviewing this.

---

_Comment by @ntBre on 2025-07-04 19:26_

Thanks for tidying up and for your work on this! I think we'll still want to resolve the `needs-decision` label on the issue before giving this a full review.

---

_Comment by @MichaReiser on 2025-07-07 14:21_

I agree that we still need to answer the question if we want to have such an opinionated rule that enforces specific grouping of numbers. 

I do like clippy's rule that are less opinionated but enforce good practice:

* Enforce consistent grouping inside a single number: https://rust-lang.github.io/rust-clippy/master/index.html?groups=pedantic%2Cstyle#inconsistent_digit_grouping
* Warn about uncommon byte groupings: https://rust-lang.github.io/rust-clippy/master/index.html?groups=pedantic%2Cstyle#unusual_byte_groupings
* Too large groups (it's not quiet clear to me what that means): https://rust-lang.github.io/rust-clippy/master/index.html?groups=pedantic%2Cstyle#large_digit_groups

The first two seem very useful to me. It's less clear to me if we want to add any more opinionated rules.

---

_Comment by @guillp on 2025-08-07 14:25_

I think the point of having an opinionated rule is to allow auto-fixing, which should be suitable for most cases. If some codebase has very specific requirements, the rule should be configurable enough to accommodate them (as long as those requirements are consistent across the whole code-base). Or the rule can be disabled otherwise.

Another different rule that only _warns_ about inconsistent, uncommon or too large grouping (like in clippy) may also be created, and user may choose to activate one or the other. Or even both, because if the "enforcer" rule (from this PR) is ran before the "warning" rule, the former should fix any issue before any warning is issued by the later. Would that suit what you had in mind? I can implement that _warning_ rule if needed.

---

_Comment by @ntBre on 2025-08-07 15:22_

> Another different rule that only _warns_ about inconsistent, uncommon or too large grouping (like in clippy) may also be created, and user may choose to activate one or the other. Or even both, because if the "enforcer" rule (from this PR) is ran before the "warning" rule, the former should fix any issue before any warning is issued by the later. Would that suit what you had in mind? I can implement that _warning_ rule if needed.

I think it would be very unusual, probably unprecedented, to have two overlapping rules with different severities, so I don't think that's a viable option. We do plan to add a warning level/severity (https://github.com/astral-sh/ruff/issues/1256) in the future, though.

---
