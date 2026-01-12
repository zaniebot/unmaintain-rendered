```yaml
number: 2152
title: Simple search hangs possibly due to unmet promise
type: issue
state: closed
author: fpdotmonkey
labels: []
assignees: []
created_at: 2022-02-28T23:20:08Z
updated_at: 2022-03-11T13:12:29Z
url: https://github.com/BurntSushi/ripgrep/issues/2152
synced_at: 2026-01-12T16:13:24Z
```

# Simple search hangs possibly due to unmet promise

---

_@fpdotmonkey_

#### What version of ripgrep are you using?

```
ripgrep 12.1.1 (rev 7cb211378a)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime
```

#### How did you install ripgrep?

via apt

#### What operating system are you using ripgrep on?

Ubuntu 18.04, Linux 5.10.0-051000-generic

#### Describe your bug.

ripgrep sometimes panics or hangs when searching a modestly-sized file hierarchy.

#### What are the steps to reproduce the behavior?

I'm able to reproduce the hang ~25% of the time by calling `rg foo` in the root of my corpus.  It panics very rarely, <1% of the time.

Unfortunately, I can't share the corpus I'm working with and I'm struggling to reproduce against other corpuses.  I'll try to share some details that hopefully could be helpful.

The corpus is a git repo with the below stats.  As such, it's mostly text files (I don't know of any blobs) and it's mostly ASCII-encoded.

```
226 matches
188 matched lines
41 files contained matches
554 files searched
0 bytes printed
6329653 bytes searched
0.014501 seconds spent searching
0.021117 seconds
```

The corpus I'm searching has a bunch of minified javascript in it, so there are some very long individual lines.

It's also often searched along with another corpus that has a badly-formatted `.gitignore` that creates the below non-fatal errors.  I don't know if ripgrep does some caching that could cause problems.

```
./projects/another-corpus/path/to/.gitignore: line 11: error parsing glob 'oblong-gs{{g_speak_version}}-{{project_name}}': nested alternate groups are not allowed
./projects/another-corpus/path/to/.gitignore: line 12: error parsing glob 'oblong-gs{{g_speak_version}}-{{project_name}}-dbg': nested alternate groups are not allowed
```

I was also able to capture some of the state of ripgrep while it was in this hanging state.

The kernel stack while its hanging according to `/proc/<PID>/stack` is

```
[<0>] futex_wait_queue_me+0xbb/0x120
[<0>] futex_wait+0x105/0x240
[<0>] do_futex+0x10f/0x1e0
[<0>] __x64_sys_futex+0x13d/0x1d0
[<0>] do_syscall_64+0x38/0x90
[<0>] entry_SYSCALL_64_after_hwframe+0x44/0xa9
```

According to `/proc/<PID>/status`, ripgrep is in a sleeping state.  And according to `/proc/<PID>/wchan`, ripgrep is sleeping in `futex_wait_queue_me`, so it seems as though there's some unmet promise that's being waited on.

If I pipe STDOUT to /dev/null, i.e.. `rg foo 1>/dev/null`, then the error doesn't occur.  At least not after a few dozen tries.

Again, I apologize for not providing more helpful reproduction steps.  I'm happy to pull more data from procfs to see what's going on.  If there are any open corpuses that you think might hit similar parts of the code, I can try that as well.

#### What is the actual behavior?

https://gist.github.com/fpdotmonkey/39decc4530e78d0f75510996a157dc04

#### What is the expected behavior?

The search doesn't hang


---

_Renamed from "Simple search hangs due possibly due to unmet promise" to "Simple search hangs possibly due to unmet promise" by @fpdotmonkey on 2022-02-28 23:20_

---

_Comment by @BurntSushi on 2022-02-28 23:40_

Could you please try the latest version of ripgrep?

---

_Comment by @fpdotmonkey on 2022-03-01 00:23_

Doesn't seem to be happening on 13.0.0 installed with `cargo install ripgrep`

---

_Comment by @BurntSushi on 2022-03-11 13:12_

If we can't reproduce this with the latest version, then I'm going to close this as fixed.

If you do see this again... Yeah, unfortunately, the only way forward here is for me to either get a reproduction or for you to debug it. And by "debug" it, I probably mean "insert some printf statements in the code, re-compile, then reproduce it." Then repeat that procedure until you find the place in the code that it hangs.

My best guess is that the hanging comes from the parallel directory traversal in the `ignore` crate. There have been some subtle bugs there in the past, but none have been reported in quite some time.

You also mentioned that you see a panic occasionally. If you can get that panic with a full stack trace (by setting `RUST_BACKTRACE=1`), then that would help too.

---

_Closed by @BurntSushi on 2022-03-11 13:12_

---
