```yaml
number: 929
title: Windows eol anchor option
type: pull_request
state: closed
author: BatmanAoD
labels: []
assignees: []
base: master
head: WindowsEOLAnchorOption
created_at: 2018-05-28T09:37:34Z
updated_at: 2022-07-30T22:08:29Z
url: https://github.com/BurntSushi/ripgrep/pull/929
synced_at: 2026-01-12T18:23:13Z
```

# Windows eol anchor option

---

_@BatmanAoD_

Potential fix for #416 via a command-line option.

I decided it would be a good time to implement my old suggestion of just providing a command-line argument to modify the search pattern(s) now that there is config file support.

I've tested by hand that this works as expected on Windows, and the regression test I've added works on Linux.

However, the regression test fails on Windows with no information other than that the exit-code is `1`. Is there some problem with the test framework?

Here's the complete output from running the test on Windows:

```
   Compiling ripgrep v0.8.1 (file:///C:/Users/kjstrand/personal-projects/ripgrep)
    Finished dev [unoptimized + debuginfo] target(s) in 7.2 secs
     Running C:\Users\kjstrand\personal-projects\ripgrep\target\debug\deps\rg-275c12485214083c.exe

running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 86 filtered out

     Running C:\Users\kjstrand\personal-projects\ripgrep\target\debug\deps\integration-3d1ca51b299d90b8.exe

running 1 test
test regression_416 ... FAILED

failures:

---- regression_416 stdout ----
        thread 'regression_416' panicked at '

==========
command failed but expected success!

Did your search end up with no results?

command: "C:\\Users\\kjstrand\\personal-projects\\ripgrep\\target\\debug\\deps\\../rg.exe" "--eol-anchor-include-cr" "--with-filename" "bar$"
cwd: C:\Users\kjstrand\personal-projects\ripgrep\target\debug\deps\ripgrep-tests\regression_416\0

status: exit code: 1

stdout:

stderr:

==========
', tests\workdir.rs:243:13
note: Run with `RUST_BACKTRACE=1` for a backtrace.


failures:
    regression_416

test result: FAILED. 0 passed; 1 failed; 0 ignored; 0 measured; 157 filtered out
```

---

_Comment by @BatmanAoD on 2018-05-28 09:40_

There are also some stylistic issues you will probably want to weigh in on, e.g. whether the flag name is appropriate.

---

_Comment by @BurntSushi on 2018-05-28 12:14_

Thanks for working on this! I'm not sure when I'll get a chance to look at this. I'm also not 100% sold on this particular approach.

---

_Comment by @BatmanAoD on 2018-05-28 15:07_

I don't think it's ideal, which is why I hid it behind a long feature flag. But I think it's a lot better than nothing; the current behavior renders `$` essentially useless on Windows. 

---

_Closed by @BatmanAoD on 2018-05-28 15:07_

---

_Reopened by @BatmanAoD on 2018-05-28 15:07_

---

_Comment by @BatmanAoD on 2018-05-28 19:24_

An alternate API would be to have something like `--eol-handling-mode <MODE>`, and we can introduce modes gradually. Some desirable (non-default) modes might be:

 * This mode, since it's simplest to implement
 * Single-byte newlines, but using some character other than `\n`, such as `\r` (it seems you were planning to do this at some point anyway since you have a single-byte `eol` member in `Args`)
 * GNU-grep style CR-stripping
 * Once there's an opt-in flag for rust-lang/regex/issues/244, a mode that uses this option

---

_Comment by @BatmanAoD on 2018-05-29 19:14_

It appears that on Windows, but on no other supported platform, the test framework requires one or more explicit paths to search (e.g. using `cmd.arg("./")`. So I've fixed the test that was failing.

---

_Renamed from "(Help needed) Windows eol anchor option" to "Windows eol anchor option" by @BatmanAoD on 2018-05-29 19:32_

---

_@BurntSushi reviewed on 2018-06-05 17:39_

---

_Review comment by @BurntSushi on `src/args.rs`:611 on 2018-06-05 17:39_

This should be done by parsing the regex and doing a substitution at the HIR level.

---

_@BatmanAoD reviewed on 2018-06-05 21:31_

---

_Review comment by @BatmanAoD on `src/args.rs`:611 on 2018-06-05 21:31_

@BurntSushi That seems sensible. Are you more amenable to the configuration-option design now, then?

---

_Review comment by @BurntSushi on `src/args.rs`:611 on 2018-06-05 21:49_

Not particularly... I just happened to see this particular issue in passing.

Honestly, this just isn't a priority for me now, and it needs thought.

---

_@BurntSushi reviewed on 2018-06-05 21:49_

---

_@BatmanAoD reviewed on 2018-06-06 00:01_

---

_Review comment by @BatmanAoD on `src/args.rs`:611 on 2018-06-06 00:01_

I understand it's not a priority for you, but as I mentioned when I opened the original issue, it makes `$` essentially useless on Windows.

I agree that the *right* solution requires thought, and I can understand why you wouldn't want to introduce a half-baked solution just to deprecate it later. On the other hand, my suggested solution is extremely simple to implement and, I expect, to maintain, and personally I think it is reasonable to introduce command-line options that may be deprecated in favor of a better solution with a warning that this is the case.

I suppose that if you're definitely opposed to a partial solution, I can just continue to maintain my fork until there's a better solution ready to ship, but I don't want to publish a competing crate on Cargo or anything like that, so it's not terribly convenient.

---

_@BurntSushi reviewed on 2018-06-06 00:25_

---

_Review comment by @BurntSushi on `src/args.rs`:611 on 2018-06-06 00:25_

I've heard you.

> but I don't want to publish a competing crate on Cargo or anything like that, so it's not terribly convenient

If it's on GitHub, `cargo install` can install straight from the git repo.

---

_Review comment by @BatmanAoD on `src/args.rs`:611 on 2018-06-06 00:43_

Okay, I didn't know that. That's helpful; I'll try that out. Thank you.

---

_@BatmanAoD reviewed on 2018-06-06 00:43_

---

_Comment by @BatmanAoD on 2018-06-28 21:24_

Closing due to [this comment in #416](https://github.com/BurntSushi/ripgrep/issues/416#issuecomment-400084219) indicating that this will be handled in libripgrep.

---

_Closed by @BatmanAoD on 2018-06-28 21:24_

---

_Branch deleted on 2022-07-30 22:08_

---
