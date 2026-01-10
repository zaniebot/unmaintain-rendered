```yaml
number: 10826
title: "docs: rework alternative indexes documentation"
type: pull_request
state: merged
author: mkniewallner
labels:
  - documentation
assignees: []
merged: true
base: main
head: docs/rework-alternative-indexes
created_at: 2025-01-21T21:16:23Z
updated_at: 2025-02-26T16:07:12Z
url: https://github.com/astral-sh/uv/pull/10826
synced_at: 2026-01-10T11:10:34Z
```

# docs: rework alternative indexes documentation

---

_Pull request opened by @mkniewallner on 2025-01-21 21:16_

## Summary

Closes #9867.

Update alternative indexes documentation to use `[[tool.uv.index]]` and the associated environment variables instead of `UV_INDEX`.

This also globally reworks the documentation by:
- adding AWS CodeArtifact keyring example
- adding packages publishing examples for all providers
- making it more consistent for all providers

It might be best to show how to publish packages only once for all providers, but the publish URL usually being different than the URL used to retrieve packages, even if this duplicates things, it might still be more straightforward for users to see exactly what is needed for each provider.

## Test Plan

Manually tested retrieving packages from AWS CodeArtifact and GCP Artifact Registry using both token and keyring.

Could not test:
- Publishing packages
- Azure Artifacts (not using it at all)

---

_Marked ready for review by @mkniewallner on 2025-02-18 22:21_

---

_Comment by @zanieb on 2025-02-19 23:33_

Hey! Thank you so much for taking this on.

I gave this a quick look and made some stylistic edits to the Azure section https://github.com/astral-sh/uv/pull/10826/commits/6a9b5e4e453574523f895ba08665493a967886eb

Before I apply them elsewhere, what do you think of those? I'm mostly editing for consistency with the rest of the documentation.

Are you interested in applying the same changes to the other sections, or should I?

---

_Label `documentation` added by @zanieb on 2025-02-19 23:33_

---

_Assigned to @zanieb by @zanieb on 2025-02-19 23:33_

---

_Comment by @mkniewallner on 2025-02-22 11:49_

> Before I apply them elsewhere, what do you think of those? I'm mostly editing for consistency with the rest of the documentation.
> 
> Are you interested in applying the same changes to the other sections, or should I?

Sorry for the delay, changes look good, thanks! I've applied them to the other sections as well.

---

_@zanieb approved on 2025-02-26 15:52_

---

_Comment by @zanieb on 2025-02-26 15:52_

Thank you! 

---

_Merged by @zanieb on 2025-02-26 15:52_

---

_Closed by @zanieb on 2025-02-26 15:52_

---

_Branch deleted on 2025-02-26 16:07_

---
