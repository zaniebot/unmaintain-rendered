```yaml
number: 8246
title: "Don't prefetch unreachable packages"
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
  - performance
  - resolver
assignees: []
merged: true
base: main
head: konsti/stop-prefetching-the-wrong-packages
created_at: 2024-10-16T09:09:20Z
updated_at: 2024-10-22T11:02:20Z
url: https://github.com/astral-sh/uv/pull/8246
synced_at: 2026-01-12T16:08:13Z
```

# Don't prefetch unreachable packages

---

_@konstin_

When batch prefetching we can fetch versions we know that are incompatible. In the following example, we were prefetching sentry-kafka-schemas below version 1.50.0.

```
python-rapidjson<=1.20,>=1.4
sentry-kafka-schemas<=0.1.113,>=0.1.50
```

Using a new pubgrub interface from https://github.com/astral-sh/pubgrub/pull/32, we can avoid those prefetches by asking for incompatibilities that won't change anymore (those with root).

main:

![image](https://github.com/user-attachments/assets/c8475069-856e-4072-a688-0bb426e32160)

branch:

![image](https://github.com/user-attachments/assets/0b0da8e0-2f09-4137-a2c9-2af0cd706be5)

For the tested case, the performance impact was negligible.

Draft since it depends on #8245 and then another pubgrub update.

---

_Label `enhancement` added by @konstin on 2024-10-16 09:09_

---

_Label `resolver` added by @konstin on 2024-10-16 09:09_

---

_Comment by @charliermarsh on 2024-10-16 12:20_

Wow, that's amazing!

---

_Comment by @charliermarsh on 2024-10-16 12:20_

We might need to check... two levels. Because in the context of a workspace, the root depends on the members, right?

---

_Comment by @konstin on 2024-10-16 12:30_

That's why we're asking pubgrub instead of using the uv logic: If we have `root -> workspace_member` and `workspace_member -> foo>=1.50`, then pubgrub should be able to infer a derivation `root -> foo>=1.50`, or put differently, see that `foo>=1.50` is still at decision level 1. Theoretically, this should be able to even infer bounds for non-workspace transitive dependencies.

I haven't tested through the details, but I still want to go ahead since it solves my real world test case and the pubgrub derivations are the right place to improve if we find more complex cases later.

---

_Marked ready for review by @konstin on 2024-10-17 09:45_

---

_Review requested from @charliermarsh by @konstin on 2024-10-17 09:46_

---

_Comment by @mpizenberg on 2024-10-17 12:00_

Indeed, any incompatibility with a single solution could be transitively added to the set of unchangable facts.

---

_@zanieb approved on 2024-10-17 13:35_

Cool!

---

_Label `performance` added by @zanieb on 2024-10-17 13:35_

---

_Merged by @konstin on 2024-10-18 11:44_

---

_Closed by @konstin on 2024-10-18 11:44_

---

_Branch deleted on 2024-10-18 11:44_

---

_Comment by @Eh2406 on 2024-10-21 22:00_

PubGrub theoretically could do either optimization discussed here. But I doubt that in practice it does. (Well, not until backtracking hits forcing some generalizations.)

I suspect that it will start out with a chain:
- decision level 0: `Root` is needed
- decision level 1: `Root` need `workspace_member`
- decision level 2: `workspace_member` needs `bar`

Making the information about `bar` unavailable for this optimization. In some sense PubGrub is correct, perhaps there is a different version of `workspace_member` that doesn't depend on `bar`.

If this becomes a problem I can see 2 directions for solving it:
- Teach PubGrub to identify singleton requirements. When a singleton is identified, selecting a version for that requirement does not bump the decision level. Alternatively, load in the dependencies for that version before "activating" the version. This should squash it all into dependency level 0.
- In your solver loop keep track of the decision level when the first non-workspace_member was activated. Next change `unchanging_term_for_package` to be more like `term_for_package_as_of(&P, decision level)`, so that you can provide your own decision level that represents `unchanging` given your external knowledge.

Mostly leaving notes here while it's fresh in my mind, so that if there's a problem in the future we remember what the next steps are.

---

_Comment by @konstin on 2024-10-22 11:02_

> Teach PubGrub to identify singleton requirements.

This would be great, because we have two kinds of singleton requirements, URL requirements and `==` requirements, with which we can shortcut package and version selection in pubgrub.

---
