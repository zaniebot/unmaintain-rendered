```yaml
number: 751
title: "[WIP] Add support for compressed file formats (closes #539)"
type: pull_request
state: closed
author: balajisivaraman
labels: []
assignees: []
base: master
head: fix_539
created_at: 2018-01-14T12:02:19Z
updated_at: 2018-01-30T15:35:58Z
url: https://github.com/BurntSushi/ripgrep/pull/751
synced_at: 2026-01-12T18:23:13Z
```

# [WIP] Add support for compressed file formats (closes #539)

---

_@balajisivaraman_

Creating this PR for review purposes. I still haven't added any tests for the newer code, which is one of the main things still pending here.

Notes:
- Had to add `globset` as dependency for `ripgrep` module to easily check for supported file formats. The in-built `path.extension()` method did not allow me to verify for `*.tar.gz` files. 😞 
- We could've used [`output`](https://doc.rust-lang.org/std/process/struct.Command.html#method.output) from Command instead of [`spawn`](https://doc.rust-lang.org/std/process/struct.Command.html#method.spawn), which pipes stdout and stderr by default and gives us access to the process' exit status. However, it would've meant loading the entire file into memory in the form of a `Vec<u8>`. I've therefore gone with the `spawn` approach here, coupled with peeking at the `stderr` to verify whether the command succeeded or failed. It works nicely enough in practice in my testing.
- Also, I noticed that the `spawn` method ends up returning an `Err` immediately if the command is not found. All other errors are obtained from the process' stderr. That's why I've handled it such that if `spawn` returns an `Error`, then `rg` will output the appropriate debug message.
- I couldn't think of any other CLI flag that conflicts with `-z/--search-zip`. One thing to think of is how should `rg` handle when the `-a` argument is provided? Right now, if `-a` and `-z` are both provided, `rg` will search zip files as per the new code, but will also end up treating it as a binary file and try searching it. Are we OK with this?
- Not really fond of passing the `no-messages` flag to the `decompressor`, but there's no better option if we want all error handling to remain within the module itself. In this way, the `worker` module remains largely unaffected if we choose to change how we do decompression at a later point in time.

Any other thoughts and suggestions are welcome.

---

_Renamed from "[WIP] Add support for compressed file formats" to "[WIP] Add support for compressed file formats (closes #539)" by @balajisivaraman on 2018-01-14 12:03_

---

_@okdana reviewed on 2018-01-14 12:39_

---

_Review comment by @okdana on `complete/_rg`:90 on 2018-01-14 12:39_

The exclusions and the syntax itself are wrong here. At a minimum it should be something like this:

```
'(-z --search-zip)'{-z,--search-zip}'[search through zip files]'
```

We might also want to add other exclusions, but i'd like to do another pass on this file within the next few weeks, so it can wait until then to get fancier.

I would also suggest changing the description wording to something like *search contents of compressed files* — 'through' is a bit ambiguous, and we don't actually search *zip* files at all.

---

_@okdana reviewed on 2018-01-14 12:41_

---

_Review comment by @okdana on `doc/rg.1`:194 on 2018-01-14 12:41_

'On your `PATH`' feels strange to me — '*in* your `PATH`'?

---

_@okdana reviewed on 2018-01-14 12:42_

---

_Review comment by @okdana on `src/app.rs`:590 on 2018-01-14 12:42_

Same comment about 'on your `PATH`', but also i think 'uncompression' should be 'decompression' here

---

_Comment by @okdana on 2018-01-14 12:48_

There is a problem with the completion function, and i added a few bike-shed wording remarks. I won't comment on the Rust stuff except to say that it does at least make sense to me.

I tested the patch on macOS (just briefly) and it works! This is neat as hell, ty!

---

_@balajisivaraman reviewed on 2018-01-14 12:50_

---

_Review comment by @balajisivaraman on `complete/_rg`:90 on 2018-01-14 12:50_

I'll fix the syntax as per your suggestion. This file is always throwing me off. Sorry about that.

I thought of using `-C/--search-compressed` instead of `-z/--search-zip`. The only reason I went with the latter is that it would be familiar to folks coming from using `ag`. Like you said, `search-zip` doesn't really make a lot of sense here. Should we just go with the former option?

---

_@okdana reviewed on 2018-01-14 13:01_

---

_Review comment by @okdana on `complete/_rg`:90 on 2018-01-14 13:01_

The completion-function syntax takes a while to get used to, yeah. Every few weeks i learn that something i used to do in these files is wrong, lol.

I like `-z` for the short option. It's what `ag` and `sift` both use, and the connection with 'Ziv' is immediately intuitive. As far as the long option, idk. `--search-zip` feels slightly inaccurate, but you're right about `ag` — it's one of the most common recursive file searchers, and definitely the most well known one that supports searching compressed files, so it's not unreasonable to borrow the terminology. `--search-compressed` makes sense too tho, w/e 🤷‍♀️

