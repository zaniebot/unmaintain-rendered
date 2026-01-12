```yaml
number: 8389
title: Fix Rust toolchain for sdist testing in CI
type: pull_request
state: closed
author: stinodego
labels: []
assignees: []
base: main
head: fix-rustup-default
created_at: 2023-10-31T19:53:49Z
updated_at: 2023-11-01T18:51:15Z
url: https://github.com/astral-sh/ruff/pull/8389
synced_at: 2026-01-10T23:40:55Z
```

# Fix Rust toolchain for sdist testing in CI

---

_Pull request opened by @stinodego on 2023-10-31 19:53_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

I noticed the CI step sets the default Rust toolchain by calling `rustup default ($cat rust-toolchain)`. However, this fails, as there is no `rust-toolchain` file.

The CI currently continues without setting the default toolchain. Seems like this hasn't tripped anyone up yet, but it might in the future.

I replaced `($cat rust-toolchain)` with a command to read the `rust-toolchain.toml` file. 

## Test Plan

You can verify that the command works by running it locally.

Since this triggered the workflow, you can compare and see that it works.

The previous release workflow is here:
https://github.com/astral-sh/ruff/actions/runs/6658624231/job/18095891817

In the logs for the `test-sdist` step:
```
cat: rust-toolchain: No such file or directory
stable-x86_64-unknown-linux-gnu (default)
```

In the workflow triggered by this PR:
https://github.com/astral-sh/ruff/actions/runs/6711771325/job/18239884406?pr=8389

```
info: using existing install for '1.73-x86_64-unknown-linux-gnu'
info: default toolchain set to '1.73-x86_64-unknown-linux-gnu'
```

EDIT: Seems like I melted some icebergs by triggering your release workflow this way 😕 I'm sorry about that.

---

_Marked ready for review by @stinodego on 2023-10-31 19:56_

---

_Comment by @zanieb on 2023-10-31 19:59_

Welcome to the project :)

I wonder if we need to change the default toolchain at all since the toolchain is not specified in the package? Could this just be removed?

Also yeah triggering the release pipeline is expected when this file changes for verification purposes.

---

_Comment by @stinodego on 2023-10-31 20:07_

> Welcome to the project :)

Thanks! I've been following the progress closely as I'm a linting enthusiast myself and we make good use of ruff in the @pola-rs project. I've opened some issues in the past.

> Could this just be removed?

I don't think so. The subsequent `pip install` will trigger `maturin` to compile the Rust code. If the right toolchain is not set, this will fail. The `rust-toolchain.toml` is not picked up during a `pip install`.

The reason I know this (and made this PR), is that I set up the Polars release workflow based on the excellent work here. Polars requires a specific nightly version which is specified in the toolchain. This fails without the `rust default` command.

So you _could_ remove it, which will be fine as long as ruff compiles with the latest stable Rust version (as is currently the case). But it's better to set the default Rust version to the one specified in the toolchain.

---

_Comment by @zanieb on 2023-10-31 20:22_

Well thanks for taking the time to contribute!

Interesting. I'm just a little hesitant to pull it out of the toml file like this and am a bit confused as to why this isn't handled by the build tooling. I wonder if it would be respected by maturin if we added it to the Python package's sdist via the manifest? cc @konstin

---

_Comment by @stinodego on 2023-10-31 20:24_

> I wonder if it would be respected by maturin if we added it to the Python package's sdist via the manifest?

If that works, that would be great! I didn't consider that possibility 🤔 

---

_Comment by @konstin on 2023-11-01 17:19_

>  I wonder if it would be respected by maturin if we added it to the Python package's sdist via the manifest? 

It does, maturin just unpacks and calls cargo the normal way, it even has precedence over a rust-toolchain.toml in the working directory

---

_Comment by @konstin on 2023-11-01 17:21_

Necessary changes if we want to include rust-toolchain.toml in the source distribution: https://github.com/astral-sh/ruff/compare/include-rust-toolchain

---

_Comment by @zanieb on 2023-11-01 17:27_

I have a strong preference for that :)

---

_Closed by @zanieb on 2023-11-01 18:51_

---
