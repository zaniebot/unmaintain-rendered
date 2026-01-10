```yaml
number: 20005
title: "[ty] Demonstrate SIGSEGV via salsa"
type: pull_request
state: closed
author: BurntSushi
labels:
  - do-not-merge
  - ty
assignees: []
draft: true
base: main
head: ag/salsa-ub
created_at: 2025-08-20T14:47:16Z
updated_at: 2025-08-20T16:18:14Z
url: https://github.com/astral-sh/ruff/pull/20005
synced_at: 2026-01-10T17:52:17Z
```

# [ty] Demonstrate SIGSEGV via salsa

---

_Pull request opened by @BurntSushi on 2025-08-20 14:47_

This is a demonstration of what I believe must imply unsoundness
somewhere inside of Salsa. That is, I don't use any `unsafe`, but I get
an "invalid memory reference" error.

Here is how to reproduce it:

```
$ cargo test -p ty_python_semantic --test mdtest -- mdtest__generics_pep695_classes
    Finished `test` profile [unoptimized + debuginfo] target(s) in 0.08s
     Running tests/mdtest.rs (target/debug/deps/mdtest-02b235fab831eb7c)

running 1 test
error: test failed, to rerun pass `-p ty_python_semantic --test mdtest`

Caused by:
  process didn't exit successfully: `/home/andrew/astral/ruff/pr1/target/debug/deps/mdtest-02b235fab831eb7c mdtest__generics_pep695_classes` (signal: 11, SIGSEGV: invalid memory reference)
```

Note that I don't _always_ get a SIGSEGV here, but I _usually_ do.

Here is the backtrace I get from `gdb`:

```
(gdb) bt
 #0  ty_python_semantic::module_resolver::path::{impl#33}::eq (self=<error reading variable: Cannot access memory at address 0x7ff80efb0bd0>, other=0x7ffff106fcb0) at crates/ty_python_semantic/src/module_resolver/path.rs:452
 #1  0x0000555555ccc9fb in alloc::sync::{impl#50}::eq<ty_python_semantic::module_resolver::path::SearchPathInner, alloc::alloc::Global> (self=0x7ffff0110470, other=0x7ffff1033660)
     at /home/andrew/.rustup/toolchains/1.89-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/alloc/src/sync.rs:3358
 #2  0x0000555555ccc6a4 in alloc::sync::{impl#51}::eq<ty_python_semantic::module_resolver::path::SearchPathInner, alloc::alloc::Global> (self=0x7ffff0110470, other=0x7ffff1033660)
     at /home/andrew/.rustup/toolchains/1.89-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/alloc/src/sync.rs:3388
 #3  0x0000555555b92744 in ty_python_semantic::module_resolver::path::{impl#40}::eq (self=0x7ffff0110470, other=0x7ffff1033660) at crates/ty_python_semantic/src/module_resolver/path.rs:491
 #4  0x00005555562a4239 in core::cmp::impls::{impl#9}::eq<ty_python_semantic::module_resolver::path::SearchPath, ty_python_semantic::module_resolver::path::SearchPath> (self=0x7ffff0e88f50, other=0x7ffff7c755c8)
     at /home/andrew/.rustup/toolchains/1.89-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/cmp.rs:2027
 #5  0x00005555562a4436 in core::tuple::{impl#10}::eq<(), &ty_python_semantic::module_resolver::path::SearchPath> (self=0x7ffff0e88f50, other=0x7ffff7c755c8)
     at /home/andrew/.rustup/toolchains/1.89-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/tuple.rs:32
 #6  0x00005555562a44f2 in core::cmp::impls::{impl#9}::eq<((), &ty_python_semantic::module_resolver::path::SearchPath), ((), &ty_python_semantic::module_resolver::path::SearchPath)> (self=0x7ffff7c75178, other=0x7ffff7c75180)
     at /home/andrew/.rustup/toolchains/1.89-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/cmp.rs:2027
 #7  salsa::interned::{impl#14}::eq<((), &ty_python_semantic::module_resolver::path::SearchPath)> (self=0x7ffff0e88f50, data=0x7ffff7c755c8) at /home/andrew/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a3ffa22/src/interned.rs:1161
 #8  0x0000555555c172e7 in salsa::interned::IngredientImpl<ty_python_semantic::module_resolver::list::list_modules_in::list_modules_in_Configuration_>::value_eq<ty_python_semantic::module_resolver::list::list_modules_in::list_modules_in_Configuration_, ((), &ty_python_semantic::module_resolver::path::SearchPath)> (id=..., key=0x7ffff7c755c8, zalsa=0x7ffff000daa0, found_value=0x7ffff7c755e8)
     at /home/andrew/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a3ffa22/src/interned.rs:763
 #9  0x0000555555ca12b7 in salsa::interned::{impl#7}::intern_id::{closure#0}<ty_python_semantic::module_resolver::list::list_modules_in::list_modules_in_Configuration_, ((), &ty_python_semantic::module_resolver::path::SearchPath), ty_python_semantic::module_resolver::list::list_modules_in::{closure#0}::{closure_env#0}> (id=0x7ffff0e3a588) at /home/andrew/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a3ffa22/src/interned.rs:374
 #10 0x00005555565a907e in hashbrown::raw::{impl#8}::find::{closure#0}<salsa::id::Id, allocator_api2::stable::alloc::global::Global, salsa::interned::{impl#7}::intern_id::{closure_env#0}<ty_python_semantic::module_resolver::list::list_modules_in::list_modules_in_Configuration_, ((), &ty_python_semantic::module_resolver::path::SearchPath), ty_python_semantic::module_resolver::list::list_modules_in::{closure#0}::{closure_env#0}>> (index=0)
     at /home/andrew/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/hashbrown-0.15.5/src/raw/mod.rs:1197
 #11 0x00005555565821ec in hashbrown::raw::RawTableInner::find_inner (self=0x7ffff0098e08, hash=16887370829962692076, eq=...) at /home/andrew/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/hashbrown-0.15.5/src/raw/mod.rs:1921
 #12 hashbrown::raw::RawTable<salsa::id::Id, allocator_api2::stable::alloc::global::Global>::find<salsa::id::Id, allocator_api2::stable::alloc::global::Global, salsa::interned::{impl#7}::intern_id::{closure_env#0}<ty_python_semantic::module_resolver::list::list_modules_in::list_modules_in_Configuration_, ((), &ty_python_semantic::module_resolver::path::SearchPath), ty_python_semantic::module_resolver::list::list_modules_in::{closure#0}::{closure_env#0}>> (
     self=0x7ffff0098e08, hash=16887370829962692076, eq=...) at /home/andrew/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/hashbrown-0.15.5/src/raw/mod.rs:1197
 #13 0x0000555555c4f7f7 in hashbrown::raw::RawTable<salsa::id::Id, allocator_api2::stable::alloc::global::Global>::get<salsa::id::Id, allocator_api2::stable::alloc::global::Global, salsa::interned::{impl#7}::intern_id::{closure_env#0}<ty_python_semantic::module_resolver::list::list_modules_in::list_modules_in_Configuration_, ((), &ty_python_semantic::module_resolver::path::SearchPath), ty_python_semantic::module_resolver::list::list_modules_in::{closure#0}::{closure_env#0}>> (self=0x7ffff0098e08, hash=16887370829962692076, eq=...) at /home/andrew/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/hashbrown-0.15.5/src/raw/mod.rs:1212
 #14 hashbrown::table::HashTable<salsa::id::Id, allocator_api2::stable::alloc::global::Global>::find<salsa::id::Id, allocator_api2::stable::alloc::global::Global, salsa::interned::{impl#7}::intern_id::{closure_env#0}<ty_python_semantic::module_resolver::list::list_modules_in::list_modules_in_Configuration_, ((), &ty_python_semantic::module_resolver::path::SearchPath), ty_python_semantic::module_resolver::list::list_modules_in::{closure#0}::{closure_env#0}>> (
     self=0x7ffff0098e08, hash=16887370829962692076, eq=...) at /home/andrew/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/hashbrown-0.15.5/src/table.rs:224
 #15 salsa::interned::IngredientImpl<ty_python_semantic::module_resolver::list::list_modules_in::list_modules_in_Configuration_>::intern_id<ty_python_semantic::module_resolver::list::list_modules_in::list_modules_in_Configuration_, ((), &ty_python_semantic::module_resolver::path::SearchPath), ty_python_semantic::module_resolver::list::list_modules_in::{closure#0}::{closure_env#0}> (self=0x7ffff00982c0, zalsa=0x7ffff000daa0, zalsa_local=0x7ffff7c785a8,
     key=..., assemble=...) at /home/andrew/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a3ffa22/src/interned.rs:377
 #16 0x0000555555a24e68 in ty_python_semantic::module_resolver::list::list_modules_in::{closure#0} () at /home/andrew/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a3ffa22/components/salsa-macro-rules/src/setup_tracked_fn.rs:473
 #17 0x000055555647410c in salsa::attach::Attached::attach<dyn ty_python_semantic::db::Db, alloc::vec::Vec<ty_python_semantic::module_resolver::module::Module, alloc::alloc::Global>, ty_python_semantic::module_resolver::list::list_modules_in::{closure_env#0}> (self=0x7ffff7c7a618, db=..., op=...) at /home/andrew/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a3ffa22/src/attach.rs:79
 #18 0x0000555556470f92 in salsa::attach::attach::{closure#0}<alloc::vec::Vec<ty_python_semantic::module_resolver::module::Module, alloc::alloc::Global>, dyn ty_python_semantic::db::Db, ty_python_semantic::module_resolver::list::list_modules_in::{closure_env#0}> (a=0x7ffff7c7a618) at /home/andrew/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a3ffa22/src/attach.rs:103
 #19 0x0000555556304feb in std::thread::local::LocalKey<salsa::attach::Attached>::try_with<salsa::attach::Attached, salsa::attach::attach::{closure_env#0}<alloc::vec::Vec<ty_python_semantic::module_resolver::module::Module, alloc::alloc::Global>, dyn ty_python_semantic::db::Db, ty_python_semantic::module_resolver::list::list_modules_in::{closure_env#0}>, alloc::vec::Vec<ty_python_semantic::module_resolver::module::Module, alloc::alloc::Global>> (
     self=0x555557db8810, f=...) at /home/andrew/.rustup/toolchains/1.89-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/thread/local.rs:315
 #20 0x00005555562e7c8d in std::thread::local::LocalKey<salsa::attach::Attached>::with<salsa::attach::Attached, salsa::attach::attach::{closure_env#0}<alloc::vec::Vec<ty_python_semantic::module_resolver::module::Module, alloc::alloc::Global>, dyn ty_python_semantic::db::Db, ty_python_semantic::module_resolver::list::list_modules_in::{closure_env#0}>, alloc::vec::Vec<ty_python_semantic::module_resolver::module::Module, alloc::alloc::Global>> (
     self=0x555557db8810, f=<error reading variable: Cannot access memory at address 0x0>) at /home/andrew/.rustup/toolchains/1.89-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/thread/local.rs:279
 #21 0x000055555646f5e4 in salsa::attach::attach<alloc::vec::Vec<ty_python_semantic::module_resolver::module::Module, alloc::alloc::Global>, dyn ty_python_semantic::db::Db, ty_python_semantic::module_resolver::list::list_modules_in::{closure_env#0}> (db=..., op=...) at /home/andrew/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a3ffa22/src/attach.rs:101
 #22 0x0000555555cc9f94 in ty_python_semantic::module_resolver::list::list_modules_in (db=..., search_path=0x7ffff1033660)
     at /home/andrew/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a3ffa22/components/salsa-macro-rules/src/setup_tracked_fn.rs:468
 #23 0x0000555555cc95b5 in ty_python_semantic::module_resolver::list::list_modules::{impl#2}::execute::inner_ (db=...) at crates/ty_python_semantic/src/module_resolver/list.rs:20
 #24 0x0000555555cc946b in ty_python_semantic::module_resolver::list::list_modules::{impl#2}::execute (db=...)
     at /home/andrew/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a3ffa22/components/salsa-macro-rules/src/setup_tracked_fn.rs:302
 #25 0x00005555561c575a in salsa::function::IngredientImpl<ty_python_semantic::module_resolver::list::list_modules::list_modules_Configuration_>::execute_query<ty_python_semantic::module_resolver::list::list_modules::list_modules_Configuration_> (db=..., zalsa=0x7ffff000daa0, active_query=..., opt_old_memo=..., id=...) at /home/andrew/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a3ffa22/src/function/execute.rs:311
 #26 0x0000555556211b9d in salsa::function::IngredientImpl<ty_python_semantic::module_resolver::list::list_modules::list_modules_Configuration_>::execute<ty_python_semantic::module_resolver::list::list_modules::list_modules_Configuration_> (self=0x7ffff0098010, db=..., active_query=..., opt_old_memo=...) at /home/andrew/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a3ffa22/src/function/execute.rs:46
 #27 0x0000555556143974 in salsa::function::IngredientImpl<ty_python_semantic::module_resolver::list::list_modules::list_modules_Configuration_>::fetch_cold<ty_python_semantic::module_resolver::list::list_modules::list_modules_Configuration_> (self=0x7ffff0098010, zalsa=0x7ffff000daa0, zalsa_local=0x7ffff7c785a8, db=..., id=..., memo_ingredient_index=...) at /home/andrew/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a3ffa22/src/function/fetch.rs:235
 #28 0x00005555561775d0 in salsa::function::IngredientImpl<ty_python_semantic::module_resolver::list::list_modules::list_modules_Configuration_>::fetch_cold_with_retry<ty_python_semantic::module_resolver::list::list_modules::list_modules_Configuration_> (self=0x7ffff0098010, zalsa=0x7ffff000daa0, zalsa_local=0x7ffff7c785a8, db=..., id=..., memo_ingredient_index=...) at /home/andrew/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a3ffa22/src/function/fetch.rs:107
 #29 0x0000555556754069 in salsa::function::fetch::{impl#0}::refresh_memo::{closure#0}<ty_python_semantic::module_resolver::list::list_modules::list_modules_Configuration_> ()
     at /home/andrew/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a3ffa22/src/function/fetch.rs:64
 #30 0x00005555562d0891 in core::option::Option<&salsa::function::memo::Memo<ty_python_semantic::module_resolver::list::list_modules::list_modules_Configuration_>>::or_else<&salsa::function::memo::Memo<ty_python_semantic::module_resolver::list::list_modules::list_modules_Configuration_>, salsa::function::fetch::{impl#0}::refresh_memo::{closure_env#0}<ty_python_semantic::module_resolver::list::list_modules::list_modules_Configuration_>> (self=..., f=...)
 --Type <RET> for more, q to quit, c to continue without paging--
     at /home/andrew/.rustup/toolchains/1.89-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/option.rs:1609
 #31 0x0000555556183e04 in salsa::function::IngredientImpl<ty_python_semantic::module_resolver::list::list_modules::list_modules_Configuration_>::refresh_memo<ty_python_semantic::module_resolver::list::list_modules::list_modules_Configuration_> (self=0x7ffff0098010, db=..., zalsa=0x7ffff000daa0, zalsa_local=0x7ffff7c785a8, id=...) at /home/andrew/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a3ffa22/src/function/fetch.rs:63
 #32 salsa::function::IngredientImpl<ty_python_semantic::module_resolver::list::list_modules::list_modules_Configuration_>::fetch<ty_python_semantic::module_resolver::list::list_modules::list_modules_Configuration_> (
     self=0x7ffff0098010, db=..., zalsa=0x7ffff000daa0, zalsa_local=0x7ffff7c785a8, id=...) at /home/andrew/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a3ffa22/src/function/fetch.rs:30
 #33 0x0000555555a24d85 in ty_python_semantic::module_resolver::list::list_modules::{closure#0} () at /home/andrew/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a3ffa22/components/salsa-macro-rules/src/setup_tracked_fn.rs:474
 #34 0x0000555556474dd4 in salsa::attach::Attached::attach<dyn ty_python_semantic::db::Db, alloc::vec::Vec<ty_python_semantic::module_resolver::module::Module, alloc::alloc::Global>, ty_python_semantic::module_resolver::list::list_modules::{closure_env#0}> (self=0x7ffff7c7a618, db=..., op=...) at /home/andrew/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a3ffa22/src/attach.rs:79
 #35 0x0000555556470491 in salsa::attach::attach::{closure#0}<alloc::vec::Vec<ty_python_semantic::module_resolver::module::Module, alloc::alloc::Global>, dyn ty_python_semantic::db::Db, ty_python_semantic::module_resolver::list::list_modules::{closure_env#0}> (a=0x7ffff7c7a618) at /home/andrew/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a3ffa22/src/attach.rs:103
 #36 0x00005555563064e6 in std::thread::local::LocalKey<salsa::attach::Attached>::try_with<salsa::attach::Attached, salsa::attach::attach::{closure_env#0}<alloc::vec::Vec<ty_python_semantic::module_resolver::module::Module, alloc::alloc::Global>, dyn ty_python_semantic::db::Db, ty_python_semantic::module_resolver::list::list_modules::{closure_env#0}>, alloc::vec::Vec<ty_python_semantic::module_resolver::module::Module, alloc::alloc::Global>> (
     self=0x555557db8810, f=...) at /home/andrew/.rustup/toolchains/1.89-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/thread/local.rs:315
 #37 0x00005555562e897d in std::thread::local::LocalKey<salsa::attach::Attached>::with<salsa::attach::Attached, salsa::attach::attach::{closure_env#0}<alloc::vec::Vec<ty_python_semantic::module_resolver::module::Module, alloc::alloc::Global>, dyn ty_python_semantic::db::Db, ty_python_semantic::module_resolver::list::list_modules::{closure_env#0}>, alloc::vec::Vec<ty_python_semantic::module_resolver::module::Module, alloc::alloc::Global>> (self=0x555557db8810,
     f=<error reading variable: Cannot access memory at address 0x0>) at /home/andrew/.rustup/toolchains/1.89-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/thread/local.rs:279
 #38 0x000055555646f265 in salsa::attach::attach<alloc::vec::Vec<ty_python_semantic::module_resolver::module::Module, alloc::alloc::Global>, dyn ty_python_semantic::db::Db, ty_python_semantic::module_resolver::list::list_modules::{closure_env#0}> (db=..., op=...) at /home/andrew/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a3ffa22/src/attach.rs:101
 #39 0x0000555555cc93c7 in ty_python_semantic::module_resolver::list::list_modules (db=...) at /home/andrew/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a3ffa22/components/salsa-macro-rules/src/setup_tracked_fn.rs:468
 #40 0x00005555557e2186 in ty_test::run_module_resolution_consistency_test (db=0x7ffff7c78598) at crates/ty_test/src/lib.rs:450
 #41 0x00005555557defec in ty_test::run (absolute_fixture_path=..., relative_fixture_path=..., snapshot_path=..., short_title=..., test_name=..., output_format=ty_test::OutputFormat::Cli) at crates/ty_test/src/lib.rs:73
 #42 0x0000555555793c06 in mdtest::mdtest (fixture=...) at crates/ty_python_semantic/tests/mdtest.rs:29
 #43 0x00005555557959c1 in mdtest::mdtest__generics_pep695_classes () at crates/ty_python_semantic/tests/mdtest.rs:7
 #44 0x00005555557903a7 in mdtest::mdtest__generics_pep695_classes::{closure#0} () at crates/ty_python_semantic/tests/mdtest.rs:10
 #45 0x000055555579a1b6 in core::ops::function::FnOnce::call_once<mdtest::mdtest__generics_pep695_classes::{closure_env#0}, ()> ()
     at /home/andrew/.rustup/toolchains/1.89-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/ops/function.rs:250
 #46 0x00005555557d98eb in core::ops::function::FnOnce::call_once<fn() -> core::result::Result<(), alloc::string::String>, ()> () at library/core/src/ops/function.rs:250
 #47 test::__rust_begin_short_backtrace<core::result::Result<(), alloc::string::String>, fn() -> core::result::Result<(), alloc::string::String>> () at library/test/src/lib.rs:648
 #48 0x00005555557d8b6e in test::run_test_in_process::{closure#0} () at library/test/src/lib.rs:671
 #49 core::panic::unwind_safe::{impl#23}::call_once<core::result::Result<(), alloc::string::String>, test::run_test_in_process::{closure_env#0}> () at library/core/src/panic/unwind_safe.rs:272
 #50 std::panicking::catch_unwind::do_call<core::panic::unwind_safe::AssertUnwindSafe<test::run_test_in_process::{closure_env#0}>, core::result::Result<(), alloc::string::String>> () at library/std/src/panicking.rs:589
 #51 std::panicking::catch_unwind<core::result::Result<(), alloc::string::String>, core::panic::unwind_safe::AssertUnwindSafe<test::run_test_in_process::{closure_env#0}>> () at library/std/src/panicking.rs:552
 #52 std::panic::catch_unwind<core::panic::unwind_safe::AssertUnwindSafe<test::run_test_in_process::{closure_env#0}>, core::result::Result<(), alloc::string::String>> () at library/std/src/panic.rs:359
 #53 test::run_test_in_process () at library/test/src/lib.rs:671
 #54 test::run_test::{closure#0} () at library/test/src/lib.rs:592
 #55 0x000055555579d944 in test::run_test::{closure#1} () at library/test/src/lib.rs:622
 #56 std::sys::backtrace::__rust_begin_short_backtrace<test::run_test::{closure_env#1}, ()> () at library/std/src/sys/backtrace.rs:152
 #57 0x00005555557a10ca in std::thread::{impl#0}::spawn_unchecked_::{closure#1}::{closure#0}<test::run_test::{closure_env#1}, ()> () at library/std/src/thread/mod.rs:559
 #58 core::panic::unwind_safe::{impl#23}::call_once<(), std::thread::{impl#0}::spawn_unchecked_::{closure#1}::{closure_env#0}<test::run_test::{closure_env#1}, ()>> () at library/core/src/panic/unwind_safe.rs:272
 #59 std::panicking::catch_unwind::do_call<core::panic::unwind_safe::AssertUnwindSafe<std::thread::{impl#0}::spawn_unchecked_::{closure#1}::{closure_env#0}<test::run_test::{closure_env#1}, ()>>, ()> ()
     at library/std/src/panicking.rs:589
 #60 std::panicking::catch_unwind<(), core::panic::unwind_safe::AssertUnwindSafe<std::thread::{impl#0}::spawn_unchecked_::{closure#1}::{closure_env#0}<test::run_test::{closure_env#1}, ()>>> () at library/std/src/panicking.rs:552
 #61 std::panic::catch_unwind<core::panic::unwind_safe::AssertUnwindSafe<std::thread::{impl#0}::spawn_unchecked_::{closure#1}::{closure_env#0}<test::run_test::{closure_env#1}, ()>>, ()> () at library/std/src/panic.rs:359
 #62 std::thread::{impl#0}::spawn_unchecked_::{closure#1}<test::run_test::{closure_env#1}, ()> () at library/std/src/thread/mod.rs:557
 #63 core::ops::function::FnOnce::call_once<std::thread::{impl#0}::spawn_unchecked_::{closure_env#1}<test::run_test::{closure_env#1}, ()>, ()> () at library/core/src/ops/function.rs:250
 #64 0x000055555705088f in alloc::boxed::{impl#28}::call_once<(), dyn core::ops::function::FnOnce<(), Output=()>, alloc::alloc::Global> () at library/alloc/src/boxed.rs:1966
 #65 std::sys::pal::unix::thread::{impl#2}::new::thread_start () at library/std/src/sys/pal/unix/thread.rs:107
 #66 0x00007ffff7d127eb in ?? () from /usr/lib/libc.so.6
 #67 0x00007ffff7d9618c in ?? () from /usr/lib/libc.so.6
```


---

_Review requested from @ibraheemdev by @BurntSushi on 2025-08-20 14:47_

---

_Comment by @github-actions[bot] on 2025-08-20 14:49_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-20 14:51_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Label `do-not-merge` added by @AlexWaygood on 2025-08-20 14:52_

---

_Label `ty` added by @AlexWaygood on 2025-08-20 14:52_

---

_Comment by @MichaReiser on 2025-08-20 16:18_

@ibraheemdev narrowed this down and filled a salsa issue:  https://github.com/salsa-rs/salsa/issues/985 I'll close this as there's nothing to do on the ty side (other than not using references in interned values)

---

_Closed by @MichaReiser on 2025-08-20 16:18_

---
