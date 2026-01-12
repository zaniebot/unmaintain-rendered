```yaml
number: 3340
title: "add basic \"install from lock file\" operation"
type: pull_request
state: merged
author: BurntSushi
labels:
  - preview
assignees: []
merged: true
base: main
head: ag/lock-install
created_at: 2024-05-02T18:38:57Z
updated_at: 2024-05-03T13:54:49Z
url: https://github.com/astral-sh/uv/pull/3340
synced_at: 2026-01-12T16:05:35Z
```

# add basic "install from lock file" operation

---

_@BurntSushi_

This PR principally adds a routine for converting a `Lock` to a
`Resolution`, where a `Resolution` is a map of package names pinned to
a specific version.

I'm not sure that a `Resolution` is ultimately what we want here (we
might need more stuff), but this was the quickest route I could find to
plug a `Lock` into our existing `uv pip install` infrastructure.

This commit also does a little refactoring of the `Lock` types. The
main thing is to permit extra state on some of the types (like a
`by_id` map on `Lock` for quick lookups of distributions) that aren't
included in the serialization format of a `Lock`. We achieve this
by defining separate `Wire` types that are automatically converted
to-and-from via `serde`.

Note that like with the lock file format types themselves, we leave a
few `todo!()` expressions around. The main idea is to get something
minimally working without spending too much effort here. (A fair bit
of refactoring will be required to generate a lock file, and it's
not clear how much this code will wind up needing to change anyway.)
In particular, we only handle the case of installing wheels from a
registry.

A demonstration of the full flow:

```
$ requirements.in
anyio
$ cargo run -p uv -- pip compile -p3.10 requirements.in --unstable-uv-lock-file
$ uv venv
$ cargo run -p uv -- pip install --unstable-uv-lock-file anyio -r requirements.in
Installed 5 packages in 7ms
 + anyio==4.3.0
 + exceptiongroup==1.2.1
 + idna==3.7
 + sniffio==1.3.1
 + typing-extensions==4.11.0
```

In order to install from a lock file, we start from the root and do a
breadth first traversal over its dependencies. We aren't yet filtering
on marker expressions (since they aren't in the lock file yet), but we
should be able to add that in the future. In so doing, the traversal
should select only the subset of distributions relevant for the current
platform.


---

_Review requested from @konstin by @BurntSushi on 2024-05-02 18:39_

---

_Review requested from @charliermarsh by @BurntSushi on 2024-05-02 18:39_

---

_@charliermarsh reviewed on 2024-05-03 00:18_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock.rs`:84 on 2024-05-03 00:18_

Should this use `LockError` rather than `String`?

---

_@charliermarsh reviewed on 2024-05-03 00:19_

---

_Review comment by @charliermarsh on `crates/uv/src/cli.rs`:1334 on 2024-05-03 00:19_

Nit: can we call this `unstable_lock_file` (omitting `uv`)?

---

_@charliermarsh reviewed on 2024-05-03 00:19_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/pip_install.rs`:359 on 2024-05-03 00:19_

So this assumes that it's in the current working directory?

---

_@charliermarsh reviewed on 2024-05-03 00:20_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock.rs`:57 on 2024-05-03 00:20_

Could / should this just be a special package named `root`?

---

_@charliermarsh approved on 2024-05-03 00:23_

I'm a little confused about the root concept, and why we need to pass `anyio` as an _argument_ to `--unstable-lock`. What's the eventual vision there?

---

_Comment by @charliermarsh on 2024-05-03 00:24_

Separately, what are the expected semantics for `pip install` with a lockfile? Does the lockfile get regenerated? Should we limit it to `pip sync` for now for simplicity?

---

_Review comment by @konstin on `crates/uv-resolver/src/lock.rs`:74 on 2024-05-03 09:09_

Should we make `DistributionId.name` a `PackageName`?

---

_@konstin approved on 2024-05-03 09:21_

---

_@BurntSushi reviewed on 2024-05-03 11:33_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/lock.rs`:84 on 2024-05-03 11:33_

It's only an internal helper for right now, and its only use asserts that is succeeds. I don't know that this belongs in `LockError`, and right now, it's only used to find the "root" package. And I expect all of that to change.

---

_@BurntSushi reviewed on 2024-05-03 11:34_

---

_Review comment by @BurntSushi on `crates/uv/src/cli.rs`:1334 on 2024-05-03 11:34_

It matches the name I added in: https://github.com/astral-sh/uv/pull/3314/files#diff-ae55eef503096c43098e973865bd5ed612871dcd1d3d335631a8027deb3828d3R605

This flag is hidden, undocumented and intended to be temporary. Its only purpose is to expose "install from `uv.lock`" on the CLI so that we can interact with it.

---

_@BurntSushi reviewed on 2024-05-03 11:35_

---

_Review comment by @BurntSushi on `crates/uv/src/commands/pip_install.rs`:359 on 2024-05-03 11:35_

Yes. It's the simplest thing to do I think. I expect this code to be eventually deleted. (Like, I don't expect `uv pip install` should ever read a `uv.lock` file.)

