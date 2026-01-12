```yaml
number: 1848
title: "CI: check docs"
type: pull_request
state: closed
author: marcoieni
labels:
  - rollup
assignees: []
base: master
head: patch-2
created_at: 2021-04-15T20:53:37Z
updated_at: 2021-06-01T01:51:21Z
url: https://github.com/BurntSushi/ripgrep/pull/1848
synced_at: 2026-01-12T18:23:14Z
```

# CI: check docs

---

_@marcoieni_

working example: https://github.com/LukeMathWalker/wiremock-rs/pull/66

---

_Review comment by @BurntSushi on `.github/workflows/ci.yml`:213 on 2021-04-15 22:36_

This should probably have `--all` in it, right?

---

_@BurntSushi reviewed on 2021-04-15 22:36_

---

_@marcoieni reviewed on 2021-04-16 07:10_

---

_Review comment by @marcoieni on `.github/workflows/ci.yml`:213 on 2021-04-16 07:10_

Yes, I also added `--all-features`

---

_Comment by @marcoieni on 2021-04-16 07:25_

I have to look into this issue. By the way, I just noticed the `--all` flag is deprecated. Should we use `--workspace` instead?

```
$ cargo doc --help       
cargo-doc 
Build a package's documentation

USAGE:
    cargo doc [OPTIONS]

OPTIONS:
[...]
    -p, --package <SPEC>...          Package to document
        --all                        Alias for --workspace (deprecated)
        --workspace                  Document all packages in the workspace
[...]
```

---

_Comment by @BurntSushi on 2021-04-16 12:11_

No, not `--all-features`. Just `--all`. Errmm, well, I guess `--workspace`. I didn't know they deprecated `--all`. Sigh.

---

_Label `rollup` added by @BurntSushi on 2021-05-30 12:25_

---

_Closed by @BurntSushi on 2021-06-01 01:51_

---
