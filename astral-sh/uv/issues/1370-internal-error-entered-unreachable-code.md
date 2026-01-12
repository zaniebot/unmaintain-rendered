```yaml
number: 1370
title: "internal error: entered unreachable code"
type: issue
state: closed
author: Redoubts
labels:
  - bug
  - registry
assignees: []
created_at: 2024-02-15T22:10:30Z
updated_at: 2024-02-17T14:37:07Z
url: https://github.com/astral-sh/uv/issues/1370
synced_at: 2026-01-12T15:58:26Z
```

# internal error: entered unreachable code

---

_@Redoubts_

First, very excited about this project.

I tried to use this to reproduce my pip-tools lock file, and got 
```
⠇ Resolving dependencies...                                                                                                                                                    thread 'tokio-runtime-worker' panicked at /root/.cargo/registry/src/index.crates.io-6f17d22bba15001f/async_http_range_reader-0.5.0/src/lib.rs:510:19:
range end index 306866 out of range for slice of length 291903
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
thread 'main' panicked at /root/.cargo/registry/src/index.crates.io-6f17d22bba15001f/async_http_range_reader-0.5.0/src/lib.rs:610:25:
internal error: entered unreachable code
```

invocation was 

```
uv pip compile --index-url https://pypi.<internal>.com --extra-index-url https://pypi.python.org/simple --output-file venv_lockfile.txt venv_requirements.txt
```

Using Python 3.10.13 uv 0.1.0 on debian 11.7. Installed via pip to my venv

