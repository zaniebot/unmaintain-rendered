```yaml
number: 17537
title: "[red-knot] Panic leads to hang"
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - ty
assignees: []
created_at: 2025-04-22T07:17:56Z
updated_at: 2025-04-26T06:28:47Z
url: https://github.com/astral-sh/ruff/issues/17537
synced_at: 2026-01-12T15:54:56Z
```

# [red-knot] Panic leads to hang

---

_@sharkdp_

With a 5%-10% chance, Red Knot *hangs* when running on [hydpy](https://github.com/hydpy-dev/hydpy) (with dependencies installed). In all *other* cases, Red Knot panics with
```
thread '<unnamed>' panicked at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/function/fetch.rs:167:25:
dependency graph cycle querying try_metaclass_(Id(9673)); set cycle_fn/cycle_initial to fixpoint iterate
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```
but then sometimes, it seems as if that panic does not properly lead to a crash.

When the hang occurs, the Red Knot is in the following state (all threads waiting):
* Thread 1: The main thread waits for a new message in [this `recv()` call](https://github.com/astral-sh/ruff/blob/718b0cadf4398bf611bd76c982c64486e679d8e1/crates/red_knot/src/main.rs#L240).
* Threads 2-5: All rayon worker threads seem to be idle.
* (Thread 6: the ctrl-c thread, implicitly started [here](https://github.com/astral-sh/ruff/blob/718b0cadf4398bf611bd76c982c64486e679d8e1/crates/red_knot/src/main.rs#L139-L145)).

<details>
<summary>Full thread backtraces with <code>RAYON_NUM_THREADS=4</code></summary>

```gdb
Thread 6 (Thread 0x77abf3e5c6c0 (LWP 83184) "ctrl-c"):
#0  0x000077abf3f1ebe2 in ?? () from /usr/lib/libc.so.6
#1  0x000077abf3f12e33 in ?? () from /usr/lib/libc.so.6
#2  0x000077abf3f12e74 in ?? () from /usr/lib/libc.so.6
#3  0x000077abf3f8da3e in read () from /usr/lib/libc.so.6
#4  0x00005b20271e2285 in nix::unistd::read (fd=3, buf=...) at src/unistd.rs:1097
#5  0x00005b2027182395 in ctrlc::platform::unix::block_ctrl_c () at /home/shark/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/ctrlc-3.4.6/src/platform/unix/mod.rs:180
#6  0x00005b202717da12 in ctrlc::set_handler_inner::{closure#0}<red_knot::run_check::{closure_env#4}> () at /home/shark/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/ctrlc-3.4.6/src/lib.rs:141
#7  0x00005b20271a4156 in std::sys::backtrace::__rust_begin_short_backtrace<ctrlc::set_handler_inner::{closure_env#0}<red_knot::run_check::{closure_env#4}>, ()> (f=<error reading variable: Cannot access memory at address 0xb>) at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/sys/backtrace.rs:152
#8  0x00005b202716e6aa in std::thread::{impl#0}::spawn_unchecked_::{closure#1}::{closure#0}<ctrlc::set_handler_inner::{closure_env#0}<red_knot::run_check::{closure_env#4}>, ()> () at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/thread/mod.rs:559
#9  0x00005b20271d33d0 in core::panic::unwind_safe::{impl#23}::call_once<(), std::thread::{impl#0}::spawn_unchecked_::{closure#1}::{closure_env#0}<ctrlc::set_handler_inner::{closure_env#0}<red_knot::run_check::{closure_env#4}>, ()>> (self=...) at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/panic/unwind_safe.rs:272
#10 0x00005b20271d382b in std::panicking::try::do_call<core::panic::unwind_safe::AssertUnwindSafe<std::thread::{impl#0}::spawn_unchecked_::{closure#1}::{closure_env#0}<ctrlc::set_handler_inner::{closure_env#0}<red_knot::run_check::{closure_env#4}>, ()>>, ()> (data=0x77abf3e5ba78) at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panicking.rs:587
#11 0x00005b202716e98b in __rust_try ()
#12 0x00005b202716e093 in std::panicking::try<(), core::panic::unwind_safe::AssertUnwindSafe<std::thread::{impl#0}::spawn_unchecked_::{closure#1}::{closure_env#0}<ctrlc::set_handler_inner::{closure_env#0}<red_knot::run_check::{closure_env#4}>, ()>>> (f=...) at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panicking.rs:550
#13 std::panic::catch_unwind<core::panic::unwind_safe::AssertUnwindSafe<std::thread::{impl#0}::spawn_unchecked_::{closure#1}::{closure_env#0}<ctrlc::set_handler_inner::{closure_env#0}<red_knot::run_check::{closure_env#4}>, ()>>, ()> (f=...) at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panic.rs:358
#14 std::thread::{impl#0}::spawn_unchecked_::{closure#1}<ctrlc::set_handler_inner::{closure_env#0}<red_knot::run_check:--Type <RET> for more, q to quit, c to continue without paging--
:{closure_env#4}>, ()> () at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/thread/mod.rs:557
#15 0x00005b20271a542e in core::ops::function::FnOnce::call_once<std::thread::{impl#0}::spawn_unchecked_::{closure_env#1}<ctrlc::set_handler_inner::{closure_env#0}<red_knot::run_check::{closure_env#4}>, ()>, ()> () at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/ops/function.rs:250
#16 0x00005b2028881f3b in alloc::boxed::{impl#28}::call_once<(), dyn core::ops::function::FnOnce<(), Output=()>, alloc::alloc::Global> () at library/alloc/src/boxed.rs:1976
#17 alloc::boxed::{impl#28}::call_once<(), alloc::boxed::Box<dyn core::ops::function::FnOnce<(), Output=()>, alloc::alloc::Global>, alloc::alloc::Global> () at library/alloc/src/boxed.rs:1976
#18 std::sys::pal::unix::thread::{impl#2}::new::thread_start () at library/std/src/sys/pal/unix/thread.rs:106
#19 0x000077abf3f1670a in ?? () from /usr/lib/libc.so.6
#20 0x000077abf3f9aaac in ?? () from /usr/lib/libc.so.6

Thread 5 (Thread 0x77abf3c5b6c0 (LWP 83185) "red_knot"):
#0  0x000077abf3f9888d in syscall () from /usr/lib/libc.so.6
#1  0x00005b2028884885 in std::sys::pal::unix::futex::futex_wait () at library/std/src/sys/pal/unix/futex.rs:72
#2  std::sys::sync::condvar::futex::Condvar::wait_optional_timeout () at library/std/src/sys/sync/condvar/futex.rs:49
#3  std::sys::sync::condvar::futex::Condvar::wait () at library/std/src/sys/sync/condvar/futex.rs:33
#4  0x00005b20285bbd52 in std::sync::poison::condvar::Condvar::wait<bool> (self=0x5b205af39a08, guard=...) at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/sync/poison/condvar.rs:191
#5  0x00005b20285aa1bf in rayon_core::sleep::Sleep::sleep<rayon_core::registry::{impl#10}::wait_until_cold::{closure_env#0}> (self=0x5b205af3a458, idle_state=0x77abf3c59fa0, latch=0x5b205ae0c570, has_injected_jobs=...) at src/sleep/mod.rs:188
#6  0x00005b20285a9b86 in rayon_core::sleep::Sleep::no_work_found<rayon_core::registry::{impl#10}::wait_until_cold::{closure_env#0}> (self=0x5b205af3a458, idle_state=0x77abf3c59fa0, latch=0x5b205ae0c570, has_injected_jobs=...) at src/sleep/mod.rs:107
#7  0x00005b20285a4ef6 in rayon_core::registry::WorkerThread::wait_until_cold (self=0x77abf3c5a180, latch=0x5b205ae0c570) at src/registry.rs:798
#8  0x00005b20285a4c6b in rayon_core::registry::WorkerThread::wait_until<rayon_core::latch::OnceLatch> (self=0x77abf3c5a180, latch=0x5b205ae0c570) at src/registry.rs:769
#9  0x00005b20285a5045 in rayon_core::registry::WorkerThread::wait_until_out_of_work (self=0x77abf3c5a180) at src/registry.rs:818
#10 0x00005b20285a554f in rayon_core::registry::main_loop (thread=...) at src/registry.rs:923
#11 0x00005b20285a1ff6 in rayon_core::registry::ThreadBuilder::run (self=...) at src/registry.rs:53
--Type <RET> for more, q to quit, c to continue without paging--
#12 0x00005b20285a247d in rayon_core::registry::{impl#2}::spawn::{closure#0} () at src/registry.rs:98
#13 0x00005b20285aaa16 in std::sys::backtrace::__rust_begin_short_backtrace<rayon_core::registry::{impl#2}::spawn::{closure_env#0}, ()> (f=...) at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/sys/backtrace.rs:152
#14 0x00005b20285abe3b in std::thread::{impl#0}::spawn_unchecked_::{closure#1}::{closure#0}<rayon_core::registry::{impl#2}::spawn::{closure_env#0}, ()> () at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/thread/mod.rs:559
#15 0x00005b20285aa8b4 in core::panic::unwind_safe::{impl#23}::call_once<(), std::thread::{impl#0}::spawn_unchecked_::{closure#1}::{closure_env#0}<rayon_core::registry::{impl#2}::spawn::{closure_env#0}, ()>> (self=...) at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/panic/unwind_safe.rs:272
#16 0x00005b20285b2978 in std::panicking::try::do_call<core::panic::unwind_safe::AssertUnwindSafe<std::thread::{impl#0}::spawn_unchecked_::{closure#1}::{closure_env#0}<rayon_core::registry::{impl#2}::spawn::{closure_env#0}, ()>>, ()> (data=0x77abf3c5a9d8) at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panicking.rs:587
#17 0x00005b20285ae03b in __rust_try ()
#18 0x00005b20285abbb8 in std::panicking::try<(), core::panic::unwind_safe::AssertUnwindSafe<std::thread::{impl#0}::spawn_unchecked_::{closure#1}::{closure_env#0}<rayon_core::registry::{impl#2}::spawn::{closure_env#0}, ()>>> (f=...) at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panicking.rs:550
#19 std::panic::catch_unwind<core::panic::unwind_safe::AssertUnwindSafe<std::thread::{impl#0}::spawn_unchecked_::{closure#1}::{closure_env#0}<rayon_core::registry::{impl#2}::spawn::{closure_env#0}, ()>>, ()> (f=...) at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panic.rs:358
#20 std::thread::{impl#0}::spawn_unchecked_::{closure#1}<rayon_core::registry::{impl#2}::spawn::{closure_env#0}, ()> () at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/thread/mod.rs:557
#21 0x00005b20285b5dcf in core::ops::function::FnOnce::call_once<std::thread::{impl#0}::spawn_unchecked_::{closure_env#1}<rayon_core::registry::{impl#2}::spawn::{closure_env#0}, ()>, ()> () at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/ops/function.rs:250
#22 0x00005b2028881f3b in alloc::boxed::{impl#28}::call_once<(), dyn core::ops::function::FnOnce<(), Output=()>, alloc::alloc::Global> () at library/alloc/src/boxed.rs:1976
#23 alloc::boxed::{impl#28}::call_once<(), alloc::boxed::Box<dyn core::ops::function::FnOnce<(), Output=()>, alloc::alloc::Global>, alloc::alloc::Global> () at library/alloc/src/boxed.rs:1976
#24 std::sys::pal::unix::thread::{impl#2}::new::thread_start () at library/std/src/sys/pal/unix/thread.rs:106
#25 0x000077abf3f1670a in ?? () from /usr/lib/libc.so.6
#26 0x000077abf3f9aaac in ?? () from /usr/lib/libc.so.6
--Type <RET> for more, q to quit, c to continue without paging--

Thread 4 (Thread 0x77abeb9ff6c0 (LWP 83186) "red_knot"):
#0  0x000077abf3f9888d in syscall () from /usr/lib/libc.so.6
#1  0x00005b2028884885 in std::sys::pal::unix::futex::futex_wait () at library/std/src/sys/pal/unix/futex.rs:72
#2  std::sys::sync::condvar::futex::Condvar::wait_optional_timeout () at library/std/src/sys/sync/condvar/futex.rs:49
#3  std::sys::sync::condvar::futex::Condvar::wait () at library/std/src/sys/sync/condvar/futex.rs:33
#4  0x00005b20285bbd52 in std::sync::poison::condvar::Condvar::wait<bool> (self=0x5b205af39a88, guard=...) at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/sync/poison/condvar.rs:191
#5  0x00005b20285aa1bf in rayon_core::sleep::Sleep::sleep<rayon_core::registry::{impl#10}::wait_until_cold::{closure_env#0}> (self=0x5b205af3a458, idle_state=0x77abeb9fdfa0, latch=0x5b205ae0c5a0, has_injected_jobs=...) at src/sleep/mod.rs:188
#6  0x00005b20285a9b86 in rayon_core::sleep::Sleep::no_work_found<rayon_core::registry::{impl#10}::wait_until_cold::{closure_env#0}> (self=0x5b205af3a458, idle_state=0x77abeb9fdfa0, latch=0x5b205ae0c5a0, has_injected_jobs=...) at src/sleep/mod.rs:107
#7  0x00005b20285a4ef6 in rayon_core::registry::WorkerThread::wait_until_cold (self=0x77abeb9fe180, latch=0x5b205ae0c5a0) at src/registry.rs:798
#8  0x00005b20285a4c6b in rayon_core::registry::WorkerThread::wait_until<rayon_core::latch::OnceLatch> (self=0x77abeb9fe180, latch=0x5b205ae0c5a0) at src/registry.rs:769
#9  0x00005b20285a5045 in rayon_core::registry::WorkerThread::wait_until_out_of_work (self=0x77abeb9fe180) at src/registry.rs:818
#10 0x00005b20285a554f in rayon_core::registry::main_loop (thread=...) at src/registry.rs:923
#11 0x00005b20285a1ff6 in rayon_core::registry::ThreadBuilder::run (self=...) at src/registry.rs:53
#12 0x00005b20285a247d in rayon_core::registry::{impl#2}::spawn::{closure#0} () at src/registry.rs:98
#13 0x00005b20285aaa16 in std::sys::backtrace::__rust_begin_short_backtrace<rayon_core::registry::{impl#2}::spawn::{closure_env#0}, ()> (f=...) at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/sys/backtrace.rs:152
#14 0x00005b20285abe3b in std::thread::{impl#0}::spawn_unchecked_::{closure#1}::{closure#0}<rayon_core::registry::{impl#2}::spawn::{closure_env#0}, ()> () at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/thread/mod.rs:559
#15 0x00005b20285aa8b4 in core::panic::unwind_safe::{impl#23}::call_once<(), std::thread::{impl#0}::spawn_unchecked_::{closure#1}::{closure_env#0}<rayon_core::registry::{impl#2}::spawn::{closure_env#0}, ()>> (self=...) at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/panic/unwind_safe.rs:272
#16 0x00005b20285b2978 in std::panicking::try::do_call<core::panic::unwind_safe::AssertUnwindSafe<std::thread::{impl#0}::spawn_unchecked_::{closure#1}::{closure_env#0}<rayon_core::registry::{impl#2}::spawn::{closure_env#0}, ()>>, ()> (dat--Type <RET> for more, q to quit, c to continue without paging--
a=0x77abeb9fe9d8) at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panicking.rs:587
#17 0x00005b20285ae03b in __rust_try ()
#18 0x00005b20285abbb8 in std::panicking::try<(), core::panic::unwind_safe::AssertUnwindSafe<std::thread::{impl#0}::spawn_unchecked_::{closure#1}::{closure_env#0}<rayon_core::registry::{impl#2}::spawn::{closure_env#0}, ()>>> (f=...) at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panicking.rs:550
#19 std::panic::catch_unwind<core::panic::unwind_safe::AssertUnwindSafe<std::thread::{impl#0}::spawn_unchecked_::{closure#1}::{closure_env#0}<rayon_core::registry::{impl#2}::spawn::{closure_env#0}, ()>>, ()> (f=...) at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panic.rs:358
#20 std::thread::{impl#0}::spawn_unchecked_::{closure#1}<rayon_core::registry::{impl#2}::spawn::{closure_env#0}, ()> () at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/thread/mod.rs:557
#21 0x00005b20285b5dcf in core::ops::function::FnOnce::call_once<std::thread::{impl#0}::spawn_unchecked_::{closure_env#1}<rayon_core::registry::{impl#2}::spawn::{closure_env#0}, ()>, ()> () at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/ops/function.rs:250
#22 0x00005b2028881f3b in alloc::boxed::{impl#28}::call_once<(), dyn core::ops::function::FnOnce<(), Output=()>, alloc::alloc::Global> () at library/alloc/src/boxed.rs:1976
#23 alloc::boxed::{impl#28}::call_once<(), alloc::boxed::Box<dyn core::ops::function::FnOnce<(), Output=()>, alloc::alloc::Global>, alloc::alloc::Global> () at library/alloc/src/boxed.rs:1976
#24 std::sys::pal::unix::thread::{impl#2}::new::thread_start () at library/std/src/sys/pal/unix/thread.rs:106
#25 0x000077abf3f1670a in ?? () from /usr/lib/libc.so.6
#26 0x000077abf3f9aaac in ?? () from /usr/lib/libc.so.6

Thread 3 (Thread 0x77abf3a5a6c0 (LWP 83187) "red_knot"):
#0  0x000077abf3f9888d in syscall () from /usr/lib/libc.so.6
#1  0x00005b2028884885 in std::sys::pal::unix::futex::futex_wait () at library/std/src/sys/pal/unix/futex.rs:72
#2  std::sys::sync::condvar::futex::Condvar::wait_optional_timeout () at library/std/src/sys/sync/condvar/futex.rs:49
#3  std::sys::sync::condvar::futex::Condvar::wait () at library/std/src/sys/sync/condvar/futex.rs:33
#4  0x00005b20285bbd52 in std::sync::poison::condvar::Condvar::wait<bool> (self=0x5b205af39b08, guard=...) at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/sync/poison/condvar.rs:191
#5  0x00005b20285aa1bf in rayon_core::sleep::Sleep::sleep<rayon_core::registry::{impl#10}::wait_until_cold::{closure_env#0}> (self=0x5b205af3a458, idle_state=0x77abf3a58fa0, latch=0x5b205ae0c5d0, has_injected_jobs=...) at src/sleep/mod.rs:188
#6  0x00005b20285a9b86 in rayon_core::sleep::Sleep::no_work_found<rayon_core::registry::{impl#10}::wait_until_cold::{cl--Type <RET> for more, q to quit, c to continue without paging--
osure_env#0}> (self=0x5b205af3a458, idle_state=0x77abf3a58fa0, latch=0x5b205ae0c5d0, has_injected_jobs=...) at src/sleep/mod.rs:107
#7  0x00005b20285a4ef6 in rayon_core::registry::WorkerThread::wait_until_cold (self=0x77abf3a59180, latch=0x5b205ae0c5d0) at src/registry.rs:798
#8  0x00005b20285a4c6b in rayon_core::registry::WorkerThread::wait_until<rayon_core::latch::OnceLatch> (self=0x77abf3a59180, latch=0x5b205ae0c5d0) at src/registry.rs:769
#9  0x00005b20285a5045 in rayon_core::registry::WorkerThread::wait_until_out_of_work (self=0x77abf3a59180) at src/registry.rs:818
#10 0x00005b20285a554f in rayon_core::registry::main_loop (thread=...) at src/registry.rs:923
#11 0x00005b20285a1ff6 in rayon_core::registry::ThreadBuilder::run (self=...) at src/registry.rs:53
#12 0x00005b20285a247d in rayon_core::registry::{impl#2}::spawn::{closure#0} () at src/registry.rs:98
#13 0x00005b20285aaa16 in std::sys::backtrace::__rust_begin_short_backtrace<rayon_core::registry::{impl#2}::spawn::{closure_env#0}, ()> (f=...) at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/sys/backtrace.rs:152
#14 0x00005b20285abe3b in std::thread::{impl#0}::spawn_unchecked_::{closure#1}::{closure#0}<rayon_core::registry::{impl#2}::spawn::{closure_env#0}, ()> () at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/thread/mod.rs:559
#15 0x00005b20285aa8b4 in core::panic::unwind_safe::{impl#23}::call_once<(), std::thread::{impl#0}::spawn_unchecked_::{closure#1}::{closure_env#0}<rayon_core::registry::{impl#2}::spawn::{closure_env#0}, ()>> (self=...) at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/panic/unwind_safe.rs:272
#16 0x00005b20285b2978 in std::panicking::try::do_call<core::panic::unwind_safe::AssertUnwindSafe<std::thread::{impl#0}::spawn_unchecked_::{closure#1}::{closure_env#0}<rayon_core::registry::{impl#2}::spawn::{closure_env#0}, ()>>, ()> (data=0x77abf3a599d8) at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panicking.rs:587
#17 0x00005b20285ae03b in __rust_try ()
#18 0x00005b20285abbb8 in std::panicking::try<(), core::panic::unwind_safe::AssertUnwindSafe<std::thread::{impl#0}::spawn_unchecked_::{closure#1}::{closure_env#0}<rayon_core::registry::{impl#2}::spawn::{closure_env#0}, ()>>> (f=...) at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panicking.rs:550
#19 std::panic::catch_unwind<core::panic::unwind_safe::AssertUnwindSafe<std::thread::{impl#0}::spawn_unchecked_::{closure#1}::{closure_env#0}<rayon_core::registry::{impl#2}::spawn::{closure_env#0}, ()>>, ()> (f=...) at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panic.rs:358
#20 std::thread::{impl#0}::spawn_unchecked_::{closure#1}<rayon_core::registry::{impl#2}::spawn::{closure_env#0}, ()> () at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/thread/mod.rs:557
--Type <RET> for more, q to quit, c to continue without paging--
#21 0x00005b20285b5dcf in core::ops::function::FnOnce::call_once<std::thread::{impl#0}::spawn_unchecked_::{closure_env#1}<rayon_core::registry::{impl#2}::spawn::{closure_env#0}, ()>, ()> () at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/ops/function.rs:250
#22 0x00005b2028881f3b in alloc::boxed::{impl#28}::call_once<(), dyn core::ops::function::FnOnce<(), Output=()>, alloc::alloc::Global> () at library/alloc/src/boxed.rs:1976
#23 alloc::boxed::{impl#28}::call_once<(), alloc::boxed::Box<dyn core::ops::function::FnOnce<(), Output=()>, alloc::alloc::Global>, alloc::alloc::Global> () at library/alloc/src/boxed.rs:1976
#24 std::sys::pal::unix::thread::{impl#2}::new::thread_start () at library/std/src/sys/pal/unix/thread.rs:106
#25 0x000077abf3f1670a in ?? () from /usr/lib/libc.so.6
#26 0x000077abf3f9aaac in ?? () from /usr/lib/libc.so.6

Thread 2 (Thread 0x77abf38596c0 (LWP 83188) "red_knot"):
#0  0x000077abf3f9888d in syscall () from /usr/lib/libc.so.6
#1  0x00005b2028884885 in std::sys::pal::unix::futex::futex_wait () at library/std/src/sys/pal/unix/futex.rs:72
#2  std::sys::sync::condvar::futex::Condvar::wait_optional_timeout () at library/std/src/sys/sync/condvar/futex.rs:49
#3  std::sys::sync::condvar::futex::Condvar::wait () at library/std/src/sys/sync/condvar/futex.rs:33
#4  0x00005b20285bbd52 in std::sync::poison::condvar::Condvar::wait<bool> (self=0x5b205af39b88, guard=...) at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/sync/poison/condvar.rs:191
#5  0x00005b20285aa1bf in rayon_core::sleep::Sleep::sleep<rayon_core::registry::{impl#10}::wait_until_cold::{closure_env#0}> (self=0x5b205af3a458, idle_state=0x77abf3857fa0, latch=0x5b205ae0c600, has_injected_jobs=...) at src/sleep/mod.rs:188
#6  0x00005b20285a9b86 in rayon_core::sleep::Sleep::no_work_found<rayon_core::registry::{impl#10}::wait_until_cold::{closure_env#0}> (self=0x5b205af3a458, idle_state=0x77abf3857fa0, latch=0x5b205ae0c600, has_injected_jobs=...) at src/sleep/mod.rs:107
#7  0x00005b20285a4ef6 in rayon_core::registry::WorkerThread::wait_until_cold (self=0x77abf3858180, latch=0x5b205ae0c600) at src/registry.rs:798
#8  0x00005b20285a4c6b in rayon_core::registry::WorkerThread::wait_until<rayon_core::latch::OnceLatch> (self=0x77abf3858180, latch=0x5b205ae0c600) at src/registry.rs:769
#9  0x00005b20285a5045 in rayon_core::registry::WorkerThread::wait_until_out_of_work (self=0x77abf3858180) at src/registry.rs:818
#10 0x00005b20285a554f in rayon_core::registry::main_loop (thread=...) at src/registry.rs:923
#11 0x00005b20285a1ff6 in rayon_core::registry::ThreadBuilder::run (self=...) at src/registry.rs:53
#12 0x00005b20285a247d in rayon_core::registry::{impl#2}::spawn::{closure#0} () at src/registry.rs:98
#13 0x00005b20285aaa16 in std::sys::backtrace::__rust_begin_short_backtrace<rayon_core::registry::{impl#2}::spawn::{clo--Type <RET> for more, q to quit, c to continue without paging--
sure_env#0}, ()> (f=...) at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/sys/backtrace.rs:152
#14 0x00005b20285abe3b in std::thread::{impl#0}::spawn_unchecked_::{closure#1}::{closure#0}<rayon_core::registry::{impl#2}::spawn::{closure_env#0}, ()> () at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/thread/mod.rs:559
#15 0x00005b20285aa8b4 in core::panic::unwind_safe::{impl#23}::call_once<(), std::thread::{impl#0}::spawn_unchecked_::{closure#1}::{closure_env#0}<rayon_core::registry::{impl#2}::spawn::{closure_env#0}, ()>> (self=...) at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/panic/unwind_safe.rs:272
#16 0x00005b20285b2978 in std::panicking::try::do_call<core::panic::unwind_safe::AssertUnwindSafe<std::thread::{impl#0}::spawn_unchecked_::{closure#1}::{closure_env#0}<rayon_core::registry::{impl#2}::spawn::{closure_env#0}, ()>>, ()> (data=0x77abf38589d8) at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panicking.rs:587
#17 0x00005b20285ae03b in __rust_try ()
#18 0x00005b20285abbb8 in std::panicking::try<(), core::panic::unwind_safe::AssertUnwindSafe<std::thread::{impl#0}::spawn_unchecked_::{closure#1}::{closure_env#0}<rayon_core::registry::{impl#2}::spawn::{closure_env#0}, ()>>> (f=...) at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panicking.rs:550
#19 std::panic::catch_unwind<core::panic::unwind_safe::AssertUnwindSafe<std::thread::{impl#0}::spawn_unchecked_::{closure#1}::{closure_env#0}<rayon_core::registry::{impl#2}::spawn::{closure_env#0}, ()>>, ()> (f=...) at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panic.rs:358
#20 std::thread::{impl#0}::spawn_unchecked_::{closure#1}<rayon_core::registry::{impl#2}::spawn::{closure_env#0}, ()> () at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/thread/mod.rs:557
#21 0x00005b20285b5dcf in core::ops::function::FnOnce::call_once<std::thread::{impl#0}::spawn_unchecked_::{closure_env#1}<rayon_core::registry::{impl#2}::spawn::{closure_env#0}, ()>, ()> () at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/ops/function.rs:250
#22 0x00005b2028881f3b in alloc::boxed::{impl#28}::call_once<(), dyn core::ops::function::FnOnce<(), Output=()>, alloc::alloc::Global> () at library/alloc/src/boxed.rs:1976
#23 alloc::boxed::{impl#28}::call_once<(), alloc::boxed::Box<dyn core::ops::function::FnOnce<(), Output=()>, alloc::alloc::Global>, alloc::alloc::Global> () at library/alloc/src/boxed.rs:1976
#24 std::sys::pal::unix::thread::{impl#2}::new::thread_start () at library/std/src/sys/pal/unix/thread.rs:106
#25 0x000077abf3f1670a in ?? () from /usr/lib/libc.so.6
#26 0x000077abf3f9aaac in ?? () from /usr/lib/libc.so.6

Thread 1 (Thread 0x77abf3e7fe00 (LWP 83183) "red_knot"):
--Type <RET> for more, q to quit, c to continue without paging--
#0  0x000077abf3f9888d in syscall () from /usr/lib/libc.so.6
#1  0x00005b202886ff16 in std::sys::pal::unix::futex::futex_wait () at library/std/src/sys/pal/unix/futex.rs:72
#2  std::sys::sync::thread_parking::futex::Parker::park () at library/std/src/sys/sync/thread_parking/futex.rs:55
#3  std::thread::Thread::park () at library/std/src/thread/mod.rs:1446
#4  std::thread::park () at library/std/src/thread/mod.rs:1083
#5  0x00005b20271a0ed5 in crossbeam_channel::context::Context::wait_until (self=0x7fffcd482100, deadline=...) at /home/shark/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/crossbeam-channel-0.5.14/src/context.rs:162
#6  0x00005b2027197825 in crossbeam_channel::flavors::array::{impl#1}::recv::{closure#1}<red_knot::MainLoopMessage> (cx=0x7fffcd482100) at /home/shark/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/crossbeam-channel-0.5.14/src/flavors/array.rs:432
#7  0x00005b20271d94dd in crossbeam_channel::context::{impl#0}::with::{closure#0}<crossbeam_channel::flavors::array::{impl#1}::recv::{closure_env#1}<red_knot::MainLoopMessage>, ()> (cx=0x7fffcd482100) at /home/shark/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/crossbeam-channel-0.5.14/src/context.rs:52
#8  0x00005b20271d9a07 in crossbeam_channel::context::{impl#0}::with::{closure#1}<crossbeam_channel::flavors::array::{impl#1}::recv::{closure_env#1}<red_knot::MainLoopMessage>, ()> (cell=0x77abf3e7fd60) at /home/shark/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/crossbeam-channel-0.5.14/src/context.rs:60
#9  0x00005b2027199088 in std::thread::local::LocalKey<core::cell::Cell<core::option::Option<crossbeam_channel::context::Context>>>::try_with<core::cell::Cell<core::option::Option<crossbeam_channel::context::Context>>, crossbeam_channel::context::{impl#0}::with::{closure_env#1}<crossbeam_channel::flavors::array::{impl#1}::recv::{closure_env#1}<red_knot::MainLoopMessage>, ()>, ()> (self=0x5b2028fe8678, f=...) at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/thread/local.rs:310
#10 0x00005b20271d8670 in crossbeam_channel::context::Context::with<crossbeam_channel::flavors::array::{impl#1}::recv::{closure_env#1}<red_knot::MainLoopMessage>, ()> (f=...) at /home/shark/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/crossbeam-channel-0.5.14/src/context.rs:55
#11 0x00005b202719745e in crossbeam_channel::flavors::array::Channel<red_knot::MainLoopMessage>::recv<red_knot::MainLoopMessage> (self=0x5b205ae40c00, deadline=...) at /home/shark/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/crossbeam-channel-0.5.14/src/flavors/array.rs:421
#12 0x00005b2027184e0f in crossbeam_channel::channel::Receiver<red_knot::MainLoopMessage>::recv<red_knot::MainLoopMessage> (self=0x7fffcd4864f8) at /home/shark/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/crossbeam-channel-0.5.14/src/channel.rs:814
#13 0x00005b20271b4407 in red_knot::MainLoop::main_loop (self=0x7fffcd486478, db=0x7fffcd485990) at crates/red_knot/src/main.rs:240
#14 0x00005b20271b31ba in red_knot::MainLoop::run (self=..., db=0x7fffcd485990) at crates/red_knot/src/main.rs:227
#15 0x00005b20271b0f0a in red_knot::run_check (args=...) at crates/red_knot/src/main.rs:150
--Type <RET> for more, q to quit, c to continue without paging--
#16 0x00005b20271af405 in red_knot::run () at crates/red_knot/src/main.rs:66
#17 0x00005b20271af20e in red_knot::main () at crates/red_knot/src/main.rs:29

```

</details>

I also still see a message about the panic before the hang occurs, e.g.
```
thread '<unnamed>' panicked at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/function/fetch.rs:167:25:
dependency graph cycle querying try_metaclass_(Id(64829)); set cycle_fn/cycle_initial to fixpoint iterate
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace

thread '<unnamed>' panicked at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/function/fetch.rs:167:25:
dependency graph cycle querying try_metaclass_(Id(64829)); set cycle_fn/cycle_initial to fixpoint iterate
```


**How to reproduce**

```bash
git clone https://github.com/hydpy-dev/hydpy
cd hydpy
uv venv
uv pip install numpy pandas-stubs scipy-stubs types-docutils types-networkx

while true; do red_knot check --python .venv hydpy; done
```

---

_Label `bug` added by @sharkdp on 2025-04-22 07:17_

---

_Label `red-knot` added by @sharkdp on 2025-04-22 07:17_

---

_Added to milestone `Red Knot Alpha` by @AlexWaygood on 2025-04-22 07:30_

---

_Assigned to @MichaReiser by @MichaReiser on 2025-04-25 10:44_

---

_Comment by @MichaReiser on 2025-04-25 11:38_

I'm no longer convinced that the hang is due to a panic because:

* Rayon exits the process if it can't propagate a panic (this is the case for the thread started inside the main loop)
* Adding a `catch_unwind` to the `check` handler in the main loop doesn't fix the issue
* Pressing Ctrl+C doesn't exit the process
* Running Red Knot with extra verbose logging shows that some threads are inside type inference

```
16      ┌─┘
5      ├─   3.455161s 125ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(5), kind: WillExecute { database_key: infer_definition_types(Id(5a717)) } }
5      └─┐red_knot_python_semantic::types::infer::infer_definition_types{range=124225..124226, file=File(System("/Users/micha/astral/ecosystem/hydpy/hydpy/models/ga_garto.py"))}
4          ├─   3.455186s  13ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(4), kind: WillExecute { database_key: member_lookup_with_policy_(Id(193be)) } }
4          ├─   3.455195s  13ms TRACE red_knot_python_semantic::types member_lookup_with_policy: CM.load_file
15        └─┐red_knot_python_semantic::types::infer::infer_expression_types{expression=Id(3a05), range=20639..20678, file=File(System("/Users/micha/astral/ecosystem/hydpy/hydpy/models/sw1d/sw1d_model.py"))}
6          └─┐red_knot_python_semantic::symbol::symbol{name="Calc_Precipitation_V1"}
6            ├─   3.455239s   0ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(6), kind: DidInternValue { id: Id(1e08c), revision: R2 } }
12      ├─   3.455256s  38ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(12), kind: WillExecute { database_key: infer_definition_types(Id(56f92)) } }
10          └─┐red_knot_python_semantic::symbol::symbol{name="FunctionType"}
10          ┌─┘
7          └─┐red_knot_python_semantic::symbol::symbol{name="extract"}
10          ├─   3.455310s  13ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(10), kind: DidInternValue { id: Id(3781e), revision: R2 } }
9      ├─   3.455323s   0ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(9), kind: DidInternValue { id: Id(16922), revision: R2 } }
16      ├─   3.455336s  77ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(16), kind: WillExecute { database_key: infer_definition_types(Id(5868f)) } }
16      └─┐red_knot_python_semantic::types::infer::infer_definition_types{range=63264..63278, file=File(System("/Users/micha/astral/ecosystem/hydpy/hydpy/exe/xmltools.py"))}
5        ├─   3.455359s   0ms TRACE red_knot_python_semantic::types::infer Resolving imported object `*` from module `hydpy.core.typingtools` into file `/Users/micha/astral/ecosystem/hydpy/hydpy/models/ga_garto.py`
14        └─┐red_knot_python_semantic::symbol::symbol{name="pointerutils"}
4          ├─   3.455383s  14ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(4), kind: WillExecute { database_key: use_def_map(Id(24d96)) } }
4          └─┐red_knot_python_semantic::semantic_index::use_def_map{scope=Id(24d96), file=File(System("/Users/micha/astral/ecosystem/hydpy/hydpy/core/importtools.py"))}
4          ┌─┘
15        ┌─┘
4          ├─   3.455432s  14ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(4), kind: WillExecute { database_key: use_def_map(Id(3cd88)) } }
4          └─┐red_knot_python_semantic::semantic_index::use_def_map{scope=Id(3cd88), file=File(System("/Users/micha/astral/ecosystem/hydpy/hydpy/core/filetools.py"))}
4          ┌─┘
7          ┌─┘
10          ├─   3.455478s  13ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(10), kind: DidInternValue { id: Id(230fb), revision: R2 } }
10          ├─   3.455490s  13ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(10), kind: WillExecute { database_key: class_member_with_policy_(Id(230fb)) } }
10          ├─   3.455499s  13ms TRACE red_knot_python_semantic::types class_member: def __init__(self, *, modelname: str | None = None) -> None.__set__
16        ├─   3.455516s   0ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(16), kind: DidInternValue { id: Id(56497), revision: R2 } }
7          └─┐red_knot_python_semantic::symbol::symbol{name="str"}
14        ┌─┘
13        ┌─┘
6            ├─   3.455561s   0ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(6), kind: WillExecute { database_key: symbol_by_id(Id(1e08c)) } }
13        └─┐red_knot_python_semantic::symbol::symbol{name="str"}
6            ├─   3.455578s   0ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(6), kind: WillExecute { database_key: infer_definition_types(Id(4c20b)) } }
6            └─┐red_knot_python_semantic::types::infer::infer_definition_types{range=88210..88231, file=File(System("/Users/micha/astral/ecosystem/hydpy/hydpy/models/meteo/meteo_model.py"))}
3      ├─   3.455605s  29ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(3), kind: DidInternValue { id: Id(2c48d), revision: R2 } }
4          ├─   3.455616s  14ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(4), kind: WillExecute { database_key: infer_definition_types(Id(4bd6e)) } }
4          └─┐red_knot_python_semantic::types::infer::infer_definition_types{range=35332..35341, file=File(System("/Users/micha/astral/ecosystem/hydpy/hydpy/core/filetools.py"))}
11      └─┐red_knot_python_semantic::types::infer::infer_definition_types{range=172..173, file=File(System("/Users/micha/astral/ecosystem/hydpy/hydpy/interfaces/stateinterfaces.py"))}
10          └─┐red_knot_python_semantic::symbol::symbol{name="FunctionType"}
5        ├─   3.455668s   0ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(5), kind: DidInternValue { id: Id(60038), revision: R2 } }
5        ├─   3.455678s   0ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(5), kind: WillExecute { database_key: member_lookup_with_policy_(Id(60038)) } }
5        ├─   3.455687s   0ms TRACE red_knot_python_semantic::types member_lookup_with_policy: <module 'hydpy.core.typingtools'>.MayNonerable2
14      ┌─┘
12      └─┐red_knot_python_semantic::types::infer::infer_definition_types{range=34..35, file=File(System("/Users/micha/astral/ecosystem/hydpy/hydpy/data/HydPy-H-Lahn/conditions/init_1996_01_01_00_00_00/land_lahn_kalk.py"))}
8        ┌─┘
13        ┌─┘
15      ┌─┘
13        └─┐red_knot_python_semantic::symbol::symbol{name="NoneType"}
15      ├─   3.455760s   9ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(15), kind: WillExecute { database_key: infer_definition_types(Id(3a44f)) } }
9      ├─   3.455772s   1ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(9), kind: WillExecute { database_key: infer_definition_types(Id(5c86e)) } }
9      └─┐red_knot_python_semantic::types::infer::infer_definition_types{range=94367..94371, file=File(System("/Users/micha/astral/ecosystem/hydpy/hydpy/exe/servertools.py"))}
11        ├─   3.455796s   0ms TRACE red_knot_python_semantic::types::infer Resolving imported object `*` from module `hydpy.core.typingtools` into file `/Users/micha/astral/ecosystem/hydpy/hydpy/interfaces/stateinterfaces.py`
10          ┌─┘
16      ┌─┘
10          ├─   3.455825s  13ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(10), kind: DidInternValue { id: Id(230fc), revision: R2 } }
16      ├─   3.455832s  78ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(16), kind: WillExecute { database_key: infer_definition_types(Id(58693)) } }
16      └─┐red_knot_python_semantic::types::infer::infer_definition_types{range=70984..70995, file=File(System("/Users/micha/astral/ecosystem/hydpy/hydpy/exe/xmltools.py"))}
12        ├─   3.455857s   0ms TRACE red_knot_python_semantic::types::infer Resolving imported object `*` from module `hydpy.models.hland_96` into file `/Users/micha/astral/ecosystem/hydpy/hydpy/data/HydPy-H-Lahn/conditions/init_1996_01_01_00_00_00/land_lahn_kalk.py`
6              ├─   3.455870s   0ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(6), kind: DidInternValue { id: Id(39cac), revision: R2 } }
13        ┌─┘
8      ┌─┘
6            ┌─┘
3      ├─   3.455907s  29ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(3), kind: WillExecute { database_key: try_mro_(Id(2c48d)) } }
6          ┌─┘
4            └─┐red_knot_python_semantic::symbol::symbol{name="NoneType"}
11        ├─   3.455940s   0ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(11), kind: DidInternValue { id: Id(60429), revision: R2 } }
11        ├─   3.455951s   0ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(11), kind: WillExecute { database_key: member_lookup_with_policy_(Id(60429)) } }
11        ├─   3.455961s   0ms TRACE red_knot_python_semantic::types member_lookup_with_policy: <module 'hydpy.core.typingtools'>.VectorObject
11        └─┐red_knot_python_semantic::symbol::symbol{name="VectorObject"}
14      ├─   3.455987s  55ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(14), kind: WillExecute { database_key: infer_definition_types(Id(c464)) } }
16        ├─   3.456000s   0ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(16), kind: DidInternValue { id: Id(56498), revision: R2 } }
12        ├─   3.456014s   0ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(12), kind: DidInternValue { id: Id(5fc32), revision: R2 } }
12        ├─   3.456026s   0ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(12), kind: WillExecute { database_key: member_lookup_with_policy_(Id(5fc32)) } }
12        ├─   3.456034s   0ms TRACE red_knot_python_semantic::types member_lookup_with_policy: <module 'hydpy.models.hland_96'>.MethodGroup
3      ├─   3.456047s  29ms TRACE red_knot_python_semantic::types::class ClassLiteralType::try_mro: SG1
13        └─┐red_knot_python_semantic::symbol::symbol{name="bool"}
13        ┌─┘
4            ┌─┘
13        └─┐red_knot_python_semantic::symbol::symbol{name="bool"}
4            └─┐red_knot_python_semantic::symbol::symbol{name="NoneType"}
10          ├─   3.456109s  14ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(10), kind: WillExecute { database_key: class_member_with_policy_(Id(230fc)) } }
10          ├─   3.456120s  14ms TRACE red_knot_python_semantic::types class_member: def __init__(self, *, modelname: str | None = None) -> None.__delete__
11        ┌─┘
14      └─┐red_knot_python_semantic::types::infer::infer_definition_types{range=981..982, file=File(System("/Users/micha/astral/ecosystem/hydpy/hydpy/core/testtools.py"))}
16      ┌─┘
15      └─┐red_knot_python_semantic::types::infer::infer_definition_types{range=20683..20700, file=File(System("/Users/micha/astral/ecosystem/hydpy/hydpy/models/sw1d/sw1d_model.py"))}
8      ├─   3.456187s  23ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(8), kind: WillExecute { database_key: infer_definition_types(Id(74b6)) } }
8      └─┐red_knot_python_semantic::types::infer::infer_definition_types{range=5159..5166, file=File(System("/Users/micha/astral/ecosystem/hydpy/hydpy/core/parametertools.py"))}
3      ├─   3.456211s  29ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(3), kind: DidInternValue { id: Id(2c48e), revision: R2 } }
3      ├─   3.456222s  29ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(3), kind: WillExecute { database_key: try_mro_(Id(2c48e)) } }
3      ├─   3.456230s  30ms TRACE red_knot_python_semantic::types::class ClassLiteralType::try_mro: State1DSequence
3      ├─   3.456241s  30ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(3), kind: WillExecute { database_key: inheritance_cycle_(Id(25cd9)) } }
3      ├─   3.456249s  30ms TRACE red_knot_python_semantic::types::class Class::inheritance_cycle: State1DSequence
4            ┌─┘
5        └─┐red_knot_python_semantic::symbol::symbol{name="MayNonerable2"}
4            ├─   3.456277s   0ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(4), kind: DidInternValue { id: Id(52460), revision: R2 } }
5        ┌─┘
4          ┌─┘
5      ┌─┘
15        ├─   3.456310s   0ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(15), kind: WillExecute { database_key: infer_expression_types(Id(3a06)) } }
15        └─┐red_knot_python_semantic::types::infer::infer_expression_types{expression=Id(3a06), range=20703..20827, file=File(System("/Users/micha/astral/ecosystem/hydpy/hydpy/models/sw1d/sw1d_model.py"))}
8        ├─   3.456340s   0ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(8), kind: WillExecute { database_key: infer_expression_types(Id(7c1b)) } }
9        ├─   3.456355s   0ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(9), kind: WillExecute { database_key: infer_expression_types(Id(5c48a)) } }
6          ├─   3.456369s   1ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(6), kind: DidInternValue { id: Id(af92), revision: R2 } }
6          ├─   3.456380s   1ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(6), kind: WillExecute { database_key: member_lookup_with_policy_(Id(af92)) } }
6          ├─   3.456387s   1ms TRACE red_knot_python_semantic::types member_lookup_with_policy: <module 'hydpy.models.meteo.meteo_model'>.Adjust_Precipitation_V1
10          └─┐red_knot_python_semantic::symbol::symbol{name="FunctionType"}
3      ├─   3.456410s  30ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(3), kind: WillExecute { database_key: symbol_table(Id(10742)) } }
3      └─┐red_knot_python_semantic::semantic_index::symbol_table{scope=Id(10742), file=File(System("/Users/micha/astral/ecosystem/hydpy/hydpy/models/hland/hland_sequences.py"))}
3      ┌─┘
16      ├─   3.456445s  78ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(16), kind: WillExecute { database_key: infer_definition_types(Id(58697)) } }
16      └─┐red_knot_python_semantic::types::infer::infer_definition_types{range=72404..72415, file=File(System("/Users/micha/astral/ecosystem/hydpy/hydpy/exe/xmltools.py"))}
12        └─┐red_knot_python_semantic::symbol::symbol{name="MethodGroup"}
4          ├─   3.456485s  15ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(4), kind: DidInternValue { id: Id(218e8), revision: R2 } }
4          ├─   3.456497s  15ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(4), kind: WillExecute { database_key: class_member_with_policy_(Id(218e8)) } }
4          ├─   3.456506s  15ms TRACE red_knot_python_semantic::types class_member: CM.load_file
15          ├─   3.456517s   0ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(15), kind: DidInternValue { id: Id(5f814), revision: R2 } }
15          ├─   3.456527s   0ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(15), kind: WillExecute { database_key: member_lookup_with_policy_(Id(5f814)) } }
15          ├─   3.456535s   0ms TRACE red_knot_python_semantic::types member_lookup_with_policy: <module 'hydpy.models.sw1d.sw1d_factors'>.WaterLevelUpstream
6          └─┐red_knot_python_semantic::symbol::symbol{name="Adjust_Precipitation_V1"}
10          ┌─┘
11      ┌─┘
14        ├─   3.456583s   0ms TRACE red_knot_python_semantic::types::infer Resolving imported object `*` from module `hydpy.core.typingtools` into file `/Users/micha/astral/ecosystem/hydpy/hydpy/core/testtools.py`
10        ┌─┘
14        ├─   3.456603s   0ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(14), kind: DidInternValue { id: Id(1ab66), revision: R2 } }
14        ├─   3.456613s   0ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(14), kind: WillExecute { database_key: member_lookup_with_policy_(Id(1ab66)) } }
14        ├─   3.456623s   0ms TRACE red_knot_python_semantic::types member_lookup_with_policy: <module 'hydpy.core.typingtools'>.TensorInputObject
8        └─┐red_knot_python_semantic::types::infer::infer_expression_types{expression=Id(7c1b), range=5169..5198, file=File(System("/Users/micha/astral/ecosystem/hydpy/hydpy/core/parametertools.py"))}
9        └─┐red_knot_python_semantic::types::infer::infer_expression_types{expression=Id(5c48a), range=94374..94434, file=File(System("/Users/micha/astral/ecosystem/hydpy/hydpy/exe/servertools.py"))}
4          ├─   3.456662s  15ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(4), kind: DidInternValue { id: Id(18d15), revision: R2 } }
4          ├─   3.456675s  15ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(4), kind: WillExecute { database_key: symbol_by_id(Id(18d15)) } }
7          ┌─┘
15          └─┐red_knot_python_semantic::symbol::symbol{name="WaterLevelUpstream"}
7          └─┐red_knot_python_semantic::symbol::symbol{name="str"}
7          ┌─┘
5      ├─   3.456740s 126ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(5), kind: WillExecute { database_key: infer_definition_types(Id(5a718)) } }
5      └─┐red_knot_python_semantic::types::infer::infer_definition_types{range=124225..124226, file=File(System("/Users/micha/astral/ecosystem/hydpy/hydpy/models/ga_garto.py"))}
7          └─┐red_knot_python_semantic::symbol::symbol{name="bool"}
16        ├─   3.456764s   0ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(16), kind: DidInternValue { id: Id(56499), revision: R2 } }
12        ┌─┘
14        └─┐red_knot_python_semantic::symbol::symbol{name="TensorInputObject"}
13        ┌─┘
8          ├─   3.456842s   0ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(8), kind: DidInternValue { id: Id(1ccb1), revision: R2 } }
4          ├─   3.456855s  15ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(4), kind: DidInternValue { id: Id(37447), revision: R2 } }
4          ├─   3.456866s  15ms TRACE red_knot_project::db Salsa event: Event { thread_id: ThreadId(4), kind: WillExecute { database_key: try_call_dunder_get_(Id(37447)) } }
```

---

_Comment by @MichaReiser on 2025-04-25 12:12_

Maybe there are two issues:

* With `-vvv`. Hangs are very frequent, Ctrl + C doesn't work
* Without `-vvv`. Hangs are less frequent, Ctrl+C still works (suggesting that the main loop is waiting and that the rayon panic handler didn't trigger)

---

_Comment by @MichaReiser on 2025-04-25 12:49_

Interesting, removing the `Cancelled::catch` (which we absolutely need) makes the error go away (or much harder to reproduce)

```
    pub(crate) fn with_db<F, T>(&self, f: F) -> Result<T, Cancelled>
    where
        F: FnOnce(&ProjectDatabase) -> T + std::panic::UnwindSafe,
    {
        // Cancelled::catch(|| f(self))
        Ok(f(self))
    }
```

---

_Comment by @MichaReiser on 2025-04-25 13:25_

Uff, this was annoying to track down but I found the root cause:

When a salsa query panics that is used from two threads, then two panics are propagated:

* The panic on the thread that was executing the query (regular panic)
* A `Cancelled::PropagatedPanic` on the thread that waited on the result of the panicking thread

`rayon::scope` will pick one of the two panics to propagate, which is why we haven't been able to reproduce this consistently. 

Now, to my surprise, the `Cancelled::PropagatedPanic` is catched by `Cancelled::catch` in which case we return an `Err` from `db.check`. Our `main_loop` doesn't handle the `Err` case because because I assumed `Cancelled` would only be triggered when any input has changed (in which case we can just wait for the next check call to complete). 

Unfortunately, Salsa doesn't provide a way to distinguish between `Cancelled::PendingWrite` and `Cancelled::PropagatedPanic`, which means this requires an upstream change first. See https://salsa.zulipchat.com/#narrow/channel/333573-Contributing-to-Salsa/topic/.60Cancelled.3A.3APropagatedPanic.60/with/514378835

---

_Closed by @MichaReiser on 2025-04-26 06:28_

---
