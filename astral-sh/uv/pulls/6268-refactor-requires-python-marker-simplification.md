```yaml
number: 6268
title: refactor requires-python marker simplification
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/reveal-what-is-hidden
created_at: 2024-08-20T18:52:15Z
updated_at: 2024-09-03T22:41:16Z
url: https://github.com/astral-sh/uv/pull/6268
synced_at: 2026-01-12T16:07:18Z
```

# refactor requires-python marker simplification

---

_@BurntSushi_

This commit refactors how deal with `requires-python` so that instead of
simplifying markers of dependencies inside the resolver, we do it at the
edges of our system. When writing markers to output, we simplify when
there's an obvious `requires-python` context. And when reading markers
as input, we complexity markers with the relevant `requires-python`
constraint.

There are some lingering problems though:

* This PR doesn't leverage the type system as much as I would hope to
force folks into dealing with marker serialization correctly. It's a
very subtle problem and I think we should invest in that, but I think
it would be better in a follow-up PR since this one is already pretty
big.
* Some of the snapshot updates are wrong, some are fixing a bug and
still others require investigation to figure out whether they're
correct or not.
* I side-stepped the issue of `MarkerTree`'s hidden state by
implementing requires-python marker simplification in a cursed way:
run `restrict_versions`, serialize the marker to string form and then
re-parse it. This should ensure any hidden state in the `MarkerTree`
is removed. Of course, this isn't what we should do. I think we should
just remove the hidden state entirely, but I took a shortcut to prove
out the idea.

Fixes #6269, Fixes #6412, Fixes #6836

---

_@zanieb reviewed on 2024-08-20 18:54_

---

_Review comment by @zanieb on `crates/uv/tests/lock.rs`:8666 on 2024-08-20 18:54_

Yeah — this seems wrong in this context.

---

_@zanieb reviewed on 2024-08-20 21:36_

---

_Review comment by @zanieb on `crates/uv/tests/lock.rs`:8666 on 2024-08-20 21:36_

Looking at this..

Moving the transformation call so we can see the display before mutation...

```diff
diff --git a/crates/uv-resolver/src/error.rs b/crates/uv-resolver/src/error.rs
index 4a38e3e30..dc9eeecf8 100644
--- a/crates/uv-resolver/src/error.rs
+++ b/crates/uv-resolver/src/error.rs
@@ -222,7 +222,6 @@ impl std::fmt::Display for NoSolutionError {
 
         // Transform the error tree for reporting
         let mut tree = self.error.clone();
-        simplify_derivation_tree_markers(&self.python_requirement, &mut tree);
         let should_display_tree = std::env::var_os("UV_INTERNAL__SHOW_DERIVATION_TREE").is_some()
             || tracing::enabled!(tracing::Level::TRACE);
 
@@ -230,6 +229,7 @@ impl std::fmt::Display for NoSolutionError {
             display_tree(&tree, "Resolver derivation tree before reduction");
         }
 
+        simplify_derivation_tree_markers(&self.python_requirement, &mut tree);
         collapse_no_versions_of_workspace_members(&mut tree, &self.workspace_members);
```

Using a `cargo insta test` invocation:

```
UV_INTERNAL__SHOW_DERIVATION_TREE=1 cit unconditional_overlapping_marker_disjoint_version_constraints
```

We get the following trees:

```
Resolver derivation tree before reduction
  root==0a0.dev0 depends on project*
      project==0.1.0 depends on datasets{python_full_version >= '3.11'}>=2.19
      no versions of datasets{python_full_version >= '3.11'}>=2.19
    no versions of project<0.1.0 | >0.1.0
Resolver derivation tree after reduction
  project==0.1.0 depends on datasets>=2.19
  no versions of datasets>=2.19
```

It looks like your simplifications here are fine, the tree is wrong before then.

Here are the (correct) trees from main:

```
Resolver derivation tree before reduction
  root==0a0.dev0 depends on project*
      project==0.1.0 depends on datasets>=2.19
      project==0.1.0 depends on datasets<2.19
    no versions of project<0.1.0 | >0.1.0
Resolver derivation tree after reduction
  project==0.1.0 depends on datasets>=2.19
  project==0.1.0 depends on datasets<2.19
```