`RUST_BACKTRACE=full`
```
RUST_BACKTRACE=full uv pip compile ...

⠇ Resolving dependencies...                                                                                                                                                    thread 'tokio-runtime-worker' panicked at /root/.cargo/registry/src/index.crates.io-6f17d22bba15001f/async_http_range_reader-0.5.0/src/lib.rs:510:19:
range end index 306866 out of range for slice of length 291903
stack backtrace:
   0:     0x5649a795d29c - <unknown>
   1:     0x5649a79903b0 - <unknown>
   2:     0x5649a79591bf - <unknown>
   3:     0x5649a795d084 - <unknown>
   4:     0x5649a795eee7 - <unknown>
   5:     0x5649a795ec4f - <unknown>
   6:     0x5649a795f368 - <unknown>
   7:     0x5649a795f24e - <unknown>
   8:     0x5649a795d766 - <unknown>
   9:     0x5649a795efb2 - <unknown>
  10:     0x5649a69cf6b5 - <unknown>
  11:     0x5649a69cfcf2 - <unknown>
  12:     0x5649a6afde7b - <unknown>
  13:     0x5649a6afccac - <unknown>
  14:     0x5649a6b124f0 - <unknown>
  15:     0x5649a788e3bb - <unknown>
  16:     0x5649a788d2ec - <unknown>
  17:     0x5649a7894248 - <unknown>
  18:     0x5649a788cd68 - <unknown>
  19:     0x5649a789b7d3 - <unknown>
  20:     0x5649a78a5187 - <unknown>
  21:     0x5649a787aa2a - <unknown>
  22:     0x5649a789aa6b - <unknown>
  23:     0x5649a7889597 - <unknown>
  24:     0x5649a78a2da9 - <unknown>
  25:     0x5649a7966835 - <unknown>
⠋ Resolving dependencies...                                                                                                                                                      26:     0x7fe282166f07 - start_thread
                               at ./nptl/pthread_create.c:477:8
thread 'tokio-runtime-worker' panicked at /root/.cargo/registry/src/index.crates.io-6f17d22bba15001f/async_http_range_reader-0.5.0/src/lib.rs:510:19:
range end index 17780 out of range for slice of length 17082
  27:     0x7fe281ad9cff - __clone
                               at ./misc/../sysdeps/unix/sysv/linux/x86_64/clone.S:95
  28:                0x0 - <unknown>
stack backtrace:
   0:     0x5649a795d29c - <unknown>
   1:     0x5649a79903b0 - <unknown>
   2:     0x5649a79591bf - <unknown>
   3:     0x5649a795d084 - <unknown>
   4:     0x5649a795eee7 - <unknown>
   5:     0x5649a795ec4f - <unknown>
   6:     0x5649a795f368 - <unknown>
   7:     0x5649a795f24e - <unknown>
   8:     0x5649a795d766 - <unknown>
   9:     0x5649a795efb2 - <unknown>
  10:     0x5649a69cf6b5 - <unknown>
  11:     0x5649a69cfcf2 - <unknown>
  12:     0x5649a6afde7b - <unknown>
  13:     0x5649a6afccac - <unknown>
  14:     0x5649a6b124f0 - <unknown>
  15:     0x5649a788e28e - <unknown>
  16:     0x5649a788d685 - <unknown>
  17:     0x5649a7894248 - <unknown>
  18:     0x5649a788cd68 - <unknown>
  19:     0x5649a789b7d3 - <unknown>
  20:     0x5649a78a5187 - <unknown>
  21:     0x5649a787aa2a - <unknown>
  22:     0x5649a789aa6b - <unknown>
  23:     0x5649a7889597 - <unknown>
  24:     0x5649a78a2da9 - <unknown>
  25:     0x5649a7966835 - <unknown>
  26:     0x7fe282166f07 - start_thread
                               at ./nptl/pthread_create.c:477:8
  27:     0x7fe281ad9cff - __clone
                               at ./misc/../sysdeps/unix/sysv/linux/x86_64/clone.S:95
  28:                0x0 - <unknown>
thread 'main' panicked at /root/.cargo/registry/src/index.crates.io-6f17d22bba15001f/async_http_range_reader-0.5.0/src/lib.rs:610:25:
internal error: entered unreachable code
stack backtrace:
   0:     0x5649a795d29c - <unknown>
   1:     0x5649a79903b0 - <unknown>
   2:     0x5649a79591bf - <unknown>
   3:     0x5649a795d084 - <unknown>
   4:     0x5649a795eee7 - <unknown>
   5:     0x5649a795ec4f - <unknown>
   6:     0x5649a795f368 - <unknown>
   7:     0x5649a795f219 - <unknown>
   8:     0x5649a795d766 - <unknown>
   9:     0x5649a795efb2 - <unknown>
  10:     0x5649a69cf6b5 - <unknown>
  11:     0x5649a69cf753 - <unknown>
  12:     0x5649a70bbef9 - <unknown>
  13:     0x5649a6d38ede - <unknown>
  14:     0x5649a6d342f7 - <unknown>
  15:     0x5649a6a0dd39 - <unknown>
  16:     0x5649a6d30e33 - <unknown>
  17:     0x5649a6db6fcb - <unknown>
  18:     0x5649a6db06f1 - <unknown>
  19:     0x5649a6daf7a2 - <unknown>
  20:     0x5649a6b37e84 - <unknown>
  21:     0x5649a6badf10 - <unknown>
  22:     0x5649a6c85341 - <unknown>
  23:     0x5649a6c510dc - <unknown>
  24:     0x5649a6c3c5bc - <unknown>
  25:     0x5649a6acb71f - <unknown>
  26:     0x5649a6bf7e87 - <unknown>
  27:     0x5649a6c1dd87 - <unknown>
  28:     0x5649a6c38fd2 - <unknown>
  29:     0x5649a6c13c16 - <unknown>
  30:     0x5649a6c63e56 - <unknown>
  31:     0x5649a6cd8151 - <unknown>
  32:     0x5649a6b3f0a3 - <unknown>
  33:     0x5649a6c00f69 - <unknown>
  34:     0x5649a794eba7 - <unknown>
  35:     0x5649a6c00f5e - <unknown>
  36:     0x7fe281a01d0a - __libc_start_main
                               at ./csu/../csu/libc-start.c:308:16
  37:     0x5649a69cff09 - <unknown>
  38:                0x0 - <unknown>
```

---

_Comment by @charliermarsh on 2024-02-15 22:11_

Oooh thank you. I'm worried that this will be hard to solve without having access to the index (since it looks somewhat specific to behavior of the index), but will of course take a look and see what we can do!

---

_Label `bug` added by @MichaReiser on 2024-02-15 22:11_

---

_Comment by @jimhendy on 2024-02-16 07:24_

+1 on this, I also experience this error when repointing `UV_INDEX_URL` to our internal artifactory pypi index.
```
range end index 339002 out of range for slice of length 335716
```

Thanks for your hard work on this awesome project!

---

_Comment by @scottiegarcia on 2024-02-16 22:16_

Also am getting this error when trying to install package from internal artifactory. Including rust backtrace in case that helps.

