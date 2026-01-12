```yaml
number: 1508
title: "globset: Access violation when built with lto = \"thin\" on Windows"
type: issue
state: closed
author: forbjok
labels: []
assignees: []
created_at: 2020-03-04T23:00:19Z
updated_at: 2020-03-05T17:03:30Z
url: https://github.com/BurntSushi/ripgrep/issues/1508
synced_at: 2026-01-12T16:13:23Z
```

# globset: Access violation when built with lto = "thin" on Windows

---

_@forbjok_

Version: globset v0.4.4
OS: Windows 10 Pro x64 v1909

Calling globset.is_match(...) causes the program to crash with an access violation if built with `lto = "thin"` under certain circumstances.

The circumstances being (as far as I can tell):
* OS = Windows (tested on Arch Linux and could not reproduce it there)
* Target = x86_64-pc-windows-msvc (I haven't tested every other target, but it does not happen with i686-pc-windows-msvc)

I don't know if this is actually an error in globset, or if it's some obscure compiler error that just happens to be triggered by globset.

It happens both with stable and nightly Rust.

Error message:
![ConEmu64_WJEnWXAnNJ](https://user-images.githubusercontent.com/4687674/75930152-4f6c4600-5e72-11ea-92ec-4750f271240d.png)

Minimal reproduction project:
[lto-thin-repro.zip](https://github.com/BurntSushi/ripgrep/files/4289999/lto-thin-repro.zip)


---

_Comment by @BurntSushi on 2020-03-04 23:15_

Thanks for the bug report, but yeah, this doesn't look like it belongs here. This looks more like a compiler bug I guess? You might want to file it on rust-lang/rust, although this might be hard to diagnose without a smaller reproduction (i.e., that doesn't use a fairly big crate like globset).

---

_Closed by @BurntSushi on 2020-03-04 23:15_

---

_Comment by @lespea on 2020-03-04 23:27_

Do you have ryzen by chance?  https://github.com/rust-lang/rust/issues/63959

Should be fixed on stable soon.

---

_Comment by @forbjok on 2020-03-04 23:30_

> Do you have ryzen by chance? [rust-lang/rust#63959](https://github.com/rust-lang/rust/issues/63959)
> 
> Should be fixed on stable soon.

Nope, it's an Intel Core i9-9900K CPU on Intel Z390 chipset.

---

_Comment by @lespea on 2020-03-04 23:34_

Interesting.  Does it panic on nightly as well?

---

_Comment by @forbjok on 2020-03-05 11:27_

Yep, same error in both stable and nightly. I also tested on an older machine (Intel Core i7-6700, Intel Z170 chipset), and I get it there as well.

---

_Comment by @forbjok on 2020-03-05 17:03_

I remembered I had configured cargo to use the LLVM linker, because it's faster. Turns out the problem is also restricted to the LLVM linker, but still only the 64-bit target.
Seems pretty likely that it's actually an LLVM linker bug.

---
