```yaml
number: 15171
title: "Request: Merge `D203`+`D211` and `D212`+`D213` and make them configurable instead"
type: issue
state: open
author: Avasam
labels:
  - question
assignees: []
created_at: 2024-12-28T21:32:21Z
updated_at: 2025-01-02T05:36:59Z
url: https://github.com/astral-sh/ruff/issues/15171
synced_at: 2026-01-12T15:54:54Z
```

# Request: Merge `D203`+`D211` and `D212`+`D213` and make them configurable instead

---

_@Avasam_

The rule `one-blank-line-before-class (D203)` and `blank-line-before-class (D211)` are directly opposed. Same for `multi-line-summary-first-line (D212)` with `multi-line-summary-second-line (D213)`.
I believe these should instead be a single rule that is configurable to act either one way or another. It is very odd and a bit unwieldy that a rule is configurable by having to disable a code, within the same group. If you're extending from another configuration, you have to re-select a rule *and* disable the other.
This would also make more sense in the context of shared configs, where one can enable the `pydocstyle (D)` group and configure how it acts separately.

(note this is a different request than https://github.com/astral-sh/ruff/issues/8470, which wants to hide the "incompatible rules" warning with `ALL`, whilst I want these specific incompatibilities to not be a thing in the first place)

Ruff: 0.8.4

---

_Comment by @Avasam on 2024-12-28 22:14_

After reading a bit more, I found https://docs.astral.sh/ruff/settings/#lint_pydocstyle_convention which kinda does what I'd like simply because I happen to personally prefer `D211` + `D213` which all 3 conventions also prefer.

But which rules each individual convention enable/disable by default isn't listed. It's only written down on the specific rules pages.

---

_Comment by @dhruvmanila on 2024-12-30 08:57_

Cross posting here from Discord, the list is documented here: https://docs.astral.sh/ruff/faq/#does-ruff-support-numpy-or-google-style-docstrings although I think it's not immediately visible and it might be useful to have this either in `lint.pydocstyle.convention` setting page or in each of the Pydocstyle rule doc.

---

_Comment by @Avasam on 2024-12-30 18:38_

I completely missed that section! I didn't check the FAQ for that. Only the convention page https://docs.astral.sh/ruff/settings/#lint_pydocstyle_convention

I agree discoverability is not the best. Would be nice if it was a bit easier to find.

I still find it odd for those rules to be completely opposite. But maybe the added complexity to also modify options in the convention system isn't worth. At least not until "the great rules recategorization". Selecting a base convention removes a lots of cruft around configuring pydocstyle.

I'd be fine if the decision is to keep the rules/config as-is at this time. But with improved discovery of the specific conventions.


---

_Comment by @dhruvmanila on 2025-01-02 05:36_

> I agree discoverability is not the best. Would be nice if it was a bit easier to find.

Opened https://github.com/astral-sh/ruff/issues/15217 to track this.

> I still find it odd for those rules to be completely opposite. But maybe the added complexity to also modify options in the convention system isn't worth. At least not until "the great rules recategorization". Selecting a base convention removes a lots of cruft around configuring pydocstyle.
> 
> I'd be fine if the decision is to keep the rules/config as-is at this time. But with improved discovery of the specific conventions.

Agreed. I think it would be useful to think about this during the rule-categorization instead.

---

_Label `question` added by @dhruvmanila on 2025-01-02 05:36_

---
