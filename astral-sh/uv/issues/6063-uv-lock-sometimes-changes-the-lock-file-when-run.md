```yaml
number: 6063
title: "`uv lock` sometimes changes the lock file when run for a second time"
type: issue
state: closed
author: BurntSushi
labels:
  - bug
assignees: []
created_at: 2024-08-13T17:24:04Z
updated_at: 2024-08-15T00:00:16Z
url: https://github.com/astral-sh/uv/issues/6063
synced_at: 2026-01-12T15:59:01Z
```

# `uv lock` sometimes changes the lock file when run for a second time

---

_@BurntSushi_

I believe it is true that this should never happen. To reproduce, create a `pyproject.toml` with:

```toml
[project]
name = "uv-lock-instability"
version = "0.1.0"
description = "whatever"
requires-python = ">=3.9.0"
dependencies = [
    "jax==0.4.17",
]
```

And now do a lock followed by another lock:

```
$ rm -f *.lock
$ cargo r --manifest-path ~/astral/uv/Cargo.toml -p uv -- lock
$ cp uv.lock before.lock
$ cargo r --manifest-path ~/astral/uv/Cargo.toml -p uv -- lock
$ cp uv.lock after.lock
$ diff before.lock after.lock
15c15
<     { name = "zipp" },
---
>     { name = "zipp", marker = "python_version < '3.10'" },
```

The `diff` output at the end _should_ be empty, but it isn't.

Running the same steps in the `ecosystem/transformers` directory produces a larger diff (and this MRE was minimized from that).

---

_Label `bug` added by @BurntSushi on 2024-08-13 17:24_

---

_Comment by @BurntSushi on 2024-08-13 17:59_

If the dependency specification is changed to `jax==0.4.18`, then the instability no longer happens. So there's something in the difference in dependency metadata here that might give a clue:

```
[andrew@duff instability-mre]$ diff 0.4.17.md 0.4.18.md
3c3
< Version: 0.4.17
---
> Version: 0.4.18
20a21,22
> Requires-Dist: numpy >=1.23.2 ; python_version >= "3.11"
> Requires-Dist: numpy >=1.26.0 ; python_version >= "3.12"
24c26
< Requires-Dist: jaxlib ==0.4.16 ; extra == 'ci'
---
> Requires-Dist: jaxlib ==0.4.17 ; extra == 'ci'
26c28
< Requires-Dist: jaxlib ==0.4.17 ; extra == 'cpu'
---
> Requires-Dist: jaxlib ==0.4.18 ; extra == 'cpu'
28c30
< Requires-Dist: jaxlib ==0.4.17+cuda11.cudnn86 ; extra == 'cuda'
---
> Requires-Dist: jaxlib ==0.4.18+cuda11.cudnn86 ; extra == 'cuda'
30c32
< Requires-Dist: jaxlib ==0.4.17+cuda11.cudnn86 ; extra == 'cuda11_cudnn86'
---
> Requires-Dist: jaxlib ==0.4.18+cuda11.cudnn86 ; extra == 'cuda11_cudnn86'
32c34
< Requires-Dist: jaxlib ==0.4.17+cuda11.cudnn86 ; extra == 'cuda11_local'
---
> Requires-Dist: jaxlib ==0.4.18+cuda11.cudnn86 ; extra == 'cuda11_local'
34c36
< Requires-Dist: jaxlib ==0.4.17+cuda11.cudnn86 ; extra == 'cuda11_pip'
---
> Requires-Dist: jaxlib ==0.4.18+cuda11.cudnn86 ; extra == 'cuda11_pip'
42a45
> Requires-Dist: nvidia-nccl-cu11 >=2.18.3 ; extra == 'cuda11_pip'
44c47
< Requires-Dist: jaxlib ==0.4.17+cuda12.cudnn89 ; extra == 'cuda12_local'
---
> Requires-Dist: jaxlib ==0.4.18+cuda12.cudnn89 ; extra == 'cuda12_local'
46c49
< Requires-Dist: jaxlib ==0.4.17+cuda12.cudnn89 ; extra == 'cuda12_pip'
---
> Requires-Dist: jaxlib ==0.4.18+cuda12.cudnn89 ; extra == 'cuda12_pip'
54a58
> Requires-Dist: nvidia-nccl-cu12 >=2.18.3 ; extra == 'cuda12_pip'
58,59c62,63
< Requires-Dist: jaxlib ==0.4.17 ; extra == 'tpu'
< Requires-Dist: libtpu-nightly ==0.1.dev20231003 ; extra == 'tpu'
---
> Requires-Dist: jaxlib ==0.4.18 ; extra == 'tpu'
> Requires-Dist: libtpu-nightly ==0.1.dev20231006 ; extra == 'tpu'
```

---

_Comment by @charliermarsh on 2024-08-13 21:53_

I think the source of this instability is that _some_ pieces of code are able to enforce the connection between `python_version` and `python_full_version`, but the marker disjointness operations do not.

