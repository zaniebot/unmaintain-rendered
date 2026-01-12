```yaml
number: 2178
title: Strip BOM from ignore files
type: pull_request
state: closed
author: tvrg
labels: []
assignees: []
base: master
head: gitignore-bom
created_at: 2022-04-12T08:24:21Z
updated_at: 2023-07-08T13:57:20Z
url: https://github.com/BurntSushi/ripgrep/pull/2178
synced_at: 2026-01-12T18:23:14Z
```

# Strip BOM from ignore files

---

_@tvrg_

Fixes #2177

---

_Review comment by @tvrg on `crates/ignore/src/gitignore.rs`:395 on 2022-04-12 08:26_

I'm not sure if we actually want to enable bom_sniffing here. `bom_sniffing(false)` seems to be behavior preserving to me. But maybe we want to support ignore files in utf-16 as well?

---

_Review comment by @tvrg on `crates/ignore/tests/bom_test.rs`:9 on 2022-04-12 08:29_

I added this as an integration test because the existing unit tests in gitignore.rs don't create any files but the logic to test is in `GitignoreBuilder.add` which only works on files.

I could also
1. either add a unit test that creates a temporary gitignore file,
2. or extract a method from `add` that works on a `Read` and test that. However, since the errors need the path, the extracted method would also need to know the path.

---

_@tvrg reviewed on 2022-04-12 08:29_

---

_Renamed from "ignore: strip BOM" to "Strip BOM from ignore files" by @tvrg on 2022-04-12 10:54_

---

_Comment by @BurntSushi on 2023-07-08 13:57_

Thanks for the work on this. So to me, this is an overpowered solution. I do not think we need to bring in a absolutely enormous dependency on `encoding_rs` just to strip a few bytes from the prefix of a file that is usually small. I'd rather see this done manually. Hell, if it's easier, I'd prefer we just read the entire ignore file into memory. (Although I'd prefer not doing that.)

I'm going to close this PR because of its age (and because I'm working through the backlog), but if you want to try this again without `encoding_rs` that would be great. Otherwise I'm sure I'll get around to fixing it when I overhaul the `ignore` crate.

---

_Closed by @BurntSushi on 2023-07-08 13:57_

---
