```yaml
number: 11446
title: "A minimal build backend for uv: uv_build"
type: pull_request
state: merged
author: konstin
labels:
  - preview
assignees: []
merged: true
base: main
head: konsti/standalone-build-backend-package
created_at: 2025-02-12T14:58:24Z
updated_at: 2025-03-06T19:27:22Z
url: https://github.com/astral-sh/uv/pull/11446
synced_at: 2026-01-12T16:09:51Z
```

# A minimal build backend for uv: uv_build

---

_@konstin_

uv itself is a large package with many dependencies and lots of features. To build a package using the uv build backend, you shouldn't have to download and install the entirety of uv. For platform where we don't provide wheels, it should be possible and fast to compile the uv build backend. To that end, we're introducing a python package that contains a trimmed down version of uv that only contains the build backend, with a minimal dependency tree in rust.

The `uv_build` package is publish from CI just like uv itself. It is part of the workspace, but has much less dependencies for its own binary. We're using cargo deny to enforce that the network stack is not part of the dependencies. A new build profile ensure we're getting the minimum possible binary size for a rust binary.


---

_Review comment by @cthoyt on `python/uv/_build_backend.py`:42 on 2025-02-12 15:58_

typo check on the `replace()`?

---

_@cthoyt reviewed on 2025-02-12 15:58_

---

_Review comment by @Gankra on `Cargo.toml`:287 on 2025-02-13 13:48_

panic = "abort" *may* also reduce size if we're desperate for every byte: https://doc.rust-lang.org/cargo/reference/profiles.html#panic

---

_Review comment by @Gankra on `crates/uv-build/src/main.rs`:1 on 2025-02-13 13:55_

I would love to have a comment here explaining why we are doing this. just to produce an anyhow error instead of panic when stdout is closed?

---

_Review comment by @Gankra on `crates/uv-build/pyproject.toml`:42 on 2025-02-13 13:56_

i don't know why but this is incredibly funny to me (it makes sense)

---

_Review comment by @Gankra on `crates/uv-build/deny.toml`:8 on 2025-02-13 13:57_

rad

---

_Review comment by @Gankra on `.github/workflows/build-binaries.yml`:145 on 2025-02-13 14:00_

Hmm I was gonna say this will slow down releases a lot if it's serial but in theory this thing should build super fast so it's fine?

---

_Renamed from "CI: uv-build" to "A minimal build backend for uv: uv_build" by @konstin on 2025-02-13 14:26_

---

_@Gankra reviewed on 2025-02-13 14:45_

looked over everything but the python

---

_@charliermarsh reviewed on 2025-02-13 14:54_

---

_Review comment by @charliermarsh on `Cargo.toml`:287 on 2025-02-13 14:54_

Probably worth at least checking how much it saves us

---

_@konstin reviewed on 2025-02-13 14:55_

---

_Review comment by @konstin on `.github/workflows/build-binaries.yml`:145 on 2025-02-13 14:55_

It's 22s on a fresh target directory for me locally. I had initially put them in succession to reuse the target dir, but that mostly doesn't apply anymore with the different opt mode.

---

_@Gankra reviewed on 2025-02-13 14:56_

---

_Review comment by @Gankra on `.github/workflows/build-binaries.yml`:145 on 2025-02-13 14:56_

Yeah basically nothing to share

---

_@konstin reviewed on 2025-02-13 14:58_

---

_Review comment by @konstin on `crates/uv-build/src/main.rs`:1 on 2025-02-13 14:58_

I've mostly done it for consistency with `writeln!(printer.stdout(), ...)` we're using everywhere else, i don't feel strongly about.

---

_Review comment by @konstin on `Cargo.toml`:287 on 2025-02-13 15:11_

I've added it for, it gets us down to 1217927 bytes on linux. I'm not sure with we want to deploy that, we've had very few panics in the past but if the happen the panic message is the only indication we get.


---

_@konstin reviewed on 2025-02-13 15:11_

---

_Review comment by @T-256 on `python/uv/__init__.py`:30 on 2025-02-13 15:16_

consider either remove or change the message to use `uv_build` instead.
https://github.com/astral-sh/uv/blob/a4bd73f9229eabce1ecc70cc605ab5872d40e27c/python/uv/__init__.py#L33-L46

---

_@T-256 reviewed on 2025-02-13 15:25_

IIUC, to configure backend, we should use `tool.uv.build-backend`, but since `uv_build` is now standalone package could expect it to be changed?

IMO, `tool.uv.build` or `tool.uv-build` is closer to `uv_build`, the package name.

---

_Label `preview` added by @konstin on 2025-02-19 10:36_

---

