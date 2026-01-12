```yaml
number: 164
title: Segmentation fault on Sierra
type: issue
state: closed
author: elmart
labels:
  - bug
assignees: []
created_at: 2016-10-11T12:24:24Z
updated_at: 2016-10-12T07:59:24Z
url: https://github.com/BurntSushi/ripgrep/issues/164
synced_at: 2026-01-12T18:23:11Z
```

# Segmentation fault on Sierra

---

_@elmart_

I just installed release 0.2.2 on Sierra, and have a segfault when running `rg anything`.
Core dump attached.
[rg_2016-10-11-142243_Eliseos-MacBook-Pro.txt](https://github.com/BurntSushi/ripgrep/files/521767/rg_2016-10-11-142243_Eliseos-MacBook-Pro.txt)


---

_Comment by @BurntSushi on 2016-10-11 12:26_

Does the same happen with `0.2.1`?

How did you install `ripgrep`?

Can you run `rg --help`?

What if you run `rg -j1 anything`?


---

_Comment by @elmart on 2016-10-11 15:47_

> Does the same happen with 0.2.1?

No. 0.2.1 works right.

> How did you install ripgrep?

Manually. I downloaded the x86_64 darwign package, and copied `rg`to `/usr/local/bin` and `rg.1` to `/usr/local/share/man/man1`.

> Can you run rg --help?

Yes.

> What if you run rg -j1 anything?

Same. Segfault.


---

_Label `bug` added by @BurntSushi on 2016-10-11 21:07_

---

_Comment by @BurntSushi on 2016-10-11 21:07_

I can reproduce this on Yosemite:

```
mac:ripgrep andrew$ system_profiler SPSoftwareDataType
Software:

    System Software Overview:

      System Version: OS X 10.10.5 (14F1808)
      Kernel Version: Darwin 14.5.0
      Boot Volume: Macintosh HD
      Boot Mode: Normal
      Computer Name: mac
      User Name: Andrew Gallant (andrew)
      Secure Virtual Memory: Enabled
      Time since boot: 35 days 22:43
```


---

_Comment by @BurntSushi on 2016-10-11 21:22_

This looks like the specific point of failure:

```
==40663== Process terminating with default action of signal 11 (SIGSEGV)
==40663==  General Protection Fault
==40663==    at 0x100017A7B: rg::gitignore::Gitignore::matched_stripped::hdea6753d4bbf5227 (mem.rs:452)
==40663==    by 0x10001AADA: rg::ignore::IgnoreDir::matched::hac186e75072c724e (gitignore.rs:131)
==40663==    by 0x100031FFD: rg::run::h729464c93a862527 (ignore.rs:233)
==40663==    by 0x10002C95B: rg::main::h4d3f7b1f43247509 (result.rs:601)
==40663==    by 0x10010AFBA: __rust_maybe_catch_panic (in ./target/release/rg)
==40663==    by 0x100109566: std::rt::lang_start::h14cbded5fe3cd915 (in ./target/release/rg)
==40663==    by 0x1003605C8: start (in /usr/lib/system/libdyld.dylib)
==40663==    by 0x1: ???
==40663==    by 0x104A24D0E: ???
==40663==    by 0x104A24D22: ???
```

This points back into the implementation of `std::mem::swap`, which doesn't make a lot of sense.


---

_Comment by @BurntSushi on 2016-10-11 21:25_

I can reproduce this when compiling from source, but only on Mac, not Linux.


---

_Comment by @BurntSushi on 2016-10-11 21:30_

A debug build does not segfault.


---

_Comment by @BurntSushi on 2016-10-11 21:35_

_sigh_ It looks like this is happening because of thread locals. I don't believe I'm misusing them, but if I remove the thread local, things work again.


---

_Comment by @BurntSushi on 2016-10-11 21:40_

To clarify, I changed this code:

``` rust
    pub fn matched_stripped(&self, path: &Path, is_dir: bool) -> Match {
        thread_local! {
            static MATCHES: RefCell<Vec<usize>> = {
                RefCell::new(vec![])
            }
        };
        MATCHES.with(|matches| {
            let mut matches = matches.borrow_mut();
            let candidate = Candidate::new(path);
            self.set.matches_candidate_into(&candidate, &mut *matches);
            for &i in matches.iter().rev() {
                let pat = &self.patterns[i];
                if !pat.only_dir || is_dir {
                    return if pat.whitelist {
                        Match::Whitelist(pat)
                    } else {
                        Match::Ignored(pat)
                    };
                }
            }
            Match::None
        })
    }
```

to this:

``` rust
    pub fn matched_stripped(&self, path: &Path, is_dir: bool) -> Match {
        let mut matches = vec![];
        let candidate = Candidate::new(path);
        self.set.matches_candidate_into(&candidate, &mut matches);
        for &i in matches.iter().rev() {
            let pat = &self.patterns[i];
            if !pat.only_dir || is_dir {
                return if pat.whitelist {
                    Match::Whitelist(pat)
                } else {
                    Match::Ignored(pat)
                };
            }
        }
        Match::None
    }
```


---

_Comment by @BurntSushi on 2016-10-11 21:48_

Compiling with Rust stable works. Compiling with Rust beta or nightly yields the sigsegv above.


---

_Comment by @elmart on 2016-10-11 21:48_

Not sure if (still) relevant, but I found this.
https://bugzilla.mozilla.org/show_bug.cgi?id=1164109


---

_Comment by @BurntSushi on 2016-10-11 21:49_

@elmart Hmm that looks related to OS X 10.6 or older.


---

_Comment by @BurntSushi on 2016-10-11 21:54_

I can't seem to come up with a minimally reproducible example. Trying this works fine on Rust beta/nightly on Mac:

``` rust
use std::cell::RefCell;

fn foo(n: usize) -> bool {
    thread_local! {
        static MATCHES: RefCell<Vec<usize>> = {
            RefCell::new(vec![])
        }
    };
    MATCHES.with(|matches| {
        let mut matches = matches.borrow_mut();
        matches.clear();
        for i in 0..n {
            matches.push(i);
        }
        for &i in matches.iter().rev() {
            if i > 0 && i % 7 == 0 {
                return true;
            }
        }
        false
    })
}

fn main() {
    println!("{}", foo(::std::env::args().count()));
    println!("{}", foo(::std::env::args().count() * 2));
}
```


---

_Comment by @BurntSushi on 2016-10-11 21:55_

cc @alexcrichton Do you have any ideas about this? TL;DR - I'm seeing a segfault on macos (not Linux) from using thread locals on current Rust beta/nightly, but _not_ on current Rust stable. Efforts to repro a small example have failed. :-(


---

_Comment by @alexcrichton on 2016-10-11 22:10_

Oh dear, sounds quite suspicious! The only funkiness I know of with OSX and thread locals has to do with destructors _maybe_, but there's no interesting destructors here.

@BurntSushi what's the current reproduction? (large or small).

This is quite worrisome!


---

_Comment by @BurntSushi on 2016-10-11 22:21_

@alexcrichton This should do it:

```
$ git clone git://github.com/BurntSushi/ripgrep
$ cd ripgrep
$ git checkout 0.2.2
$ cargo build --release
$ ./target/release/rg cpu
Segmentation fault: 11
```

(I'm about to commit a work around that stops using thread locals, so this issue is hopefully going to get closed, but it's not because I've figured out the underlying problem. :-))


---

_Closed by @BurntSushi on 2016-10-11 22:26_

---

_Comment by @BurntSushi on 2016-10-11 22:27_

I've fixed this on master and will put out a `0.2.3` tonight.

(The fix was to switch to the `thread_local` crate.)


---

_Comment by @alexcrichton on 2016-10-11 22:38_

The plot thickens! I can't seem to reproduce...

```
$ system_profiler SPSoftwareDataType
Software:

    System Software Overview:

      System Version: macOS 10.12 (16A323)
      Kernel Version: Darwin 16.0.0
      Boot Volume: Macintosh HD
      Boot Mode: Normal
      Computer Name: e95b3ef3e8a3ef74
      User Name: Alexander Crichton (acrichton)
      Secure Virtual Memory: Enabled
      System Integrity Protection: Enabled
      Time since boot: 15 days 18:28

$ git rev-parse HEAD
4981991a6e4d1808d54c8d20fc4d93fcf0c1abfe
$ rustc +nightly -vV
rustc 1.14.0-nightly (a3bc191b5 2016-10-10)                
binary: rustc                                              
commit-hash: a3bc191b5f41df5143cc65084b13999896411817      
commit-date: 2016-10-10                                    
host: x86_64-apple-darwin                                  
release: 1.14.0-nightly                                    
$ cargo +nightly build --release
...
$ ./target/release/rg cpu
...
```

(no segfaults)


---

_Comment by @BurntSushi on 2016-10-11 23:46_

ripgrep `0.2.3` should be out momentarily. Thanks for reporting this @elmart!


---

_Comment by @elmart on 2016-10-12 07:59_

You're welcome. Thx for fixing it so quickly.


---
