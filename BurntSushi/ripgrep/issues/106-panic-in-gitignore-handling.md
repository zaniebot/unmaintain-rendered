```yaml
number: 106
title: panic in gitignore handling
type: issue
state: closed
author: daaku
labels:
  - bug
assignees: []
created_at: 2016-09-26T18:56:52Z
updated_at: 2016-09-26T23:24:46Z
url: https://github.com/BurntSushi/ripgrep/issues/106
synced_at: 2026-01-12T18:23:11Z
```

# panic in gitignore handling

---

_@daaku_

I got this panic. Looks like it's in the gitignore logic -- I can't share the whole file, but if you could guide me to figure out which line it's failing on I can share that.

```
thread 'main' panicked at 'called `Option::unwrap()` on a `None` value', ../src/libcore/option.rs|325| 5
|| stack backtrace:
|1| :           0x4fb292 - std::sys::backtrace::tracing::imp::write::h4b09e6e8c01db097
|2| :           0x4ff86d - std::panicking::default_hook::{{closure}}::h1d3243f546573ff4
|3| :           0x4fe6fa - std::panicking::default_hook::h96c288d728df3ebf
|4| :           0x4fecf8 - std::panicking::rust_panic_with_hook::hb1322e5f2588b4db
|5| :           0x4feb92 - std::panicking::begin_panic::hfbeda5aad583dc32
|6| :           0x4fead0 - std::panicking::begin_panic_fmt::h4fe9fb9d5109c4bf
|7| :           0x4fea51 - rust_begin_unwind
|8| :           0x5548ec - core::panicking::panic_fmt::h4395919ece15c671
|9| :           0x55481b - core::panicking::panic::hc74ff52ed78364e1
10:         || x41ae18 - rg::gitignore::GitignoreBuilder::add_path::h706ac92f76fbec29
at /buildslave/rust-buildbot/slave/nightly-dist-rustc-musl-linux/build/obj/../src/libcore/macros.rs|21| 1
at /home/travis/build/BurntSushi/ripgrep/src/gitignore.rs|301| 1
at /home/travis/build/BurntSushi/ripgrep/src/gitignore.rs|265| 5
11:         || x41f708 - rg::ignore::IgnoreDir::with_ignore_names::h6235506834840d62
at /home/travis/build/BurntSushi/ripgrep/src/ignore.rs|365| 5
12:         || x41eb56 - rg::ignore::Ignore::push::he3c5dc3b90e6c6a8
at /home/travis/build/BurntSushi/ripgrep/src/ignore.rs|326| 6
at /home/travis/build/BurntSushi/ripgrep/src/ignore.rs|192| 2
13:         || x43d4c7 - rg::run::hacc2ea9b10689e6d
at /home/travis/build/BurntSushi/ripgrep/src/walk.rs|64| 4
at /home/travis/build/BurntSushi/ripgrep/src/main.rs|132| 2
14:         || x437f4b - rg::main::h38b472398f351607
at /buildslave/rust-buildbot/slave/nightly-dist-rustc-musl-linux/build/obj/../src/libcore/result.rs|601| 1
at /home/travis/build/BurntSushi/ripgrep/src/main.rs|78| 8
15:         || x5071c6 - __rust_maybe_catch_panic
16:         || x4fe0ca - std::rt::lang_start::haaae1186de9de8cb
```


---

_Comment by @kevin-vigor on 2016-09-26 20:09_

I am experiencing this problem as well. Attached is a .gitignore file which leads to this crash.

[gitignore-of-doom.zip](https://github.com/BurntSushi/ripgrep/files/494004/gitignore-of-doom.zip)


---

_Comment by @BurntSushi on 2016-09-26 20:56_

My bet is that this is fixed by #104. Do you have lines that are only whitespace but otherwise empty? (I probably introduced this regression in my last round of bug fixes.)


---

_Label `bug` added by @BurntSushi on 2016-09-26 20:56_

---

_Closed by @BurntSushi on 2016-09-26 22:55_

---

_Comment by @daaku on 2016-09-26 23:24_

I looked at the `.gitignore` and did in fact find such a line! Will wait for the next release -- thanks for the quick fix!


---
