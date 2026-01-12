```yaml
number: 1407
title: "Expose Ruff's public API as a Python library"
type: pull_request
state: closed
author: squiddy
labels: []
assignees: []
draft: true
base: main
head: python-library
created_at: 2022-12-27T16:04:33Z
updated_at: 2023-01-27T10:21:06Z
url: https://github.com/astral-sh/ruff/pull/1407
synced_at: 2026-01-12T04:51:59Z
```

# Expose Ruff's public API as a Python library

---

_Pull request opened by @squiddy on 2022-12-27 16:04_

See https://github.com/charliermarsh/ruff/issues/659

- [x] Accept options (need to check what a good way here is, either just a partial dictionary similar to WASM, or proper classes with type hints?)
- [ ] Configure and document testing
- [ ] Include in CI
- [x] Build a wheel with binary and library
- [x] Finalize what the API exactly is (https://github.com/charliermarsh/ruff/pull/1407#discussion_r1057768626)

---

_@charliermarsh reviewed on 2022-12-27 16:07_

---

_Review comment by @charliermarsh on `src/lib_python.rs`:86 on 2022-12-27 16:07_

We should also accept an optional filename.

---

_@squiddy reviewed on 2022-12-27 16:10_

---

_Review comment by @squiddy on `src/lib_python.rs`:86 on 2022-12-27 16:10_

On that topic, do we mirror the current API 1:1, i.e. should this also resolve settings by looking into the filesystem?

There was some discussion on the issue itself, but no clear direction yet (at least for me).

---

_@charliermarsh reviewed on 2022-12-27 16:16_

---

_Review comment by @charliermarsh on `src/lib_python.rs`:86 on 2022-12-27 16:16_

Yeah I think we should. Similar to the Rust API (which doesn't accept settings... but should).

I think the rules are:

- If settings are provided, use those.
- Else, if filepath is provided, infer.
- Else, use defaults.

---

_Comment by @squiddy on 2022-12-28 11:48_

Most pieces are in place. Since maturin doesn't support binary+library in one package (at least I didn't find a way), I had to switch to the lower-level setuptools-rust. I need to do some more CI adjustments for that, so I'll be moving that into a separate PR.

---

_Comment by @charliermarsh on 2022-12-28 12:12_

Sweet! I haven't reviewed yet, but @messense, do you know if Maturin can be used here to build a binary + library in one package?

---

_Comment by @squiddy on 2022-12-28 12:16_

> I haven't reviewed yet

For the record, it's also not in a state where I'm waiting for a review. I can still do some cleaning up, adding docs and function signatures to the library, adjust CI etc.

---

_Comment by @messense on 2022-12-28 12:26_

> do you know if Maturin can be used here to build a binary + library in one package?

Not yet, supporting both lib and bin will complicate bindings detection and our auditwheel Rust implementation, but it's been worked on slowly.

---

_Comment by @not-my-profile on 2023-01-26 06:31_

I really think that this is too soon ... we have not even stabilized the Rust API ... I have been refactoring our Rust API a lot recently but it's still very much not ready to be used by any third party, since it's very much subject to change drastically.

---

_Comment by @charliermarsh on 2023-01-26 17:35_

Yeah, I agree -- I was excited for this but I've realized that it's too soon. Let's close for now, since it's getting a bit stale anyway.

---

_Closed by @charliermarsh on 2023-01-26 17:35_

---

_Branch deleted on 2023-01-27 10:21_

---