_Marked ready for review by @konstin on 2025-02-19 10:54_

---

_@johnthagen reviewed on 2025-02-26 14:33_

---

_Review comment by @johnthagen on `Cargo.toml`:287 on 2025-02-26 14:33_

You could see if this helps:

```toml
codegen-units = 1
```

- https://github.com/johnthagen/min-sized-rust?tab=readme-ov-file#reduce-parallel-code-generation-units-to-increase-optimization

Note, as described in the link above, this (like LTO and others) can slow down the compile time, but could reduce the binary size.

---

_@zanieb reviewed on 2025-02-26 14:44_

---

_Review comment by @zanieb on `Cargo.toml`:283 on 2025-02-26 14:44_

It'd be nice to talk briefly about the goals here, since we're doing some different things.

---

_Review comment by @zanieb on `crates/uv-build-backend/src/metadata.rs`:226 on 2025-02-26 14:45_

```suggestion
                Without bounding the `uv_build` version, the source distribution will break \
```

---

_@zanieb reviewed on 2025-02-26 14:45_

---

_@zanieb reviewed on 2025-02-26 14:45_

---

_Review comment by @zanieb on `crates/uv-build-backend/src/metadata.rs`:227 on 2025-02-26 14:45_

```suggestion
                when a future, breaking version of `uv_build` is released.",
```

---

_@zanieb reviewed on 2025-02-26 14:45_

---

_Review comment by @zanieb on `crates/uv-build-backend/src/metadata.rs`:230 on 2025-02-26 14:45_

We should report what they used verbatim, right?

---

_@zanieb reviewed on 2025-02-26 14:46_

---

_Review comment by @zanieb on `crates/uv-build-backend/src/metadata.rs`:225 on 2025-02-26 14:46_

```suggestion
                upper bound on the `uv_build` version such as `<{next_breaking}`. \
```

---

_@zanieb reviewed on 2025-02-26 14:49_

---

_Review comment by @zanieb on `crates/uv-build/pyproject.toml`:4 on 2025-02-26 14:49_

Maybe?

```suggestion
description = "The uv build backend"
```

It's awkward as phrased currently. I'm not sure we need to explain it's a partial distribution here.

---

_Review comment by @zanieb on `crates/uv-build/python/uv_build/__main__.py`:6 on 2025-02-26 14:50_

Did you want to use `uv_build` consistently?

---

_@zanieb reviewed on 2025-02-26 14:50_

---

_@zanieb reviewed on 2025-02-26 14:54_

---

_Review comment by @zanieb on `python/uv/__init__.py`:20 on 2025-02-26 14:54_

Can we say a bit more here? Like. 
```suggestion
            f"Using `uv.{attr_name}` is not allowed; build backend functionality is in the `uv_build` package. Did you mean to use `uv_build` as your build system?"
```

---

_@zanieb reviewed on 2025-02-26 14:56_

---

_Review comment by @zanieb on `Cargo.lock`:4673 on 2025-02-26 14:56_

Need to update the version before merge

---

_@zanieb reviewed on 2025-02-26 14:58_

---

_Review comment by @zanieb on `Cargo.lock`:4673 on 2025-02-26 14:58_

We should also add the child `pyproject.toml` to https://github.com/astral-sh/uv/blob/a0b9f22a21d4ddfc80dcefa63f4bf15e1e252cc9/pyproject.toml#L79 — right?

Rooster only updates the root `pyproject.toml`.

---

_@konstin reviewed on 2025-02-26 14:59_

---

_Review comment by @konstin on `crates/uv-build-backend/src/metadata.rs`:230 on 2025-02-26 14:59_

Without extra hacks, we don't retain the verbatim representation here, we normalize all package just after reading (like in the resolver, too). If we care about the exact input value, we can change the serde types and do late parsing here.

---

_@zanieb reviewed on 2025-02-26 15:00_

---

_Review comment by @zanieb on `crates/uv-build-backend/src/metadata.rs`:230 on 2025-02-26 15:00_

If it's a pain, this seems fine.

---

_@konstin reviewed on 2025-02-26 15:00_

---

_Review comment by @konstin on `crates/uv-build/python/uv_build/__main__.py`:6 on 2025-02-26 15:00_

yes good catch

---

_@zanieb approved on 2025-02-26 15:00_

---

_@konstin reviewed on 2025-02-26 15:01_

---

_Review comment by @konstin on `Cargo.lock`:4673 on 2025-02-26 15:01_

ah yes i was looking for that option.

---

_Comment by @konstin on 2025-02-27 10:17_