```
$ RUST_BACKTRACE=full uv pip install <internal-package>
⠼ <internal-package>==<version>                                                                                                                                                      thread 'tokio-runtime-worker' panicked at /Users/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/async_http_range_reader-0.5.0/src/lib.rs:510:19:
range end index 1026335 out of range for slice of length 1018959
stack backtrace:
   0:        0x101a5f074 - <std::sys_common::backtrace::_print::DisplayBacktrace as core::fmt::Display>::fmt::h298c9ab285ff3934
   1:        0x101a82d00 - core::fmt::write::h4e276abdb6d0c2a1
   2:        0x101a5b130 - std::io::stdio::_eprint::hacf0f815ff08f81b
   3:        0x101a5eeb0 - <std::time::SystemTimeError as core::fmt::Display>::fmt::ha84dff9814e351ac
   4:        0x101a607b8 - std::panicking::default_hook::ha6550ffe49b63df1
   5:        0x101a60500 - std::panicking::default_hook::ha6550ffe49b63df1
   6:        0x101a60be0 - std::panicking::rust_panic_with_hook::hddb0e884a202de7c
   7:        0x101a60ae0 - <std::panicking::begin_panic_handler::StaticStrPayload as core::panic::PanicPayload>::take_box::h172fd006fbea5e8c
   8:        0x101a5f4dc - <std::sys_common::backtrace::_print::DisplayBacktrace as core::fmt::Display>::fmt::h298c9ab285ff3934
   9:        0x101a6087c - _rust_begin_unwind
  10:        0x101ae65c8 - core::panicking::panic_fmt::h4d5168028d4c43c7
  11:        0x101ae6a08 - core::slice::index::slice_end_index_len_fail::h0c78261d5ea9147b
  12:        0x1010c397c - _main
  13:        0x1010c2a6c - _main
  14:        0x1010d422c - _main
  15:        0x1010dd18c - _main
  16:        0x1019a3b40 - tokio::runtime::scheduler::multi_thread::worker::run::hc2b55d263db0b591
  17:        0x1019a2a74 - tokio::runtime::scheduler::multi_thread::worker::run::hc2b55d263db0b591
  18:        0x1019c0740 - <tokio::runtime::context::current::SetCurrentGuard as core::ops::drop::Drop>::drop::h21a0d425655bd88f
  19:        0x1019a28ac - tokio::runtime::scheduler::multi_thread::worker::run::hc2b55d263db0b591
  20:        0x1019befe0 - tokio::runtime::task::id::Id::next::hbcdbe7e920fe1020
  21:        0x1019b0d60 - <tokio::runtime::task::core::TaskIdGuard as core::ops::drop::Drop>::drop::h11a15f1d96f85a7c
  22:        0x101997c58 - tokio::runtime::task::raw::RawTask::shutdown::h9483da84c93d4b6f
  23:        0x10199e908 - tokio::runtime::blocking::pool::Spawner::spawn_task::h09d7778db4eccfb1
  24:        0x1019b1ec4 - tokio::sync::oneshot::State::load::hf12e6bf1c47c5c76
  25:        0x1019af338 - tokio::signal::unix::<impl tokio::signal::registry::Init for alloc::vec::Vec<tokio::signal::unix::SignalInfo>>::init::ha9724bdae774f5c1
  26:        0x101a66620 - std::sys::unix::thread::Thread::new::h29cb33f18b852413
  27:        0x19076ffa8 - __pthread_joiner_wake
thread 'main' panicked at /Users/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/async_http_range_reader-0.5.0/src/lib.rs:610:25:
internal error: entered unreachable code
stack backtrace:
   0:        0x101a5f074 - <std::sys_common::backtrace::_print::DisplayBacktrace as core::fmt::Display>::fmt::h298c9ab285ff3934
   1:        0x101a82d00 - core::fmt::write::h4e276abdb6d0c2a1
   2:        0x101a5b130 - std::io::stdio::_eprint::hacf0f815ff08f81b
   3:        0x101a5eeb0 - <std::time::SystemTimeError as core::fmt::Display>::fmt::ha84dff9814e351ac
   4:        0x101a607b8 - std::panicking::default_hook::ha6550ffe49b63df1
   5:        0x101a60500 - std::panicking::default_hook::ha6550ffe49b63df1
   6:        0x101a60be0 - std::panicking::rust_panic_with_hook::hddb0e884a202de7c
   7:        0x101a60abc - <std::panicking::begin_panic_handler::StaticStrPayload as core::panic::PanicPayload>::take_box::h172fd006fbea5e8c
   8:        0x101a5f4dc - <std::sys_common::backtrace::_print::DisplayBacktrace as core::fmt::Display>::fmt::h298c9ab285ff3934
   9:        0x101a6087c - _rust_begin_unwind
  10:        0x101ae65c8 - core::panicking::panic_fmt::h4d5168028d4c43c7
  11:        0x101ae663c - core::panicking::panic::h40561ff494e2b577
  12:        0x10142facc - <async_http_range_reader::AsyncHttpRangeReader as tokio::io::async_read::AsyncRead>::poll_read::h37183f6627efea91
  13:        0x1010aee8c - _main
  14:        0x100ea0e94 - __mh_execute_header
  15:        0x1010acc68 - _main
  16:        0x101107924 - _main
  17:        0x101102ea4 - _main
  18:        0x1011023f0 - _main
  19:        0x1010667a4 - _main
  20:        0x100faee44 - __mh_execute_header
  21:        0x101113454 - _main
  22:        0x100f1e55c - __mh_execute_header
  23:        0x100f12ba4 - __mh_execute_header
  24:        0x100fef630 - __mh_execute_header
  25:        0x10117fe90 - _main
  26:        0x1011a1d44 - _main
  27:        0x1011b5e04 - _main
  28:        0x1011951dc - _main
  29:        0x100f32110 - __mh_execute_header
  30:        0x10102eebc - __mh_execute_header
  31:        0x10106c620 - _main
  32:        0x101014528 - __mh_execute_header
  33:        0x101a52870 - std::rt::lang_start_internal::h5b246d44f1526226
  34:        0x10101450c - __mh_execute_header
  35:        0x101052668 - _main                                                                                                                                       /0.8s
```

