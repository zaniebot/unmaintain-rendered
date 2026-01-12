```yaml
number: 1862
title: Actually fix wasm-pack build command
type: pull_request
state: merged
author: not-my-profile
labels: []
assignees: []
merged: true
base: main
head: wasm-pack-fix
created_at: 2023-01-14T04:07:19Z
updated_at: 2023-01-14T04:20:21Z
url: https://github.com/astral-sh/ruff/pull/1862
synced_at: 2026-01-12T15:55:07Z
```

# Actually fix wasm-pack build command

---

_@not-my-profile_

I initially attempted to run `wasm-pack build -p ruff` which gave the error message:

    Error: crate directory is missing a `Cargo.toml` file; is `-p` the wrong directory?

I interpreted that as wasm-pack looking for the "ruff" directory because I specified -p ruff, however actually the wasm-pack build usage is:

    wasm-pack build [FLAGS] [OPTIONS] <path> <cargo-build-options>

And I was missing the `<path>` argument. So this actually wasn't at all a bug in wasm-pack but just a confusing error message. And the symlink hack I introduced in the previous commit didn't actually work ... I only accidentally omitted the `-p` when testing (which ended up as `ruff` being the <path> argument) ... CLIs are fun.

---

_Comment by @charliermarsh on 2023-01-14 04:20_

Lol. Thank you :)

---

_Merged by @charliermarsh on 2023-01-14 04:20_

---

_Closed by @charliermarsh on 2023-01-14 04:20_

---
