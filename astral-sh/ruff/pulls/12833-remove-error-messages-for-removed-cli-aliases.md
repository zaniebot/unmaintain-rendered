```yaml
number: 12833
title: Remove error messages for removed CLI aliases
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - cli
assignees: []
merged: true
base: ruff-0.7
head: remove-deprecated-cli-aliases
created_at: 2024-08-12T09:00:00Z
updated_at: 2024-10-08T11:52:31Z
url: https://github.com/astral-sh/ruff/pull/12833
synced_at: 2026-01-10T20:59:36Z
```

# Remove error messages for removed CLI aliases

---

_Pull request opened by @MichaReiser on 2024-08-12 09:00_

## Summary

This PR removes the following CLI aliases: 

* `ruff <path>` to `ruff check <path>`
* `ruff --explain` to `ruff rule`
* `ruff --clean` to `ruff clean`
* `ruff --generate-shell-completion` to `ruff generate-shell-completion`

The aliases are deprecated since Ruff 0.3 and using them became a hard error in 0.5. 

Closes https://github.com/astral-sh/ruff/issues/10171


## Test Plan

I tested that using any of the aliases becomes a hard error. 

## Alternatives

We could keep the error messages around a little longer. It's not a lot of code but I would also expect that most users have migrated at this time (unless they skip 0.5)


---

_Label `breaking` added by @MichaReiser on 2024-08-12 09:00_

---

_Label `cli` added by @MichaReiser on 2024-08-12 09:00_

---

_Label `breaking` removed by @MichaReiser on 2024-08-12 09:00_

---

_Comment by @github-actions[bot] on 2024-08-12 09:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood reviewed on 2024-08-12 11:24_

> We could keep the error messages around a little longer. It's not a lot of code but I would also expect that most users have migrated at this time (unless they skip 0.5)

I'd lean towards keeping these for another minor release, but I don't feel strongly

---

_Comment by @zanieb on 2024-08-12 12:31_

Like until 0.7.0?

---

_Comment by @AlexWaygood on 2024-08-12 12:37_

> Like until 0.7.0?

yes

---

_Comment by @MichaReiser on 2024-08-12 12:42_

> yes

Are you sure you won't feel the same. IMO, we should either not remove the messages until like 1.0 or go ahead with it. 

---

_Comment by @AlexWaygood on 2024-08-12 12:49_

> Are you sure you won't feel the same.

Yeah, pretty sure :P

With a one-month period, I feel like folks who aren't super speedy in updating their dependencies could easily just bump their Ruff pin from 0.4 to 0.6 and never see the nice error message we're giving them right now. Doubling the period to two months doesn't get rid of that risk (nothing can), but I feel like there'll be substantially fewer people falling into that bucket.

Again, though, I don't feel strongly. They've been deprecated for ages and this abides by our versioning scheme.

---

_Comment by @zanieb on 2024-08-12 12:54_

This isn't that much code to keep around for another cycle 🤷‍♀️ 

---

_Comment by @zanieb on 2024-08-12 12:56_

> IMO, we should either not remove the messages until like 1.0 or go ahead with it.

I don't really agree with this. I don't think we should special-case 1.0.

---

_Comment by @MichaReiser on 2024-08-12 13:59_

> This isn't that much code to keep around for another cycle 🤷‍♀️

That's not my concern. My concern is that we keep spending time debating the same. That's why I rather postpone the removal indefinitely because the win of removing is very minimal (with the exception of `--output-format=text`)

---

_Added to milestone `v0.7` by @MichaReiser on 2024-08-12 13:59_

---

_Comment by @zanieb on 2024-08-12 14:37_

Sounds like Alex and I have loosely held opinions, maybe make the decision based on code maintenance? I agree we shouldn't discuss it every cycle: we should set a future milestone and stick to it unless we're getting clear feedback from the community that it is incorrect. We can also always revert (restore the helpful error) if we get reports from confused users.

Regardless, I think you should do what you think is best here. 


---

_Comment by @tmke8 on 2024-08-13 08:48_

Just stumbling upon this PR... as a more long-term solution, it might be nice if there were a general mechanism that checks whether an unknown flag matches a subcommand, and then to suggest that subcommand in the error message. Could even be checking if there is a match with a small edit distance.

---

_@AlexWaygood approved on 2024-10-08 11:45_

---

_Label `internal` added by @AlexWaygood on 2024-10-08 11:47_

---

_Merged by @MichaReiser on 2024-10-08 11:52_

---

_Closed by @MichaReiser on 2024-10-08 11:52_

---

_Branch deleted on 2024-10-08 11:52_

---
