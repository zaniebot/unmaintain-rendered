```yaml
number: 8358
title: Run ecosystem checks with preview mode enabled
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zanie/ecosystem-preview
created_at: 2023-10-30T15:14:08Z
updated_at: 2023-11-01T17:12:04Z
url: https://github.com/astral-sh/ruff/pull/8358
synced_at: 2026-01-12T15:55:26Z
```

# Run ecosystem checks with preview mode enabled

---

_@zanieb_

Until https://github.com/astral-sh/ruff/issues/8076 is ready, it seems beneficial to get feedback on preview mode changes.

Tested locally, updated logs to output the flags passed to `ruff` and verified `--preview` is used.

---

_Review comment by @zanieb on `.github/workflows/ci.yaml`:221 on 2023-10-30 15:15_

Alternatively, we could get fancy and set this flag if a pull request has the `preview` label. I'm not sure that's worth it.

---

_@zanieb reviewed on 2023-10-30 15:15_

---

_Label `internal` added by @zanieb on 2023-10-30 15:25_

---

_Comment by @github-actions[bot] on 2023-10-30 15:45_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no linter changes.

✅ ecosystem check detected no format changes.



---

_Review requested from @charliermarsh by @zanieb on 2023-10-30 15:58_

---

_@charliermarsh approved on 2023-10-30 22:06_

I support this, probably worth getting @MichaReiser's take too.

---

_Review requested from @MichaReiser by @zanieb on 2023-10-30 22:15_

---

_Comment by @MichaReiser on 2023-10-31 01:11_

I'm sure if I understand this change. Does it mean that we'll run the ecosystem checks in preview only? Or will we run the ecosystem checks in preview and normal mode? 

I believe running the ecosystem checks in non-preview mode helps identify unintentional breaking changes. 

For the formatter, I think we need to run both to assess if (even a preview only change) unintentionally breaks non-preview formatting AND in preview to assess how preview style changes formatting.

---

_Comment by @zanieb on 2023-10-31 01:33_

While I would like to run both preview and not preview as described in #8076, until we implement that we gain more (in the linter, at least) from the ecosystem checks with preview enabled than not.

Basically, this is a temporary solution that also moves us a step closer to supporting both outputs.

My primary concern with naively enabling both preview and non-preview checks  is that it doubles the amount of output we can display — and it already feels very constrained.

---

_@MichaReiser approved on 2023-10-31 07:27_

Sounds reasonable for the inter, but I fear we need to invest a little more into the ecosystem check for us to implement changes with confidence. 

For the formatter, the most important in my view, is to ensure we don't introduce unintentional changes to the stable style. Or we risk shipping breaking changes which are annoying for users. This is why I would prefer to run the ecosystem checks with stable style formatting. 

I see it essential for implementing preview style successfully to have a way (at least locally) to run the ecosystem checks with preview style enabled. Could you document how this can be done in the CONTRIBUTION.md if it's not already the case? If you can find the time to make this work in CI during next month would be awesome, but I know that you've already planned a lot. 





---

_Merged by @zanieb on 2023-11-01 17:12_

---

_Closed by @zanieb on 2023-11-01 17:12_

---

_Branch deleted on 2023-11-01 17:12_

---
