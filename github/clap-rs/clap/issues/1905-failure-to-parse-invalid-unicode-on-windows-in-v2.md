---
number: 1905
title: failure to parse invalid Unicode on Windows in v2.33.0 (but not master)
type: issue
state: closed
author: oconnor663
labels:
  - C-bug
assignees: []
created_at: 2020-05-05T06:12:02Z
updated_at: 2020-05-11T16:42:30Z
url: https://github.com/clap-rs/clap/issues/1905
synced_at: 2026-01-07T13:12:19-06:00
---

# failure to parse invalid Unicode on Windows in v2.33.0 (but not master)

---

_Issue opened by @oconnor663 on 2020-05-05 06:12_

The following is for the current release version of Clap, v2.33.0. (The bug does not appear to repro in Clap `master`. More on that below.) I've set up a repository containing a minimal repro of this problem: https://github.com/oconnor663/clap_unicode_test. You can see a CI run demonstrating the failure here: https://github.com/oconnor663/clap_unicode_test/runs/645077320. To repro the failure yourself, run `cargo test` in that repository.

The issue is that passing invalid Unicode on the command line to a Clap binary causes an "unexpected invalid UTF-8 code point" error, but only on Windows. Here's the `main.rs`:

```rust
use clap::{App, Arg};

fn main() {
    let matches = App::new("test")
        .arg(Arg::with_name("ARG").multiple(true))
        .get_matches();
    if let Some(args) = matches.values_of_os("ARG") {
        for arg in args {
            if arg.to_str().is_some() {
                println!("{:?} (valid Unicode)", arg);
            } else {
                println!("{:?} (INVALID Unicode!!!)", arg);
            }
        }
    }
}
```

Here's the test that runs on Linux and macOS. This one passes:

```rust
#[test]
#[cfg(unix)]
fn test_invalid_unicode_on_unix() {
    use std::os::unix::ffi::OsStringExt;
    let mut cmd = Command::new(env!("CARGO_BIN_EXE_test"));
    cmd.arg("abcdef");
    cmd.arg(OsString::from_vec(b"abc\xffdef".to_vec()));
    let status = cmd.status().unwrap();
    assert!(status.success());
}
```

In that case, the actual output of the (successful) binary is:

```
"abcdef" (valid Unicode)
"abc\xFFdef" (INVALID Unicode!!!)
```

And here's the test that fails on Windows:

```rust
#[test]
#[cfg(windows)]
fn test_invalid_unicode_on_windows() {
    use std::os::windows::ffi::OsStringExt;
    let mut cmd = Command::new(env!("CARGO_BIN_EXE_test"));
    cmd.arg("abcdef");
    let surrogate_char = 0xDC00;
    let bad_unicode_wchars = [
        'a' as u16,
        'b' as u16,
        'c' as u16,
        surrogate_char,
        'd' as u16,
        'e' as u16,
        'f' as u16,
    ];
    let bad_osstring = OsString::from_wide(&bad_unicode_wchars);
    cmd.arg(bad_osstring);
    let status = cmd.status().unwrap();
    assert!(status.success());
}
```

In this one, the binary crashes in the `clap::App::get_matches` call.

The tests are different between Unix and Windows because the way to construct an invalid Unicode `OsString` is different. On Unix, we just insert a non-UTF-8 byte into a byte slice. On Windows, we need to insert a unpaired surrogate character into a slice of `u16`. Other than that, the tests are effectively the same.

