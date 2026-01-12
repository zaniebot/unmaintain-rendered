```yaml
number: 1602
title: "test misc::compressed_uncompress ... FAILED"
type: issue
state: closed
author: gyakovlev
labels:
  - bug
assignees: []
created_at: 2020-05-28T12:13:57Z
updated_at: 2020-05-29T13:50:46Z
url: https://github.com/BurntSushi/ripgrep/issues/1602
synced_at: 2026-01-12T16:13:23Z
```

# test misc::compressed_uncompress ... FAILED

---

_@gyakovlev_

Hi, got a test falure here. Does not look like a huge deal, but wanted to let you know.

#### What version of ripgrep are you using?

ripgrep 12.1.0
-SIMD -AVX (compiled)

#### How did you install ripgrep?

from gentoo repositories (I maintain ripgrep package/ebuild)

#### What operating system are you using ripgrep on?

Genroo rolling. tested on x86_64 and ppc64, same test failure.
distribution package of rustc 1.43.1 (which I also maintain)
also reproducable with official tarball of  rustc-1.43.1

#### Describe your bug.

.Z compress test fails with both uncompress provided by gzip and ncompress package.
gentoo bug and full buld log available here
https://bugs.gentoo.org/725778

#### What are the steps to reproduce the behavior?

cargo test

#### What is the actual behavior?

```
test misc::compressed_uncompress ... FAILED

failures:

---- misc::compressed_uncompress stdout ----
thread 'misc::compressed_uncompress' panicked at '

==========
command failed but expected success!

Did your search end up with no results?

command: "/var/tmp/portage/sys-apps/ripgrep-12.1.0/work/ripgrep-12.1.0/target/release/deps/../rg" "--path-separator" "/" "-z" "Sherlock" "sherlock.Z"

cwd: /var/tmp/portage/sys-apps/ripgrep-12.1.0/temp/ripgrep-tests/compressed_uncompress/108

dir list: ["/var/tmp/portage/sys-apps/ripgrep-12.1.0/temp/ripgrep-tests/compressed_uncompress/108", "/var/tmp/portage/sys-apps/ripgrep-12.1.0/temp/ripgrep-tests/compressed_uncompress/108/sherlock.Z"]

status: exit code: 1

stdout:

stderr:

==========
==========
', tests/util.rs:406:13
stack backtrace:
   0:        0x106826d94 - <std::sys_common::backtrace::_print::DisplayBacktrace as core::fmt::Display>::fmt::h1e21cbf03e4e5763
   1:        0x10685a364 - core::fmt::write::hb3ff83338e8b2d66
   2:        0x1067d3d58 - std::io::Write::write_fmt::h5fd38946bf8c7a11
   3:        0x10680f8dc - std::io::impls::<impl std::io::Write for alloc::boxed::Box<W>>::write_fmt::h0e0d8e73cfc9ac67
   4:        0x106810964 - std::panicking::default_hook::{{closure}}::ha159ab335d6d2780
   5:        0x1068104dc - std::panicking::default_hook::hc6f8b73d36ecf078
   6:        0x1068112ac - std::panicking::rust_panic_with_hook::h2b8f1d2ef0497000
   7:        0x106810d08 - rust_begin_unwind
   8:        0x106810c30 - std::panicking::begin_panic_fmt::h4ffe547a81dbf942
   9:        0x1067a621c - integration::util::TestCommand::expect_success::h742640728124be65
  10:        0x1067a4c74 - integration::util::TestCommand::stdout::h9c875b01ea94d149
  11:        0x106781210 - integration::misc::compressed_uncompress::h040d70216738de96
  12:        0x1067b7194 - test::__rust_begin_short_backtrace::h5d4bbc96f3e45b05
  13:        0x1067bdad8 - <alloc::boxed::Box<F> as core::ops::function::FnOnce<A>>::call_once::he06369dcdc359518
  14:        0x1068341d0 - __rust_try
  15:        0x10683404c - __rust_maybe_catch_panic
  16:        0x1067b7440 - test::run_test_in_process::hab58b4da0b1dab28
  17:        0x1067c3fa4 - std::sys_common::backtrace::__rust_begin_short_backtrace::h71a54a46a35eda28
  18:        0x1067c5378 - std::panicking::try::do_call::h3448860c56f573f7
  19:        0x1068341d0 - __rust_try
  20:        0x10683404c - __rust_maybe_catch_panic
  21:        0x1067bc87c - core::ops::function::FnOnce::call_once{{vtable.shim}}::h513d035f57eb50fe
  22:        0x10680f628 - <alloc::boxed::Box<F> as core::ops::function::FnOnce<A>>::call_once::hefcd91f4e78e44b5
  23:        0x106828074 - std::sys_common::thread::start_thread::hf3c0bc78acf4ce1e
  24:        0x10681c798 - std::sys::unix::thread::Thread::new::thread_start::he003e4e14f3e450a
  25:     0x7fff9de1850c - start_thread
                               at /var/tmp/portage/sys-libs/glibc-2.31-r3/work/glibc-2.31/nptl/pthread_create.c:477
  26:     0x7fff9dcf8b34 - __clone


failures:
    misc::compressed_uncompress

test result: FAILED. 254 passed; 1 failed; 0 ignored; 0 measured; 0 filtered out
```

