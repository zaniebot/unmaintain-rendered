---
number: 16903
title: "[Panic] ruff crashes when executed from a deleted folder on mac"
type: issue
state: closed
author: cbeauchesne
labels:
  - bug
assignees: []
created_at: 2025-03-21T18:04:28Z
updated_at: 2025-04-01T12:17:09Z
url: https://github.com/astral-sh/ruff/issues/16903
synced_at: 2026-01-07T13:12:16-06:00
---

# [Panic] ruff crashes when executed from a deleted folder on mac

---

_Issue opened by @cbeauchesne on 2025-03-21 18:04_

How to reproduce : 

in terminal 1 : 

```bash
mkdir tmp
cd tmp
```

in terminal 2

```bash
rm -rf tmp
```

back in terminal 1 : 

```
RUST_BACKTRACE=full ruff check

error: Ruff crashed. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BPanic%5D

...quoting the executed command, along with the relevant file contents and `pyproject.toml` settings, we'd be very appreciative!

thread 'main' panicked at /Users/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/path-dedot-3.1.1/src/lib.rs:330:70:
called `Result::unwrap()` on an `Err` value: Os { code: 2, kind: NotFound, message: "No such file or directory" }
stack backtrace:
   0:        0x10f6a86e8 - <std::sys::backtrace::BacktraceLock::print::DisplayBacktrace as core::fmt::Display>::fmt::hcaf66bc4c0c453df
   1:        0x10e7e360b - core::fmt::write::hc9c5f1836b413410
   2:        0x10f6a3f12 - std::io::Write::write_fmt::h49df280499063c09
   3:        0x10f6a99da - std::panicking::default_hook::{{closure}}::h52c0b2f44f6107c5
   4:        0x10f6a9642 - std::panicking::default_hook::h5a6cf31501c161b2
   5:        0x10ecd794a - ruff::run::{{closure}}::hd41b263f42227ea1
   6:        0x10f6aab9e - std::panicking::rust_panic_with_hook::hda4640ee332466e9
   7:        0x10f6a9ff5 - std::panicking::begin_panic_handler::{{closure}}::haa3060694b34ea3d
   8:        0x10f6a8bc9 - std::sys::backtrace::__rust_end_short_backtrace::h8eb44913cfe71457
   9:        0x10f6a9c3c - _rust_begin_unwind
  10:        0x10f750d4a - core::panicking::panic_fmt::h31edc3d6ff0aadca
  11:        0x10f7511c5 - core::result::unwrap_failed::h2169d1cb8033298b
  12:        0x10e9b57c9 - core::ops::function::FnOnce::call_once::h4a1f9b6c37c4efb9
  13:        0x10ec9e540 - once_cell::imp::OnceCell<T>::initialize::{{closure}}::hd57f3dc11ebdb7bf
  14:        0x10e9b392a - once_cell::imp::initialize_or_wait::h1b6a1b2a2c5ea054
  15:        0x10f776c3c - once_cell::imp::OnceCell<T>::initialize::h6c5acec7735a0040
  16:        0x10ebcd23b - ruff::resolve::resolve::h3abf4f7e3ef26cd9
  17:        0x10ecd7ad1 - ruff::check::h93bfe06211e45642
  18:        0x10ecd6d01 - ruff::run::h4a796964f3396cea
  19:        0x10ecdc441 - ruff::main::h12661c1f2feb882f
  20:        0x10ece1936 - std::sys::backtrace::__rust_begin_short_backtrace::h61cdb7c3f5f8eba8
  21:        0x10ecdfb0c - std::rt::lang_start::{{closure}}::hf5a90ee77f97c307
  22:        0x10f69b27f - std::rt::lang_start_internal::hb1473645dbe40065
  23:        0x10ecdc97c - _main
  24:     0x7ff80d741345 - <unknown>
```

## Context

* Python 3.12.4
* MacOS Sonoma 14.6 


---

_Renamed from "[Panic] ruff crash when executed from a deleted folder on mac" to "[Panic] ruff crashes when executed from a deleted folder on mac" by @cbeauchesne on 2025-03-21 18:05_

---

_Comment by @ntBre on 2025-03-21 18:40_

Thanks for the report! I think this is pretty much the expected behavior and was reported before in https://github.com/astral-sh/ruff/issues/16017.

It would be a pretty defensive check to ensure that the current working directory exists, but I guess we *could* do it.

---

_Label `question` added by @ntBre on 2025-03-21 18:40_

---

_Comment by @cbeauchesne on 2025-03-21 20:17_

 I see two ~good~ not so bad reasons for handling this : 

* saving 3s of user's brain when it happen, users loves nice and polished CLIs
* saving yourself to respond to the same issue that may come later (I loved your output with the link to create an issue 👍  -> as a side effect, we're strongly encouraged to create them)

BTW, I take the opportunity to congrats, I replaced black+pylint by ruff months ago, and it working amazingly well !! 

---

_Comment by @ntBre on 2025-03-21 20:51_

I think those are good points. And thank you for the kind words!

---

_Label `question` removed by @ntBre on 2025-03-21 20:51_

---

_Label `bug` added by @ntBre on 2025-03-21 20:51_

---

_Referenced in [astral-sh/ruff#17054](../../astral-sh/ruff/pulls/17054.md) on 2025-03-28 23:07_

---

_Closed by @MichaReiser on 2025-04-01 12:17_

---

_Closed by @MichaReiser on 2025-04-01 12:17_

---
