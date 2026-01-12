```yaml
number: 2335
title: "ignore/types: add motoko and candid"
type: pull_request
state: merged
author: smallstepman
labels: []
assignees: []
merged: true
base: master
head: ignore-types_add_motoko_and_candid
created_at: 2022-10-20T13:02:48Z
updated_at: 2022-10-20T13:22:42Z
url: https://github.com/BurntSushi/ripgrep/pull/2335
synced_at: 2026-01-12T18:23:14Z
```

# ignore/types: add motoko and candid

---

_@smallstepman_

candid: https://internetcomputer.org/docs/current/developer-docs/build/candid/candid-concepts

motoko: https://internetcomputer.org/docs/current/developer-docs/build/cdks/motoko-dfinity/motoko/

---

_Comment by @BurntSushi on 2022-10-20 13:09_

It looks like that's a source-available and not a open-source project, right? (It took me forever to find their GitHub.) I generally try to stick to FOSS stuff for the default types, and expect non-FOSS types to maintain their own file types (which can be added to ripgrep on every CLI invocation via `--type-add`, typically via an alias, wrapper script or config file).

---

_Comment by @smallstepman on 2022-10-20 13:14_

I believe both candid and motoko are FOSS but I'm no licensing expert (both are Apache 2.0):
- https://github.com/dfinity/candid
- https://github.com/dfinity/motoko

The blockchain platform which uses these languages (Internet Computer) is source-available, [but not FOSS](https://github.com/dfinity/ic/blob/master/LICENSE).

---

_Comment by @BurntSushi on 2022-10-20 13:22_

Ah okay, yeah, I was looking at https://github.com/dfinity/ic. Like I said, the dfinity web site makes it pretty hard to find the source.

Since candid and motoko are indeed open source, I'll bring this in. Thanks!

---

_Merged by @BurntSushi on 2022-10-20 13:22_

---

_Closed by @BurntSushi on 2022-10-20 13:22_

---
