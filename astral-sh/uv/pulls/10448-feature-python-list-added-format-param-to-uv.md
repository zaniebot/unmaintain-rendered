```yaml
number: 10448
title: "feature(python list): added --format param to uv python list"
type: pull_request
state: closed
author: jzlotek
labels: []
assignees: []
base: main
head: jzlotek/python-list-json
created_at: 2025-01-09T22:03:12Z
updated_at: 2025-01-14T17:04:14Z
url: https://github.com/astral-sh/uv/pull/10448
synced_at: 2026-01-12T16:09:18Z
```

# feature(python list): added --format param to uv python list

---

_@jzlotek_

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

_Review requested from @Gankra by @zanieb on 2025-01-09 22:22_

---

_Review requested from @zanieb by @zanieb on 2025-01-09 22:22_

---

_Review comment by @Gankra on `crates/uv/src/commands/python/list.rs`:194 on 2025-01-10 14:54_

afaict these 3 paths only differ by one field each. I'd like to see this refactored to just compute the 2 special fields and then make one PrintData at the end.

---

_@Gankra requested changes on 2025-01-10 14:57_

Everything looks good except one detail. Although not 100% confident on if there's any fields that would be "bad" to expose since machine-readable outputs are ideally making a stronger guarantee of stable output.

---

_@jzlotek reviewed on 2025-01-10 23:13_

---

_Review comment by @jzlotek on `crates/uv/src/commands/python/list.rs`:194 on 2025-01-10 23:13_

Yeah you're right! I'll make that adjustment. Thanks for the pointer.

---

_Comment by @jzlotek on 2025-01-10 23:15_

> Everything looks good except one detail. Although not 100% confident on if there's any fields that would be "bad" to expose since machine-readable outputs are ideally making a stronger guarantee of stable output.

I'll think about it. The only thing I can think of adding that is probably worthwhile is to add the version parts. Thoughts? I am not super familiar with the api as this is my first time doing rust/uv work so any suggestions would be appriciated

---

_Review comment by @Gankra on `crates/uv/src/commands/python/list.rs`:228 on 2025-01-13 14:32_

(Sharing Tips) I think either is fine here but letting you know the [copied](https://doc.rust-lang.org/beta/std/option/enum.Option.html#method.copied) api exists:

`patch: release.get(2).copied().unwrap_or(0),`

---

_Review comment by @Gankra on `crates/uv/src/commands/python/list.rs`:208 on 2025-01-13 14:35_

```suggestion
                            let is_symlink = fs_err::symlink_metadata(path)?.is_symlink();
```


---

_Review comment by @Gankra on `crates/uv/src/commands/python/list.rs`:211 on 2025-01-13 14:38_

```suggestion
                                    Some(path.read_link()?.user_display().to_string());
```

---

_Review comment by @Gankra on `crates/uv/src/commands/python/list.rs`:228 on 2025-01-13 14:39_

Also in this case also `patch: release.get(2).copied().unwrap_or_default(),` but i think what you have is clearer in that regard.

---

_@Gankra approved on 2025-01-14 13:34_

Thanks a bunch, I'll do the last little fixes because they need some turbofish surgery

---

_Comment by @Gankra on 2025-01-14 16:48_

Merged in #10596, thanks so much!

---

_Closed by @Gankra on 2025-01-14 16:48_

---

_Comment by @jzlotek on 2025-01-14 17:04_

> Merged in #10596, thanks so much!

No problem. Thanks for the tips

---

_Branch deleted on 2025-01-14 17:04_

---
