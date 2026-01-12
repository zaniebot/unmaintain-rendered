```yaml
number: 8887
title: uv add workspace fails misteriously from git but not locally
type: issue
state: closed
author: PhilipVinc
labels:
  - bug
  - help wanted
assignees: []
created_at: 2024-11-07T14:04:12Z
updated_at: 2024-12-03T05:07:22Z
url: https://github.com/astral-sh/uv/issues/8887
synced_at: 2026-01-12T15:59:37Z
```

# uv add workspace fails misteriously from git but not locally

---

_@PhilipVinc_

I tried to add as a dependency to a project a workspace that is found on git by doing

```python
uv init
uv add git+https://github.com/NeuralQXLab/omnia  --no-sync
```

this fails with the error
```bash
❯ uv add git+https://github.com/NeuralQXLab/omnia  --no-sync
 Updated https://github.com/NeuralQXLab/omnia (5d97a5b)
error: Requirements contain conflicting URLs for package `netket` in split `python_full_version == '3.12.*'`:
- file:///Users/filippo.vicentini/.cache/uv/git-v0/checkouts/75e22c3618b1966a/5d97a5b/external/netket
- file:///Users/filippo.vicentini/.cache/uv/git-v0/checkouts/75e22c3618b1966a/5d97a5b/external/netket
```

Instead, if I do the same but from a cloned version of the workspace, it just works

```python
uv init
git clone https://github.com/NeuralQXLab/omnia
uv add ./omnia
```

While the workspace is private, the structure is something that looks like
```
# omnia/pyproject.toml
[project]
name = "omnia"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "ipykernel>=6.29.5",
    "netket",
    "netket_pro",
    "netket_checkpoint",
    "deepnets",
]

[tool.uv]
dev-dependencies = [
    "networkx>=3.4.2",
    "pytest-xdist>=3.6.1",
    "pytest>=8.3.3",
]

[tool.uv.sources]
netket = { workspace = true }
netket_pro = { workspace = true }
netket_checkpoint = { workspace = true }
deepnets = { workspace = true }

[tool.uv.workspace]
members = ["packages/*", "utils/*", "external/netket"]
exclude = ["utils/jax-rocm-autoinstall"]
```

However the error is not very telling, and I do not understand what I can do to fix it.

---

_Comment by @charliermarsh on 2024-11-07 14:07_

Are you on a fairly recent version of uv? \cc @konstin 

---

_Comment by @charliermarsh on 2024-11-07 14:08_

Separately @konstin any idea how we're getting that specific error when the URLs _are_ the same...?

---

_Renamed from "uv add git+....workspace " to "uv add workspace fails misteriously from git but not locally" by @PhilipVinc on 2024-11-07 14:22_

---

_Comment by @PhilipVinc on 2024-11-07 14:23_

❯ uv --version
uv 0.4.30 (61ed2a236 2024-11-04)

Here is the gist of the error when running in verbose mode.
https://gist.github.com/PhilipVinc/b6929ae0d315a90cd08df864bc839e83

---

_Comment by @PhilipVinc on 2024-11-07 14:32_

@charliermarsh I cleaned up the workspace to publish it. 
You can run the following to reproduce

```python
mkdir test
cd test
uv init
uv add git+https://github.com/PhilipVinc/uvtest
```

---

_Label `bug` added by @konstin on 2024-11-07 14:34_

---

_Comment by @ericmarkmartin on 2024-11-08 05:50_

