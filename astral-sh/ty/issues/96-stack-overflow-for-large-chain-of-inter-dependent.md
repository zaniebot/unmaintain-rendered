```yaml
number: 96
title: Stack overflow for large chain of inter-dependent definitions
type: issue
state: open
author: sharkdp
labels:
  - bug
  - fatal
assignees: []
created_at: 2025-05-05T14:22:57Z
updated_at: 2025-10-17T08:17:47Z
url: https://github.com/astral-sh/ty/issues/96
synced_at: 2026-01-10T02:06:24Z
```

# Stack overflow for large chain of inter-dependent definitions

---

_Issue opened by @sharkdp on 2025-05-05 14:22_

EDIT to summarize discussion below: the issue is related to large chains of dependent bindings, which (if analyzed lazily from last to first) are inferred recursively, potentially leading to large stack usage, especially in debug builds. We increased our max stack size and can now handle up to around 2500 such chained definitions in a release build (~500 in a debug build.) Low priority to do more here unless we see this as an issue in real-world codebases.

The full stack trace is attached, but is not very informative. The stack overflow is not deterministic. This might be related to some huge files like [manticore/tests/native/test_x86.py](https://github.com/trailofbits/manticore/blob/master/tests/native/test_x86.py) (120,000 lines).

[backtrace_manticore.txt](https://github.com/user-attachments/files/20038462/backtrace_manticore.txt)



---

_Label `bug` added by @sharkdp on 2025-05-05 14:22_

---

_Assigned to @sharkdp by @sharkdp on 2025-05-05 14:23_

---

_Comment by @sharkdp on 2025-05-06 14:49_

Just noting down some results while continuing to work on this.

This is hard to debug because (a) it is non-deterministic and (b) it changes behavior if you try to run it via `gdb` or if you enable tracing, and it also changes behavior between debug and release:
* running normally in debug mode: occasional stack overflow
* running the debug version in gdb: segfault
* running the debug version with tracing: almost always leads to a hang
* running the release version: seems to work fine

I do not fully understand what's going on. It seems to have something to do with [`cgcrandom.py`](https://github.com/trailofbits/manticore/blob/master/manticore/platforms/cgcrandom.py), which looks like it *might* cause problems, but is actually seemingly trivial to [check in isolation](https://play.ty.dev/27338443-16b8-4d67-8db6-565b896cfdf9) (side remark: we should consider setting a max length for bytes literals). Even if I generate a similar file of larger size (number of assignments or length of individual bytes literals), we manage to check the resulting file without problems.

---

_Comment by @MichaReiser on 2025-05-06 15:21_

> running the debug version with tracing: almost always leads to a hang 

Are you using the tree subscriber or the "default" one? If it's the tree, try using the normal one.

> running the release version: seems to work fine

Makes sort of sense because a lot more code gets inlined

---

_Comment by @sharkdp on 2025-05-06 21:32_

> > running the debug version with tracing: almost always leads to a hang
> 
> Are you using the tree subscriber or the "default" one? If it's the tree, try using the normal one.

Yes, that helps. I can also manually `eprintln!(…)` to try to find out where it crashes.

---

_Comment by @sharkdp on 2025-05-06 21:42_

My summary from today is... this looks scary. Reminds me of debugging C++ programs. It does not reproduce single-threaded. The stack traces that I managed to get so far (via gdb and via address-sanitizer-instrumented builds on nightly) always look slightly different. I've seen at least ~5 different variations. It happens for a relatively large call stack, but when I analyze the stack usage, it appears to be much smaller than what is available (and it does not seem to be sensitive to the max. size of the stack). I'm really not sure but it almost looks like some kind of stack corruption.

one of the gdb backtraces: it fails in the middle of nowhere during a seemingly harmless `definition.file(db)` call.

```
#0  0x0000555556d49a63 in core::num::{impl#11}::overflowing_mul (self=8, rhs=4)
    at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/num/uint_macros.rs:2580
astral-sh/ruff#1  core::num::{impl#11}::checked_mul (self=8, rhs=4)
    at /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/num/uint_macros.rs:871
astral-sh/ruff#2  0x0000555556d203da in hashbrown::raw::TableLayout::calculate_layout_for (self=..., buckets=4)
    at /home/shark/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/hashbrown-0.15.3/src/raw/mod.rs:205
astral-sh/ruff#3  0x0000555556d22b1c in hashbrown::raw::RawTableInner::new_uninitialized<allocator_api2::stable::alloc::global::Global> (
    alloc=0x7fffc4947a80, table_layout=..., buckets=4, fallibility=hashbrown::raw::Fallibility::Infallible)
    at /home/shark/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/hashbrown-0.15.3/src/raw/mod.rs:1471
astral-sh/ruff#4  0x0000555556d23070 in hashbrown::raw::RawTableInner::fallible_with_capacity<allocator_api2::stable::alloc::global::Global>
    (alloc=0x7fffc4947a80, table_layout=..., capacity=1, fallibility=hashbrown::raw::Fallibility::Infallible)
    at /home/shark/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/hashbrown-0.15.3/src/raw/mod.rs:1515
astral-sh/ruff#5  0x0000555556d2145f in hashbrown::raw::RawTableInner::prepare_resize<allocator_api2::stable::alloc::global::Global> (
    self=0x7fffc4947a60, alloc=0x7fffc4947a80, table_layout=..., capacity=1, 
    fallibility=hashbrown::raw::Fallibility::Infallible)
    at /home/shark/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/hashbrown-0.15.3/src/raw/mod.rs:2584
astral-sh/ruff#6  0x0000555556d29958 in hashbrown::raw::RawTableInner::resize_inner<allocator_api2::stable::alloc::global::Global> (
    self=0x7fffc4947a60, alloc=0x7fffc4947a80, capacity=1, hasher=..., fallibility=hashbrown::raw::Fallibility::Infallible, 
    layout=...) at /home/shark/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/hashbrown-0.15.3/src/raw/mod.rs:2782
astral-sh/ruff#7  hashbrown::raw::RawTableInner::reserve_rehash_inner<allocator_api2::stable::alloc::global::Global> (self=0x7fffc4947a60, 
    alloc=0x7fffc4947a80, additional=1, hasher=..., fallibility=hashbrown::raw::Fallibility::Infallible, layout=..., drop=...)
    at /home/shark/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/hashbrown-0.15.3/src/raw/mod.rs:2670
astral-sh/ruff#8  hashbrown::raw::RawTable<usize, allocator_api2::stable::alloc::global::Global>::reserve_rehash<usize, allocator_api2::stable::alloc::global::Global, indexmap::map::core::get_hash::{closure_env#0}<salsa::zalsa_local::QueryEdge, ()>> (
    self=0x7fffc4947a60, additional=1, hasher=..., fallibility=hashbrown::raw::Fallibility::Infallible)
    at /home/shark/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/hashbrown-0.15.3/src/raw/mod.rs:991
astral-sh/ruff#9  0x0000555556d36d0d in hashbrown::raw::RawTable<usize, allocator_api2::stable::alloc::global::Global>::reserve<usize, allocator_api2::stable::alloc::global::Global, indexmap::map::core::get_hash::{closure_env#0}<salsa::zalsa_local::QueryEdge, ()>> (
    self=0x7fffc4947a60, additional=1, hasher=...)
    at /home/shark/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/hashbrown-0.15.3/src/raw/mod.rs:939
astral-sh/ruff#10 0x0000555556d31b7b in hashbrown::raw::RawTable<usize, allocator_api2::stable::alloc::global::Global>::find_or_find_insert_slot<usize, allocator_api2::stable::alloc::global::Global, indexmap::map::core::equivalent::{closure_env#0}<salsa::zalsa_local::QueryEdge, (), salsa::zalsa_local::QueryEdge>, indexmap::map::core::get_hash::{closure_env#0}<salsa::zalsa_local::QueryEdge, ()>> (self=0x7fffc4947a60, hash=10971646976834494491, eq=..., hasher=...)
    at /home/shark/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/hashbrown-0.15.3/src/raw/mod.rs:1146
astral-sh/ruff#11 0x0000555556d3ed53 in hashbrown::table::HashTable<usize, allocator_api2::stable::alloc::global::Global>::entry<usize, allocator_api2::stable::alloc::global::Global, indexmap::map::core::equivalent::{closure_env#0}<salsa::zalsa_local::QueryEdge, (), salsa::zalsa_local::QueryEdge>, indexmap::map::core::get_hash::{closure_env#0}<salsa::zalsa_local::QueryEdge, ()>> (
    self=0x7fffc4947a60, hash=10971646976834494491, eq=<error reading variable: Cannot access memory at address 0x0>, 
    hasher=...) at /home/shark/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/hashbrown-0.15.3/src/table.rs:365
astral-sh/ruff#12 0x0000555556d3aa1c in indexmap::map::core::IndexMapCore<salsa::zalsa_local::QueryEdge, ()>::insert_full<salsa::zalsa_local::QueryEdge, ()> (self=0x7fffc4947a48, hash=..., key=..., value=())
    at /home/shark/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/indexmap-2.9.0/src/map/core.rs:344
astral-sh/ruff#13 0x0000555556d5653b in indexmap::map::IndexMap<salsa::zalsa_local::QueryEdge, (), core::hash::BuildHasherDefault<rustc_hash:--Type <RET> for more, q to quit, c to continue without paging--
:FxHasher>>::insert_full<salsa::zalsa_local::QueryEdge, (), core::hash::BuildHasherDefault<rustc_hash::FxHasher>> (
    self=0x7fffc4947a48, key=..., value=())
    at /home/shark/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/indexmap-2.9.0/src/map.rs:418
astral-sh/ruff#14 0x0000555556d5670f in indexmap::map::IndexMap<salsa::zalsa_local::QueryEdge, (), core::hash::BuildHasherDefault<rustc_hash::FxHasher>>::insert<salsa::zalsa_local::QueryEdge, (), core::hash::BuildHasherDefault<rustc_hash::FxHasher>> (
    self=0x7fffc4947a48, key=<error reading variable: Cannot access memory at address 0x4>, value=())
    at /home/shark/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/indexmap-2.9.0/src/map.rs:401
astral-sh/ruff#15 0x0000555556d44d8f in indexmap::set::IndexSet<salsa::zalsa_local::QueryEdge, core::hash::BuildHasherDefault<rustc_hash::FxHasher>>::insert<salsa::zalsa_local::QueryEdge, core::hash::BuildHasherDefault<rustc_hash::FxHasher>> (self=0x7fffc4947a48, 
    value=<error reading variable: Cannot access memory at address 0x4>)
    at /home/shark/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/indexmap-2.9.0/src/set.rs:345
astral-sh/ruff#16 0x0000555556d45e02 in salsa::active_query::ActiveQuery::add_read_simple (self=0x7fffc4947a48, input=..., durability=..., 
    revision=...) at src/active_query.rs:110
astral-sh/ruff#17 0x000055555669fbe9 in salsa::zalsa_local::{impl#0}::report_tracked_read_simple::{closure#0} (stack=0x7ffff7b816c0)
    at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b2b82bc/src/zalsa_local.rs:230
astral-sh/ruff#18 0x00005555564811c2 in salsa::zalsa_local::ZalsaLocal::with_query_stack_mut<(), salsa::zalsa_local::{impl#0}::report_tracked_read_simple::{closure_env#0}> (self=0x7ffff7b816b8, c=...)
    at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b2b82bc/src/zalsa_local.rs:119
astral-sh/ruff#19 salsa::zalsa_local::ZalsaLocal::report_tracked_read_simple (self=0x7ffff7b816b8, input=..., durability=..., 
    changed_at=...) at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b2b82bc/src/zalsa_local.rs:228
astral-sh/ruff#20 salsa::tracked_struct::IngredientImpl<ty_python_semantic::semantic_index::definition::Definition>::untracked_field<ty_python_semantic::semantic_index::definition::Definition> (self=0x7ffff006a200, db=..., s=...)
    at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b2b82bc/src/tracked_struct.rs:706
astral-sh/ruff#21 0x0000555555ebe1fa in ty_python_semantic::semantic_index::definition::Definition::file<dyn ty_python_semantic::db::Db> (
    self=..., db=...)
    at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b2b82bc/components/salsa-macro-rules/src/setup_tracked_struct.rs:278
astral-sh/ruff#22 0x0000555555f69354 in ty_python_semantic::types::infer::infer_definition_types::{impl#1}::execute::inner_ (db=..., 
    definition=...) at crates/ty_python_semantic/src/types/infer.rs:151
astral-sh/ruff#23 0x0000555555f692ef in ty_python_semantic::types::infer::infer_definition_types::{impl#1}::execute (db=..., definition=...)
    at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b2b82bc/components/salsa-macro-rules/src/setup_tracked_fn.rs:207
astral-sh/ruff#24 0x00005555561be344 in salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_definition_types::Configuration_>::execute_query<ty_python_semantic::types::infer::infer_definition_types::Configuration_> (db=..., active_query=..., 
    opt_old_memo=..., current_revision=..., id=...)
    at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b2b82bc/src/function/execute.rs:273
astral-sh/ruff#25 0x0000555556239d1e in salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_definition_types::Configuration_>::execute_maybe_iterate<ty_python_semantic::types::infer::infer_definition_types::Configuration_> (self=0x7ffff00a6ad0, 
    db=..., active_query=..., opt_old_memo=..., zalsa=0x55555794e200, id=..., memo_ingredient_index=...)
    at /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b2b82bc/src/function/execute.rs:138
astral-sh/ruff#26 0x00005555562675b5 in salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_definition_types::Configuration_>::execute<ty_python_semantic::types::infer::infer_definition_types::Configuration_> (self=0x7ffff00a6ad0, db=..., 
    active_query=..., opt_old_memo=...)
[…]
```

one of the address sanitizer traces. `infer_definition_types` also appears in the backtrace...

```
AddressSanitizer:DEADLYSIGNAL
=================================================================
==986280==ERROR: AddressSanitizer: stack-overflow on address 0x7756b25f6fd8 (pc 0x5b585a10ca99 bp 0x7756b25f70b0 sp 0x7756b25f6fe0 T2)
    #0 0x5b585a10ca99 in core::hash::BuildHasher::hash_one::h878f537b4ab41e38 /home/shark/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/hash/mod.rs:694
    astral-sh/ruff#1 0x5b585a0f1533 in hashbrown::map::make_hash::hf992b4f885fb148b /home/shark/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/hashbrown-0.15.3/src/map.rs:258:5
    astral-sh/ruff#2 0x5b585948331a in hashbrown::rustc_entry::_$LT$impl$u20$hashbrown..map..HashMap$LT$K$C$V$C$S$C$A$GT$$GT$::rustc_entry::h19426e7e635cc962 /home/shark/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/hashbrown-0.15.3/src/rustc_entry.rs:35:20
    astral-sh/ruff#3 0x5b585935e3c1 in std::collections::hash::map::HashMap$LT$K$C$V$C$S$GT$::entry::haaa62f43948a4ad8 /home/shark/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/collections/hash/map.rs:884:19
    astral-sh/ruff#4 0x5b58592af257 in salsa::function::sync::SyncTable::try_claim::hcc5f294b6ad27792 /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b2b82bc/src/function/sync.rs:49:15
    astral-sh/ruff#5 0x5b5858f336fc in salsa::function::fetch::_$LT$impl$u20$salsa..function..IngredientImpl$LT$C$GT$$GT$::fetch_cold::h8515575fd8f4730c /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b2b82bc/src/function/fetch.rs:100:34
    astral-sh/ruff#6 0x5b5858713bc0 in salsa::function::fetch::_$LT$impl$u20$salsa..function..IngredientImpl$LT$C$GT$$GT$::refresh_memo::_$u7b$$u7b$closure$u7d$$u7d$::h3dbd0030dcd791fd /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b2b82bc/src/function/fetch.rs:46:29
    astral-sh/ruff#7 0x5b58583ffc13 in core::option::Option$LT$T$GT$::or_else::h76ec5b4b78a84c6c /home/shark/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/option.rs:1577:21
    astral-sh/ruff#8 0x5b5858f739ef in salsa::function::fetch::_$LT$impl$u20$salsa..function..IngredientImpl$LT$C$GT$$GT$::refresh_memo::hd458a655a8d51c8f /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b2b82bc/src/function/fetch.rs:44:33
    astral-sh/ruff#9 0x5b5858f739ef in salsa::function::fetch::_$LT$impl$u20$salsa..function..IngredientImpl$LT$C$GT$$GT$::fetch::hc8d76e96d6a29ee0 /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b2b82bc/src/function/fetch.rs:17:20
    astral-sh/ruff#10 0x5b58584c6efd in ty_python_semantic::types::infer::infer_definition_types::_$u7b$$u7b$closure$u7d$$u7d$::hbdbaec1c5e70037a /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b2b82bc/components/salsa-macro-rules/src/setup_tracked_fn.rs:365:25
    astral-sh/ruff#11 0x5b58584dc684 in salsa::attach::Attached::attach::h8c019be707a3a6c7 /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b2b82bc/src/attach.rs:73:9
    astral-sh/ruff#12 0x5b58584d1dc0 in salsa::attach::attach::_$u7b$$u7b$closure$u7d$$u7d$::h4b65bff936bc3f22 /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b2b82bc/src/attach.rs:97:13
    astral-sh/ruff#13 0x5b58587dc5db in std::thread::local::LocalKey$LT$T$GT$::try_with::h496c374c7ad77d76 /home/shark/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/thread/local.rs:315:12
    astral-sh/ruff#14 0x5b58587d9f1e in std::thread::local::LocalKey$LT$T$GT$::with::hf6f5e645eb47b135 /home/shark/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/thread/local.rs:279:15
    astral-sh/ruff#15 0x5b58584ce3a2 in salsa::attach::attach::h84e0e1fe8e364894 /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b2b82bc/src/attach.rs:95:5
    astral-sh/ruff#16 0x5b5858899527 in ty_python_semantic::types::infer::infer_definition_types::h9e6065a300a07cbe /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b2b82bc/components/salsa-macro-rules/src/setup_tracked_fn.rs:357:13
    astral-sh/ruff#17 0x5b585890da13 in ty_python_semantic::types::binding_type::hf1d2367d2ac8bacc /home/shark/ruff/crates/ty_python_semantic/src/types.rs:111:21
    astral-sh/ruff#18 0x5b5858d30a1f in ty_python_semantic::symbol::symbol_from_bindings_impl::_$u7b$$u7b$closure$u7d$$u7d$::h3851ec34c6b1bb59 /home/shark/ruff/crates/ty_python_semantic/src/symbol.rs:794:30
    astral-sh/ruff#19 0x5b5858d26f76 in core::ops::function::impls::_$LT$impl$u20$core..ops..function..FnMut$LT$A$GT$$u20$for$u20$$RF$mut$u20$F$GT$::call_mut::hcd864688dfc31394 /home/shark/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/ops/function.rs:294:13
    astral-sh/ruff#20 0x5b5858422de7 in core::iter::traits::iterator::Iterator::find_map::check::_$u7b$$u7b$closure$u7d$$u7d$::h933b0ab2c8961819 /home/shark/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/iter/traits/iterator.rs:2891:32
    astral-sh/ruff#21 0x5b585842f9cf in _$LT$core..iter..adapters..peekable..Peekable$LT$I$GT$$u20$as$u20$core..iter..traits..iterator..Iterator$GT$::try_fold::h1d2b78ae075ba411 /home/shark/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/iter/adapters/peekable.rs:100:30
    astral-sh/ruff#22 0x5b585843013b in core::iter::traits::iterator::Iterator::find_map::hb19302c36f0f9c67 /home/shark/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/iter/traits/iterator.rs:2897:9
    astral-sh/ruff#23 0x5b585870120d in _$LT$core..iter..adapters..filter_map..FilterMap$LT$I$C$F$GT$$u20$as$u20$core..iter..traits..iterator..Iterator$GT$::next::h1adcf5a28774113c /home/shark/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/iter/adapters/filter_map.rs:64:9
    astral-sh/ruff#24 0x5b5858af4221 in ty_python_semantic::symbol::symbol_from_bindings_impl::h42f1bc04bd174cec /home/shark/ruff/crates/ty_python_semantic/src/symbol.rs:799:26
    astral-sh/ruff#25 0x5b5858af18e9 in ty_python_semantic::symbol::symbol_from_bindings::ha1179998af229b43 /home/shark/ruff/crates/ty_python_semantic/src/symbol.rs:392:5
    astral-sh/ruff#26 0x5b585886f8ff in ty_python_semantic::types::infer::TypeInferenceBuilder::infer_name_load::h1a82ea7f6ae1af1b /home/shark/ruff/crates/ty_python_semantic/src/types/infer.rs:5187:26
    astral-sh/ruff#27 0x5b58588574bd in ty_python_semantic::types::infer::TypeInferenceBuilder::infer_augment_assignment::ha69889538ca3b90c /home/shark/ruff/crates/ty_python_semantic/src/types/infer.rs:3498:38
    astral-sh/ruff#28 0x5b58588570c6 in ty_python_semantic::types::infer::TypeInferenceBuilder::infer_augment_assignment_definition::h139ca799dbc10173 /home/shark/ruff/crates/ty_python_semantic/src/types/infer.rs:3483:25
    astral-sh/ruff#29 0x5b5858840209 in ty_python_semantic::types::infer::TypeInferenceBuilder::infer_region_definition::h615d0f927f83f861 /home/shark/ruff/crates/ty_python_semantic/src/types/infer.rs:1263:17
    astral-sh/ruff#30 0x5b5858837b60 in ty_python_semantic::types::infer::TypeInferenceBuilder::infer_region::h10168ce35cadf64b /home/shark/ruff/crates/ty_python_semantic/src/types/infer.rs:699:56
    astral-sh/ruff#31 0x5b58588872d7 in ty_python_semantic::types::infer::TypeInferenceBuilder::finish::h708948440d02d3e6 /home/shark/ruff/crates/ty_python_semantic/src/types/infer.rs:7187:9
    astral-sh/ruff#32 0x5b585889b234 in _$LT$ty_python_semantic..types..infer..infer_definition_types..Configuration_$u20$as$u20$salsa..function..Configuration$GT$::execute::inner_::hae5e0031e58fcfbf /home/shark/ruff/crates/ty_python_semantic/src/types/infer.rs:165:5
    astral-sh/ruff#33 0x5b5858899bae in _$LT$ty_python_semantic..types..infer..infer_definition_types..Configuration_$u20$as$u20$salsa..function..Configuration$GT$::execute::hc355e3ee8d01251b /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b2b82bc/components/salsa-macro-rules/src/setup_tracked_fn.rs:207:21
    astral-sh/ruff#34 0x5b5858f90a74 in salsa::function::execute::_$LT$impl$u20$salsa..function..IngredientImpl$LT$C$GT$$GT$::execute_query::haa0e60d725061bfe /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b2b82bc/src/function/execute.rs:273:25
    astral-sh/ruff#35 0x5b5858fe6d38 in salsa::function::execute::_$LT$impl$u20$salsa..function..IngredientImpl$LT$C$GT$$GT$::execute_maybe_iterate::hc540fdeca158cdd0 /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b2b82bc/src/function/execute.rs:138:50
    astral-sh/ruff#36 0x5b585901113b in salsa::function::execute::_$LT$impl$u20$salsa..function..IngredientImpl$LT$C$GT$$GT$::execute::h4dda132e9f04f7af /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b2b82bc/src/function/execute.rs:89:48
    astral-sh/ruff#37 0x5b5858f34df5 in salsa::function::fetch::_$LT$impl$u20$salsa..function..IngredientImpl$LT$C$GT$$GT$::fetch_cold::h8515575fd8f4730c /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b2b82bc/src/function/fetch.rs:192:20
    astral-sh/ruff#38 0x5b5858713bc0 in salsa::function::fetch::_$LT$impl$u20$salsa..function..IngredientImpl$LT$C$GT$$GT$::refresh_memo::_$u7b$$u7b$closure$u7d$$u7d$::h3dbd0030dcd791fd /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b2b82bc/src/function/fetch.rs:46:29
    astral-sh/ruff#39 0x5b58583ffc13 in core::option::Option$LT$T$GT$::or_else::h76ec5b4b78a84c6c /home/shark/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/option.rs:1577:21
    astral-sh/ruff#40 0x5b5858f739ef in salsa::function::fetch::_$LT$impl$u20$salsa..function..IngredientImpl$LT$C$GT$$GT$::refresh_memo::hd458a655a8d51c8f /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b2b82bc/src/function/fetch.rs:44:33
    astral-sh/ruff#41 0x5b5858f739ef in salsa::function::fetch::_$LT$impl$u20$salsa..function..IngredientImpl$LT$C$GT$$GT$::fetch::hc8d76e96d6a29ee0 /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b2b82bc/src/function/fetch.rs:17:20
    astral-sh/ruff#42 0x5b58584c6efd in ty_python_semantic::types::infer::infer_definition_types::_$u7b$$u7b$closure$u7d$$u7d$::hbdbaec1c5e70037a /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b2b82bc/components/salsa-macro-rules/src/setup_tracked_fn.rs:365:25
    astral-sh/ruff#43 0x5b58584dc684 in salsa::attach::Attached::attach::h8c019be707a3a6c7 /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b2b82bc/src/attach.rs:73:9
    astral-sh/ruff#44 0x5b58584d1dc0 in salsa::attach::attach::_$u7b$$u7b$closure$u7d$$u7d$::h4b65bff936bc3f22 /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b2b82bc/src/attach.rs:97:13
    astral-sh/ruff#45 0x5b58587dc5db in std::thread::local::LocalKey$LT$T$GT$::try_with::h496c374c7ad77d76 /home/shark/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/thread/local.rs:315:12
    astral-sh/ruff#46 0x5b58587d9f1e in std::thread::local::LocalKey$LT$T$GT$::with::hf6f5e645eb47b135 /home/shark/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/thread/local.rs:279:15
    astral-sh/ruff#47 0x5b58584ce3a2 in salsa::attach::attach::h84e0e1fe8e364894 /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b2b82bc/src/attach.rs:95:5
    astral-sh/ruff#48 0x5b5858899527 in ty_python_semantic::types::infer::infer_definition_types::h9e6065a300a07cbe /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/b2b82bc/components/salsa-macro-rules/src/setup_tracked_fn.rs:357:13
    astral-sh/ruff#49 0x5b585890da13 in ty_python_semantic::types::binding_type::hf1d2367d2ac8bacc /home/shark/ruff/crates/ty_python_semantic/src/types.rs:111:21
    astral-sh/ruff#50 0x5b5858d30a1f in ty_python_semantic::symbol::symbol_from_bindings_impl::_$u7b$$u7b$closure$u7d$$u7d$::h3851ec34c6b1bb59 /home/shark/ruff/crates/ty_python_semantic/src/symbol.rs:794:30
    astral-sh/ruff#51 0x5b5858d26f76 in core::ops::function::impls::_$LT$impl$u20$core..ops..function..FnMut$LT$A$GT$$u20$for$u20$$RF$mut$u20$F$GT$::call_mut::hcd864688dfc31394 /home/shark/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/ops/function.rs:294:13
    astral-sh/ruff#52 0x5b5858422de7 in core::iter::traits::iterator::Iterator::find_map::check::_$u7b$$u7b$closure$u7d$$u7d$::h933b0ab2c8961819 /home/shark/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/iter/traits/iterator.rs:2891:32
    astral-sh/ruff#53 0x5b585842f9cf in _$LT$core..iter..adapters..peekable..Peekable$LT$I$GT$$u20$as$u20$core..iter..traits..iterator..Iterator$GT$::try_fold::h1d2b78ae075ba411 /home/shark/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/iter/adapters/peekable.rs:100:30
    astral-sh/ruff#54 0x5b585843013b in core::iter::traits::iterator::Iterator::find_map::hb19302c36f0f9c67 /home/shark/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/iter/traits/iterator.rs:2897:9
    astral-sh/ruff#55 0x5b585870120d in _$LT$core..iter..adapters..filter_map..FilterMap$LT$I$C$F$GT$$u20$as$u20$core..iter..traits..iterator..Iterator$GT$::next::h1adcf5a28774113c /home/shark/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/iter/adapters/filter_map.rs:64:9
    astral-sh/ruff#56 0x5b5858af4221 in ty_python_semantic::symbol::symbol_from_bindings_impl::h42f1bc04bd174cec /home/shark/ruff/crates/ty_python_semantic/src/symbol.rs:799:26
    astral-sh/ruff#57 0x5b5858af18e9 in ty_python_semantic::symbol::symbol_from_bindings::ha1179998af229b43 /home/shark/ruff/crates/ty_python_semantic/src/symbol.rs:392:5
    astral-sh/ruff#58 0x5b585886f8ff in ty_python_semantic::types::infer::TypeInferenceBuilder::infer_name_load::h1a82ea7f6ae1af1b /home/shark/ruff/crates/ty_python_semantic/src/types/infer.rs:5187:26
    astral-sh/ruff#59 0x5b58588574bd in ty_python_semantic::types::infer::TypeInferenceBuilder::infer_augment_assignment::ha69889538ca3b90c /home/shark/ruff/crates/ty_python_semantic/src/types/infer.rs:3498:38
    astral-sh/ruff#60 0x5b58588570c6 in ty_python_semantic::types::infer::TypeInferenceBuilder::infer_augment_assignment_definition::h139ca799dbc10173 /home/shark/ruff/crates/ty_python_semantic/src/types/infer.rs:3483:25
    astral-sh/ruff#61 0x5b5858840209 in ty_python_semantic::types::infer::TypeInferenceBuilder::infer_region_definition::h615d0f927f83f861 /home/shark/ruff/crates/ty_python_semantic/src/types/infer.rs:1263:17
    astral-sh/ruff#62 0x5b5858837b60 in ty_python_semantic::types::infer::TypeInferenceBuilder::infer_region::h10168ce35cadf64b /home/shark/ruff/crates/ty_python_semantic/src/types/infer.rs:699:56
    astral-sh/ruff#63 0x5b58588872d7 in ty_python_semantic::types::infer::TypeInferenceBuilder::finish::h708948440d02d3e6 /home/shark/ruff/crates/ty_python_semantic/src/types/infer.rs:7187:9
[…]
```

From what I can tell from my attempts at using some form of tracing, it appears to (usually?) crash while checking [this definition](https://github.com/trailofbits/manticore/blob/8861005396ed3e25ecef9cd229e5319ae2fe2612/manticore/platforms/cgcrandom.py#L117). But as noted above, that file can usually be checked fine (in isolation or with the rest of the project).

---

_Comment by @MichaReiser on 2025-05-07 06:29_

Hmm, this looks hairy. Do you think it would be valuable to try running miri on ty (e.g. on tests) to see if it reveals any unsoundness in ty itself or salsa (which contains most unsafe code). 

This also makes me think that this issue might be related to the other salsa issues that @ibraheemdev is investigating.

---

_Comment by @sharkdp on 2025-05-07 10:47_

Ok, I finally know what's going on. Posting here now just to prevent anyone else from looking into this. This is not related to salsa. And it's not that scary after all. Will post a full analysis later.

---

_Comment by @sharkdp on 2025-05-07 12:59_

I spent some more time looking into this, and now I'm fairly certain that I understand what is going on.

> it does not seem to be sensitive to the max. size of the stack

This is not true. I may have confused myself by not running often enough to get a meaningful sample size. Even at very small stack sizes, a crash is not guaranteed. It's even unlikely if the number of threads is high.

> It does not reproduce single-threaded.

This is *also not true*. Depending on some preconditions (more below) it can either be reproduced with a probability of 0% (that's what happened for me yesterday), or with 100% (today).

But let's start at the beginning. I did a much larger analysis than this, but the most interesting experiment is the following. I ran (the debug version of) `ty` for varying worker thread stack sizes and thread counts (`TY_MAX_PARALLELISM`) on manticore. `x` indicates a crash with a stack overflow, `_` indicates a clean run:

```
num_threads = 1

4 MiB xxxxxxxxxx crashed 10/10 times
8 MiB _________  crashed  0/10 times

num_threads = 2

4 MiB x_xxx_xx__ crashed  6/10 times
8 MiB __________ crashed  0/10 times

num_threads = 16

2 MiB ____x_____ crashed  1/10 times
8 MiB __________ crashed  0/10 times
```

As we can see, in this particular setting, *no* stack overflows occur for a stack size of 8 MiB (or larger; this means that the particular problem on `manticore` is actually not present on `main` after https://github.com/astral-sh/ruff/pull/17869, but the general problem still persists). We can also see that stack overflows happen with 100% probability on a single thread, with ~50% probability for two threads, and with low probability for 16 threads.

The other crucial thing was to see what kind of impact changes to the codebase of `manticore` had. Almost everything I changed had *some* kind of effect, but the most important facts were:

* Stack overflows disappear completely if [`cgcrandom.py`](https://github.com/trailofbits/manticore/blob/8861005396ed3e25ecef9cd229e5319ae2fe2612/manticore/platforms/cgcrandom.py) is removed.
* Stack overflows also disappear completely if [`decree.py`](https://github.com/trailofbits/manticore/blob/8861005396ed3e25ecef9cd229e5319ae2fe2612/manticore/platforms/decree.py#L1) is removed — the only module which imports `cgcrandom`.

So it turns out that both of these files need to be present. It then depends on the order in which they are checked whether the stack overflow occurs or not. If `cgcrandom.py` is checked first, everything works fine. If `decree.py` is checked first, the stack overflow occurs. When running multi-threaded, it's a race. This also explains why I wasn't able to reproduce on a single thread yesterday. It depends on the filesystem order. After restarting my system, I got a new `mypy_primer` checkout of `manticore` today in `/tmp`, and this one apparently sorts `cgcrandom.py` first. I'm not entirely sure why crashes are much less frequent for high thread counts, but it seems to make sense to me that it would be roughly 50:50 for just two threads.

Knowing this, I was able to reproduce the bug in a much cleaner setting. And by increasing the number of definitions, it's easy to also reproduce the stack overflow on `main` (and in release mode). The setting is the following:

`mod.py`
```py
n = 0
n += 1
n += 1
n += 1
n += 1
# <repeat 4000 times>
```

`main.py`
```py
from mod import n
```

When running `ty` single threaded, we can control the order in which the files are checked. And in fact, everything works fine if we do:

```
▶ TY_MAX_PARALLELISM=1 ty check mod.py main.py
All checks passed!
```

But if we reverse the order, we can consistently reproduce the crash:

```
▶ TY_MAX_PARALLELISM=1 ty check main.py mod.py

thread '<unknown>' has overflowed its stack
fatal runtime error: stack overflow
```

So what is going on? If we check `mod.py` first, we check the file *top to bottom*. This means that we check one statement
after the other. When checking a particular `n += 1` definition, we need to know the type of `n` first. It depends on the
previous definition, *but we have already analyzed it*. So the corresponding query that resolves the type immediately returns
the cached value. However, when we check `main.py` first, we resolve the public type of `n`. The public use of `n` depends on one visible binding: the very last of the `n += 1` definitions. In order to resolve the type of `n` in that binding, we need to resolve the second-to-last binding *in a recursive call*. There are no previously cached values this time. By going *bottom to top* like this, we build up a huge call stack of 4000 `infer_definition_types` queries (with around 25 other functions in between two `infer_defition_types` calls, resulting in ~ 100,000 call frames). This then results in a stack overflow.

What can we do about this? I'm not sure we currently *need* to do anything. Everything is working as expected here. The way
we process `mod.py` bottom-to-top when importing `n` is just our normal lazy approach to looking up symbols. In release mode, and with the newly increased stack size, we can handle ~2500 dependent definitions. It would obviously be great to find a way to attack the underlying growing-stack problem here, but I don't think it's urgent.

Where did I go wrong yesterday? I was completely mislead by the fact that I could not reproduce this on a single thread. The other thing that confused me was the segfaults, that turned out to be just a symptom of the stack overflows. Attaching a debugger appears to interfere with (or bypass) Rust's stack guards, which normally trigger the "has overflowed its stack" message. No single stack frame is excessively large, which is why we saw these segfaults in random places.

---

While we can handle 2500 definitions in release mode, we can currently only handle ~500 definitions in debug mode (and previously only ~100, which resulted in the stack overflows on `cgcrandom.py`). While I don't think it's worth spending much time on this, I used an [old gdb plugin](https://github.com/sharkdp/stack-inspector) of mine to analyze the stack, just to see where stack space is being used. The following is an extract of the analysis between two consecutive `infer_definition_types` calls. Everything that is shown here is repeated for every definition in `mod.py`. The biggest stack allocations come from `Zalsa` [here](https://github.com/salsa-rs/salsa/blob/b2b82bccdbef3e7ce7f302c52f43a0c98ac7177a/src/function/fetch.rs#L46) (I don't understand why this is not just a 8 byte reference?), and from `IngredientImpl<infer::infer_definition_types::Configuration_>`, a structure set up by a salsa macro for the `infer_definition_types` query. The latter would probably be much smaller with the changes in [this PR](https://github.com/astral-sh/ruff/pull/17894)? `TypeInferenceBuilder` is also pretty large [here](https://github.com/astral-sh/ruff/blob/0d9b6a097574e49f40f31758b1b8af420da458c2/crates/ty_python_semantic/src/types/infer.rs#L7233-L7238). I'm not sure if this analysis has any relevance for release builds, where most of this is probably optimized away.

<details>

<summary>stack analysis</summary>

```
  astral-sh/ruff#16928 ty_python_semantic::types::infer::infer_definition_types::{impl#1}::execute::inner_ @ crates/ty_python_semantic/src/types/infer.rs:162

                40   _span (tracing::span::EnteredSpan)
                16   db (&dyn ty_python_semantic::db::Db)
                 8   index (*mut ty_python_semantic::semantic_index::SemanticIndex)
                 4   file (ruff_db::files::File)
                 4   definition (ty_python_semantic::semantic_index::definition::Definition)

  astral-sh/ruff#16929 ty_python_semantic::types::infer::infer_definition_types::{impl#1}::execute @ /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/components/salsa-macro-rules/src/setup_tracked_fn.rs:207

                16   db (&dyn ty_python_semantic::db::Db)
                 4   definition (ty_python_semantic::semantic_index::definition::Definition)

  astral-sh/ruff#16930 salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_definition_types::Configuration_>::execute_query<ty_python_semantic::types::infer::infer_definition_types::Configuration_> @ /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/execute.rs:266

                24   active_query (salsa::zalsa_local::ActiveQueryGuard)
                16   db (&dyn ty_python_semantic::db::Db)
                 8   opt_old_memo (core::option::Option<&salsa::function::memo::Memo<ty_python_semantic::types::infer::TypeInference>>)
                 8   current_revision (salsa::revision::Revision)
                 4   id (salsa::id::Id)

  astral-sh/ruff#16931 salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_definition_types::Configuration_>::execute_maybe_iterate<ty_python_semantic::types::infer::infer_definition_types::Configuration_> @ /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/execute.rs:146

                24   active_query (salsa::zalsa_local::ActiveQueryGuard)
                16   db (&dyn ty_python_semantic::db::Db)
                 8   opt_last_provisional (core::option::Option<&salsa::function::memo::Memo<ty_python_semantic::types::infer::TypeInference>>)
                 8   database_key_index (salsa::key::DatabaseKeyIndex)
                 8   self (*mut salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_definition_types::Configuration_>)
                 8   opt_old_memo (core::option::Option<&salsa::function::memo::Memo<ty_python_semantic::types::infer::TypeInference>>)
                 8   zalsa (*mut salsa::zalsa::Zalsa)
                 4   iteration_count (u32)
                 4   id (salsa::id::Id)
                 4   memo_ingredient_index (salsa::zalsa::MemoIngredientIndex)
                 1   fell_back (bool)

  astral-sh/ruff#16932 salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_definition_types::Configuration_>::execute<ty_python_semantic::types::infer::infer_definition_types::Configuration_> @ /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/execute.rs:89

                24   active_query (salsa::zalsa_local::ActiveQueryGuard)
                16   db (&dyn ty_python_semantic::db::Db)
                 8   zalsa (*mut salsa::zalsa::Zalsa)
                 8   database_key_index (salsa::key::DatabaseKeyIndex)
                 8   self (*mut salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_definition_types::Configuration_>)
                 8   opt_old_memo (core::option::Option<&salsa::function::memo::Memo<ty_python_semantic::types::infer::TypeInference>>)
                 4   memo_ingredient_index (salsa::zalsa::MemoIngredientIndex)
                 4   id (salsa::id::Id)

  astral-sh/ruff#16933 salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_definition_types::Configuration_>::fetch_cold<ty_python_semantic::types::infer::infer_definition_types::Configuration_> @ /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/fetch.rs:197

                24   _claim_guard (salsa::function::sync::ClaimGuard)
                16   db (&dyn ty_python_semantic::db::Db)
                 8   opt_old_memo (core::option::Option<&salsa::function::memo::Memo<ty_python_semantic::types::infer::TypeInference>>)
                 8   self (*mut salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_definition_types::Configuration_>)
                 8   zalsa (*mut salsa::zalsa::Zalsa)
                 4   id (salsa::id::Id)
                 4   memo_ingredient_index (salsa::zalsa::MemoIngredientIndex)

  astral-sh/ruff#16934 salsa::function::fetch::{impl#0}::refresh_memo::{closure#0}<ty_python_semantic::types::infer::infer_definition_types::Configuration_> @ /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/fetch.rs:46

             2,264   zalsa (salsa::zalsa::Zalsa)
               632   self (salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_definition_types::Configuration_>)
                 4   id (salsa::id::Id)
                 4   memo_ingredient_index (salsa::zalsa::MemoIngredientIndex)
                 0   db (dyn ty_python_semantic::db::Db)

  astral-sh/ruff#16935 core::option::Option<&salsa::function::memo::Memo<ty_python_semantic::types::infer::TypeInference>>::or_else<&salsa::function::memo::Memo<ty_python_semantic::types::infer::TypeInference>, salsa::function::fetch::{impl#0}::refresh_memo::{closure_env#0}<ty_python_semantic::types::infer::infer_definition_types::Configuration_>> @ /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/option.rs:1552

                48   f (salsa::function::fetch::{impl#0}::refresh_memo::{closure_env#0}<ty_python_semantic::types::infer::infer_definition_types::Configuration_>)
                 8   self (core::option::Option<&salsa::function::memo::Memo<ty_python_semantic::types::infer::TypeInference>>)

  astral-sh/ruff#16936 salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_definition_types::Configuration_>::refresh_memo<ty_python_semantic::types::infer::infer_definition_types::Configuration_> @ /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/fetch.rs:44

                16   db (&dyn ty_python_semantic::db::Db)
                 8   memo (*mut salsa::function::memo::Memo<ty_python_semantic::types::infer::TypeInference>)
                 8   self (*mut salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_definition_types::Configuration_>)
                 8   zalsa (*mut salsa::zalsa::Zalsa)
                 8   zalsa_local (*mut salsa::zalsa_local::ZalsaLocal)
                 4   memo_ingredient_index (salsa::zalsa::MemoIngredientIndex)
                 4   id (salsa::id::Id)

  astral-sh/ruff#16937 salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_definition_types::Configuration_>::fetch<ty_python_semantic::types::infer::infer_definition_types::Configuration_> @ /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/function/fetch.rs:17

                16   db (&dyn ty_python_semantic::db::Db)
                 8   zalsa (*mut salsa::zalsa::Zalsa)
                 8   zalsa_local (*mut salsa::zalsa_local::ZalsaLocal)
                 8   self (*mut salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_definition_types::Configuration_>)
                 4   id (salsa::id::Id)

  astral-sh/ruff#16938 ty_python_semantic::types::infer::infer_definition_types::{closure#0} @ /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/components/salsa-macro-rules/src/setup_tracked_fn.rs:368

                 4   definition (ty_python_semantic::semantic_index::definition::Definition)
                 0   db (dyn ty_python_semantic::db::Db)

  astral-sh/ruff#16939 salsa::attach::Attached::attach<dyn ty_python_semantic::db::Db, &ty_python_semantic::types::infer::TypeInference, ty_python_semantic::types::infer::infer_definition_types::{closure_env#0}> @ /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/attach.rs:73

                24   op (ty_python_semantic::types::infer::infer_definition_types::{closure_env#0})
                16   db (&dyn ty_python_semantic::db::Db)
                 8   _guard (salsa::attach::{impl#0}::attach::DbGuard)
                 8   self (*mut salsa::attach::Attached)

  astral-sh/ruff#16940 salsa::attach::attach::{closure#0}<&ty_python_semantic::types::infer::TypeInference, dyn ty_python_semantic::db::Db, ty_python_semantic::types::infer::infer_definition_types::{closure_env#0}> @ /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/attach.rs:97

                24   op (ty_python_semantic::types::infer::infer_definition_types::{closure_env#0})
                 8   a (*mut salsa::attach::Attached)
                 0   db (dyn ty_python_semantic::db::Db)

  astral-sh/ruff#16941 std::thread::local::LocalKey<salsa::attach::Attached>::try_with<salsa::attach::Attached, salsa::attach::attach::{closure_env#0}<&ty_python_semantic::types::infer::TypeInference, dyn ty_python_semantic::db::Db, ty_python_semantic::types::infer::infer_definition_types::{closure_env#0}>, &ty_python_semantic::types::infer::TypeInference> @ /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/thread/local.rs:310

                40   f (salsa::attach::attach::{closure_env#0}<&ty_python_semantic::types::infer::TypeInference, dyn ty_python_semantic::db::Db, ty_python_semantic::types::infer::infer_definition_types::{closure_env#0}>)
                 8   thread_local (*mut salsa::attach::Attached)
                 8   self (*mut std::thread::local::LocalKey<salsa::attach::Attached>)

  astral-sh/ruff#16942 std::thread::local::LocalKey<salsa::attach::Attached>::with<salsa::attach::Attached, salsa::attach::attach::{closure_env#0}<&ty_python_semantic::types::infer::TypeInference, dyn ty_python_semantic::db::Db, ty_python_semantic::types::infer::infer_definition_types::{closure_env#0}>, &ty_python_semantic::types::infer::TypeInference> @ /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/thread/local.rs:274

                40   f (salsa::attach::attach::{closure_env#0}<&ty_python_semantic::types::infer::TypeInference, dyn ty_python_semantic::db::Db, ty_python_semantic::types::infer::infer_definition_types::{closure_env#0}>)
                 8   self (*mut std::thread::local::LocalKey<salsa::attach::Attached>)

  astral-sh/ruff#16943 salsa::attach::attach<&ty_python_semantic::types::infer::TypeInference, dyn ty_python_semantic::db::Db, ty_python_semantic::types::infer::infer_definition_types::{closure_env#0}> @ /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/src/attach.rs:95

                24   op (ty_python_semantic::types::infer::infer_definition_types::{closure_env#0})
                16   db (&dyn ty_python_semantic::db::Db)

  astral-sh/ruff#16944 ty_python_semantic::types::infer::infer_definition_types @ /home/shark/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/42f1583/components/salsa-macro-rules/src/setup_tracked_fn.rs:360

                16   db (&dyn ty_python_semantic::db::Db)
                 4   definition (ty_python_semantic::semantic_index::definition::Definition)

  astral-sh/ruff#16945 ty_python_semantic::types::binding_type @ crates/ty_python_semantic/src/types.rs:111

                16   db (&dyn ty_python_semantic::db::Db)
                 4   definition (ty_python_semantic::semantic_index::definition::Definition)

  astral-sh/ruff#16946 ty_python_semantic::symbol::symbol_from_bindings_impl::{closure#2} @ crates/ty_python_semantic/src/symbol.rs:794

                40   unbound_visibility (ty_python_semantic::symbol::symbol_from_bindings_impl::{closure_env#1})
                24   narrowing_constraint (ty_python_semantic::semantic_index::use_def::ConstraintsIterator)
                24   is_non_exported (ty_python_semantic::symbol::symbol_from_bindings_impl::{closure_env#0})
                24   visibility_constraints (ty_python_semantic::semantic_index::visibility_constraints::VisibilityConstraints)
                24   predicates (ruff_index::vec::IndexVec<ty_python_semantic::semantic_index::predicate::ScopedPredicateId, ty_python_semantic::semantic_index::predicate::Predicate>)
                 4   binding (ty_python_semantic::semantic_index::definition::Definition)
                 4   visibility_constraint (ty_python_semantic::semantic_index::visibility_constraints::ScopedVisibilityConstraintId)
                 1   static_visibility (ty_python_semantic::types::Truthiness)
                 0   db (dyn ty_python_semantic::db::Db)

  astral-sh/ruff#16947 core::ops::function::impls::{impl#3}::call_mut<(ty_python_semantic::semantic_index::use_def::BindingWithConstraints), ty_python_semantic::symbol::symbol_from_bindings_impl::{closure_env#2}> @ /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/ops/function.rs:294

                32   args ((ty_python_semantic::semantic_index::use_def::BindingWithConstraints))
                 8   self (*mut *mut ty_python_semantic::symbol::symbol_from_bindings_impl::{closure_env#2})

  astral-sh/ruff#16948 core::iter::traits::iterator::Iterator::find_map::check::{closure#0}<ty_python_semantic::semantic_index::use_def::BindingWithConstraints, ty_python_semantic::types::Type, &mut ty_python_semantic::symbol::symbol_from_bindings_impl::{closure_env#2}> @ /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/iter/traits/iterator.rs:2859

                32   x (ty_python_semantic::semantic_index::use_def::BindingWithConstraints)
                 8   f (*mut ty_python_semantic::symbol::symbol_from_bindings_impl::{closure_env#2})

  astral-sh/ruff#16949 core::iter::adapters::peekable::{impl#1}::try_fold<ty_python_semantic::semantic_index::use_def::BindingWithConstraintsIterator, (), core::iter::traits::iterator::Iterator::find_map::check::{closure_env#0}<ty_python_semantic::semantic_index::use_def::BindingWithConstraints, ty_python_semantic::types::Type, &mut ty_python_semantic::symbol::symbol_from_bindings_impl::{closure_env#2}>, core::ops::control_flow::ControlFlow<ty_python_semantic::types::Type, ()>> @ /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/iter/adapters/peekable.rs:100

                32   v (ty_python_semantic::semantic_index::use_def::BindingWithConstraints)
                 8   self (*mut core::iter::adapters::peekable::Peekable<ty_python_semantic::semantic_index::use_def::BindingWithConstraintsIterator>)
                 8   f (core::iter::traits::iterator::Iterator::find_map::check::{closure_env#0}<ty_python_semantic::semantic_index::use_def::BindingWithConstraints, ty_python_semantic::types::Type, &mut ty_python_semantic::symbol::symbol_from_bindings_impl::{closure_env#2}>)
                 0   init (())

  astral-sh/ruff#16950 core::iter::traits::iterator::Iterator::find_map<core::iter::adapters::peekable::Peekable<ty_python_semantic::semantic_index::use_def::BindingWithConstraintsIterator>, ty_python_semantic::types::Type, &mut ty_python_semantic::symbol::symbol_from_bindings_impl::{closure_env#2}> @ /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/iter/traits/iterator.rs:2865

                 8   self (*mut core::iter::adapters::peekable::Peekable<ty_python_semantic::semantic_index::use_def::BindingWithConstraintsIterator>)
                 8   f (*mut ty_python_semantic::symbol::symbol_from_bindings_impl::{closure_env#2})

  astral-sh/ruff#16951 core::iter::adapters::filter_map::{impl#2}::next<ty_python_semantic::types::Type, core::iter::adapters::peekable::Peekable<ty_python_semantic::semantic_index::use_def::BindingWithConstraintsIterator>, ty_python_semantic::symbol::symbol_from_bindings_impl::{closure_env#2}> @ /home/shark/.rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/iter/adapters/filter_map.rs:64

                 8   self (*mut core::iter::adapters::filter_map::FilterMap<core::iter::adapters::peekable::Peekable<ty_python_semantic::semantic_index::use_def::BindingWithConstraintsIterator>, ty_python_semantic::symbol::symbol_from_bindings_impl::{closure_env#2}>)

  astral-sh/ruff#16952 ty_python_semantic::symbol::symbol_from_bindings_impl @ crates/ty_python_semantic/src/symbol.rs:799

               136   types (core::iter::adapters::filter_map::FilterMap<core::iter::adapters::peekable::Peekable<ty_python_semantic::semantic_index::use_def::BindingWithConstraintsIterator>, ty_python_semantic::symbol::symbol_from_bindings_impl::{closure_env#2}>)
                88   bindings_with_constraints (core::iter::adapters::peekable::Peekable<ty_python_semantic::semantic_index::use_def::BindingWithConstraintsIterator>)
                40   unbound_visibility (ty_python_semantic::symbol::symbol_from_bindings_impl::{closure_env#1})
                32   first (ty_python_semantic::types::Type)
                24   is_non_exported (ty_python_semantic::symbol::symbol_from_bindings_impl::{closure_env#0})
                16   db (&dyn ty_python_semantic::db::Db)
                 8   unbound_visibility_constraint (core::option::Option<ty_python_semantic::semantic_index::visibility_constraints::ScopedVisibilityConstraintId>)
                 8   visibility_constraints (*mut ty_python_semantic::semantic_index::visibility_constraints::VisibilityConstraints)
                 8   predicates (*mut ruff_index::vec::IndexVec<ty_python_semantic::semantic_index::predicate::ScopedPredicateId, ty_python_semantic::semantic_index::predicate::Predicate>)
                 1   requires_explicit_reexport (ty_python_semantic::symbol::RequiresExplicitReExport)

  astral-sh/ruff#16953 ty_python_semantic::symbol::symbol_from_bindings @ crates/ty_python_semantic/src/symbol.rs:392

                48   bindings_with_constraints (ty_python_semantic::semantic_index::use_def::BindingWithConstraintsIterator)
                16   db (&dyn ty_python_semantic::db::Db)

  astral-sh/ruff#16954 ty_python_semantic::types::infer::TypeInferenceBuilder::infer_name_load @ crates/ty_python_semantic/src/types/infer.rs:5181

                32   narrow_with_applicable_constraints (ty_python_semantic::types::infer::{impl#2}::infer_name_load::{closure_env#0})
                24   constraint_keys (alloc::vec::Vec<(ty_python_semantic::semantic_index::symbol::FileScopeId, ty_python_semantic::semantic_index::narrowing_constraints::ConstraintKey), alloc::alloc::Global>)
                16   db (&dyn ty_python_semantic::db::Db)
                 8   use_def (alloc::sync::Arc<ty_python_semantic::semantic_index::use_def::UseDefMap, alloc::alloc::Global>)
                 8   symbol_table (alloc::sync::Arc<ty_python_semantic::semantic_index::symbol::SymbolTable, alloc::alloc::Global>)
                 8   symbol_name (*mut ruff_python_ast::name::Name)
                 8   self (*mut ty_python_semantic::types::infer::TypeInferenceBuilder)
                 8   name_node (*mut ruff_python_ast::generated::ExprName)
                 4   use_id (ty_python_semantic::semantic_index::ast_ids::ScopedUseId)
                 4   file_scope_id (ty_python_semantic::semantic_index::symbol::FileScopeId)
                 4   scope (ty_python_semantic::semantic_index::symbol::ScopeId)

  astral-sh/ruff#16955 ty_python_semantic::types::infer::TypeInferenceBuilder::infer_augment_assignment @ crates/ty_python_semantic/src/types/infer.rs:3495

                 8   name (*mut ruff_python_ast::generated::ExprName)
                 8   target (*mut *mut ruff_python_ast::generated::Expr)
                 8   value (*mut *mut ruff_python_ast::generated::Expr)
                 8   self (*mut ty_python_semantic::types::infer::TypeInferenceBuilder)
                 8   assignment (*mut ruff_python_ast::generated::StmtAugAssign)

  astral-sh/ruff#16956 ty_python_semantic::types::infer::TypeInferenceBuilder::infer_augment_assignment_definition @ crates/ty_python_semantic/src/types/infer.rs:3480

                 8   self (*mut ty_python_semantic::types::infer::TypeInferenceBuilder)
                 8   assignment (*mut ruff_python_ast::generated::StmtAugAssign)
                 4   definition (ty_python_semantic::semantic_index::definition::Definition)

  astral-sh/ruff#16957 ty_python_semantic::types::infer::TypeInferenceBuilder::infer_region_definition @ crates/ty_python_semantic/src/types/infer.rs:1260

                 8   augmented_assignment (*mut ty_python_semantic::ast_node_ref::AstNodeRef<ruff_python_ast::generated::StmtAugAssign>)
                 8   self (*mut ty_python_semantic::types::infer::TypeInferenceBuilder)
                 4   definition (ty_python_semantic::semantic_index::definition::Definition)

  astral-sh/ruff#16958 ty_python_semantic::types::infer::TypeInferenceBuilder::infer_region @ crates/ty_python_semantic/src/types/infer.rs:696

                 8   self (*mut ty_python_semantic::types::infer::TypeInferenceBuilder)
                 4   definition (ty_python_semantic::semantic_index::definition::Definition)

  astral-sh/ruff#16959 ty_python_semantic::types::infer::TypeInferenceBuilder::finish @ crates/ty_python_semantic/src/types/infer.rs:7181

               440   self (ty_python_semantic::types::infer::TypeInferenceBuilder)
```

</details>


---

_Comment by @MichaReiser on 2025-05-07 13:15_

This is an excellent write up! Thank you. 

Are there a few (or maybe just one) query that we call when crossing cross-file boundaries? I'm asking because these would be great places to insert `stacker` calls to ensure a minimum stack size

---

_Comment by @sharkdp on 2025-05-07 13:21_

> Are there a few (or maybe just one) query that we call when crossing cross-file boundaries? I'm asking because these would be great places to insert `stacker` calls to ensure a minimum stack size

I don't think it would help for this particular scenario, though? The stack grows while checking `mod.py` only. It's also easy to turn this into a single-file example. Checking this file on main/release also produces a stack overflow because we infer the `n` definitions lazily:
```py
class C:
    n = 0
    n += 1
    n += 1
    n += 1
    # <repeat 4000 times>

C.n
```

---

_Comment by @carljm on 2025-05-07 13:26_

Awesome investigation and writeup! I agree that we can keep this issue open, but if it no longer repros with the increased worker stack size, it's not a priority to do anything else here right now.

---

_Closed by @carljm on 2025-05-07 13:27_

---

_Closed by @carljm on 2025-05-07 13:27_

---

_Reopened by @MichaReiser on 2025-05-07 13:44_

---

_Added to milestone `ty beta` by @MichaReiser on 2025-05-07 13:44_

---

_Renamed from "[ty] Stack overflow when running on 'manticore'" to "[ty] Stack overflow for large chain of inter-dependent definitions" by @sharkdp on 2025-05-07 13:47_

---

_Unassigned @sharkdp by @sharkdp on 2025-05-07 13:52_

---

_Label `bug` added by @MichaReiser on 2025-05-07 15:17_

---

_Closed by @MichaReiser on 2025-05-07 15:17_

---

_Closed by @MichaReiser on 2025-05-07 15:17_

---

_Renamed from "[ty] Stack overflow for large chain of inter-dependent definitions" to "Stack overflow for large chain of inter-dependent definitions" by @MichaReiser on 2025-05-07 15:24_

---

_Reopened by @sharkdp on 2025-05-07 18:35_

---

_Label `fatal` added by @AlexWaygood on 2025-05-10 18:00_

---

_Comment by @MichaReiser on 2025-09-22 08:19_

See https://github.com/astral-sh/ty/issues/1226 for a stack overflow with address sanitizing enabled which suggest that this might still be a problem for very large files or projects with very nested definitions

---

_Comment by @MichaReiser on 2025-10-17 08:17_

This hasn't been reported by any users so far. Because of that, I think it's okay to move it to GA

---

_Removed from milestone `Beta` by @MichaReiser on 2025-10-17 08:17_

---

_Added to milestone `GA` by @MichaReiser on 2025-10-17 08:17_

---
