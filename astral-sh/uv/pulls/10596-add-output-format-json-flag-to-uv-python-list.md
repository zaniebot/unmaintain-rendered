```yaml
number: 10596
title: "add `--output-format=json` flag to `uv python list`"
type: pull_request
state: merged
author: Gankra
labels: []
assignees: []
merged: true
base: main
head: jzlotek/python-list-json
created_at: 2025-01-14T14:11:51Z
updated_at: 2025-01-14T16:47:51Z
url: https://github.com/astral-sh/uv/pull/10596
synced_at: 2026-01-12T16:09:23Z
```

# add `--output-format=json` flag to `uv python list`

---

_@Gankra_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

I use `uv` for automation on remote hosts and it would be useful to have it be able to tell me the supported versions of python (for the remote machine) in a machine readable manner so I do not need to parse `uv python list`.

This change adds `--format (json|text)` to `uv python list` to make it's output machine readable

Loosely related:

- https://github.com/astral-sh/uv/issues/411

## Test Plan

Manually tested via

```
# quick inspection without pretty print
cargo run -- python list --format json
```

### Short example of output (trimmed down)

Cmd: `cargo run -- python list --format json | jq '.[:2]'`

```json
[
  {
    "key": "cpython-3.13.1+freethreaded-linux-x86_64-gnu",
    "version": "3.13.1",
    "version_parts": {
      "major": 3,
      "minor": 13,
      "patch": 1
    },
    "path": null,
    "symlink": null,
    "url": "https://github.com/astral-sh/python-build-standalone/releases/download/20241219/cpython-3.13.1%2B20241219-x86_64-unknown-linux-gnu-freethreaded%2Bpgo%2Blto-full.tar.zst",
    "os": "linux",
    "variant": "freethreaded",
    "implementation": "cpython",
    "arch": "x86_64",
    "libc": "gnu"
  },
  {
    "key": "cpython-3.13.1-linux-x86_64-gnu",
    "version": "3.13.1",
    "version_parts": {
      "major": 3,
      "minor": 13,
      "patch": 1
    },
    "path": "/usr/bin/python3.13",
    "symlink": null,
    "url": null,
    "os": "linux",
    "variant": "default",
    "implementation": "cpython",
    "arch": "x86_64",
    "libc": "gnu"
  }
]
```


---

_Comment by @Gankra on 2025-01-14 14:12_

(This is an appended version of #10448 because I don't have push perms to their PR branch)

---

_Review requested from @zanieb by @Gankra on 2025-01-14 14:17_

---

_@zanieb reviewed on 2025-01-14 14:56_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4303 on 2025-01-14 14:56_

In `uv version` we use `--output-format`. I think we should do the same here. We may use `--format` in `uv pip` just for `pip` compatibility?

---

_@zanieb reviewed on 2025-01-14 14:57_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4303 on 2025-01-14 14:57_

We can consider a `--format` alias too. I guess we're less likely to need that for something else like we did in Ruff? (where we needed `--format` for enabling the formatter)

---

_@zanieb reviewed on 2025-01-14 14:57_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:44 on 2025-01-14 14:57_

These comments are wrong.

---

_@zanieb reviewed on 2025-01-14 14:59_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4301 on 2025-01-14 14:59_

The default is already rendered in the help, I think. You may want to omit that here and just say "Select the output format".

---

_@Gankra reviewed on 2025-01-14 15:42_

---

_Review comment by @Gankra on `crates/uv-cli/src/lib.rs`:4301 on 2025-01-14 15:42_

good catch!

---

_@Gankra reviewed on 2025-01-14 15:42_

---

_Review comment by @Gankra on `crates/uv-cli/src/lib.rs`:44 on 2025-01-14 15:42_

dang yep

---

_@Gankra reviewed on 2025-01-14 15:44_

---

_Review comment by @Gankra on `crates/uv-cli/src/lib.rs`:4303 on 2025-01-14 15:44_

There's 1 --output-format (version) and 2 --format (pip list, project export). The project export command is referring to lock-file so yes on balance --output-format is more consistent.

---

_@Gankra reviewed on 2025-01-14 15:53_

---

_Review comment by @Gankra on `crates/uv-cli/src/lib.rs`:4301 on 2025-01-14 15:53_

I also fixed this in the pip list command that this was copied from.

---

_Renamed from "add --format=json flag to `uv python list`" to "add --output-format=json flag to `uv python list`" by @Gankra on 2025-01-14 15:55_

---

_Renamed from "add --output-format=json flag to `uv python list`" to "add `--output-format=json` flag to `uv python list`" by @Gankra on 2025-01-14 15:55_

---

_@zanieb reviewed on 2025-01-14 16:15_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4303 on 2025-01-14 16:15_

Ah yes — good example of us using `--format` for a dedicated purpose already.

---

_Comment by @zanieb on 2025-01-14 16:16_

You need to update the generated CLI reference with `cargo dev generate-all`

---

_@zanieb approved on 2025-01-14 16:21_

---

_Merged by @Gankra on 2025-01-14 16:47_

---

_Closed by @Gankra on 2025-01-14 16:47_

---

_Branch deleted on 2025-01-14 16:47_

---
