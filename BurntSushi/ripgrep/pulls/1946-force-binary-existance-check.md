```yaml
number: 1946
title: Force binary existance check
type: pull_request
state: closed
author: sstadick
labels:
  - rollup
assignees: []
base: master
head: force_binary_existance_check
created_at: 2021-07-22T14:04:56Z
updated_at: 2023-07-08T22:53:11Z
url: https://github.com/BurntSushi/ripgrep/pull/1946
synced_at: 2026-01-12T18:23:14Z
```

# Force binary existance check

---

_@sstadick_

This PR allows for explicit checking for binary existence when using the `associate` and `try_associate` methods on the `DecompressionMatcherBuilder`. I have tried to preserve the current `resolve_binary` api being a no-op on non-windows as well.

Currently, a use tries to `{try_}associate` a program on a non-windows OS, `resolve_binary` no-ops and never checks the path to see if that binary actually exists or not.

This change forces a path check for existence even on non-windows OS so that a user can try to associate several different binaries with a glob and only the binaries found on the system will be associated.

Concrete example: https://github.com/sstadick/hck/blob/f5aca37b0a06523074fe229fa77bb863e4f8574c/src/lib/core.rs#L365

I want to associate `pigz` with `*.gz` only if `pigz` is installed on the users system, otherwise it can fail and `gzip` can be used. Currently `pigz` is not actually tested for existence, it's added as a matcher and then fails to find it when called resulting in no decompression happening. 

---

_Label `rollup` added by @BurntSushi on 2023-07-08 21:16_

---

_@BurntSushi approved on 2023-07-08 21:18_

I'll take this (with a minor refactoring I did to get rid of the boolean parameter), but I am a little worried about this. Because this now forces our ad hoc (and previously Windows specific) binary lookup logic to apply on all platforms. See, the point of `resolve_binary` is not to check the existence of a binary, but to find the full path to the binary without falling prey to binaries relative to the CWD.

I think it's probably fine and I do think the behavior implemented by this PR is more sensible. Although I note it isn't strictly necessary. One could write a wrapper program to dynamically select between `pigz` or `gz`.

---

_Closed by @BurntSushi on 2023-07-08 22:53_

---