Some datapoints: A `maturin build --profile minimal-size --zig` for uv-build takes 17s on my (high-end) desktop, for a 1,2MB wheel. I can link almost any crate compiled to an actual function we perform, except for tokio and rkyv (the build backend is sync and doesn't use the uv cache). It's still a lost of crates, but it's also a lot of features (zip and tar.gz files, SHA256 checksums, parsing the email format for METADATA, toml for pyproject.toml, globs and regex for configuration and PEP 639, SPDX license checking, URL parsing, colorized cli messages, PEP 440, PEP 508 and PEP 625, csv for the record, tracing for logging, ...).

---

_@charliermarsh approved on 2025-02-28 01:39_

---

_Comment by @charliermarsh on 2025-02-28 01:41_

Looks great. My only question is: did we settle on the desired identifiers for this piece?

```toml
requires = ["uv_build>=0.4.15,<5"]
build-backend = "uv_build"
```

I'm cool with it overall, I assume @zanieb is too. I might have a slight preference for:

```toml
requires = ["uv-build>=0.4.15,<5"]
build-backend = "uv.build"
```

But I assume that's more complicated.


---

_Comment by @zanieb on 2025-02-28 03:10_

Yeah I prefer the aesthetics of 

```
requires = ["uv-build>=0.4.15,<5"]
build-backend = "uv.build"
```

but then we will clobber the root `uv/__init__.py`, I think?

Agree we should nail down our preferred spelling before merging. Perhaps a brief summary of the trade-offs would be helpful?

---

_@T-256 reviewed on 2025-02-28 10:01_

---

_Review comment by @T-256 on `crates/uv-build/python/uv_build/__init__.py`:44 on 2025-02-28 10:01_

IMO, at least by `uv-build` we could use it as library instead of calling it through binary's cli.
do we want still to support `build-system.build-backend = "uv"`? If no, the _call_ function could be entirely removed and directly use `uv_build_backend` methods.

---

_Comment by @konstin on 2025-02-28 18:04_

While I agree with

```toml
requires = ["uv-build>=0.4.15,<5"]
build-backend = "uv.build"
```

being better looking, I see two aspects that drew me towards `uv_build`: With `uv.build`, every action (such as `from uv.build import build_wheel`) has to first import `uv/__init__.py`, too. This couples uv's own `uv/__init__.py` to `uv-build` across all versions (since they don't depend on each other, any version combination is possible). As separation from `uv` through `build-backend = "uv-build"` future proofs the module import. The other aspect is that I regularly have to help with user questions where the problem turns out to be two packages that overwrite each other partially or fully being in the dependency tree, so I'm biased against packages that ship modules of a different name.

To also mention a discarded alternative, there was also an option of having `build-backend = "uv.build"` because both `uv` and `uv-build` would ship the build backend, or similarly allowing `build-backend = "uv"`, but I couldn't find an option to make those work without the risk of two different versions of `uv` and `uv-build` conflicting on installed files (and overwriting in a way that breaks something).

---

_Comment by @zanieb on 2025-03-03 21:58_

On the package collision / overwriting note: this should be quite rare, right? Since the build backend is usually installed in an isolated build environment?

---

_Comment by @notatallshaw on 2025-03-03 22:35_

> On the package collision / overwriting note: this should be quite rare, right? Since the build backend is usually installed in an isolated build environment?

While less common there are various legitimate reasons to build in a non-isolated environment. The first one that comes to mind is having pinned build dependencies, which has somewhat limited tooling support outside manually doing it. 

---

_Comment by @zanieb on 2025-03-03 23:02_

> The first one that comes to mind is having pinned build dependencies, which has somewhat limited tooling support outside manually doing it.

We definitely intend to fix this though. (but I understand that doesn't mean this isn't a problem for people using other build frontends). I still see that as a niche or advanced use-case where it's fine if the packages overlap.

---

_Comment by @zanieb on 2025-03-04 01:53_

(I'm not attached to a particular outcome here. I think `uv_build` is probably simpler — but I want to poke at our motivations a bit)

---

_Comment by @konstin on 2025-03-04 13:04_

Currently, it's always just `requires = ["uv_build..."]`, but expect that to change in the future as the build backend becomes more powerful and people want to add other packages, too, for additional functionality.

Overall my experience is that when used correctly, namespace packages/packages with renaming and isolated packages work equally well. Even though, users are regularly hitting broken installs with cases where two packages conflict on a filesystem level, and then it's hard to figure out what and why even broke. `uv.build` also puts an implicit constraint on `uv/__init__.py` because it always gets imported for `uv.build` no matter the `uv` version.

---

_Merged by @zanieb on 2025-03-06 19:27_

---

_Closed by @zanieb on 2025-03-06 19:27_

---

_Branch deleted on 2025-03-06 19:27_

---
