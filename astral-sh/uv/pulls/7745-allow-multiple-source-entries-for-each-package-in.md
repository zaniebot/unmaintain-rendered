```yaml
number: 7745
title: "Allow multiple source entries for each package in `tool.uv.sources`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/multi-sources
created_at: 2024-09-27T18:29:43Z
updated_at: 2024-09-30T21:16:46Z
url: https://github.com/astral-sh/uv/pull/7745
synced_at: 2026-01-10T12:53:54Z
```

# Allow multiple source entries for each package in `tool.uv.sources`

---

_Pull request opened by @charliermarsh on 2024-09-27 18:29_

## Summary

This PR enables users to provide multiple source entries in `tool.uv.sources`, e.g.:

```toml
[tool.uv.sources]
httpx = [
  { git = "https://github.com/encode/httpx", tag = "0.27.2", marker = "sys_platform == 'darwin'" },
  { git = "https://github.com/encode/httpx", tag = "0.24.1", marker = "sys_platform == 'linux'" },
]
```

The implementation is relatively straightforward: when we lower the requirement, we now return an iterator rather than a single requirement. In other words, the above is transformed into two requirements:

```txt
httpx @ git+https://github.com/encode/httpx@0.27.2 ; sys_platform == 'darwin'
httpx @ git+https://github.com/encode/httpx@0.24.1 ; sys_platform == 'linux'
```

We verify (at deserialization time) that the markers are non-overlapping.

Closes https://github.com/astral-sh/uv/issues/3397.


---

_Review requested from @BurntSushi by @charliermarsh on 2024-09-27 23:23_

---

_Review requested from @zanieb by @charliermarsh on 2024-09-27 23:23_

---

_Review requested from @konstin by @charliermarsh on 2024-09-27 23:23_

---

_Label `enhancement` added by @charliermarsh on 2024-09-27 23:23_

---

_Marked ready for review by @charliermarsh on 2024-09-27 23:23_

---

_@charliermarsh reviewed on 2024-09-27 23:24_

---

_Review comment by @charliermarsh on `crates/uv-workspace/src/pyproject.rs`:509 on 2024-09-27 23:24_

@BurntSushi -- I'd love your eyes on this file in particular. Should I be enforcing these at deserialization time? I could easily enforce them when we actually _use_ the lowered requirement. I'm not certain what's best.

---

_Review comment by @charliermarsh on `crates/uv-workspace/src/pyproject.rs`:517 on 2024-09-27 23:24_

@ibraheemdev -- Is it correct to use `Option<MarkerTreeContents>` for this? It's a field that users can provide in TOML.

---

_@charliermarsh reviewed on 2024-09-27 23:24_

---

_@charliermarsh reviewed on 2024-09-27 23:25_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/metadata/lowering.rs`:60 on 2024-09-27 23:25_

I find this slightly awkward: we return an iterator, but if the "overall" requirement fails, we use once with an error. Perhaps the return type should more truthfully be: `Result<impl Iterator<Item = Result<LoweredRequirement, LoweringError>>, LoweringError>`

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/metadata/lowering.rs`:292 on 2024-09-27 23:25_

This file is mostly just the code being indented and put under an iterator. Very minimal changes.

---

_@charliermarsh reviewed on 2024-09-27 23:25_

---

_@charliermarsh reviewed on 2024-09-27 23:27_

---

_Review comment by @charliermarsh on `crates/pep508-rs/src/marker/tree.rs`:1645 on 2024-09-27 23:27_

\cc @ibraheemdev -- Does this make sense for a user-provided marker field?

---

_@charliermarsh reviewed on 2024-09-27 23:29_

---

_Review comment by @charliermarsh on `uv.schema.json`:1437 on 2024-09-27 23:29_

I think this is wrong (it thinks it _has_ to be a list).

---

_Review comment by @konstin on `docs/concepts/dependencies.md`:263 on 2024-09-28 14:22_

In these expressions, do we accept extras and support the following (assuming index api has landed)?
```toml
[project.optional-dependencies]
cu118 = []
cu121 = []
cu124 = []

[tool.uv.sources]
torch = [
    { index = "torch-cu118", marker = "extra == 'cu118'"},
    { index = "torch-cu124", marker = "extra == 'cu124'"},
    # cu121 is on pypi
]

[[tool.uv.index]]
name = "torch-cu118"
url = "https://download.pytorch.org/whl/cu118"

[[tool.uv.index]]
name = "torch-cu124"
url = "https://download.pytorch.org/whl/cu124"
```

---

_Review comment by @konstin on `uv.schema.json`:1437 on 2024-09-28 14:44_

It looks correct to and check-jsonschema does only complain about "tree" below:
```toml
[tool.uv.sources]
tqdm = { git = "https:/github.com/tqdm/tqdm" }
httpx = [
  { git = "https://github.com/encode/httpx", tag = "0.27.2", marker = "sys_platform == 'darwin'" },
  { git = "https://github.com/encode/httpx", tag = "0.24.1", marker = "sys_platform == 'linux'" },
]
foo = "tree"
```

---

_Review comment by @konstin on `crates/uv/src/commands/project/run.rs`:208 on 2024-09-28 14:51_