---

_@BurntSushi reviewed on 2024-05-03 11:39_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/lock.rs`:57 on 2024-05-03 11:39_

I'm not sure that privileging the notion of "root" in the lock file makes sense, particularly given that we want to have workspace support. Cargo went in the same direction:

* https://github.com/rust-lang/cargo/issues/3003
* https://github.com/rust-lang/cargo/issues/4563
* https://github.com/rust-lang/cargo/issues/3704

---

_Comment by @BurntSushi on 2024-05-03 11:46_

@charliermarsh 

> I'm a little confused about the root concept, and why we need to pass `anyio` as an _argument_ to `--unstable-lock`. What's the eventual vision there?

Hmm, so I added this as a TODO comment in the source:

```rust
            // TODO: In the future, we should derive the root distribution
            // from the pyproject.toml, but I don't think the infrastructure
            // for that is in place yet. For now, we ask the caller to specify
            // the root package name explicitly, and we assume here that it is
            // correct.
```

The basic idea here is that the lock file should contain an entry for each package in the current `uv` workspace. That concept doesn't really exist yet as far as I know, so those packages do not yet get added to the lock file. So we need some other way to pick the root. Right now, we just ask for it. But this is the sort of thing we should be auto-detecting by reading the `pyproject.toml`.

> Separately, what are the expected semantics for `pip install` with a lockfile? Does the lockfile get regenerated? Should we limit it to `pip sync` for now for simplicity?

I'm not sure there are any expected semantics for `pip install` with `uv.lock`. It's not an obviously well defined operation to me. It's only here now as a temporary way of exposing interaction with `uv.lock`.

---

_@BurntSushi reviewed on 2024-05-03 11:56_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/lock.rs`:74 on 2024-05-03 11:56_

Ah yeah, nice idea. Done.

---

_Merged by @BurntSushi on 2024-05-03 12:18_

---

_Closed by @BurntSushi on 2024-05-03 12:18_

---

_Branch deleted on 2024-05-03 12:18_

---

_Label `preview` added by @BurntSushi on 2024-05-03 13:30_

---

_@charliermarsh reviewed on 2024-05-03 13:54_

---

_Review comment by @charliermarsh on `crates/uv/src/cli.rs`:1334 on 2024-05-03 13:54_

I mostly just have a strong dislike for using `uv` in things (outside of crate names, since it's roughly a namespace), like structs or arguments. But not a big deal if it's temporary.

---

_@charliermarsh reviewed on 2024-05-03 13:54_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/pip_install.rs`:359 on 2024-05-03 13:54_

Ok agreed.

---

_@charliermarsh reviewed on 2024-05-03 13:54_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock.rs`:57 on 2024-05-03 13:54_

Ok agreed. This makes more sense now that I see that this isn't ever intended to be part of `pip install`, but rather, something that stems from a known project root.

---