---

_@zanieb reviewed on 2024-08-20 21:45_

---

_Review comment by @zanieb on `crates/uv/tests/lock.rs`:8666 on 2024-08-20 21:45_

Without `UV_EXCLUDE_NEWER`,  the message is entirely unhinged. 

```
Resolver derivation tree before reduction
    root==0a0.dev0 depends on project*
        project==0.1.0 depends on datasets{python_full_version >= '3.11'}>=2.19
        project==0.1.0 depends on datasets<2.19
        no versions of datasets{python_full_version >= '3.11'}>2.19.0, <2.19.1 | >2.19.1, <2.19.2 | >2.19.2, <2.20.0 | >2.20.0, <2.21.0 | >2.21.0
    no versions of project<0.1.0 | >0.1.0
Resolver derivation tree after reduction
    project==0.1.0 depends on datasets>=2.19
    project==0.1.0 depends on datasets<2.19
    no versions of datasets>2.19.0, <2.19.1 | >2.19.1, <2.19.2 | >2.19.2, <2.20.0 | >2.20.0, <2.21.0 | >2.21.0
```

```
× No solution found when resolving dependencies:
╰─▶ Because only the following versions of datasets are available:
        datasets<=2.19.0
        datasets==2.19.1
        datasets==2.19.2
        datasets==2.20.0
        datasets==2.21.0
    and your project depends on datasets<2.19, we can conclude that your project and datasets>=2.19.0 are incompatible.
    And because your project depends on datasets>=2.19, we can conclude that your project's requirements are unsatisfiable.
```

---

_Comment by @codspeed-hq[bot] on 2024-08-21 18:56_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/ag/reveal-what-is-hidden)

### Merging #6268 will **not alter performance**

<sub>Comparing <code>ag/reveal-what-is-hidden</code> (92e68dd) with <code>main</code> (4a6bea5)</sub>



### Summary

`✅ 14` untouched benchmarks






---

_Comment by @BurntSushi on 2024-08-23 19:04_

Here is my current state: I still cannot fully explain the diff on the `transformers` ecosystem test and I haven't yet investigated the root cause in the change to the error message.

I've spent a lot of time looking at the `transformers` test case. I've minimized the `pyproject.toml` down a bit, although it is still quite a large number of dependencies:

```toml
[project]
name = "transformers"
version = "4.39.0.dev0"
requires-python = ">=3.9.0"
dependencies = []

[project.urls]
repository = "https://github.com/huggingface/transformers"

[project.optional-dependencies]
accelerate = [
    "accelerate>=0.21.0",
]
flax = [
    "flax>=0.4.1,<=0.7.0",
]
torch-speech = [
    "librosa",
]
quality = [
    "datasets!=2.5.0",
]
all = [
    "tensorflow>=2.6,<2.16",
    "onnxconverter-common",
    "tensorflow-text<2.16",
    "jax>=0.4.1,<=0.4.13",
]
agents = [
    "opencv-python",
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

Then lock this using `main`. Then `mv uv.lock main.lock` and re-lock using this PR. You end up with a pretty sizeable diff:

```
$ diff main.lock pr1.lock | wc -l
434
```

Here's one example of a difference. This is from `main`:

```toml
[[package]]
name = "tensorflow-macos"
version = "2.15.1"
source = { registry = "https://pypi.org/simple" }
resolution-markers = [
    "python_full_version < '3.10' and platform_machine == 'aarch64' and platform_system == 'Linux'",
]
dependencies = [
    { name = "tensorflow-cpu-aws", marker = "python_full_version < '3.10' and platform_machine == 'aarch64' and platform_system == 'Linux'" },
]
wheels = [
    { url = "https://files.pythonhosted.org/packages/b3/c8/b90dc41b1eefc2894801a120cf268b1f25440981fcf966fb055febce8348/tensorflow_macos-2.15.1-cp310-cp310-macosx_12_0_arm64.whl", hash = "sha256:b8f01d7615fe4ff3b15a12f84471bd5344fed187543c4a091da3ddca51b6dc26", size = 2158 },
    { url = "https://files.pythonhosted.org/packages/bc/11/b73387ad260614ec43c313a630d14fe5522455084abc207fce864aaa3d73/tensorflow_macos-2.15.1-cp311-cp311-macosx_12_0_arm64.whl", hash = "sha256:58fca6399665f19e599c591c421672d9bc8b705409d43ececd0931d1d3bc6a7e", size = 2159 },
    { url = "https://files.pythonhosted.org/packages/3a/54/95b9459cd48d92a0522c8dd59955210e51747a46461bcedb64a9a77ba822/tensorflow_macos-2.15.1-cp39-cp39-macosx_12_0_arm64.whl", hash = "sha256:cca3c9ba5b96face05716792cb1bcc70d84c5e0c34bfb7735b39c65d0334b699", size = 2158 },
]
```

And this is from this branch:

```toml
[[package]]
name = "tensorflow-macos"
version = "2.15.1"
source = { registry = "https://pypi.org/simple" }
resolution-markers = [
    "python_full_version < '3.10' and platform_machine == 'arm64' and platform_system == 'Darwin'",
]
wheels = [
    { url = "https://files.pythonhosted.org/packages/b3/c8/b90dc41b1eefc2894801a120cf268b1f25440981fcf966fb055febce8348/tensorflow_macos-2.15.1-cp310-cp310-macosx_12_0_arm64.whl", hash = "sha256:b8f01d7615fe4ff3b15a12f84471bd5344fed187543c4a091da3ddca51b6dc26", size = 2158 },
    { url = "https://files.pythonhosted.org/packages/bc/11/b73387ad260614ec43c313a630d14fe5522455084abc207fce864aaa3d73/tensorflow_macos-2.15.1-cp311-cp311-macosx_12_0_arm64.whl", hash = "sha256:58fca6399665f19e599c591c421672d9bc8b705409d43ececd0931d1d3bc6a7e", size = 2159 },
    { url = "https://files.pythonhosted.org/packages/3a/54/95b9459cd48d92a0522c8dd59955210e51747a46461bcedb64a9a77ba822/tensorflow_macos-2.15.1-cp39-cp39-macosx_12_0_arm64.whl", hash = "sha256:cca3c9ba5b96face05716792cb1bcc70d84c5e0c34bfb7735b39c65d0334b699", size = 2158 },
]
```

Here's another example. From `main`:

```toml
[[package]]
name = "flatbuffers"
version = "24.3.25"
source = { registry = "https://pypi.org/simple" }
resolution-markers = [
    "python_full_version < '3.10' and platform_machine == 'aarch64' and platform_system == 'Linux'",
]
```

From this branch:

```toml
[[package]]
name = "flatbuffers"
version = "24.3.25"
source = { registry = "https://pypi.org/simple" }
resolution-markers = [
    "python_full_version < '3.10' and platform_machine == 'arm64' and platform_system == 'Darwin'",
]
```

This represents a key thing I don't yet understand: _why_ does `flatbuffers 24.3.25` end up in only one fork? Why doesn't it end up in _both_ forks in _both_ cases? I don't know.

One thing I do know is that this is related to fork prioritization. If I remove `forks.sort()` from `uv-resolver/src/resolver/mod.rs` in _both_ `main` and this branch, then the differences in the lock file go away.

Because of that, I spent some time trying to making fork prioritization in this branch work in the same way as in `main`. But I think this is very hard to do because of the different `requires-python` handling _and_ because of the hidden state for Python versions inside of `MarkerTree`. I think fork prioritization in this branch, as-is, matches the spirit of what it's trying to do: solve for more constraining `python_version` requirements before less constraining.

Here are my thoughts in summary for where to go from here:

* Keep trying to discover whether the resolution in this branch is actually incorrect, or if it is just a "different but correct" resolution. If both resolutions are correct, then I'd be tempted to merge (after fixing the error message issue) since this PR does fix other outstanding bugs.
* I am wondering whether _both_ resolutions are actually incorrect, and perhaps there is a deeper unresolved bug that this PR is exacerbating or just letting manifest in a different way. What leads me to this thought is the shenanigans around `tensorflow-macos` in both lock files. Moreover, in both cases, dependencies like `flatbuffers` have a version that end up in one single fork, but the fork it ends up in is different between this branch in `main`. Why is that? Should it be in both in both cases?
* I believe it is correct to say that the instigator for all of this is `numpy`. Because the dependency tree is so large, there are a number of different `numpy` dependencies with interesting markers. I believe it is the only reason we fork at all in this case.

This is rather odd overall

---

_Marked ready for review by @BurntSushi on 2024-08-30 16:07_

---

_Review requested from @charliermarsh by @BurntSushi on 2024-08-30 16:07_

---

_Review requested from @konstin by @BurntSushi on 2024-08-30 16:07_

---

_Review requested from @ibraheemdev by @BurntSushi on 2024-08-30 16:07_

---

_@BurntSushi reviewed on 2024-08-30 16:17_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/requires_python.rs`:365 on 2024-08-30 16:17_

