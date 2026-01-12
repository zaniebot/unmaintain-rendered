```yaml
number: 11443
title: Update scripts docs with package indexes
type: pull_request
state: merged
author: lewis-wf
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2025-02-12T12:27:23Z
updated_at: 2025-02-12T13:52:26Z
url: https://github.com/astral-sh/uv/pull/11443
synced_at: 2026-01-12T16:09:51Z
```

# Update scripts docs with package indexes

---

_@lewis-wf_

## Summary

The [current scripts docs page](https://docs.astral.sh/uv/guides/scripts/) doesn't include detail on how to use a custom package index when setting up a script. I believe this might be because it didn't use to work (see #6688 ) but it now does (thanks for that, by the way! 😄) 

Given it's a useful feature, I suggest adding a quick example to the scripts page, with the details of authentication, etc. left to the main `indexes.md` doc.

I'd also suggests that this closes #6688, though it doesn't actually add that feature - that appears to have already been done :) 

## Test Plan

No testing is needed, I think!


---

_Comment by @lewis-wf on 2025-02-12 12:37_

Not entirely sure why the linter is failing, I have run it as per the contributing guide and applied the suggested fixes. 

---

_@zanieb approved on 2025-02-12 13:49_

---

_Comment by @zanieb on 2025-02-12 13:51_

If it's helpful, my invocation is `npx prettier --write docs/**/*.md --prose-wrap always`

It didn't look like the lines were wrapped.

---

_@zanieb approved on 2025-02-12 13:52_

Thanks for contributing! I edited it briefly to match the surrounding style

---

_Merged by @zanieb on 2025-02-12 13:52_

---

_Closed by @zanieb on 2025-02-12 13:52_

---

_Label `documentation` added by @zanieb on 2025-02-12 13:52_

---