nit:
```suggestion
                    .map_ok(move |requirement| requirement.into_inner())
```

---

_Review comment by @konstin on `crates/uv/tests/lock.rs`:13459 on 2024-09-28 14:55_

powerful

---

_Review comment by @konstin on `crates/uv-distribution/src/metadata/lowering.rs`:60 on 2024-09-28 14:58_

We lower only for workspace packages, there are few items and the iterator looks too complex for this use case, should we collect into a `Result<Vec<LoweredRequirement>, LoweringError>`?

---

_Review comment by @konstin on `docs/concepts/dependencies.md`:226 on 2024-09-28 15:13_

The is one big catch we need to mention (and should check in code): The metadata needs to consistent between the sources, otherwise it's UB.

---

_@konstin approved on 2024-09-28 15:13_

---

_@charliermarsh reviewed on 2024-09-28 16:02_

---

_Review comment by @charliermarsh on `docs/concepts/dependencies.md`:226 on 2024-09-28 16:02_

I think different metadata is totally fine, no?

---

_@charliermarsh reviewed on 2024-09-28 16:02_

---

_Review comment by @charliermarsh on `uv.schema.json`:1437 on 2024-09-28 16:02_

(I think I fixed it between the time I wrote this comment and your testing. Thanks so much for testing it.)

---

_@charliermarsh reviewed on 2024-09-28 16:04_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/metadata/lowering.rs`:60 on 2024-09-28 16:04_

I suppose that's true -- this does only apply to workspace packages. I'd like @BurntSushi opinion on it too.

---

_@konstin reviewed on 2024-09-28 16:16_

---

_Review comment by @konstin on `docs/concepts/dependencies.md`:226 on 2024-09-28 16:16_

Given:
```toml

        [tool.uv.sources]
        iniconfig = [
            { url = "https://files.pythonhosted.org/packages/ef/a6/62565a6e1cf69e10f5727360368e451d4b7f58beeac6173dc9db836a5b46/iniconfig-2.0.0-py3-none-any.whl", marker = "sys_platform != 'win32'" },
            { url = "https://files.pythonhosted.org/packages/d7/4b/cbd8e699e64a6f16ca3a8220661b5f83792b3017d0f79807cb8708d33913/iniconfig-2.0.0.tar.gz", marker = "sys_platform == 'win32'" },
        ]
```
do we check the metadata for both packages individually?

---

_Review comment by @charliermarsh on `docs/concepts/dependencies.md`:263 on 2024-09-28 16:17_

We definitely can't support the above since we don't support disjoint extras, and those markers are overlapping. I talk about this at length here: https://www.notion.so/astral-sh/PyTorch-10a48797e1ca803ea593c456e9bf23f8?pvs=4#10a48797e1ca80f4ab5af90ebcd77000.

---

_@charliermarsh reviewed on 2024-09-28 16:17_

---

_@charliermarsh reviewed on 2024-09-28 16:19_

---

_Review comment by @charliermarsh on `docs/concepts/dependencies.md`:263 on 2024-09-28 16:19_

I need to see if we already propagate extras here though. I.e., this should probably work, right?

```toml
[project.optional-dependencies]
cu118 = []

[tool.uv.sources]
torch = [
    { index = "torch-cu118", marker = "extra == 'cu118'"},
]