---

_Comment by @charliermarsh on 2024-02-16 22:32_

If anyone has a _public_ index that exhibits this behavior I'd be super grateful. It looks like maybe the index is reporting a length that doesn't match the content-length header.

---

_Label `index` added by @zanieb on 2024-02-16 22:37_

---

_Comment by @mattmess1221 on 2024-02-17 02:08_

I started a free trial of artifactory and added a remote pypi repository. I've confirmed the bug is reproduced on it.

```
% uv pip install requests -i https://killjoyuvbug.jfrog.io/artifactory/api/pypi/pypi/simple 
⠦ requests==2.31.0
thread 'tokio-runtime-worker' panicked at /root/.cargo/registry/src/index.crates.io-6f17d22bba15001f/async_http_range_reader-0.5.0/src/lib.rs:510:19:
range end index 64965 out of range for slice of length 62574
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
thread 'main' panicked at /root/.cargo/registry/src/index.crates.io-6f17d22bba15001f/async_http_range_reader-0.5.0/src/lib.rs:610:25:
internal error: entered unreachable code
```

The trial has a period of 14 days and I've enabled anonymous access. You can use it to debug the issue.

Repository: https://killjoyuvbug.jfrog.io/artifactory/api/pypi/pypi/simple 

---

_Comment by @charliermarsh on 2024-02-17 02:14_

This is perfect, thank you so much!

---

_Comment by @charliermarsh on 2024-02-17 03:45_

So far, the only thing I notice is that the server does return `"accept-ranges": "bytes"`, but then it seems to totally ignore the passed-in byte ranges? Like I can pass _anything_ for the range header, and the server gives me the same response (200, arbitrary ranges).

---

_Comment by @charliermarsh on 2024-02-17 04:02_

Ok, it looks like Artifactory may not support range requests despite the response headers claiming to do so:

- https://github.com/pypa/pip/issues/8723
- https://jfrog.atlassian.net/browse/RTFACT-23108

That's a bummer, it'll make things much slower for Artifactory users. We'll need to update the HTTP parser to correctly error here. (Then we'll automatically fall back to downloading the entire wheel.)

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-17 04:02_

---

_Comment by @zanieb on 2024-02-17 04:13_

Related context on why range requests matter:

- https://github.com/devpi/devpi/issues/1021

Previous work on this here:

- https://github.com/astral-sh/uv/pull/702

---

_Closed by @charliermarsh on 2024-02-17 14:37_

---
