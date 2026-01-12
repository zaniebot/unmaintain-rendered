```yaml
number: 2361
title: "fix: sorting nested results by system time"
type: pull_request
state: closed
author: nguyenvukhang
labels:
  - rollup
assignees: []
draft: true
base: master
head: sort
created_at: 2022-11-28T04:44:07Z
updated_at: 2023-07-09T14:14:05Z
url: https://github.com/BurntSushi/ripgrep/pull/2361
synced_at: 2026-01-12T18:23:14Z
```

# fix: sorting nested results by system time

---

_@nguyenvukhang_

Bug is aptly described in https://github.com/BurntSushi/ripgrep/issues/2243.

This PR aims to address this by delaying result sorting to after every match has been found.

---

_Marked ready for review by @nguyenvukhang on 2022-11-28 07:11_

---

_Converted to draft by @nguyenvukhang on 2022-11-28 07:11_

---

_Renamed from "fix: nested results sorted wrongly" to "fix: sorting nested results by system time" by @nguyenvukhang on 2022-11-28 15:34_

---

_Review comment by @BurntSushi on `crates/core/args.rs`:361 on 2022-11-29 19:17_

So this isn't really `unzip`. `unzip` is `Vec<(K, V)> -> (Vec<K>, Vec<V>)`. And functions like this shouldn't be marked as `#[inline]`: they are polymorphic so it will get inlined anyway. And even if it weren't, the perf overhead of a function call in this context is negligible.

In any case, I would get rid of this function entirely. Instead, do:

```rust
let items = match sorted.kind { ... };
items.into_iter().map(|v| v.1).collect()
```

---

_Review comment by @BurntSushi on `crates/core/main.rs`:108 on 2022-11-29 19:19_

This is a big no-no from what I can tell. It looks like you're _unconditionally_ collecting all paths before iterating over them even if no `--sort` flag is passed. That means you're incurring the perf/memory/latency---at least in part---of sorting even when you don't need to.

---

_Review comment by @BurntSushi on `crates/core/main.rs`:258 on 2022-11-29 19:20_

Same deal here as for `search` above.

---

_@BurntSushi requested changes on 2022-11-29 19:23_

Nice work but yeah, there is a fair bit of work left here. The two big items:

1. It is a hard requirement that if `--sort` is not given, then there should be no additional overhead here. But in this PR, it looks like you always collect all of the paths before starting a search. Today, ripgrep will start searching well before it finishes directory traversal.
2. There are a number of places where errors are being swallowed. We can't do that.

I'm also not sure about moving sorting to the `main` functions. I think it's probably fine, although I was trying to keep the `main` functions as simple as I could. But if we are going to deal with sorting in `main.rs`, then I think it would be nice to add a short comment in the parallel searcher stating why it doesn't handle sorting. (Because if sorting is enabled, then parallel searching is disabled in `args.rs`.)

---

_Comment by @nguyenvukhang on 2022-11-30 11:21_

> 1. It is a hard requirement that if `--sort` is not given, then there should be no additional overhead here. But in this PR, it looks like you always collect all of the paths before starting a search. Today, ripgrep will start searching well before it finishes directory traversal.

Yep. Fixed it. Top-level `search()` and `files()` functions will now specifically check if a sort that requires calls to `stat` is required.

The latest implementation using new functions `continue_search()` and `continue_files()` is a result of trying to coerce the types of the two possible states of preliminary search results:
1. uncollected and unconsumed iterator. (no sort)
2. collected, sorted, and re-made into an iterator.

I'd prefer to stick to having just one function for `search` and `files` each, though I'm not sure if that's possible.

> 2. There are a number of places where errors are being swallowed. We can't do that.

Agreed. I noticed that the previously reviewed code swallowed the `Result<Walk>` that `args.walker()?` emitted, and that has been fixed.

https://github.com/BurntSushi/ripgrep/pull/2361#discussion_r1035183685 has also been addressed.

---

_Review comment by @nguyenvukhang on `crates/core/args.rs`:361 on 2022-11-30 11:24_

Yep, changed it.

---

_@nguyenvukhang reviewed on 2022-11-30 11:24_

---

_@nguyenvukhang reviewed on 2022-11-30 11:24_

---

_Review comment by @nguyenvukhang on `crates/core/main.rs`:108 on 2022-11-30 11:24_

resolved by https://github.com/BurntSushi/ripgrep/pull/2361/commits/d4706913adf88cb3c6b395d0afcf493f39208169

---

_@nguyenvukhang reviewed on 2022-11-30 11:24_

---

_Review comment by @nguyenvukhang on `crates/core/main.rs`:258 on 2022-11-30 11:24_

also resolved by https://github.com/BurntSushi/ripgrep/pull/2361/commits/d4706913adf88cb3c6b395d0afcf493f39208169

---

_Review requested from @BurntSushi by @nguyenvukhang on 2022-11-30 11:25_

---

_@BurntSushi reviewed on 2023-07-09 12:13_

---

_Review comment by @BurntSushi on `crates/ignore/src/walk.rs`:829 on 2023-07-09 12:13_

Why did you do this? It's a breaking API change for the `ignore` crate. Same for `sort_by_file_name` below.

---

_Label `rollup` added by @BurntSushi on 2023-07-09 12:19_

---

_@nguyenvukhang reviewed on 2023-07-09 13:10_

---

_Review comment by @nguyenvukhang on `crates/ignore/src/walk.rs`:829 on 2023-07-09 13:10_

Sorry about that, this change was not necessary. It's been reverted by a8c3e89.

---

_Comment by @BurntSushi on 2023-07-09 13:29_

You're all set. I already did the fixups needed to get this PR into shape. You can see it in #2555.

---

_Comment by @nguyenvukhang on 2023-07-09 14:09_

I see. Thanks for patching up my tests and adding delays so the `stat` calls are correct.

---

_Closed by @BurntSushi on 2023-07-09 14:14_

---
