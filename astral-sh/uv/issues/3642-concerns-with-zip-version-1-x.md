```yaml
number: 3642
title: Concerns with zip version 1.x
type: issue
state: closed
author: musicinmybrain
labels: []
assignees: []
created_at: 2024-05-17T16:05:56Z
updated_at: 2025-03-15T20:06:53Z
url: https://github.com/astral-sh/uv/issues/3642
synced_at: 2026-01-10T03:50:30Z
```

# Concerns with zip version 1.x

---

_Issue opened by @musicinmybrain on 2024-05-17 16:05_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

I am working on packaging `uv` for Fedora Linux. Currently, we are wary of updating the [`zip` crate](https://crates.io/crates/zip) past version 0.6.x (and will hold off on doing so for now) due to concerns with how the [`zip-rs` project](https://github.com/zip-rs) project is being handled:

All releases starting with v1.0.0 were developed and released by different people than the original crate, with the [hand-off](https://github.com/zip-rs/zip-old/issues/446) being handled in a kind of weird way. Additionally, releases 1.2.0+ of the "new" zip crate contain breaking API changes which are [not going to be fixed](https://github.com/zip-rs/zip2/issues/124) in the 1.x series.

Since `uv` *can* be made to work with version 0.6.6, would it be possible to either:

- depend on `zip` `0.6.6` for now, if you decide you share these concerns, or:
- at least relax the dependency to something like `>=0.6.6, <2.0` to explicitly allow building with both `zip` v0.6 and v1?

Note that the `zip` crate maintainer is planning a 2.0 release in a couple of weeks.

In order to build with `zip` `0.6.6`, with tests passing, one source change is necessary:

https://github.com/astral-sh/uv/blob/39af09f09b89611953f503927499b4129091bf68/crates/install-wheel-rs/src/wheel.rs#L14

needs to be `use zip::write::FileOptions as SimpleFileOptions`, since the `SimpleFileOptions` alias was added in 1.x.

If the `uv` project decides to change nothing, I will just carry a small downstream patch to support `zip` `0.6.6` for the time being.

---

_Comment by @musicinmybrain on 2024-05-17 18:02_

See also: https://github.com/PyO3/python-pkginfo-rs/issues/13

---

_Comment by @zanieb on 2024-05-17 18:14_

We're still considering this as a team, but it seems likely we'd pin until we have more details. Would you be willing to open a pull request?

---

_Comment by @musicinmybrain on 2024-05-17 19:13_

> We're still considering this as a team, but it seems likely we'd pin until we have more details. Would you be willing to open a pull request?

Sure! I hope https://github.com/astral-sh/uv/pull/3645 is what you needed. Thanks for considering this.

---

_Closed by @charliermarsh on 2024-05-18 17:31_

---

_Comment by @AlexWaygood on 2024-06-07 09:25_

In https://github.com/astral-sh/ruff/pull/11779, we're adding `zip` as a dependency to Ruff as well, for red-knot. For now we're pinning it to the same version that `uv` has it pinned to.

---

_Comment by @musicinmybrain on 2025-03-15 19:27_

An update: with the passage of time, and with an increasing number of upstream projects moving past `zip` 0.6.x, we’ve become comfortable enough with the situation to start shipping a current version of the `zip` crate (now 2.2.3) in Fedora.

This comment isn’t *asking* you to update the `zip` dependency – we expect that we’ll be maintaining a  `rust-zip0.6` compat package for some time – but to let you know that if, in your own judgement, you decide the time is right to update the `zip` dependency, we won’t show up asking you to reconsider.

---

_Comment by @charliermarsh on 2025-03-15 20:06_

Thanks!

---
