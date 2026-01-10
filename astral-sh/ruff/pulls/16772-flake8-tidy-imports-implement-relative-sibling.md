```yaml
number: 16772
title: "[`flake8-tidy-imports`] Implement `relative-sibling-imports` (`TID254`)"
type: pull_request
state: open
author: noirbizarre
labels:
  - rule
  - needs-decision
assignees: []
base: main
head: feature/tid254-relative-sibling-imports
created_at: 2025-03-15T23:32:24Z
updated_at: 2025-03-16T13:17:51Z
url: https://github.com/astral-sh/ruff/pull/16772
synced_at: 2026-01-10T19:49:02Z
```

# [`flake8-tidy-imports`] Implement `relative-sibling-imports` (`TID254`)

---

_Pull request opened by @noirbizarre on 2025-03-15 23:32_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
This PR follows the discussion from #1014 and add a new opt-in rule `TID254` which is requiring relative import for sibling (and nested) modules.
It requires the `linter.flake8-tidy-imports.relative-sibling-imports` setting to be `true` to avoid users having enabled all `TID` to have their imports rewritten if they did not choose to as it is one of [2 suggested PEP8 alternatives](https://peps.python.org/pep-0008/#imports):

> However, explicit relative imports are an acceptable alternative to absolute imports, especially when dealing with complex package layouts where using absolute imports would be unnecessarily verbose:
> 
> ```python
> from . import sibling
> from .sibling import example
> ```

Fixes #1014

## Test Plan

<!-- How was it tested? -->

Multiple test cases have been added to the test suite, with associated expected snapshots, and it has been tested on some external codebases.


---

_Comment by @MichaReiser on 2025-03-16 09:05_

Thanks for working on this. It might take me a while before I get to thinking through the implications of this change. The concerns are:

* It creates a new rule that doesn't exist upstream. We've made exceptions for some categories in the past but it comes with its own set of problems, which is why we're more hesitant to keep doing that (e.g. what if upstream adds a TID254...)
* Rules that require explicit opt-in with an option tend to be confusing for users. I see the motivation here, but it still applies. What's the rationale for not integrating this into TID252?
* I'd have to double check if this rule doesn't conflict with any other rule (that's not really a concern, only a thing that needs doing)

---

_Label `rule` added by @MichaReiser on 2025-03-16 09:05_

---

_Label `needs-decision` added by @MichaReiser on 2025-03-16 09:05_

---

_Comment by @noirbizarre on 2025-03-16 13:15_

Hi 👋🏼 

Thanks for the prompt response.

I'll answer the bullet points in the same order:

> It creates a new rule that doesn't exist upstream. We've made exceptions for some categories in the past but it comes with its own set of problems, which is why we're more hesitant to keep doing that (e.g. what if upstream adds a TID254...)

I took the liberty of submitting this pull request as the TID namespace already diverged from the upstream (`TID253` doesn't exist in `flake8-tidy-import`), so this PR doesn't introduce this problem, it takes benefit from the fact the problem already exists (I know, weird thinking). 
I actually submitted an upstream PR 2 years ago (https://github.com/adamchainz/flake8-tidy-imports/pull/441), I didn't have feedback since, and I stop hoping for (I didn't insist either as the maintainer doesn't seem happy about it, and I didn't want to pressure him)

> Rules that require explicit opt-in with an option tend to be confusing for users. I see the motivation here, but it still applies. What's the rationale for not integrating this into TID252?

Actually, I can, the initial PR (https://github.com/astral-sh/ruff/pull/3986) was doing that.
It was more about the semantic: the rule is advertising "Prefer absolute imports over relative imports from parent modules" and this PR is doing the opposite for siblings. It would mean that the same rule could say "Prefer absolute imports over relative imports from parents modules" and "Prefer relative imports over absolute imports for siblings modules".
But if this is acceptable, I'll rework this PR to make this a TID252 setting and change the documentation sentence for something more generic.

> I'd have to double check if this rule doesn't conflict with any other rule (that's not really a concern, only a thing that needs doing)

If TID252 `ban-relative-imports` is set to `all`, it is by design contradictory with this rule. That's why I documented that it should be set to `parents` in the docstring.

What would be acceptable to you if I just integrate it with TID252?

1. Simply use a 3rd `ban-relative-import` value like `parents-with-relative-siblings` with would ensure no conflict at the cost of being a bit contradictory with the `ban` term
2. Keep the separate setting with a warning about the conflict?
3. Another better approach

I'll do any necessary change to have this feature merged because I still have the need for this and from #1014, I don't seem to be the only one.


---
