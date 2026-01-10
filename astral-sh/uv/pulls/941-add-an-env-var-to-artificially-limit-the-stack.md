```yaml
number: 941
title: " Add an env var to artificially limit the stack size "
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: konsti/set-explicit-thread-size
created_at: 2024-01-16T16:24:23Z
updated_at: 2024-01-19T09:34:47Z
url: https://github.com/astral-sh/uv/pull/941
synced_at: 2026-01-10T15:39:02Z
```

#  Add an env var to artificially limit the stack size 

---

_Pull request opened by @konstin on 2024-01-16 16:24_

By default, windows has a stack size limit of 1MB which we run against in debug without any explicit culprit. A new environment variable `PUFFIN_STACK_SIZE` allows setting an artificially smaller stack size.

---

_Review comment by @BurntSushi on `crates/puffin-cli/src/main.rs`:736 on 2024-01-16 17:25_

The [tokio docs don't make it clear under what conditions this routine fails](https://docs.rs/tokio/latest/tokio/runtime/struct.Builder.html#method.build), but it returns an `io::Error` on failure. I do wonder under what conditions this can fail, and perhaps it might be better to not panic? On the other hand, if `unwrap()` is what `#[tokio::main]` was already doing, then I guess it's probably fine.

---

_@BurntSushi approved on 2024-01-16 17:26_

Can we create an issue for looking into this? The thing I'm worried about here is that we have 1MB+ stacks lying around, and that probably isn't a great thing. With that said, if this fixes things on Windows I'm fine with it as a temporary work-around I think.

---

_Converted to draft by @konstin on 2024-01-17 12:49_

---

_@konstin reviewed on 2024-01-18 10:10_

---

_Review comment by @konstin on `crates/puffin-cli/src/main.rs`:736 on 2024-01-18 10:10_

```rust
#[tokio::main]
async fn main() {
    println!("Hello, world!");
}
```
expands to 
```rust
#![feature(prelude_import)]
#[prelude_import]
use std::prelude::rust_2021::*;
#[macro_use]
extern crate std;
fn main() {
    let body = async {
        {
            ::std::io::_print(format_args!("Hello, world!\n"));
        };
    };
    #[allow(clippy::expect_used, clippy::diverging_sub_expression)]
    {
        return tokio::runtime::Builder::new_multi_thread()
            .enable_all()
            .build()
            .expect("Failed building the Runtime")
            .block_on(body);
    }
}
```
so i think that's what you're expected to do.

---

_Renamed from "Set explicit stack size to avoid stack overflows on windows" to " Add a feature to simulate a smaller stack size " by @konstin on 2024-01-18 10:22_

---

_Marked ready for review by @konstin on 2024-01-18 11:13_

---

_Review requested from @BurntSushi by @konstin on 2024-01-18 11:13_

---

_Comment by @charliermarsh on 2024-01-18 13:39_

@konstin - Do you only see these failures in debug, or release too?

---

_Comment by @konstin on 2024-01-18 13:42_

Only in debug mode

---

_@BurntSushi approved on 2024-01-18 13:52_

Nice I like it.

---

_@BurntSushi reviewed on 2024-01-18 13:56_

Actually, given #963, I now realize I'm less a fan of this approach. It'd be really nice to make `--all-features` continue to work. Otherwise, it [breaks things like this](https://github.com/BurntSushi/dotfiles/blob/fcba29da12d05e78331bf03c7799f2d44e6a72d1/.config/ag/nvim/default#L13). Requiring a non-standard way to enable all features _just_ so we can test with a smaller stack size seems non-ideal to me.

What do you think about configuring this with an environment variable instead?

---

_Renamed from " Add a feature to simulate a smaller stack size " to " Add an env var to artificially limit the stack size " by @konstin on 2024-01-18 14:30_

---

_Review comment by @BurntSushi on `crates/puffin-cli/src/main.rs`:719 on 2024-01-18 16:21_

Do you need to also set the stack size here? i.e., https://docs.rs/tokio/latest/tokio/runtime/struct.Builder.html#method.thread_stack_size

---

_@BurntSushi reviewed on 2024-01-18 16:22_

---

_Review comment by @konstin on `crates/puffin-cli/src/main.rs`:719 on 2024-01-18 16:52_

Done

---

_@konstin reviewed on 2024-01-18 16:52_

---

_@BurntSushi approved on 2024-01-18 17:13_

Love it!

---

_Merged by @konstin on 2024-01-19 09:34_

---

_Closed by @konstin on 2024-01-19 09:34_

---

_Branch deleted on 2024-01-19 09:34_

---
