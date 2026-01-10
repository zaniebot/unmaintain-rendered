```yaml
number: 22054
title: Add minimum fix safety to violation metadata
type: pull_request
state: open
author: ntBre
labels: []
assignees: []
draft: true
base: main
head: brent/document-safety
created_at: 2025-12-18T17:47:22Z
updated_at: 2025-12-19T15:45:28Z
url: https://github.com/astral-sh/ruff/pull/22054
synced_at: 2026-01-10T16:42:11Z
```

# Add minimum fix safety to violation metadata

---

_Pull request opened by @ntBre on 2025-12-18 17:47_

Summary
--

I included fix safety in my draft of default rule criteria, but there's no
existing way to retrieve the safety of a rule's fixes. This PR is a quick draft of adding
this information to the `violation_metadata` attribute macro and then enforcing
its accuracy in our `test_contents` linter tests. The tests are failing here
because I wanted to gauge interest in adding this feature before adding all of
the proper attributes on the affected rules.

A couple of things I think we can do with this information:
- include it in the `ruff rule --output-format=json` (this would help my default
  rules work)
- display it in our docs along with the fix availability
- enforce the presence of a `## Fix safety` section for rules with unsafe or
  display-only fixes

It's a bit annoying to add all these attributes again, but at least this one has
a sensible default (`Applicability::Safe`), which will work for safe rules and
rules without fixes.

I went with a string attribute again to avoid requiring `Applicability` imports
everywhere, but it would also be nice to make the attribute more like:

```rust
 #[violation_metadata(safety = Applicability::Safe)]
```

instead of the current:

```rust
 #[violation_metadata(safety = "safe")]
```

Test Plan
--

Updated `test_contents` to fail if a diagnostic's attached fix is less safe than
its documented safety


---

_@MichaReiser reviewed on 2025-12-18 17:50_

I'm a bit concerned about us forgetting to update the applicability but maybe it's fine thanks to the test integration?

It's just that always fixable is already something that's very easy to get outdated and adding more such information doesn't make me too excited :)

---

_Comment by @astral-sh-bot[bot] on 2025-12-18 17:54_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Comment by @ntBre on 2025-12-18 17:57_

Yeah I definitely wouldn't want this without the test integration. The existing integration also only enforces the minimum safety, so setting `safety = "display-only"` everywhere would pass all tests, *and* it only checks the safety of tests that run, so it won't be enforced if we're missing an unsafe test case, for example. So it's not without limitations.

I'm happy to close this if you think it will be too annoying to keep updated or correct. Just a quick proof of concept if it sounded interesting!

---

_Comment by @ntBre on 2025-12-19 15:29_

(Not trying to push this forward with the additional commits, necessarily. I just added the pieces I needed for my own data analysis)

---
