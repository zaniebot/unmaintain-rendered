```yaml
number: 1766
title: Resource leak on Linux
type: issue
state: closed
author: 3point2
labels:
  - bug
  - rollup
assignees: []
created_at: 2020-12-22T23:07:27Z
updated_at: 2021-06-01T01:51:20Z
url: https://github.com/BurntSushi/ripgrep/issues/1766
synced_at: 2026-01-12T16:13:24Z
```

# Resource leak on Linux

---

_@3point2_

#### What version of ripgrep are you using?

ripgrep 12.1.1
-SIMD -AVX (compiled)
+SIMD -AVX (runtime)

#### How did you install ripgrep?

Downloaded x86_64 binary from GitHub releases and placed on PATH.

#### What operating system are you using ripgrep on?

Oracle Linux 6 and Arch Linux

#### Describe your bug.

The issue manifests itself when all three of the following are true:

* the number of files being searched is greater than the user's process limit (ulimit -u)
* ripgrep spawns a child process to read the files (e.g. -z or --pre)
* ripgrep is invoked with an option that causes searching to stop before the end of the files, for example --files-with-matches or --max-count

The real life scenario that caused the issue for me was using `rg -z -l PATTERN_IN_MANY_FILES` on a directory containing many tens of thousands of gzipped files on a server where ulimit -u is 4000.

While ripgrep runs, a defunct child process persists for each and every file searched until the user's process limit is reached and ripgrep crashes with
```
thread '<unnamed>' panicked at 'failed to spawn thread: Os { code: 11, kind: WouldBlock, message: "Resource temporarily unavailable" }', /build/rust/src/rustc-1.43.1-src/src/libstd/thread/mod.rs:619:5
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
thread 'main' panicked at 'called `Result::unwrap()` on an `Err` value: Any', crates/ignore/src/walk.rs:1294:17
```

#### What are the steps to reproduce the behavior?

1. Create a directory with a hundred .gz files in it, each containing the same $STRING:
2. Use `ps` to determine the number of currently running processes (including threads) for your user
3. Set your ulimit for the current shell to the value above plus 30
4. Run `rg -z -l $STRING directory_from_step_1`

The gist linked below contains sample commands.

#### What is the actual behavior?

https://gist.github.com/3point2/e772b151899a866e563ee9c52a94ead7

#### What is the expected behavior?

The search should have printed 100 matches without crashing.

---

_Comment by @3point2 on 2020-12-22 23:19_

I took a bit of time to dig into this, and found that although CommandReader [calls wait()](https://github.com/BurntSushi/ripgrep/blob/a6d05475fb353c756e88f605fd5366a67943e591/crates/cli/src/process.rs#L216) on child processes, that never happens if the Sink's [should_quit()](https://github.com/BurntSushi/ripgrep/blob/a6d05475fb353c756e88f605fd5366a67943e591/crates/printer/src/standard.rs#L715) returns true. This is because CommandReader::read only calls wait() if we reach the end of stdout, whereas should_quit() bails earlier than that.

I've created a PR to sketch out a suggested fix. One issue I think might need further work is the use of warn!().

If you're OK with the general gist of the PR I'd be happy to add a test and polish it, but wanted to get some early feedback.

Finally, thanks for ripgrep! It has saved me and my team of 40 co-workers countless hours of time searching through massive log files - it's four times faster than zgrep on average.

---

_Label `bug` added by @BurntSushi on 2020-12-27 16:54_

---

_Label `rollup` added by @BurntSushi on 2021-05-30 01:36_

---

_Closed by @BurntSushi on 2021-06-01 01:51_

---