Note that this seems redundant with the recently added `markers` method. I tried using `markers` instead, but it actually resulted in a bunch of test failures. There are some subtle differences between my implementation (which revives it from older code) and the new `markers` method that I haven't investigated yet. It looks like it might come down to `python_full_version` versus `python_version`. cc @charliermarsh 

---

_Comment by @BurntSushi on 2024-08-30 16:20_

I think the `resolve_warm_jupyter_universal` benchmark is pretty noisy. We should probably be good to merge. With that said, this PR does put more pressure on marker ops that could probably be amortized with some refactoring. For example:

```
impl Forks {
    fn new(
        name_to_deps: BTreeMap<PackageName, Vec<PubGrubDependency>>,
        python_requirement: &PythonRequirement,
    ) -> Forks {
        let python_marker = python_requirement.to_marker_tree();
        let python_marker = python_marker.as_ref();
```

---

_Review comment by @konstin on `crates/uv-resolver/src/python_requirement.rs`:91 on 2024-08-30 17:51_

target: "If `None`, the target version is the same as the installed version."

Shouldn't this method always return a marker tree since we always have some minimum Python version?

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/mod.rs`:1581 on 2024-08-30 17:53_

```suggestion
                            Some(Cow::Owned(constraint))
```

---

_@konstin approved on 2024-08-30 18:01_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/requires_python.rs`:365 on 2024-09-01 17:39_

My implementation actually had a crucial bug in it (#6902).

---

_@charliermarsh reviewed on 2024-09-01 17:39_

---

_@charliermarsh reviewed on 2024-09-01 17:40_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:1581 on 2024-09-01 17:40_

`Some(Cow::Owned(constraint.clone()))` seems strange, can the `.clone` be dropped?

---

_@charliermarsh approved on 2024-09-01 17:40_

---

_@charliermarsh reviewed on 2024-09-01 17:41_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/requires_python.rs`:365 on 2024-09-01 17:41_

Can you try removing my impl's usages with this method? (Or use my impl?) Just a brief try after rebase would be appreciated. If you still see failures, let me know.

---

_@charliermarsh reviewed on 2024-09-01 17:41_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/requires_python.rs`:545 on 2024-09-01 17:41_

Can you document `greater`? Or make it an enum to self-document a bit?

---

_@BurntSushi reviewed on 2024-09-03 14:51_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/python_requirement.rs`:91 on 2024-09-03 14:51_

Yup. @charliermarsh's recent refactor forced this issue as well (for the better).

---

_@BurntSushi reviewed on 2024-09-03 14:51_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/mod.rs`:1581 on 2024-09-03 14:51_

Yup!

---

_@BurntSushi reviewed on 2024-09-03 14:52_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/requires_python.rs`:545 on 2024-09-03 14:52_

Thankfully I just got rid of this entirely as your `markers()` implementation (with that bug fix) now works.

(I'm also not a huge fan of boolean parameters either, but tend to abide them for unexported helpers that are used once or twice in lieu of adding another type. But I don't feel too strongly about it.)

---

_@BurntSushi reviewed on 2024-09-03 14:53_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/requires_python.rs`:365 on 2024-09-03 14:53_

Yup, nice fix. It works.

---

_Comment by @BurntSushi on 2024-09-03 17:48_

I've added regression tests for #6269, #6412 and #6836.

---

_Merged by @BurntSushi on 2024-09-03 22:41_

---

_Closed by @BurntSushi on 2024-09-03 22:41_

---

_Branch deleted on 2024-09-03 22:41_

---
