```yaml
number: 2160
title: Symlink tests panic with privilege errors on Windows.
type: issue
state: closed
author: gskt17
labels: []
assignees: []
created_at: 2022-03-10T20:36:52Z
updated_at: 2022-03-11T21:50:19Z
url: https://github.com/BurntSushi/ripgrep/issues/2160
synced_at: 2026-01-12T16:13:24Z
```

# Symlink tests panic with privilege errors on Windows.

---

_@gskt17_

#### What version of ripgrep are you using?

13.0.0

#### How did you install ripgrep?

Built from source via Corrosion (Rust/Cargo plugin for Eclipse)

#### What operating system are you using ripgrep on?
Edition	Windows 10 Home
Version	20H2
Installed on	‎8/‎13/‎2020
OS build	19042.1586
Experience	Windows Feature Experience Pack 120.2212.4170.0

#### Describe your bug.
Some kind of permission error in tests involving symlinks on windows.


#### What are the steps to reproduce the behavior?

Run `cargo test symlink`

#### What is the actual behavior?

Relevant output: 
```

running 2 tests
test misc::symlink_nofollow ... FAILED
test regression::r1389_bad_symlinks_no_biscuit ... FAILED

failures:

---- misc::symlink_nofollow stdout ----
thread 'misc::symlink_nofollow' panicked at 'C:\Users\NXTangl\AppData\Local\Temp\ripgrep-tests\symlink_nofollow\0\foo/bar/baz: Os { code: 1314, kind: Uncategorized, message: "A required privilege is not held by the client." }', tests\util.rs:433:21
stack backtrace:
   0: std::panicking::begin_panic_handler
             at /rustc/9d1b2106e23b1abd32fce1f17267604a5102f57a\/library\std\src\panicking.rs:498
   1: core::panicking::panic_fmt
             at /rustc/9d1b2106e23b1abd32fce1f17267604a5102f57a\/library\core\src\panicking.rs:116
   2: integration::util::nice_err<tuple$<>,std::io::error::Error>
             at .\tests\util.rs:433
   3: integration::util::Dir::link_dir<str,str>
             at .\tests\util.rs:210
   4: integration::misc::symlink_nofollow::closure$0
             at .\tests\misc.rs:736
   5: integration::misc::symlink_nofollow
             at .\tests\macros.rs:7
   6: integration::misc::symlink_nofollow::closure$0
             at .\tests\macros.rs:5
   7: core::ops::function::FnOnce::call_once<integration::misc::symlink_nofollow::closure$0,tuple$<> >
             at /rustc/9d1b2106e23b1abd32fce1f17267604a5102f57a\library\core\src\ops\function.rs:227
   8: core::ops::function::FnOnce::call_once
             at /rustc/9d1b2106e23b1abd32fce1f17267604a5102f57a\library\core\src\ops\function.rs:227
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.

---- regression::r1389_bad_symlinks_no_biscuit stdout ----
thread 'regression::r1389_bad_symlinks_no_biscuit' panicked at 'C:\Users\NXTangl\AppData\Local\Temp\ripgrep-tests\r1389_bad_symlinks_no_biscuit\1\mylink: Os { code: 1314, kind: Uncategorized, message: "A required privilege is not held by the client." }', tests\util.rs:433:21
stack backtrace:
   0: std::panicking::begin_panic_handler
             at /rustc/9d1b2106e23b1abd32fce1f17267604a5102f57a\/library\std\src\panicking.rs:498
   1: core::panicking::panic_fmt
             at /rustc/9d1b2106e23b1abd32fce1f17267604a5102f57a\/library\core\src\panicking.rs:116
   2: integration::util::nice_err<tuple$<>,std::io::error::Error>
             at .\tests\util.rs:433
   3: integration::util::Dir::link_dir<str,str>
             at .\tests\util.rs:210
   4: integration::regression::r1389_bad_symlinks_no_biscuit::closure$0
             at .\tests\regression.rs:801
   5: integration::regression::r1389_bad_symlinks_no_biscuit
             at .\tests\macros.rs:7
   6: integration::regression::r1389_bad_symlinks_no_biscuit::closure$0
             at .\tests\macros.rs:5
   7: core::ops::function::FnOnce::call_once<integration::regression::r1389_bad_symlinks_no_biscuit::closure$0,tuple$<> >
             at /rustc/9d1b2106e23b1abd32fce1f17267604a5102f57a\library\core\src\ops\function.rs:227
   8: core::ops::function::FnOnce::call_once
             at /rustc/9d1b2106e23b1abd32fce1f17267604a5102f57a\library\core\src\ops\function.rs:227
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.


failures:
    misc::symlink_nofollow
    regression::r1389_bad_symlinks_no_biscuit

test result: FAILED. 0 passed; 2 failed; 0 ignored; 0 measured; 263 filtered out; finished in 0.58s
```


---

_Comment by @BurntSushi on 2022-03-10 21:08_

AIUI, this is because symlinks on Windows are weird. The best we could probably do here is detect and error and just silently skip the test. Otherwise, I'm not particularly inclined to fix this. The likely underlying problem, *as the error indicates*, is that you don't have permission to create symlinks. So tests that need to create symlinks fail.

---

_Comment by @gskt17 on 2022-03-11 21:50_

Cool, as long as it's a known problem. I'll go ahead and close this then.

---

_Closed by @gskt17 on 2022-03-11 21:50_

---
