```yaml
number: 6423
title: "FR: Encode `flit-core` build backend in `uv`"
type: issue
state: closed
author: chrisrodrigue
labels: []
assignees: []
created_at: 2024-08-22T12:03:01Z
updated_at: 2024-08-22T15:41:25Z
url: https://github.com/astral-sh/uv/issues/6423
synced_at: 2026-01-12T15:59:04Z
```

# FR: Encode `flit-core` build backend in `uv`

---

_@chrisrodrigue_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

I have previously floated the idea of `uv` defining/providing its own backend (see #3957) since the standard library does not provide this critical capability. I think that `hatchling` is a great build backend to use in the meantime and I am happy to see that it has been adopted as the default backend (#5527) when initializing projects.

I want to float the idea of base85 (or other more efficient) encoding a default backend in `uv` to bootstrap offline systems with a way to build when utilizing commands such as `uv init` or `uv sync`. Inspiration for this can be seen in the [`get-pip.py` bootstrapping script](https://raw.githubusercontent.com/pypa/get-pip/main/public/get-pip.py).

Not only do I think that encoding/decoding a blessed backend will aid offline users, but I also think that it could improve performance by eliminating downloads for the transient build environment for users who elect to use the provided default.

While `hatchling` is a nice backend, it has numerous dependencies (`packaging`, `pathspec`, `pluggy`, `tomli`, `trove-classifiers`) that would need to be co-encoded, complicating maintenance and bloating the binary. I think `flit-core` is a great modern and minimalist alternative since it has no dependencies itself. It supports editable installations and a number of other common tasks.

---

_Renamed from "FR: encode default backend in uv binary" to "FR: Encode default backend in uv binary" by @chrisrodrigue on 2024-08-22 12:15_

---

_Renamed from "FR: Encode default backend in uv binary" to "FR: Encode default backend in uv" by @chrisrodrigue on 2024-08-22 12:17_

---

_Comment by @chrisrodrigue on 2024-08-22 12:29_

I’d be happy to implement this myself as a build time job that dynamically downloads and encodes the [remote build backend](https://pypi.org/project/flit-core/#files) when there’s a new version found. 

But feel free to close this as an absolutely terrible idea 😄 

---

_Renamed from "FR: Encode default backend in uv" to "FR: Encode `flit-core` build backend in uv" by @chrisrodrigue on 2024-08-22 12:36_

---

_Renamed from "FR: Encode `flit-core` build backend in uv" to "FR: Encode `flit-core` build backend in `uv`" by @chrisrodrigue on 2024-08-22 12:43_

---

_Comment by @chrisrodrigue on 2024-08-22 12:55_

There’s a [`base116`](https://github.com/taylordotfish/base116) crate that advertises a data size increase of only 1/6 (versus 1/4 for base85) by leveraging UTF-8 over pure ASCII encoding.

---

_Comment by @zanieb on 2024-08-22 15:40_

I think we'd implement our own build backend before embedding a Python one into uv itself.

Related:

- https://github.com/astral-sh/uv/issues/6299

---

_Comment by @zanieb on 2024-08-22 15:41_

It's a cool idea though. The offline use-case for #3957 is interesting.

---

_Closed by @zanieb on 2024-08-22 15:41_

---
