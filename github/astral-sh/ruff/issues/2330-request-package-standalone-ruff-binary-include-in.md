---
number: 2330
title: "[REQUEST] Package standalone `ruff` binary & include in GitHub releases"
type: issue
state: closed
author: rahul-theorem
labels:
  - release
assignees: []
created_at: 2023-01-29T23:44:04Z
updated_at: 2025-10-23T19:22:55Z
url: https://github.com/astral-sh/ruff/issues/2330
synced_at: 2026-01-07T13:12:14-06:00
---

# [REQUEST] Package standalone `ruff` binary & include in GitHub releases

---

_Issue opened by @rahul-theorem on 2023-01-29 23:44_

**Context for the request**
I just came across `ruff` and it looks like a very promising pyflakes/isort replacement. We use Bazel to manage a large Python monorepo, and this seems like it could help shorten some critical dev loops and lead to a perf improvement in CI. 

However, given the way that ruff is packaged (ie. the main python entrypoint just shells out to the `ruff` binary under the hood), it doesn't seem like we'll be able to use `rules_python` + the `entry_point` macro from pip-parse to include ruff as a tool in our build (see https://github.com/bazelbuild/rules_python/issues/1000 for more discussion)

**Request**
OTOH, if ruff were available as a standalone binary (along w/ the source tgz/zip) as part of the GitHub release, we can pull this into our build w/ `http_archive`/set it up as an executable tool. This seems like it should be fairly straightforward to bake into the release process as a [release asset](https://docs.github.com/en/rest/releases/assets)

---

_Comment by @rahul-theorem on 2023-01-29 23:44_

@charliermarsh I'd be happy to contribute this so we can run `ruff` as an aspect in our Bazel project if this is a contribution you'd be willing to accept.

---

_Comment by @charliermarsh on 2023-01-29 23:51_

@rahul-theorem - Yeah I'm happy to include the binaries in the GitHub release. My only hesitation is that right now, our release process only includes building the Python wheels (which themselves package the per-platform binaries), so it may require more work than merely adding the wheels to the release (unless that works too?).

---

_Comment by @rahul-theorem on 2023-01-29 23:54_

Thanks for the quick reply @charliermarsh! 

I just took a look through `ruff.yaml` & also saw the same; does the `maturin` action also have the ability to build a standalone binary, or will it only emit a wheel? If attaching the wheel to the GH release is the only thing that's possible, then we can continue to use `rules_python` to download the wheel and hack around this bazel limitation in some other way.

---

_Comment by @charliermarsh on 2023-01-29 23:59_

`maturin build` should build the binary as a side-effect:

```console
> rm target/debug/ruff
> maturin build
> ls target/debug/ruff
target/debug/ruff
```

(In release, that would be `target/release/ruff`, just using `debug` for expediency.)

So, without looking at the YAML deeply, I think it should be enough to just wire those built binaries up to the final GitHub release -- we're already building them, and they exist in the filesystem after running `maturin`.

---

_Comment by @rahul-theorem on 2023-01-30 00:01_

Ah nice. Happy to take a swing at this later today or tomorrow - from some basic testing (without running it as part of our build) it's pretty mind-blowing how quickly it can lint our mono-repo!

---

_Comment by @charliermarsh on 2023-01-30 00:03_

That's what I like to hear :)

---

_Label `release` added by @charliermarsh on 2023-01-30 00:08_

---

_Comment by @messense on 2023-01-30 03:08_

Note that we should name the artifact something like `ruff-<rustc target name>.tar.gz/zip`, for example `ruff-x86_64-unknown-linux-gnu.tar.gz`, so that [cargo-binstall](https://github.com/cargo-bins/cargo-binstall) and [taiki-e/install-action](https://github.com/taiki-e/install-action) will also be able to use them.

---

_Referenced in [astral-sh/ruff#2355](../../astral-sh/ruff/pulls/2355.md) on 2023-01-30 15:41_

---

_Comment by @rahul-theorem on 2023-01-30 15:42_

@messense is that something that `maturin` could handle during the `maturin build` step? Not quite familiar with how it names the compiled binary. See the linked PR, can incorporate your suggestions to name them correctly.

---

_Comment by @messense on 2023-01-31 01:40_

> is that something that `maturin` could handle during the `maturin build` step?

No, maturin is for building python wheels so I don't think this is suitable to include in it.

---

_Comment by @charliermarsh on 2023-02-03 20:57_

[`cargo dist`](https://blog.axo.dev/2023/02/cargo-dist) would be interesting for this.

---

_Comment by @charliermarsh on 2023-02-11 03:13_

My current thinking is that we'll just copy [ripgrep's release workflow](https://github.com/BurntSushi/ripgrep/blob/master/.github/workflows/release.yml), except instead of creating the GitHub Release, we'll trigger the workflow by the creation of a GitHub Release.

---

_Referenced in [astral-sh/ruff#2930](../../astral-sh/ruff/pulls/2930.md) on 2023-02-15 16:52_

---

_Closed by @charliermarsh on 2023-02-15 17:07_

---

_Comment by @lopopolo on 2023-05-10 22:17_

@rahul-theorem I'm intrigued by the ruff aspect you mentioned for your Python Bazel project. Is this setup something you could share? I'd like to do the same.

---

_Comment by @jwnimmer-tri on 2025-10-23 19:22_

FYI on a related topic, I have recently published `ruff` binaries to the Bazel Central Registry:
- https://registry.bazel.build/modules/ruff_prebuilt
- https://github.com/astral-sh/ruff/discussions/20672

---