It looks like while the `VerbatimUrl`s are the same, the `ParsedUrl`s differ. In particular, the fork has an existing url that's a `ParsedUrl::Git` and we're trying to replace it with a `ParsedUrl::Directory` (see below). I'm guessing the former is because the `add` command is given a git url and then `tool.uv.sources` gives us a relative URL for `netket` on top of that. I'm unfamiliar with this part of the code and thus as of yet unsure where the `Directory` url is coming from. For reference, [here](https://github.com/astral-sh/uv/blob/0db38844d976d61c44bb00a38aaa0f8c9497675d/crates/uv-resolver/src/fork_urls.rs#L36)'s the check that's failing.

```
URL 1: VerbatimParsedUrl { parsed_url: Git(ParsedGitUrl { url: GitUrl { repository: Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("github.com")), port: None, path: "/PhilipVinc/uvtest", query: None, fragment: None }, reference: DefaultBranch, precise: None }, subdirectory: Some("external/netket") }), verbatim: VerbatimUrl { url: Url { scheme: "file", cannot_be_a_base: false, username: "", password: None, host: None, port: None, path: "/home/ericmarkmartin/.cache/uv/git-v0/checkouts/e76e56b4d32bffcc/dab673b/external/netket", query: None, fragment: None }, given: None } }
URL 2: VerbatimParsedUrl { parsed_url: Directory(ParsedDirectoryUrl { url: Url { scheme: "file", cannot_be_a_base: false, username: "", password: None, host: None, port: None, path: "/home/ericmarkmartin/.cache/uv/git-v0/checkouts/e76e56b4d32bffcc/dab673b/external/netket", query: None, fragment: None }, install_path: "/home/ericmarkmartin/.cache/uv/git-v0/checkouts/e76e56b4d32bffcc/dab673b/external/netket", editable: true, virtual: false }), verbatim: VerbatimUrl { url: Url { scheme: "file", cannot_be_a_base: false, username: "", password: None, host: None, port: None, path: "/home/ericmarkmartin/.cache/uv/git-v0/checkouts/e76e56b4d32bffcc/dab673b/external/netket", query: None, fragment: None }, given: None } }
```

---

_Comment by @konstin on 2024-11-08 09:14_

For a git dependency that is a workspace, we first clone the repository to the cache dir, then do workspace discovery inside the cache directory. That means if you depend on A in a git repo and A depends on B in that same git repo, we get B as a directory. If you also depend on B from that git repo, we get the conflict. I had hoped to have fixed that in https://github.com/astral-sh/uv/pull/8665 by rewriting the directory back to a git URL, but apparently this branch is still wrong.

---

_Label `help wanted` added by @konstin on 2024-11-08 09:14_

---

_Comment by @ericmarkmartin on 2024-11-10 19:58_

I could maybe take a swing at this but could use some pointers on where to look for things

---

_Comment by @konstin on 2024-11-11 14:16_

The workspace section of `lowering.rs` as well as the workspace discovery in `workspace.rs` are probably the best starting points, esp. https://github.com/astral-sh/uv/pull/8665/files#diff-56eaccbf913b25ab6ebc621027c1e8e66017e9c353001cf69c16a235e5323072R224-R241. My hunch is this is similar to #8665 overall.

---

_Comment by @PhilipVinc on 2024-11-20 23:26_

@konstin I'm not very familiar with rust, but I think that the condition you linked to above is correct.
The problem is that [`let source = if let Some(git_member) `](https://github.com/astral-sh/uv/blob/c62c83c37ada63eae4efb77551e2ec7a0f0113d8/crates/uv-distribution/src/metadata/lowering.rs#L269) tests False because [`LoweredRequirement::from_requirement(`](https://github.com/astral-sh/uv/blob/c62c83c37ada63eae4efb77551e2ec7a0f0113d8/crates/uv-distribution/src/metadata/lowering.rs#L49) is called with a `git_member` that is None in [requires_dist](https://github.com/astral-sh/uv/blob/c62c83c37ada63eae4efb77551e2ec7a0f0113d8/crates/uv-distribution/src/metadata/requires_dist.rs#L176).

I'm unsure how to proceed from here, however..


---

_Comment by @PhilipVinc on 2024-11-20 23:59_

Ok, some more debugging lead me to the conclusion that:

 - the first time that uv calls [`git_metadata`](https://github.com/astral-sh/uv/blob/c62c83c37ada63eae4efb77551e2ec7a0f0113d8/crates/uv-distribution/src/source/mod.rs#L1324) the check [`if Some(metadata)`](https://github.com/astral-sh/uv/blob/c62c83c37ada63eae4efb77551e2ec7a0f0113d8/crates/uv-distribution/src/source/mod.rs#L1363) passes, so the [`git_member`](https://github.com/astral-sh/uv/blob/c62c83c37ada63eae4efb77551e2ec7a0f0113d8/crates/uv-distribution/src/source/mod.rs#L1366) gets constructed correctly.
 - When parsing the dependencies of the sub package, and getting the git metadata the condition above fails so we end up [hitting the cache here](https://github.com/astral-sh/uv/blob/c62c83c37ada63eae4efb77551e2ec7a0f0113d8/crates/uv-distribution/src/source/mod.rs#L1399) where the git repository is not built.

By changing those lines to 
```rust
            if let Some(metadata) = read_cached_metadata(&metadata_entry).await? {
                let path = if let Some(subdirectory) = resource.subdirectory {
                    Cow::Owned(fetch.path().join(subdirectory))
                } else {
                    Cow::Borrowed(fetch.path())
                };
                let git_member = GitWorkspaceMember {
                    fetch_root: fetch.path(),
                    git_source: resource,
                };
                    return Ok(ArchiveMetadata::from(
                    Metadata::from_workspace(
                        metadata,
                        &path,
                        Some(&git_member),
                        self.build_context.locations(),
                        self.build_context.sources(),
                        self.build_context.bounds(),
                    )
```

my ignorant test case (`cargo run run --with git+https://github.com/PhilipVinc/uvtest python`) does not fail for the same reason as my bug report.
However I suspect that this change breaks ùv` but maybe this is enough for you to understand where the problem is coming from?

---

_Comment by @PhilipVinc on 2024-11-21 00:47_

Ah! 

I think I understand.

> For a git dependency that is a workspace, we first clone the repository to the cache dir, then do workspace discovery inside the cache directory. That means if you depend on A in a git repo and A depends on B in that same git repo, we get B as a directory. If you also depend on B from that git repo, we get the conflict. I had hoped to have fixed that in https://github.com/astral-sh/uv/pull/8665 by rewriting the directory back to a git URL, but apparently this branch is still wrong.

The error stems when any among repository A or B have a dynamic `pyproject.toml` file. 
The logic is too hard for me to follow, but I believe that if they have a dynamic pyproject file uv does not reuse the git workspace information...

---

_Comment by @ericmarkmartin on 2024-11-23 03:33_

> However I suspect that this change breaks ùv` 

Was looking at this again, I don't see any immediate problems with this, and doesn't seem to cause any issues with the test suite (I do have 3 failing tests locally but 2 are due to rate limiting and the last one fails even without the change, so I'm assuming it's irrelevant).

@konstin wdyt?


---

_Comment by @charliermarsh on 2024-11-23 03:46_

I'd be happy to review a PR (or at least, it'd probably be easier for me to comment on the changes as a diff since there's a lot of context here).

---

_Comment by @ericmarkmartin on 2024-11-23 16:53_

Threw something up at #9388. If you think the PR makes sense I can get into writing some tests

---

_Comment by @konstin on 2024-11-26 21:08_

The PR code makes sense to me

---

_Closed by @charliermarsh on 2024-12-03 05:07_

---
