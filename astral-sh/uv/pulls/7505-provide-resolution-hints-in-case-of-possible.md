```yaml
number: 7505
title: Provide resolution hints in case of possible local name conflicts
type: pull_request
state: merged
author: lucab
labels:
  - error messages
assignees: []
merged: true
base: main
head: ups/resolver-dependency-cycle
created_at: 2024-09-18T15:29:48Z
updated_at: 2024-09-20T18:07:34Z
url: https://github.com/astral-sh/uv/pull/7505
synced_at: 2026-01-10T12:53:49Z
```

# Provide resolution hints in case of possible local name conflicts

---

_Pull request opened by @lucab on 2024-09-18 15:29_

This enhances the hints generator in the resolver with some heuristic to detect and warn in case of failures due to version mismatches on a local package. Those may be the symptom of name conflict/shadowing with a transitive dependency.

Closes: https://github.com/astral-sh/uv/issues/7329


---

_Review comment by @lucab on `crates/uv-resolver/src/pubgrub/report.rs`:542 on 2024-09-18 16:17_

Ugh, this logic is too weak and gets tricked into false positives, I think from the root itself appearing as a normal package down the chain even on other kind of failures.
I think I need to enhance this heuristic in order to detect the cycle.

---

_@lucab reviewed on 2024-09-18 16:17_

---

_Review comment by @zanieb on `crates/uv-resolver/src/pubgrub/report.rs`:732 on 2024-09-19 00:22_

Unfortunately this heuristic doesn't work for the invocation in the original report (I tested by dropping the pin from your test). Maybe this would be helped by transforming the tree in as tracked by #7524 but I'm not sure. It looks like it might just be a bug because the tree does still contain a cycle with the same structure.

Have you used the `UV_INTERNAL__SHOW_DERIVATION_TREE=1` utility? It can be helpful to display the tree in it's raw form. 

Without a version pin:

```
Resolver derivation tree after reduction
  dagster==0.0.0 depends on dagster-webserver*
    dagster-webserver==1.8.7 depends on dagster==1.8.7
       ... an enumeration of all the versions follows
```

With a version pin:

```
Resolver derivation tree before reduction
  root==0a0.dev0 depends on dagster*
      dagster==0.0.0 depends on dagster-webserver==1.6.13
      dagster-webserver==1.6.13 depends on dagster==1.6.13
    no versions of dagster<0.0.0 | >0.0.0
Resolver derivation tree after reduction
  dagster==0.0.0 depends on dagster-webserver==1.6.13
  dagster-webserver==1.6.13 depends on dagster==1.6.13
```

On a separate note, it's pretty hard to understand what this heuristic is doing. I know you plan to do some polish on it, and it's not your fault that the data structure is complicated, but I can't review it without some additional context on the idea.

---

_@zanieb reviewed on 2024-09-19 00:22_

---

_Comment by @zanieb on 2024-09-20 02:54_

I took a look at this quick and I think the detection for the case we care about is complicated than it needs to be. I took a swing at a simpler heuristic in https://github.com/astral-sh/uv/commit/9bf7a70a7e1ae5c92913a9a19e3956ec7196e31e. There, we just check if there's a non-workspace member that depends on a workspace member; I think this should only occur if the workspace member is shadowing a remote package (though I'm not 100% certain). Since this is just a hint that happens when resolution fails, I think we can risk false positives here. I'd still like to add some tests for workspace members that depend on each other — it seems like a bit of a pain to construct a resolution failure with the proper shape though.

Curious for your thoughts.

---

_Comment by @lucab on 2024-09-20 11:12_

Thanks for taking the time for a deeper look into the tree logic.

> we just check if there's a non-workspace member that depends on a workspace member

Recapping, we came up with several heuristics:
 1. detecting a dependency failure on a local package in a non-root variant (too broad, prone to false positives in several ways)
 2. detecting a dependency loop involving a local package (too strict, prone to false negatives in case of multiple available options)
 3. loop-detection (from point 2) plus a dependency failure on a local package (overly complex)
 4. a dependency failure on a local package

Point 4 should effectively be a subset of point 3, as one of the necessary conditions in the more complex check is that "there exists a failed dependency toward a local package". I think this is also a sufficient condition, so I agree that your code above is an equivalent but simpler check, and I believe it should have a similar amount of false positive. Overall, for the task at hand I prefer yours over my version.

I massaged it into this PR and fixed the tests, CI is fine with both the previous and new version, as you prefer.

> some tests for workspace members that depend on each other

The testing space around this is quite large, which scenario do you want to check? The same as existing tests but in a sub-package, or a cycle not involving an external package?

---

_Comment by @zanieb on 2024-09-20 13:50_

> The testing space around this is quite large, which scenario do you want to check? The same as existing tests but in a sub-package, or a cycle not involving an external package?

Yeah I think we would need to have a resolution error where a workspace package depending on another workspace package is in the tree (but the error is caused by something else). We would want to see that the hint is _not_ displayed. I think this might be more trouble than it's worth at the moment, it seems hard to create.

---

_Marked ready for review by @lucab on 2024-09-20 17:41_

---

_Label `error messages` added by @zanieb on 2024-09-20 18:06_

---

_@zanieb approved on 2024-09-20 18:07_

---

_Merged by @zanieb on 2024-09-20 18:07_

---

_Closed by @zanieb on 2024-09-20 18:07_

---