Specifically, `markers::requires_python` returns a marker that constrains _both_, no matter _which_ of the two is present in the input tree. So we sometimes filter out branches based on this when resolving, but then fail to compute the correct union when we lock. At least, that's the cause of the instability in https://github.com/astral-sh/uv/pull/6065...

---

_Comment by @charliermarsh on 2024-08-13 21:55_

Another observation is that this fork shouldn't exist:

```
DEBUG Solving split python_full_version > '3.10' and python_version < '3.10' (requires-python: python_full_version > '3.10' and python_version > '3.10')
```

Notice that the marker and the requires-python are disjoint. (The marker itself is `false`, but even if we don't merge `python_full_version` and `python_version`, this should still be identified as disjoint.)

---

_Comment by @charliermarsh on 2024-08-13 22:06_

Something like this diff, which resolves the instability (but then we still write the "bad" fork marker to the lockfile):

```diff
 impl Forks {
-    fn new(name_to_deps: BTreeMap<PackageName, Vec<PubGrubDependency>>) -> Forks {
+    fn new(
+        name_to_deps: BTreeMap<PackageName, Vec<PubGrubDependency>>,
+        requires_python: Option<&MarkerTree>,
+    ) -> Self {
         let mut forks = vec![Fork {
             dependencies: vec![],
             markers: MarkerTree::TRUE,
@@ -2780,6 +2788,7 @@ impl Forks {
                 let mut new = vec![];
                 for mut fork in std::mem::take(&mut forks) {
                     if fork.markers.is_disjoint(&markers) {
+                        // If the fork is disjoint with his fork's markers, just re-add it.
                         new.push(fork);
                         continue;
                     }
@@ -2789,11 +2798,25 @@ impl Forks {
                     if !fork.markers.is_disjoint(&not_markers) {
                         let mut new_fork = fork.clone();
                         new_fork.intersect(not_markers);
-                        new.push(new_fork);
+                        if !requires_python.is_some_and(|requires_python| {
+                            requires_python.is_disjoint(&new_fork.markers)
+                                || marker::requires_python(&new_fork.markers).is_some_and(
+                                    |python| python.to_marker_tree().is_disjoint(requires_python),
+                                )
+                        }) {
+                            new.push(new_fork);
+                        }
                     }
                     fork.dependencies.push(dep.clone());
                     fork.intersect(markers);
-                    new.push(fork);
+                    if !requires_python.is_some_and(|requires_python| {
+                        requires_python.is_disjoint(&fork.markers)
+                            || marker::requires_python(&fork.markers).is_some_and(|python| {
+                                python.to_marker_tree().is_disjoint(requires_python)
+                            })
+                    }) {
+                        new.push(fork);
+                    }
                     markers = new_markers;
                 }
                 forks = new;
```

---

_Comment by @charliermarsh on 2024-08-13 22:33_

Sorry, I should clarify: all my comments are about the instability in https://github.com/astral-sh/uv/pull/6065. I haven't looked at the instability here at all.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-14 02:00_

---

_Comment by @charliermarsh on 2024-08-14 02:05_

@BurntSushi -- I'll look at this, I'm so deep on these problems right now anyway.

---

_Comment by @charliermarsh on 2024-08-14 02:17_

I think there are a few different problems, but here's one:

- The first fork we solve for is `python_version < 3.10`.
- `jax` depends on: `importlib-metadata >=4.6 ; python_version < "3.10"`.
- So in that fork, we add `importlib-metadata`.
- `importlib-metadata` depends on: `zipp >=0.5`
- So we solve and include it.

The other forks then include:
```
edge: ResolutionDependencyEdge { from: Some(PackageName("jax")), from_version: "0.4.17", from_url: None, from_extra: None, from_dev: None, to: PackageName("importlib-metadata"), to_version: "8.2.0", to_url: None, to_extra: None, to_dev: None, marker: python_version < '3.10' }
```

And:

```
edge: ResolutionDependencyEdge { from: Some(PackageName("importlib-metadata")), from_version: "8.2.0", from_url: None, from_extra: None, from_dev: None, to: PackageName("zipp"), to_version: "3.20.0", to_url: None, to_extra: None, to_dev: None, marker: true }
```

Because we solve for `importlib-metadata` _before_ we fork! So each fork includes this dependency and edge, even though it's not relevant to all of them.

The `true` marker in the second case is problematic. It causes us to remove the `python_version < '3.10'` when we write out the lockfile.


---

_Comment by @charliermarsh on 2024-08-14 02:19_

I suspect @BurntSushi's more aggressive forking strategy would fix this, for one.

---

_Comment by @BurntSushi on 2024-08-14 15:18_

I can confirm that the aggressive forking strategy does fix this particular case of instability.

---

_Comment by @charliermarsh on 2024-08-14 15:20_

I think the other way we could fix it is: write the full, standardized metadata to the lockfile.

Then, when we resolve, don't seed the forks. Just proceed with with a "normal" resolution. But continue to respect the fork-preference mapping. And maybe reject the resolution at the end if it creates a fork we didn't see last time?


---

_Closed by @charliermarsh on 2024-08-15 00:00_

---
