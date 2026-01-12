```yaml
number: 19973
title: "[relative-imports] Add setting to require relative imports"
type: pull_request
state: open
author: genevieve-me
labels: []
assignees: []
draft: true
base: main
head: feat/require-relative-imports
created_at: 2025-08-18T16:59:01Z
updated_at: 2025-12-17T00:54:09Z
url: https://github.com/astral-sh/ruff/pull/19973
synced_at: 2026-01-12T15:56:51Z
```

# [relative-imports] Add setting to require relative imports

---

_@genevieve-me_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Adds an 'always relative' import style setting to flake8-tidy-imports, closing #4188 .
    
The PR changes the flake8-tidy-imports TID252 rule and its ban-relative-imports setting to allow forcing imports to be always absolute, relative, or absolute for parents.
    
Since the rule used to just configure the maximum level of relative import (either never allowing relative or allowing at the same level), I renamed the rule from `ban-relative-imports` to `relative-import-style`. I also made some internal name changes, e.g., relative import `Strictness` became relative import `ImportStyle` (perhaps a slightly redundant name), and diagnostics were updated.
    
The implementation relies on the categorize function used for the isort rule to determine if absolute imports are first party and therefore could be relative.
    
NOTE: I realize now that this implementation is wrong, which is why this PR is marked as draft.
First party doesn't mean 'can be relative,'  as users can configure separate packages to be first party with `isort.known-first-party` but they can't be imported relatively. I'll have to take a closer look at the ImportType options and see which one applies to same-package imports that could become `from .bar import baz` instead of `from foo.bar import baz`.

### Settings

I also presume that the way this PR naively renames the setting is a big no-no for breaking configs, especially since TID252 is not in preview. I actually couldn't find any examples of renaming settings in the changelogs: presumably there should be some sort of deprecation period.

**Getting feedback on the settings is the main reason I'm opening the PR** even in its incomplete state. The renaming seems necessary, because:

* The initial feedback on the feature request said "We could probably add support for this as a configurable setting in TID252"
* The existing setting can cleanly cover all three cases, so adding a second one and having two overlapping settings for this rule would seem odd
* However, setting `ban-relative-imports` to force relative imports would be confusing. Even something like `ban-relative-imports = 'none'`, while in keeping with the current rules, doesn't make it clear that absolute imports are actually being banned.

## Test Plan

Right now, I updated the existing tests, but I still need to add new tests where absolute imports trigger diagnostics if appropriate (they are in the same package namespace and could be relative).

I also want to look into the performance. It feels redundant to re-categorize all imports when they're already being categorized elsewhere for the isort rule, but we can't assume that the isort rule is enabled and we can get the information from there. Maybe we could categorize them all at a higher level and pass that information to the relevant places, but that could also add unnecessary complexity?

---

_Comment by @ntBre on 2025-09-03 14:43_

Apologies for the delay here. I just wanted to say thanks for your work on this, and that I do intend to take a closer look at some point!

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_tidy_imports/rules/relative_imports.rs`:50 on 2025-12-12 21:40_

Hmm, I actually kind of like the "strictness" name. What do you think about using this as the setting name? `strictness = "require"` sounds nice to me, as Charlie mentioned on the issue.

---

_@ntBre reviewed on 2025-12-12 21:54_

Thanks again for your work on this and apologies again for the delay. The code changes look reasonable to me, but I had a couple of thoughts about adding the option.

As you noted, we'd definitely need to make this a preview change and also deprecate the existing option name, if we change it. #16092 is an example where we've renamed an option in the past.[^1]

I was a bit worried about the isort code because it has been a bit unreliable, with a lot of [issues](https://github.com/astral-sh/ruff/issues?q=sort%3Aupdated-desc%20is%3Aissue%20state%3Aclosed%20isort%20third-party) around the first- vs third-party categorization. But I think we can instead reuse smaller parts of the isort code, like [`match_sources`](https://github.com/astral-sh/ruff/blob/a722df6a7309c6de2eaac1ba55abe8e741ea7e17/crates/ruff_linter/src/rules/isort/categorize.rs#L169) instead of the full `categorize` call. (Assuming there aren't any weird edge cases where checking the existence of local paths isn't correct, I somewhat expect that there are).

I'd be curious to get @dylwil3 and @amyreese's thoughts on this too, but assuming we can reliably detect viable relative imports and isolate this to a preview change, I'm supportive of adding this feature.

[^1]: I think we can probably get away with just adding a third `Strictness` variant that we only respect in preview (and emit a warning if it's used outside preview) to allow landing this in a patch release. Then we could open a separate `breaking` PR changing the name, to be included in the next minor release.

---

_Comment by @amyreese on 2025-12-16 18:22_

I would personally love for *something* to enforce/convert modules using relative imports, because nothing bugs me more than using relative imports everywhere, and then VSCode's auto-import code actions creating absolute imports, even from modules that are already using relative imports, and only noticing once tests fail or a release gets flagged for breaking something. 😅

---

_Comment by @genevieve-me on 2025-12-17 00:54_

Hey Brent, thanks for taking a look and for the guidance on the preview approach. I'll be able to take a closer look over the weekend.

---
