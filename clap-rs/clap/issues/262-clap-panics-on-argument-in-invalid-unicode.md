```yaml
number: 262
title: Clap panics on argument in invalid unicode
type: issue
state: closed
author: remram44
labels:
  - C-bug
  - C-enhancement
assignees: []
created_at: 2015-09-21T14:24:07Z
updated_at: 2018-08-02T03:29:44Z
url: https://github.com/clap-rs/clap/issues/262
synced_at: 2026-01-12T16:14:08Z
```

# Clap panics on argument in invalid unicode

---

_@remram44_

I can understand your library not being designed to handle arguments that are not valid unicode (which are possible on UNIX, and can be valid filenames, but heh), however having your library panic the calling thread is not cool. Please consider using Result instead of crashing.

Reproduce:

```
echo -e 'r\xE9mi' | xargs target/debug/myprogramusingclap
```

(note that you can't use `cargo run`, it can't handle unicode either (but at least it doesn't panic))

Similar to https://github.com/rust-lang/getopts/pull/30; I'm still looking for a safe argument-parsing library :cry:


---

_Label `T: bug` added by @sru on 2015-09-21 14:27_

---

_Label `C: args` added by @sru on 2015-09-21 14:27_

---

_Comment by @sru on 2015-09-21 15:01_

Caused by [`std::env::Args`](http://doc.rust-lang.org/std/env/fn.args.html).

Using `std::env::ArgsOs` does not solve this issue because the `get_matches_*` functions take `AsRef<str>`, not `AsRef<OsStr>`. I tried to change all of them to `AsRef<OsStr>`, but it did not work.

One solution could be: If the argument passed is not valid unicode, we could first check if it is a valid directory or file. If it isn't, then produce error.


---

_Comment by @WildCryptoFox on 2015-09-21 16:28_

@remram44 Would you be okay with `OsStr::to_string_lossy`?

> Any non-Unicode sequences are replaced with U+FFFD REPLACEMENT CHARACTER.


---

_Comment by @remram44 on 2015-09-21 17:16_

I think it's kind of weird -- in the Rust stdlib, lossy functions are clearly marked as such, and that is not the case of `get_matches()`. If a filename is mangled this way, the application will not access the correct pass, and thus fail with a surprising "file not found" or worse, go on creating the wrong file (with a replacement character in it). On the other hand you can't easily change your API at this point, and this behavior is better than panicking(?)

Another option is to make [get_matches_safe()](http://kbknapp.github.io/clap-rs/clap/struct.App.html#method.get_matches_safe) return an error (not panic) on invalid unicode, and get_matches() print out a readable error message instead of the default panic text.


---

_Comment by @WildCryptoFox on 2015-09-21 17:22_

> On the other hand you can't easily change your API at this point

@remram44 *cough* I'm rewriting the entire clap-rs in #259, I'm open for changes like this. I am however, so far having lifetime issues when trying to use `OsStr`. ;-)


---

_Comment by @WildCryptoFox on 2015-09-21 17:44_

@remram44 Partly tested (compiles; no tests for invalid Unicode chars yet) I have `to_string_lossy` working in my code. Now we can use `OsArgs`. **Note**: This is for my `2.0` rewrite **NOT** `1.4`.

I might take an attempt to porting it over to `clap1.4` tomorrow.

Snapshot of current code for `2.0`: https://gist.github.com/7897c09aab4186186576
- Lossy matching prevents flags and positional arguments from containing the invalid Unicode (in raw form). While in `--foo <arg>`, arg may contain invalid Unicode.

---

I'm out for the night. zzZZZ


---

_Referenced in [clap-rs/clap#259](../../clap-rs/clap/issues/259.md) on 2015-09-21 17:52_

---

_Comment by @kbknapp on 2015-09-21 23:35_

@remram44 Returning a Result on invalid unicode should be an easy addition. If this works for you we can probably have this added by tomorrow with a new version out to crates.io

It would also be easy to add a `*_lossy()` version to the `get_matches()` family. We're also open for some discussion on what `clap2x`'s unicode handling will look like as stated by @james-darkfox 


---

_Label `T: enhancement` added by @kbknapp on 2015-09-21 23:36_

---

_Label `D: easy` added by @kbknapp on 2015-09-21 23:36_

---

_Label `P3: want to have` added by @kbknapp on 2015-09-21 23:36_

---

_Comment by @sru on 2015-09-22 01:54_

@kbknapp If we were to add this, we have to change the function signature of `get_matches` families to take iterator of `AsRef<OsStr>`. Is that okay?


---

_Referenced in [clap-rs/clap#265](../../clap-rs/clap/pulls/265.md) on 2015-09-22 02:08_

---

_Comment by @kbknapp on 2015-09-22 02:18_

@sru Yep that's exactly what I did in #265 which doesn't affect things too heavily. I compared benches before and after, and no tests had to be changed to accommodate.

I've heard it said that, "All `str`s are `OsStr`s, but not all `OsStr`s are `str`s"...if you like logic questions :wink: 


---

_Comment by @kbknapp on 2015-09-22 02:23_

@remram44 #265 allows returning a Result `Err` on invalid unicode chars, or an optional lossy version. Granted this doesn't help with things like path/file names which have invalid unicode, but it's better than the current state of `clap` because it won't `panic!` unless you don't care about it or want it to `panic!` on invalid unicode.


---

_Comment by @WildCryptoFox on 2015-09-22 05:00_

@kbknapp Just a tip: `str` and `OsStr` both implement `AsRef<OsStr>`


---

_Comment by @remram44 on 2015-09-22 05:02_

`OsStr` doesn't implement `AsRef<str>`, no.


---

_Comment by @WildCryptoFox on 2015-09-22 05:20_

Oops. My haste. Sorry. _edited previous comment_ Thanks.


---

_Comment by @kbknapp on 2015-09-22 12:28_

@remram44 try the latest master and let us know if it works for your use case. If so, we'll publish a new version to crates.io, close this issue, and open a more general unicode handling tracking issue. 


---

_Comment by @kbknapp on 2015-09-22 17:52_

@remram44 v1.4.1 is out on crates.io now which adds the safer `*_safe()` methods and `*_lossy()` version of the `get_matches` family. 

See #269 for general invalid unicode discussion for the future


---

_Closed by @kbknapp on 2015-09-25 23:18_

---
