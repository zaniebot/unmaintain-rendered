```yaml
number: 633
title: File list performance with symlinks on Windows
type: issue
state: closed
author: chrmarti
labels:
  - bug
assignees: []
created_at: 2017-10-10T22:10:46Z
updated_at: 2018-02-20T12:45:19Z
url: https://github.com/BurntSushi/ripgrep/issues/633
synced_at: 2026-01-12T16:13:22Z
```

# File list performance with symlinks on Windows

---

_@chrmarti_

I'm investigating a report that listing files (`--files`) on Windows when following symlinks (`--follow`) takes much more time than without symlinks.

It appears that in some cases `cnpm` (an `npm` replacement) uses symlinks to organize the `node_modules` folder and that then slows down ripgrep. (https://github.com/Microsoft/vscode/issues/35659#issuecomment-335535129 has an example `package.json` that when installed using `cnpm i` reproduces the problem.)

Looking at the trace in Process Monitor I see many files being opened for reading metadata in `DirEntryRaw:from_link` and to detect loops in same_file's `Handle::from_path`.

One area where this might be improved is where `check_symlink_loop()` loops over parent directories of each symlinked directory, causing is_same_file to open the same parent directory once for each of its symlinked subdirectories. Maybe a directory's id could be cached for the duration of its content being traversed?

/cc @roblourens

---

_Comment by @chrmarti on 2017-10-10 22:12_

From a recording in Process Monitor:
![image](https://user-images.githubusercontent.com/9205389/31413294-6afa6288-adcd-11e7-9e45-89451e310c8e.png)


---

_Comment by @BurntSushi on 2017-10-10 22:21_

> Maybe a directory's id could be cached for the duration of its content being traversed?

AFAIK, this is technically incorrect. See this comment here: https://github.com/BurntSushi/same-file/blob/master/src/win.rs#L19-L58

---

_Comment by @chrmarti on 2017-10-10 22:51_

Maybe the cache could keep the directories open to avoid the filesystem reusing the index numbers. That shouldn't be too expensive, because for each 'cursor' (not sure if it's all threads) in the traversal you'd only need to keep the parent directories open which should be few.

---

_Comment by @BurntSushi on 2017-10-21 04:13_

I have an attempt at fixing this here: https://github.com/BurntSushi/walkdir/pull/85/commits/5bcc5b87ee08a58a7f2a7ef4b1d23392b55155bb --- It's in the directory traversal library, so I still need to pull it into ripgrep.

---

_Closed by @BurntSushi on 2017-10-22 02:40_

---

_Comment by @BurntSushi on 2017-10-22 12:11_

This should be fixed in ripgrep 0.7.0. I didn't actually test that it resolves the performance problem, but the symlink loop checking should be opening far fewer file handles. We can definitely re-open this if it's still a problem (and having an easy to use test corpus to try this on would be a huge help, e.g., what steps can I take with `cnpm` to setup a directory big enough to notice this problem).

---

_Reopened by @BurntSushi on 2017-10-22 14:29_

---

_Comment by @BurntSushi on 2017-10-22 14:36_

Unfortunately, I had to partially revert the optimization for the parallel iterator, which is used by default in ripgrep. The problem is that hanging on to file handles in the parallel iterator can cause file descriptor limits to be exceeded quite easily since many directories are being traversed in parallel. I say "partially" revert because we are at least keeping the same child handle open while walking up the ancestor path, but unfortunately, we are still opening a handle to each ancestor in that loop.

If ripgrep is run with a single thread (`-j1`), then it will use the standard `walkdir` iterator, which still retains this optimization in full. So we should at least be able to test with that to see if this is the optimization that fixes this issue, and if so, brainstorm solutions for the parallel iterator.

---

_Comment by @chrmarti on 2017-10-24 17:42_

Great, thanks @BurntSushi! When testing it I see it panicking after a few (maybe unrelated) errors:
```
PS C:\devel\35659\cipchk> (Measure-Command { & 'C:\devel\ripgrep\target\debug\rg.exe' --files --no-ignore --hidden --follow -j1 -- . | Measure-Object –Line | Out-Default }).TotalSeconds
File system loop found: .\node_modules\babel-core\node_modules\babel-register\node_modules\babel-core points to an ancestor .\node_modules\babel-core
.\node_modules\babel-helper-define-map\node_modules\babel-helper-function-name\node_modules\babel-template\node_modules\babel-traverse\node_modules\babel-messages\node_modules\babel-runtime\node_modules\core-js\library\fn\dom-collections: The system cannot find the path specified. (os error 3)
The system cannot find the path specified. (os error 3)
.\node_modules\babel-helper-define-map\node_modules\babel-helper-function-name\node_modules\babel-template\node_modules\babel-traverse\node_modules\babel-messages\node_modules\babel-runtime\node_modules\core-js\library\fn\function\virtual: The system cannot find the path specified. (os error 3)
.\node_modules\babel-helper-define-map\node_modules\babel-helper-function-name\node_modules\babel-template\node_modules\babel-traverse\node_modules\babel-messages\node_modules\babel-runtime\node_modules\core-js\library\fn\number\virtual: The system cannot find the path specified. (os error 3)
.\node_modules\babel-helper-define-map\node_modules\babel-helper-function-name\node_modules\babel-template\node_modules\babel-traverse\node_modules\babel-messages\node_modules\babel-runtime\node_modules\core-js\library\fn\string\virtual: The system cannot find the path specified. (os error 3)
The system cannot find the path specified. (os error 3)
.\node_modules\babel-plugin-transform-es2015-classes\node_modules\babel-helper-define-map\node_modules\babel-helper-function-name\node_modules\babel-helper-get-function-arity\node_modules\babel-runtime\node_modules\core-js\fn\array\virtual: The system cannot find the path specified. (os error 3)
The system cannot find the path specified. (os error 3)
.\node_modules\babel-plugin-transform-es2015-classes\node_modules\babel-helper-define-map\node_modules\babel-helper-function-name\node_modules\babel-helper-get-function-arity\node_modules\babel-runtime\node_modules\core-js\fn\dom-collections: The system cannot find the path specified. (os error 3)
The system cannot find the path specified. (os error 3)
.\node_modules\babel-plugin-transform-es2015-classes\node_modules\babel-helper-define-map\node_modules\babel-helper-function-name\node_modules\babel-helper-get-function-arity\node_modules\babel-runtime\node_modules\core-js\fn\function\virtual: The system cannot find the path specified. (os error 3)
The system cannot find the path specified. (os error 3)
.\node_modules\babel-plugin-transform-es2015-classes\node_modules\babel-helper-define-map\node_modules\babel-helper-function-name\node_modules\babel-helper-get-function-arity\node_modules\babel-runtime\node_modules\core-js\fn\number\virtual: The system cannot find the path specified. (os error 3)
The system cannot find the path specified. (os error 3)
.\node_modules\babel-plugin-transform-es2015-classes\node_modules\babel-helper-define-map\node_modules\babel-helper-function-name\node_modules\babel-helper-get-function-arity\node_modules\babel-runtime\node_modules\core-js\fn\string\virtual: The system cannot find the path specified. (os error 3)
The system cannot find the path specified. (os error 3)
.\node_modules\babel-plugin-transform-es2015-classes\node_modules\babel-helper-define-map\node_modules\babel-helper-function-name\node_modules\babel-helper-get-function-arity\node_modules\babel-runtime\node_modules\core-js\library\fn\array: The system cannot find the path specified. (os error 3)
The system cannot find the path specified. (os error 3)
.\node_modules\babel-plugin-transform-es2015-classes\node_modules\babel-helper-define-map\node_modules\babel-helper-function-name\node_modules\babel-helper-get-function-arity\node_modules\babel-runtime\node_modules\core-js\library\fn\date: The system cannot find the path specified. (os error 3)
The system cannot find the path specified. (os error 3)
.\node_modules\babel-plugin-transform-es2015-classes\node_modules\babel-helper-define-map\node_modules\babel-helper-function-name\node_modules\babel-helper-get-function-arity\node_modules\babel-runtime\node_modules\core-js\library\fn\dom-collections: The system cannot find the path specified. (os error 3)
The system cannot find the path specified. (os error 3)
.\node_modules\babel-plugin-transform-es2015-classes\node_modules\babel-helper-define-map\node_modules\babel-helper-function-name\node_modules\babel-helper-get-function-arity\node_modules\babel-runtime\node_modules\core-js\library\fn\error: The system cannot find the path specified. (os error 3)
The system cannot find the path specified. (os error 3)
.\node_modules\babel-plugin-transform-es2015-classes\node_modules\babel-helper-define-map\node_modules\babel-helper-function-name\node_modules\babel-helper-get-function-arity\node_modules\babel-runtime\node_modules\core-js\library\fn\function: The system cannot find the path specified. (os error 3)
The system cannot find the path specified. (os error 3)
.\node_modules\babel-plugin-transform-es2015-classes\node_modules\babel-helper-define-map\node_modules\babel-helper-function-name\node_modules\babel-helper-get-function-arity\node_modules\babel-runtime\node_modules\core-js\library\fn\json: The system cannot find the path specified. (os error 3)
.\node_modules\babel-plugin-transform-es2015-classes\node_modules\babel-helper-define-map\node_modules\babel-helper-function-name\node_modules\babel-helper-get-function-arity\node_modules\babel-runtime\node_modules\core-js\library\fn\map: The system cannot find the path specified. (os error 3)
The system cannot find the path specified. (os error 3)
.\node_modules\babel-plugin-transform-es2015-classes\node_modules\babel-helper-define-map\node_modules\babel-helper-function-name\node_modules\babel-helper-get-function-arity\node_modules\babel-runtime\node_modules\core-js\library\fn\math: The system cannot find the path specified. (os error 3)
The system cannot find the path specified. (os error 3)
.\node_modules\babel-plugin-transform-es2015-classes\node_modules\babel-helper-define-map\node_modules\babel-helper-function-name\node_modules\babel-helper-get-function-arity\node_modules\babel-runtime\node_modules\core-js\library\fn\number: The system cannot find the path specified. (os error 3)
The system cannot find the path specified. (os error 3)
.\node_modules\babel-plugin-transform-es2015-classes\node_modules\babel-helper-define-map\node_modules\babel-helper-function-name\node_modules\babel-helper-get-function-arity\node_modules\babel-runtime\node_modules\core-js\library\fn\object: The system cannot find the path specified. (os error 3)
The system cannot find the path specified. (os error 3)
.\node_modules\babel-plugin-transform-es2015-classes\node_modules\babel-helper-define-map\node_modules\babel-helper-function-name\node_modules\babel-helper-get-function-arity\node_modules\babel-runtime\node_modules\core-js\library\fn\promise: The system cannot find the path specified. (os error 3)
thread 'main' panicked at 'BUG: list/path stacks out of sync', src\libcore\option.rs:819:4
note: Run with `RUST_BACKTRACE=1` for a backtrace.

 Lines Words Characters Property
 ----- ----- ---------- --------
243633


11.98369
```

---

_Comment by @BurntSushi on 2017-10-24 17:47_

@chrmarti Thanks! Few questions:

1. Does that happen with ripgrep 0.6.0?
2. Can you run it with `RUST_BACKTRACE=1`?
3. Would it be possible to list out the steps I'd need to follow to reproduce your results?

---

_Comment by @chrmarti on 2017-10-24 17:53_

This is from running https://github.com/BurntSushi/ripgrep/commit/1aec4b11231ccb7de92e3008408e4d06a714d106 . A simple reproducible case on my machine is:
```
cnpm i babel-preset-es2015
& 'C:\devel\ripgrep\target\debug\rg.exe' --files --follow -j1 -- . | Measure-Object –Line
```

The full output with this and `RUST_BACKTRACE=1` is:
```
PS C:\devel\35659\panick> & 'C:\devel\ripgrep\target\debug\rg.exe' --files --follow -j1 -- . | Measure-Object –Line
.\node_modules\babel-helper-define-map\node_modules\babel-helper-function-name\node_modules\babel-template\node_modules\babel-traverse\node_modules\babel-messages\node_modules\babel-runtime\node_modules\core-js\library\fn\dom-collections: The system cannot find the path specified. (os error 3)
The system cannot find the path specified. (os error 3)
.\node_modules\babel-helper-define-map\node_modules\babel-helper-function-name\node_modules\babel-template\node_modules\babel-traverse\node_modules\babel-messages\node_modules\babel-runtime\node_modules\core-js\library\fn\function\virtual: The system cannot find the path specified. (os error 3)
.\node_modules\babel-helper-define-map\node_modules\babel-helper-function-name\node_modules\babel-template\node_modules\babel-traverse\node_modules\babel-messages\node_modules\babel-runtime\node_modules\core-js\library\fn\number\virtual: The system cannot find the path specified. (os error 3)
.\node_modules\babel-helper-define-map\node_modules\babel-helper-function-name\node_modules\babel-template\node_modules\babel-traverse\node_modules\babel-messages\node_modules\babel-runtime\node_modules\core-js\library\fn\string\virtual: The system cannot find the path specified. (os error 3)
The system cannot find the path specified. (os error 3)
.\node_modules\babel-plugin-transform-es2015-classes\node_modules\babel-helper-define-map\node_modules\babel-helper-function-name\node_modules\babel-helper-get-function-arity\node_modules\babel-runtime\node_modules\core-js\fn\array\virtual: The system cannot find the path specified. (os error 3)
The system cannot find the path specified. (os error 3)
.\node_modules\babel-plugin-transform-es2015-classes\node_modules\babel-helper-define-map\node_modules\babel-helper-function-name\node_modules\babel-helper-get-function-arity\node_modules\babel-runtime\node_modules\core-js\fn\dom-collections: The system cannot find the path specified. (os error 3)
The system cannot find the path specified. (os error 3)
.\node_modules\babel-plugin-transform-es2015-classes\node_modules\babel-helper-define-map\node_modules\babel-helper-function-name\node_modules\babel-helper-get-function-arity\node_modules\babel-runtime\node_modules\core-js\fn\function\virtual: The system cannot find the path specified. (os error 3)
The system cannot find the path specified. (os error 3)
.\node_modules\babel-plugin-transform-es2015-classes\node_modules\babel-helper-define-map\node_modules\babel-helper-function-name\node_modules\babel-helper-get-function-arity\node_modules\babel-runtime\node_modules\core-js\fn\number\virtual: The system cannot find the path specified. (os error 3)
The system cannot find the path specified. (os error 3)
.\node_modules\babel-plugin-transform-es2015-classes\node_modules\babel-helper-define-map\node_modules\babel-helper-function-name\node_modules\babel-helper-get-function-arity\node_modules\babel-runtime\node_modules\core-js\fn\string\virtual: The system cannot find the path specified. (os error 3)
The system cannot find the path specified. (os error 3)
.\node_modules\babel-plugin-transform-es2015-classes\node_modules\babel-helper-define-map\node_modules\babel-helper-function-name\node_modules\babel-helper-get-function-arity\node_modules\babel-runtime\node_modules\core-js\library\fn\array: The system cannot find the path specified. (os error 3)
The system cannot find the path specified. (os error 3)
.\node_modules\babel-plugin-transform-es2015-classes\node_modules\babel-helper-define-map\node_modules\babel-helper-function-name\node_modules\babel-helper-get-function-arity\node_modules\babel-runtime\node_modules\core-js\library\fn\date: The system cannot find the path specified. (os error 3)
The system cannot find the path specified. (os error 3)
.\node_modules\babel-plugin-transform-es2015-classes\node_modules\babel-helper-define-map\node_modules\babel-helper-function-name\node_modules\babel-helper-get-function-arity\node_modules\babel-runtime\node_modules\core-js\library\fn\dom-collections: The system cannot find the path specified. (os error 3)
The system cannot find the path specified. (os error 3)
.\node_modules\babel-plugin-transform-es2015-classes\node_modules\babel-helper-define-map\node_modules\babel-helper-function-name\node_modules\babel-helper-get-function-arity\node_modules\babel-runtime\node_modules\core-js\library\fn\error: The system cannot find the path specified. (os error 3)
The system cannot find the path specified. (os error 3)
.\node_modules\babel-plugin-transform-es2015-classes\node_modules\babel-helper-define-map\node_modules\babel-helper-function-name\node_modules\babel-helper-get-function-arity\node_modules\babel-runtime\node_modules\core-js\library\fn\function: The system cannot find the path specified. (os error 3)
The system cannot find the path specified. (os error 3)
.\node_modules\babel-plugin-transform-es2015-classes\node_modules\babel-helper-define-map\node_modules\babel-helper-function-name\node_modules\babel-helper-get-function-arity\node_modules\babel-runtime\node_modules\core-js\library\fn\json: The system cannot find the path specified. (os error 3)
.\node_modules\babel-plugin-transform-es2015-classes\node_modules\babel-helper-define-map\node_modules\babel-helper-function-name\node_modules\babel-helper-get-function-arity\node_modules\babel-runtime\node_modules\core-js\library\fn\map: The system cannot find the path specified. (os error 3)
The system cannot find the path specified. (os error 3)
.\node_modules\babel-plugin-transform-es2015-classes\node_modules\babel-helper-define-map\node_modules\babel-helper-function-name\node_modules\babel-helper-get-function-arity\node_modules\babel-runtime\node_modules\core-js\library\fn\math: The system cannot find the path specified. (os error 3)
The system cannot find the path specified. (os error 3)
.\node_modules\babel-plugin-transform-es2015-classes\node_modules\babel-helper-define-map\node_modules\babel-helper-function-name\node_modules\babel-helper-get-function-arity\node_modules\babel-runtime\node_modules\core-js\library\fn\number: The system cannot find the path specified. (os error 3)
The system cannot find the path specified. (os error 3)
.\node_modules\babel-plugin-transform-es2015-classes\node_modules\babel-helper-define-map\node_modules\babel-helper-function-name\node_modules\babel-helper-get-function-arity\node_modules\babel-runtime\node_modules\core-js\library\fn\object: The system cannot find the path specified. (os error 3)
The system cannot find the path specified. (os error 3)
.\node_modules\babel-plugin-transform-es2015-classes\node_modules\babel-helper-define-map\node_modules\babel-helper-function-name\node_modules\babel-helper-get-function-arity\node_modules\babel-runtime\node_modules\core-js\library\fn\promise: The system cannot find the path specified. (os error 3)
thread 'main' panicked at 'BUG: list/path stacks out of sync', src\libcore\option.rs:819:4
stack backtrace:
   0: std::sys_common::backtrace::_print
             at C:\projects\rust\src\libstd\sys_common\backtrace.rs:94
   1: std::panicking::default_hook::{{closure}}
             at C:\projects\rust\src\libstd\panicking.rs:379
   2: std::panicking::default_hook
             at C:\projects\rust\src\libstd\panicking.rs:396
   3: std::panicking::rust_panic_with_hook
             at C:\projects\rust\src\libstd\panicking.rs:611
   4: std::panicking::begin_panic_new<alloc::string::String>
             at C:\projects\rust\src\libstd\panicking.rs:553
   5: std::panicking::begin_panic_fmt
             at C:\projects\rust\src\libstd\panicking.rs:521
   6: std::panicking::rust_begin_panic
             at C:\projects\rust\src\libstd\panicking.rs:497
   7: core::panicking::panic_fmt
             at C:\projects\rust\src\libcore\panicking.rs:92
   8: core::option::expect_failed
             at C:\projects\rust\src\libcore\option.rs:819
   9: core::option::Option<walkdir::Ancestor>::expect<walkdir::Ancestor>
             at C:\projects\rust\src\libcore\option.rs:302
  10: walkdir::IntoIter::pop
             at C:\Users\chrmarti\.cargo\registry\src\github.com-1ecc6299db9ec823\walkdir-2.0.1\src\lib.rs:867
  11: walkdir::{{impl}}::next
             at C:\Users\chrmarti\.cargo\registry\src\github.com-1ecc6299db9ec823\walkdir-2.0.1\src\lib.rs:663
  12: ignore::walk::{{impl}}::next::{{closure}}
             at C:\devel\ripgrep\ignore\src\walk.rs:820
  13: core::option::Option<core::result::Result<walkdir::DirEntry, walkdir::Error>>::or_else<core::result::Result<walkdir::DirEntry, walkdir::Error>,closure>
             at C:\projects\rust\src\libcore\option.rs:658
  14: ignore::walk::{{impl}}::next::{{closure}}
             at C:\devel\ripgrep\ignore\src\walk.rs:736
  15: core::option::Option<&mut ignore::walk::WalkEventIter>::and_then<&mut ignore::walk::WalkEventIter,core::result::Result<ignore::walk::WalkEvent, walkdir::Error>,closure>
             at C:\projects\rust\src\libcore\option.rs:605
  16: rg::run_files_one_thread
             at C:\devel\ripgrep\src\main.rs:226
  17: rg::run
             at C:\devel\ripgrep\src\main.rs:75
  18: core::ops::function::FnOnce::call_once<fn(alloc::arc::Arc<rg::args::Args>) -> core::result::Result<u64, alloc::boxed::Box<Error>>,(alloc::arc::Arc<rg::args::Args>)>
             at C:\projects\rust\src\libcore\ops\function.rs:143
  19: core::result::Result<alloc::arc::Arc<rg::args::Args>, alloc::boxed::Box<Error>>::and_then<alloc::arc::Arc<rg::args::Args>,alloc::boxed::Box<Error>,u64,fn(alloc::arc::Arc<rg::args::Args>) -> core::result::Result<u64, alloc::boxed::Box<Error>>>
             at C:\projects\rust\src\libcore\result.rs:602
  20: rg::main
             at C:\devel\ripgrep\src\main.rs:58
  21: panic_unwind::__rust_maybe_catch_panic
             at C:\projects\rust\src\libpanic_unwind\lib.rs:98
  22: std::rt::lang_start
             at C:\projects\rust\src\libstd\rt.rs:52
  23: main
  24: __scrt_common_main_seh
             at f:\dd\vctools\crt\vcstartup\src\startup\exe_common.inl:253
  25: BaseThreadInitThunk

 Lines Words Characters Property
 ----- ----- ---------- --------
164666
```

---

_Comment by @chrmarti on 2017-10-30 17:54_

Btw., would it be an option to say that duplicate/missing results are unlikely and not fatal when you don't keep the files open while comparing ids? Maybe that could be enabled by a command line flag?

---

_Comment by @BurntSushi on 2017-10-30 17:56_

@chrmarti That thought has crossed my mind, and it is tempting to take. It's a tough balance to strike, and I'd rather not make something like this configurable.

---

_Comment by @BurntSushi on 2018-02-01 02:07_

@chrmarti It looks like your [comment containing the panic](https://github.com/BurntSushi/ripgrep/issues/633#issuecomment-339076246) isn't actually related to symlinks, but rather, the length of the path name. As an example, I tried running this in `cmd`:

```
$ dir C:\Users\andrew\work\bugs\ripgrep-633\node_modules\babel-helper-define-map\node_modules\babel-helper-function-name\node_modules\babel-template\node_modules\babel-traverse\node_modules\babel-messages\node_modules\babel-runtime\node_modules\core-js\library\fn\reflect
```

This told me the directory couldn't be found. I then removed the `\reflect` suffix and ran:

```
$ dir C:\Users\andrew\work\bugs\ripgrep-633\node_modules\babel-helper-define-map\node_modules\babel-helper-function-name\node_modules\babel-template\node_modules\babel-traverse\node_modules\babel-messages\node_modules\babel-runtime\node_modules\core-js\library\fn
```

This showed me the contents of the directory, which included a `reflect` sub-directory. I then tried cd'ing into `fn` and running `dir reflect`, but I got the same error. I then tried this:

```
$ dir \\?\C:\Users\andrew\work\bugs\ripgrep-633\node_modules\babel-helper-define-map\node_modules\babel-helper-function-name\node_modules\babel-template\node_modules\babel-traverse\node_modules\babel-messages\node_modules\babel-runtime\node_modules\core-js\library\fn\reflect
```

My understanding is that the `\\?\` prefix permits the use of long path names, and this did indeed work. To me this implies that one of the bugs here is #364. With that said, ripgrep absolutely should not panic, so that's *another* bug.

I did a little more investigation in the `node_modules` directory, and it's... kind of ridiculous. ripgrep doesn't just slow down on Windows, it also slows on Linux, and for good reason. Running against `cnpm`:

```
$ time rg --files -L -uu -j1 ./ | wc -l
2175424

real    0m4.899s
user    0m1.938s
sys     0m5.431s
```

And now running against `npm` (specifically, the result of `npm i babel-preset-es2015`):

```
$ time rg --files -L -uu -j1 ./ | wc -l
3128

real    0m0.019s
user    0m0.004s
sys     0m0.023s
```

So `cnpm` is adding over 2.1 million additional file entries. (On Windows, I get only ~1.3 million entries, likely because ripgrep doesn't descend beyond the maximum file path length.) That's a **three order of magnitude** increase. To ground you a bit, the entire Linux kernel source tree is a mere 56,919 files while the entire chromium repository is 234,000 files. Something seems very wrong in `cnpm` land, and I'm not sure ripgrep is going to be able to do much to improve the situation. (If anything, the long path name bug is probably helping matters here by preventing ripgrep from exploring the entire tree.) Note that you might want to make sure your stderr handling is decent on your end, since ripgrep is emitting lots of errors.

I compared ripgrep's time to traverse the directory with parallelism enabled (because the single threaded walker has the bug you found) with the time it takes for `find` to run under cygwin on my Windows laptop. (Windows 10, 128GB SSD.) I redirected both programs to another file. `find` found more entries (seemingly because it handles long path names? but it reported a similar number as `find` does on my Linux machine). ripgrep took about 20 seconds while `find` took almost **9 minutes**. This isn't a surprise because the total output of `find` is almost a gigabyte while ripgrep's is about 25% of that. So you have hundreds of megabytes flowing through your pipe any time you try to crawl a cnpm produced `node_modules` directory with `-L/--follow` enabled. Just from eyeballing, it kind of seems like ripgrep's performance with respect to symlinks specifically is actually pretty decent. But there is just a metric shit ton of them.

So here's my summary:

* ripgrep can't handle long path names. That's a problem and I don't really know how to fix it. See #364.
* There is a bug in single threaded directory traversal on Windows. I haven't dug into this yet, but it should definitely get fixed.
* If ripgrep is going to print hundreds of MB worth of file paths, you're probably in for a bad time.
* I am by no means a Windows expert, so my intuition here could be way off. Please be on the look out for very basic mistakes on my part, and be skeptical of anything I say w.r.t. to Windows. :-)

---

_Comment by @BurntSushi on 2018-02-01 02:07_

cc @roblourens

---

_Comment by @BurntSushi on 2018-02-01 02:10_

w.r.t. long file names, @retep998 [claims that I can enable a switch in a manifest](https://github.com/BurntSushi/ripgrep/issues/364#issuecomment-280148931) to turn on long file name support in Windows 10, but I don't understand how to do that. That would certainly be easier than doing path conversion manually.

---

_Comment by @chrmarti on 2018-02-01 07:27_

@BurntSushi Makes sense, thanks for the detailed explanation!

---

_Comment by @roblourens on 2018-02-01 16:08_

> So cnpm is adding over 2.1 million additional file entries. 

Wow. I'm guessing this is because of duplicates in the node_modules tree. Common dependencies are probably symlinked in multiple times, everywhere they are required.

---

_Closed by @BurntSushi on 2018-02-01 22:09_

---

_Comment by @BurntSushi on 2018-02-01 22:12_

@chrmarti All right, I've fixed `walkdir` and updated ripgrep to bring in the new `walkdir` release which should fix the panic you reported above.

I'm going to mark this issue as fixed since I think all issues that I'm aware of are either fixed (the panic is fixed and I think symlink performance is reasonable) or tracked elsewhere (e.g., #364 for long path names).

I'd be happy to revisit the symlink performance issue if there's compelling evidence that we can do measurably better. A good example there might be showing the execution of another tool that can 1) successfully traverse `cnpm`'s `node_modules` directory contents and 2) print every file path that it visits and 3) doing it in a way that is faster than ripgrep today.

---

_Comment by @warpdesign on 2018-02-13 10:22_

I am using VS Code and the last version has a performance [bug](https://github.com/Microsoft/vscode/issues/35659) which seems to be related to this one.

I downgraded to RG version `0.6.0` and the performance problem is gone.

---

_Comment by @BurntSushi on 2018-02-13 12:07_

@warpdesign Thanks for the tip! Unfortunately, it doesn't really help. We need more details. Could you please open a new bug and fill in the issue template? Please remember that this is the ripgrep repo, and bugs should be reported against ripgrep using `rg` commands.

---

_Label `bug` added by @BurntSushi on 2018-02-20 12:45_

---
