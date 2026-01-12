```yaml
number: 15163
title: Remove legacy issue template
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - internal
assignees: []
merged: true
base: main
head: issue-template
created_at: 2024-12-27T17:23:09Z
updated_at: 2025-01-15T11:00:55Z
url: https://github.com/astral-sh/ruff/pull/15163
synced_at: 2026-01-12T15:55:50Z
```

# Remove legacy issue template

---

_@InSyncWithFoo_

## Summary

Resolves #15135.

## Test Plan

None.



---

_Comment by @MichaReiser on 2024-12-27 17:26_

Thanks.

Does this mean that clicking *New issue* now always requires clicking on *Bug report*?

The old issue template intended something extremely lightweight. Our goal is to keep opening an issue as simple as possible without requiring any specific structure. While the new template looks okay, I think it's more structured than we want. 

I think the best first step is to mimic what we had before: A single markdown field with a default text. 



---

_Label `internal` added by @MichaReiser on 2024-12-27 17:27_

---

_Comment by @InSyncWithFoo on 2024-12-27 17:36_

> Does this mean that clicking <i>New issue</i> now always requires clicking on <i>Bug report</i>?

As far as I'm aware, yes (though one can also choose to click the "Open a blank issue" link at the bottom).

---

_Comment by @MichaReiser on 2024-12-27 17:41_

Hmm, yeah I think this is somewhat annoying and can be confusing. E.g., what are you supposed to do when you don't want to open a bug report? It might be confusing enough that people click away. 

Considering all this, I'm leaning toward removing the template and we can explore something similar to uv's https://github.com/astral-sh/uv/issues/9452 if we see details missing in many new issues

---

_Comment by @InSyncWithFoo on 2024-12-27 17:51_

This PR does need a new "Feature request" template to go with "Bug report". I think these two will account for the vast majority of cases. People who are more involved with the project will know what to do with the blank template.

> I'm leaning toward removing the template and we can explore something similar to uv [...]

That reminds me, uv also has [a similarly outdated issue template](https://github.com/astral-sh/uv/blob/1fb7f352b117ec3751a91391c0f6c34aab87acf9/.github/ISSUE_TEMPLATE.md). I'll submit a PR over there too once we decide what to do with this one.


---

_Comment by @InSyncWithFoo on 2024-12-27 17:52_

(By the way, the new format allows automatic labeling, if that's something you might want to consider.)

---

_Comment by @zanieb on 2024-12-27 23:58_

I'm not sure if we want this. I've done a fair bit of investigation in the past. We'll need to talk about this once people are back from their holiday vacations.

---

_Label `do-not-merge` added by @zanieb on 2024-12-27 23:58_

---

_Comment by @MichaReiser on 2024-12-28 09:01_

> This PR does need a new "Feature request" template to go with "Bug report". I think these two will account for the vast majority of cases. People who are more involved with the project will know what to do with the blank template.

Yeah, but there are many more cases: changes to existing rules, questions, black deviations, style requests, etc. 

My preferred solution remains just to remove the template, considering that we haven't been using the issue templates for a few weeks and that the issues opened during this time have been as good as before.

---

_Renamed from "Migrate issue template to new format" to "Remove issue template" by @MichaReiser on 2024-12-31 11:38_

---

_Renamed from "Remove issue template" to "Remove now-unused issue template" by @MichaReiser on 2024-12-31 11:38_

---

_Renamed from "Remove now-unused issue template" to "Migrate issue template to new format" by @MichaReiser on 2024-12-31 11:39_

---

_Renamed from "Migrate issue template to new format" to "Remove legacy issue template" by @InSyncWithFoo on 2025-01-15 10:52_

---

_Label `do-not-merge` removed by @MichaReiser on 2025-01-15 10:59_

---

_Comment by @MichaReiser on 2025-01-15 10:59_

Thanks

---

_Merged by @MichaReiser on 2025-01-15 10:59_

---

_Closed by @MichaReiser on 2025-01-15 10:59_

---

_Branch deleted on 2025-01-15 11:00_

---