---

_@tamird reviewed on 2018-01-17 19:02_

---

_Review comment by @tamird on `src/decompressor.rs`:27 on 2018-01-17 19:02_

why isn't this an enum?

better yet, can't this stuff be done without spawning subprocesses? i imagine there are rust libraries for most of these.

---

_@okdana reviewed on 2018-01-17 19:20_

---

_Review comment by @okdana on `src/decompressor.rs`:27 on 2018-01-17 19:20_

The rationale for this implementation is explained in #225 and #539

There are still very few pure-Rust (de)compression libraries AFAIK

---

_Review comment by @BurntSushi on `src/decompressor.rs`:8 on 2018-01-30 02:29_

Imports should be consistent with the rest of the code. They should be grouped in three blocks, where each block is separated by whitespace. The first block are `std` imports. The second block are external crate imports. The third block are internal crate imports. Each block should be sorted lexicographically.

---

_Review comment by @BurntSushi on `src/decompressor.rs`:14 on 2018-01-30 02:30_

Since we are building these values in a `lazy_static!`, we don't actually need the `Vec`. We can use an `&'static [&'static str]` instead.

---

_Review comment by @BurntSushi on `src/decompressor.rs`:63 on 2018-01-30 02:31_

In general, I like to keep control flow straight-forward and avoid gratuitous nesting. Early returns are very useful for this. For example, this code could be written as:

```rust
if is_tar_archive(path) {
    debug!(...);
    return None;
}

let extension = ...;
```

That way, you don't need to indent the entire rest of the function.

---

_Review comment by @BurntSushi on `src/decompressor.rs`:90 on 2018-01-30 02:38_

OK, so the issue here is that this will deadlock in non-trivial cases. You might not have hit it if you only tested on small files. Basically the issue here is that if you don't read from stdout, then the process you spawned (if it's implemented sanely) will at some point block on writing to stdout because it is connected to a pipe. If the calling process never reads from stdout, then the spawned process will eventually block. If the calling process is in turn blocked on something else from the spawned process---which in this case, we are, because we're trying to read from `stderr` until the spawned process stops writing to it---then you wind up with a deadlock.

If you only test this on small files then you won't notice this because the spawned process will likely be using buffered output. And since it's connected to a pipe, the buffer is usually biggish (~4K?) as opposed to being line buffered on a tty. As a result, the spawned process will finish doing its thing before ever flushing its buffer, which gives the appearance of code that works. :-)

The "fix" to this is to couple the `child` with reading stdout and wait to read stderr until the process terminates. This basically means that you can't return `ChildStdout` directly. You need to return something else that wraps up this logic.

---

_Review comment by @BurntSushi on `src/decompressor.rs`:90 on 2018-01-30 02:39_

As an additional upside, the reader that you return can return normal error values, which are handled by the worker. This means you don't need to thread `no_messages` through the code.

---

_Review comment by @BurntSushi on `tests/tests.rs`:1699 on 2018-01-30 02:39_

Fantastic tests, thank you. :-)

---

_@BurntSushi reviewed on 2018-01-30 02:41_

@balajisivaraman Thanks so much for working on this! I really wanted to get this merged and there were some fiddly details to get right here, so I went ahead and based a new PR on your work here: #767. I hope that's OK. Most of your code made it through :-) but I did need to refactor the `get_reader` method (which is now `DecompressionReader::from_path`).

I left some comments on this PR that roughly correspond to the changes I made.

---

_Comment by @BurntSushi on 2018-01-30 14:14_

I merged this PR in #767. Thanks again @balajisivaraman!

---

_Closed by @BurntSushi on 2018-01-30 14:18_

---

_@balajisivaraman reviewed on 2018-01-30 15:27_

---

_Review comment by @balajisivaraman on `src/decompressor.rs`:90 on 2018-01-30 15:27_

Ah, I knew it. 😞  When I made this change and looked at the code, I knew there had to be something I was missing.

Thank you for the detailed write up on the issues with my code. It makes a lot of sense to me and helps me understand it better. It also gives me incentive to do some more reading up on pipes and child processes, which I will. 👍 

---

_@balajisivaraman reviewed on 2018-01-30 15:27_

---

_Review comment by @balajisivaraman on `src/decompressor.rs`:63 on 2018-01-30 15:27_

Ah, that makes a lot of sense. I did do this in the `worker.rs` file but missed it here. 👍 

---

_Comment by @balajisivaraman on 2018-01-30 15:28_

@BurntSushi, Thank you for merging this. It is one of my first significant contributions of any kind to a OSS project. I learned a lot from working on it as well as going through the changes you made on top of it. Cheers. :-)

---

_Branch deleted on 2018-01-30 15:29_

---

_Comment by @BurntSushi on 2018-01-30 15:35_

@balajisivaraman It's a helluva first contribution! It will be a highlight feature in the next release. :)

---