#### What is the expected behavior?

Test should succeed.



---

_Comment by @BurntSushi on 2020-05-28 12:25_

I'm not sure this is a problem on ripgrep's end. Here is the test in question:

```
rgtest!(compressed_uncompress, |dir: Dir, mut cmd: TestCommand| {
    if !cmd_exists("uncompress") {
        return;
    }

    dir.create_bytes("sherlock.Z", include_bytes!("./data/sherlock.Z"));
    cmd.arg("-z").arg("Sherlock").arg("sherlock.Z");

    let expected = "\
    For the Doctor Watsons of this world, as opposed to the Sherlock
be, to a very large extent, the result of luck. Sherlock Holmes
";
    eqnice!(expected, cmd.stdout());
});
```

All this is doing is doing is copying the bytes from a pre-compressed `sherlock.Z` file in this repository, and then asking ripgrep to search it with its `-z` flag. The test failing implies, to me, that `uncompress` is installed but is for some reason not decompressing the `sherlock.Z` flag. So to debug this, I'd try this in your environment (from the root of a ripgrep checkout):

```
$ uncompress -c tests/data/sherlock.Z > /tmp/sherlock
$ rg 'Sherlock' /tmp/sherlock
1:For the Doctor Watsons of this world, as opposed to the Sherlock
3:be, to a very large extent, the result of luck. Sherlock Holmes
```

Do you get the same output? If so, then I'd be perplexed. But if not, then I think you will have isolated the problem.

---

_Label `question` added by @BurntSushi on 2020-05-28 12:25_

---

_Comment by @gyakovlev on 2020-05-28 18:36_

well looks like it's not working for real.

```shell-session
ya@hydra /tmp $ which uncompress
/usr/bin/uncompress

ya@hydra /tmp $ file /usr/bin/uncompress
/usr/bin/uncompress: symbolic link to /usr/bin/compress

file /usr/bin/compress
/usr/bin/compress: ELF 64-bit LSB pie executable, 64-bit PowerPC or cisco 7500, version 1 (SYSV), dynamically linked, interpreter /lib64/ld64.so.2, for GNU/Linux 3.10.0, stripped

ya@hydra /tmp $ file sherlock.Z
sherlock.Z: compress'd data 16 bits

ya@hydra /tmp $ uncompress -c sherlock.Z
For the Doctor Watsons of this world, as opposed to the Sherlock
Holmeses, success in the province of detective work must always
be, to a very large extent, the result of luck. Sherlock Holmes
can extract a clew from a wisp of straw or a flake of cigar ash;
but Doctor Watson has to have it taken out for him and dusted,
and exhibited clearly, with a label attached.

ya@hydra /tmp $ zgrep Sherlock sherlock.Z
For the Doctor Watsons of this world, as opposed to the Sherlock
be, to a very large extent, the result of luck. Sherlock Holmes
```

```
rg --search-zip Sherlock sherlock.Z
rg  -z Sherlock  sherlock.Z
rg -z 'Sherlock'  sherlock.Z
rg 'Sherlock'  sherlock.Z
```
^ all return empty result.

what's interesting, 
```
rg  S sherlock.Z
Binary file matches (found "\u{0}" byte around offset 285)
```

so it looks like it does not honor .Z as compressed file and treats it as binary?
https://github.com/BurntSushi/ripgrep/commit/df7a3bfc7fe30f3e9e89d8775748b1239c5b5fc4 seems to be adding it.

What else can I do to find the reason it's not liking .Z ?

`strace -fk` of `rg S sherlock.Z` here https://gist.github.com/gyakovlev/8039cb9d5ef6a08e5309ceb6c9ecdbe7

---

_Comment by @BurntSushi on 2020-05-28 18:48_

> so it looks like it does not honor .Z as compressed file and treats it as binary?

That's normal. If you don't provide the `-z/--search-zip` flag, then ripgrep doesn't do any intelligent decompressing, just like `grep`. So the `strace` of that isn't interesting. Could you post the output of `strace rg -z Sherlock sherlock.Z`?