When I try the master branch of Clap (see [the `clap_master` branch of my repository](https://github.com/oconnor663/clap_unicode_test/tree/clap_master) and [this CI run](github.com/oconnor663/clap_unicode_test/runs/645103446)), the problem appears fixed. That's awesome! However, it looks like Clap might be in the middle of a substantial refactoring, and I don't know how long it will take for these fixes to see a release. Should something be backported in the meantime?

---

_Label `T: bug` added by @oconnor663 on 2020-05-05 06:12_

---

_Referenced in [BLAKE3-team/BLAKE3#33](../../BLAKE3-team/BLAKE3/issues/33.md) on 2020-05-05 06:13_

---

_Comment by @pksunkara on 2020-05-05 08:38_

I don't think we will be backporting this fix because 2.x branch is frozen and only security fixes are going to be allowed. We are working hard on clap v3 and I am confident to say that the full release will be no more than 3 months away if we keep up the current pace of development.

---

_Comment by @oconnor663 on 2020-05-05 14:08_

If I was able to figure out a fix on the 2.x branch, and it didn't look too hairy, would you be willing to release it? I'm interested in fixing this for my own work (it's a blocker for properly testing `b3sum --check`), and it might be valuable for other command line apps that never bother to upgrade out of the 2.x series.

---

_Comment by @CreepySkeleton on 2020-05-05 14:54_

@oconnor663 I say we wouldn't because the code in question is a few years old code. The last time someone touched it was a year ago or so, nobody here (maybe short of @kbknapp) is familiar with that codebase. We simply can't review your fix and be certain that it won't break something else, even if it's just a few lines.

---

_Comment by @kbknapp on 2020-05-05 15:33_

@oconnor663 

We appreciate the very detailed bug report!

@pksunkara is correct, 2.x is currently frozen except security fixes. However, I'm willing to take a look at a PR to fix the issue, since I'm familiar with the codebase. I can't promise a merge if it looks to be too high a maintenance burden. Lets at least see how far reaching the fix is, since causing a crash on valid input *could* be considered a security issue if we wend the optics a bit to include remote binaries with DDoS potential.

---

_Comment by @oconnor663 on 2020-05-05 16:47_

Thanks for all the prompt replies. It looks like the fix can be quite targeted, essentially a one-line change to the existing `OsStr::starts_with` method, plus a new pure helper function and some tests. I haven't tried to comb the codebase for all the places where `OsStr` is assumed to be valid Unicode, but this single change at least seems to fix my use case. I want to add a couple more tests, then I'll submit a proper PR.

---

_Comment by @kbknapp on 2020-05-05 16:50_

There shouldn't be many places where `OsStr` is assumed valid Unicode. Also see the potentially relevant PR #1893 the underlying code is different since its on the 3.x master branch, but deals with `OsStr::starts_with`

---

_Comment by @pksunkara on 2020-05-05 17:39_

I think this was fixed in master because of the recent refactor done on OsStr by @dylni. It was a pretty big change (IMO).

---

_Referenced in [clap-rs/clap#1907](../../clap-rs/clap/pulls/1907.md) on 2020-05-05 20:11_

---

_Comment by @oconnor663 on 2020-05-05 20:16_

I've submitted https://github.com/clap-rs/clap/pull/1907. Despite appearances, I think this is a targeted fix :) Most of the code in there is new tests. The meat of the change is that I've defined the `windows_osstr_starts_with` standalone function on Windows only, and I've changed `OsStr::starts_with` to call that instead on Windows. This fixes the simplest cases of invalid Unicode parsing, but it leaves panics in place for cases like `--arg=[invalid]`, which would require string splitting and therefore allocation and a broader API change. So this is not nearly as thorough as the refactoring that @pksunkara mentioned, but hopefully it's small enough that it's unlikely to introduce new bugs.

---

_Comment by @dylni on 2020-05-05 20:38_

Thanks for the ping @pksunkara. Yes, this was fixed in [this pull request](https://github.com/clap-rs/clap/pull/1852).

---

_Comment by @kbknapp on 2020-05-07 18:19_

> I think this was fixed in master because of the recent refactor done on OsStr by @dylni. It was a pretty big change (IMO).

Yeah that's why I referenced the other PR (part of the refactor). I haven't had time the last day or so, but I'll look at this new PR by this weekend and see if it's something that could go on the 2.x branch with the understanding that if v2-master is currently broken, so long as we don't *break more* we're in a slightly better spot even if its not a total fix.

Once v3 is fully released I fully intend to EOL v2 except for hard security patches

---

_Closed by @pksunkara on 2020-05-11 16:42_

---
