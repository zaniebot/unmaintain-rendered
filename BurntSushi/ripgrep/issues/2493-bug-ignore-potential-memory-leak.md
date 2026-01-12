```yaml
number: 2493
title: "bug/ignore: potential memory leak"
type: issue
state: closed
author: Chaostheorie
labels: []
assignees: []
created_at: 2023-04-17T20:53:51Z
updated_at: 2023-04-17T22:42:19Z
url: https://github.com/BurntSushi/ripgrep/issues/2493
synced_at: 2026-01-12T16:13:24Z
```

# bug/ignore: potential memory leak

---

_@Chaostheorie_

#### What version of ignore are you using?

- `0.4.20` and directly `master` with stable rust (`1.68.2`)
- System/ Platform: Linux 6.2.10 NixOS SMP PREEMPT_DYNAMIC x86_64 GNU/Linux

#### Describe your bug.

`ignore::walk::Walk` appears to be leaking memory during `walk` by valgrinds analysis. I haven't been able to identify specifically where or if it's an invalid detection. It might also be related to the `walkdir` crate. 

#### What are the steps to reproduce the behavior?

Use the [example](https://github.com/BurntSushi/ripgrep/blob/041544853c86dde91c49983e5ddd0aa799bd2831/crates/ignore/examples/walk.rs) code in any directory. Tested so far with both v0.4.20 and using the crate from `master`, see [the full test](https://gitlab.cobalt.rocks/cobalt/walk-test) and inspect the execution with `valgrind`.

- Example with `0.4.20`: [CI run](https://gitlab.cobalt.rocks/cobalt/walk-test/-/jobs/5291) / [repo](https://gitlab.cobalt.rocks/cobalt/walk-test)
- Example with master@0415448: [CI run](https://gitlab.cobalt.rocks/cobalt/walk-test/-/jobs/5290) / [repo](https://gitlab.cobalt.rocks/cobalt/walk-test/-/tree/ignore-git?ref_type=heads)

Excerpt of relevant part of valgrind output:
```
...
==1277== 9,242 (80 direct, 9,162 indirect) bytes in 1 blocks are definitely lost in loss record 29 of 29
==1277==    at 0x483877F: malloc (vg_replace_malloc.c:307)
==1277==    by 0x150F9E: ignore::dir::IgnoreBuilder::build (in /builds/cobalt/walk-test/target/release/walk-test)
==1277==    by 0x152A36: ignore::walk::WalkBuilder::build (in /builds/cobalt/walk-test/target/release/walk-test)
==1277==    by 0x142FA4: ignore::walk::Walk::new (in /builds/cobalt/walk-test/target/release/walk-test)
==1277==    by 0x147D8C: walk_test::main (in /builds/cobalt/walk-test/target/release/walk-test)
==1277==    by 0x143082: std::sys_common::backtrace::__rust_begin_short_backtrace (in /builds/cobalt/walk-test/target/release/walk-test)
==1277==    by 0x143378: _ZN3std2rt10lang_start28_$u7b$$u7b$closure$u7d$$u7d$17h3ded7372003c0822E.llvm.4739356907723970547 (in /builds/cobalt/walk-test/target/release/walk-test)
==1277==    by 0x20DECB: call_once<(), (dyn core::ops::function::Fn<(), Output=i32> + core::marker::Sync + core::panic::unwind_safe::RefUnwindSafe)> (function.rs:287)
==1277==    by 0x20DECB: do_call<&(dyn core::ops::function::Fn<(), Output=i32> + core::marker::Sync + core::panic::unwind_safe::RefUnwindSafe), i32> (panicking.rs:483)
==1277==    by 0x20DECB: try<i32, &(dyn core::ops::function::Fn<(), Output=i32> + core::marker::Sync + core::panic::unwind_safe::RefUnwindSafe)> (panicking.rs:447)
==1277==    by 0x20DECB: catch_unwind<&(dyn core::ops::function::Fn<(), Output=i32> + core::marker::Sync + core::panic::unwind_safe::RefUnwindSafe), i32> (panic.rs:140)
==1277==    by 0x20DECB: {closure#2} (rt.rs:148)
==1277==    by 0x20DECB: do_call<std::rt::lang_start_internal::{closure_env#2}, isize> (panicking.rs:483)
==1277==    by 0x20DECB: try<isize, std::rt::lang_start_internal::{closure_env#2}> (panicking.rs:447)
==1277==    by 0x20DECB: catch_unwind<std::rt::lang_start_internal::{closure_env#2}, isize> (panic.rs:140)
==1277==    by 0x20DECB: std::rt::lang_start_internal (rt.rs:148)
==1277==    by 0x148E24: main (in /builds/cobalt/walk-test/target/release/walk-test)
==1277== 
==1277== LEAK SUMMARY:
==1277==    definitely lost: 80 bytes in 1 blocks
==1277==    indirectly lost: 9,162 bytes in 57 blocks
==1277==      possibly lost: 0 bytes in 0 blocks
==1277==    still reachable: 32 bytes in 1 blocks
==1277==         suppressed: 0 bytes in 0 blocks
==1277== Reachable blocks (those to which a pointer was found) are not shown.
==1277== To see them, rerun with: --leak-check=full --show-leak-kinds=all
```

---

_Comment by @Chaostheorie on 2023-04-17 20:56_

Note: ripgrep doesn't appear to have any leakage issues on the same machine in the same testing environment. If this is just a problem with my usage of `ignore` any help would be welcome.

---

_Comment by @BurntSushi on 2023-04-17 21:12_

I think I need better evidence than a valgrind report to be honest. In the past, these sorts of things have been false positives. In particular, I don't believe valgrind distinguishes between "memory grows without bound" and "something was allocated but never freed." The latter is not actually a problem and it is not something I would consider a bug.

So basically, if all you have is a valgrind report then that isn't really good enough for me absent other evidence.

---

_Comment by @Chaostheorie on 2023-04-17 22:33_

Maybe it is just a false-positive/ false detection. That's why the issue is titled "potential" memory leak. English is not my native language and I'm sorry if I missed some nuance there.

What kind of tooling would you recommend for digging deeper?

---

_Comment by @BurntSushi on 2023-04-17 22:36_

I don't think there is any tooling that can distinguish between the two classes of leaks I mentioned. The difference is semantic and in how the memory is used and whether it scales with the size of the input.

So basically what has to happen is you need to trace the valgrind leaks back to a point in the source code and determine what kind of leak it is. Sometimes doing this is tricjy. IIRC certain memory allocators like jemalloc will trigger false positives, so it might be good to run valgrind under different allocators.

---

_Comment by @Chaostheorie on 2023-04-17 22:42_

Okay, thank you for the quick response. I'll try looking into it tomorrow and will close the issue for now. I will reopen it if anything specific can be identified.

---

_Closed by @Chaostheorie on 2023-04-17 22:42_

---