The part I don't understand here is why `uncompress -c sherlock.Z` is working as I'd expect, but ripgrep still isn't finding any matches. That should be the exact command that ripgrep is running. So maybe the aforementioned `strace` will reveal something that defies my expectations. Otherwise, I'm not sure what's going on. I'd need a reproduction to debug it.

---

_Comment by @gyakovlev on 2020-05-28 19:32_

thing is then I run it with `-z` flag it also thinks it's a binary
```
rg -z S sherlock.Z
Binary file matches (found "\u{0}" byte around offset 285)
```

* strace -fk -o rg_-z_S_sherlock.Z.txt rg -z S sherlock.Z  (matches binary)
https://gist.github.com/gyakovlev/90b2106c0cbf6e2c59b226aa0ffe7215

* strace -fk -o rg_-z_Sherlock_sherlock.Z.txt rg -z Sherlock sherlock.Z (empty result)
https://gist.github.com/gyakovlev/ae37b86ac6ec3dd75d43a438b7c2efe9


---

_Comment by @gyakovlev on 2020-05-28 19:41_

gzipped file works btw.
```
rg -z Sherlock sherlock.gz
1:For the Doctor Watsons of this world, as opposed to the Sherlock
3:be, to a very large extent, the result of luck. Sherlock Holmes
```

---

_Comment by @BurntSushi on 2020-05-28 23:08_

@gyakovlev Wait, but in your prior comment, you said that `rg -z Sherlock sherlock.Z` returned an empty result? Now it's giving you the `Binary file matches` message instead? Oh, I see, you're saying that `rg -z S sherlock.Z` gives you the `Binary file matches` message where as `rg -z Sherlock sherlock.Z` gives you an empty result?

Unfortunately, the `strace` output doesn't offer me any clues. One last thing you can try:

```
$ rg -z --debug Sherlock sherlock.Z
```

and


```
$ rg -z --debug S sherlock.Z
```

Hopefully that gives us the clue we need.

Thank you for your patience in helping me debug this!

---

_Comment by @gyakovlev on 2020-05-29 07:47_

correct, different pattern `S` just to show how the match looks like and now it prints that's a binary match, instead of searching inside stdout decompressed stream.

I've tried experimenting more on it and it looks like that ripgrep built outside of package manager does .Z decompression properly.

after some digging I figured out the difference.
we enforce local builds and pre-fetch all crates and create a temporary cargo repo with locked dependencies for each build.

due to my overlook the build was using downloaded `.crate` versions of some dependencies, instead of using local workspace sources that are members of workspace of this repo (crates/*)

grep-cli-0.1.4 for instance was pulled in as a crate instead of crates/cli directory.
and published version of `grep-cli-0.1.4` does not yet have `uncompress` support =)

TLDR: the build was using external sources instead of local dir.

thanks for your time and work on ripgrep, sorry for the noise =)

---

_Comment by @BurntSushi on 2020-05-29 12:53_

@gyakovlev Ug. Nope, it is _not_ your fault. It's mine! The actual ripgrep release should be using tagged versions of crates in this repo, but it looks like I did not tag a new release of `grep-cli`. For example, `cargo install ripgrep` would compile `grep-cli 0.1.4` which, as you've pointed out, doesn't have the added support for `uncompress`, which was definitely not intended. In general, `cargo install ripgrep` should correspond to the same functionality as installing one of the ripgrep binary releases.

A similarish but different thing happened a while back with the Debian packagers (see #1144 and [my comment for the realization of what happened](https://github.com/BurntSushi/ripgrep/issues/1144#issuecomment-462079123)). I would love to figure out how to catch this in my release process, because it's otherwise pretty tricky to do so. The Cargo tooling kind of makes everything seamless for crates in the same workspace/repo, so you don't tend to realize when compilation is using a tagged release versus what's on master. This is why I [created a release checklist](https://github.com/BurntSushi/ripgrep/blob/master/RELEASE-CHECKLIST.md), but I guess it's still pretty easy to screw up, apparently.

I'll tag a ripgrep 12.1.1 release.

---

_Label `question` removed by @BurntSushi on 2020-05-29 12:53_

---

_Label `bug` added by @BurntSushi on 2020-05-29 12:53_

---

_Comment by @BurntSushi on 2020-05-29 13:50_

This has been fixed with the [12.1.1 release](https://github.com/BurntSushi/ripgrep/releases/tag/12.1.1).

---

_Closed by @BurntSushi on 2020-05-29 13:50_

---