[[tool.uv.index]]
name = "torch-cu118"
url = "https://download.pytorch.org/whl/cu118"
```

---

_@charliermarsh reviewed on 2024-09-28 16:36_

---

_Review comment by @charliermarsh on `docs/concepts/dependencies.md`:226 on 2024-09-28 16:36_

Yeah. It’s just like if you had the same direct URL requirement defined twice, with disjoint markers.

---

_@konstin reviewed on 2024-09-28 16:57_

---

_Review comment by @konstin on `docs/concepts/dependencies.md`:226 on 2024-09-28 16:57_

cool!

---

_@charliermarsh reviewed on 2024-09-28 17:06_

---

_Review comment by @charliermarsh on `docs/concepts/dependencies.md`:263 on 2024-09-28 17:06_

I do think we _need_ to support it once we support disjoint extras. It makes the mixed CPU/GPU stuff _so_ much better.

---

_Review comment by @charliermarsh on `crates/uv/tests/lock.rs`:13768 on 2024-09-28 18:35_

This test is wrong (it doesn't include `iniconfig` outside of the optional dependencies, but it should). I think it's because the negation of `extra == 'cpu'` is `extra != 'cpu'`, but when we resolve, we assume all extras are enabled, so that's false? It's like I want the negation of `extra == 'cpu'` to be, like, just omitting that marker. \cc @konstin 

---

_@charliermarsh reviewed on 2024-09-28 18:35_

---

_@ibraheemdev reviewed on 2024-09-28 21:26_

---

_Review comment by @ibraheemdev on `crates/pep508-rs/src/marker/tree.rs`:1645 on 2024-09-28 21:26_

Hmm I'm not sure if this (and `Option<MarkerTreeContents>`) works. If the user wrote `x or not x`, we simplify to `MarkerTree::TRUE`, so this deserialize would fail? I don't think serde falls back to `None` in that case without serde(default).

---

_@charliermarsh reviewed on 2024-09-29 00:42_

---

_Review comment by @charliermarsh on `crates/pep508-rs/src/marker/tree.rs`:1645 on 2024-09-29 00:42_

What would you suggest? Just use `String` and parse these as needed?

---

_@ibraheemdev reviewed on 2024-09-29 06:07_

---

_Review comment by @ibraheemdev on `crates/pep508-rs/src/marker/tree.rs`:1645 on 2024-09-29 06:07_

Why not use `Option<MarkerTree>`?

---

_@charliermarsh reviewed on 2024-09-29 12:10_

---

_Review comment by @charliermarsh on `crates/pep508-rs/src/marker/tree.rs`:1645 on 2024-09-29 12:10_

I need both serialize and deserialize, and neither of those types implement both.

---

_@ibraheemdev reviewed on 2024-09-29 19:27_

---

_Review comment by @ibraheemdev on `crates/pep508-rs/src/marker/tree.rs`:1645 on 2024-09-29 19:27_

You can use the helpers like here: https://github.com/astral-sh/uv/blob/66d7ec541a7e3750ea4f66b432efe605c963678d/crates/pypi-types/src/requirement.rs#L51

---

_Review comment by @BurntSushi on `crates/uv-distribution/src/metadata/lowering.rs`:60 on 2024-09-30 13:16_

I would have probably written this as a `Result<Vec, LoweringError>` personally. Or a `SmallVec` if you're concerned about the alloc (although it seems marginal here since I see other allocs below, although I don't know their relative frequency), since I think the vast majority of these would be a single element `Vec` right?

But I think the nested `Result` would be totally fine. Logically, it makes sense: you have an iterator of results, and building that iterator may fail.

In a "real" library, I might try to take the approach you have here in order to simplify the API. But it depends on some other factors that probably aren't relevant here.

---

_Review comment by @BurntSushi on `crates/uv-distribution/src/metadata/requires_dist.rs`:267 on 2024-09-30 13:18_

These errors are kind of a bummer (and they were before too). Like end users shouldn't be faced with Serde goop like "variant of untagged enum."

---

_Review comment by @BurntSushi on `crates/uv-workspace/src/pyproject.rs`:509 on 2024-09-30 13:34_

I guess to me, it makes a lot of sense to enforce this at deserialization time. It seems much better to fail early here. And if possible, I'd take these error cases and turn them into documented invariants on the relevant types so that consumers know what they can and can't rely on. I think that probably go on `Sources::iter`?

---

_Review comment by @BurntSushi on `crates/uv-workspace/src/pyproject.rs`:517 on 2024-09-30 13:34_

I think @ibraheemdev's other comment applies here too right?

---

_@BurntSushi reviewed on 2024-09-30 13:35_

---

_@charliermarsh reviewed on 2024-09-30 17:12_

---

_Review comment by @charliermarsh on `crates/uv-workspace/src/pyproject.rs`:517 on 2024-09-30 17:12_

Yes!

---

_@charliermarsh reviewed on 2024-09-30 17:13_

---

_Review comment by @charliermarsh on `crates/pep508-rs/src/marker/tree.rs`:1645 on 2024-09-30 17:13_

Perfect, thanks! That's what I was looking for.

---

_@charliermarsh reviewed on 2024-09-30 17:32_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/metadata/requires_dist.rs`:267 on 2024-09-30 17:32_

I agree. We should fix this. I actually looked at adding https://github.com/dtolnay/serde-untagged but it looked non-trivial. How else could we do it? Could we add a variant that has no fields...? Then show a custom message?

---

_Review comment by @BurntSushi on `crates/uv-distribution/src/metadata/requires_dist.rs`:267 on 2024-09-30 17:37_

My approach would be to `cargo expand` the derive and copy that into a custom `Deserialize` impl. Then modify it as appropriate.

If the `cargo expand` output isn't reasonable, then yeah, I'd start just trying to hand-write a custom impl.

---

_@BurntSushi reviewed on 2024-09-30 17:37_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/metadata/lowering.rs`:60 on 2024-09-30 19:29_

Ok, I left it as an iterator for now... I may look into rewriting it. I feel like an iterator is actually a simpler API, despite being more complicated "internally".

---

_@charliermarsh reviewed on 2024-09-30 19:29_

---

_@charliermarsh reviewed on 2024-09-30 19:29_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/metadata/lowering.rs`:60 on 2024-09-30 19:29_

(This method is run for all dependencies declared in the workspace. It returns a single entry the vast majority of the time.)

---

_@konstin reviewed on 2024-09-30 19:44_

---

_Review comment by @konstin on `crates/uv-distribution/src/metadata/requires_dist.rs`:267 on 2024-09-30 19:44_

We can also turn it into a struct with all variants and do our own matching on top, like we do for the git reference variants already.

---

_Merged by @charliermarsh on 2024-09-30 21:16_

---

_Closed by @charliermarsh on 2024-09-30 21:16_

---

_Branch deleted on 2024-09-30 21:16_

---
