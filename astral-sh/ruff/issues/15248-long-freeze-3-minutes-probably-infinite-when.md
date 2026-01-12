```yaml
number: 15248
title: "Long freeze(>3 minutes - probably infinite) when checking file"
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2025-01-04T08:41:32Z
updated_at: 2025-01-07T17:26:06Z
url: https://github.com/astral-sh/ruff/issues/15248
synced_at: 2026-01-12T15:54:54Z
```

# Long freeze(>3 minutes - probably infinite) when checking file

---

_@qarmin_

Probably caused by #10891 - CC @augustelalande 

Command
```
timeout -v 300 ruff check . --select ALL --preview --output-format concise --no-cache --fix --unsafe-fixes --isolated
```
```
Downloading source file /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/ruff_linter/src/rules/pylint/rules/unreachable.rs
0x00005555569724b6 in ruff_linter::rules::pylint::rules::unreachable::post_process_loop (blocks=0x7fffffff2c80, start_index=..., loop_start=..., loop_exit=..., clause_exit=...)               
    at crates/ruff_linter/src/rules/pylint/rules/unreachable.rs:453
warning: 453	crates/ruff_linter/src/rules/pylint/rules/unreachable.rs: No such file or directory
(gdb) backtrace
#0  0x00005555569724b6 in ruff_linter::rules::pylint::rules::unreachable::post_process_loop (blocks=0x7fffffff2c80, start_index=..., loop_start=..., loop_exit=..., clause_exit=...)
    at crates/ruff_linter/src/rules/pylint/rules/unreachable.rs:453
#1  0x00005555569725c2 in ruff_linter::rules::pylint::rules::unreachable::post_process_loop (blocks=0x7fffffff2c80, start_index=..., loop_start=..., loop_exit=..., clause_exit=...)
    at crates/ruff_linter/src/rules/pylint/rules/unreachable.rs:484
#2  0x00005555569725c2 in ruff_linter::rules::pylint::rules::unreachable::post_process_loop (blocks=0x7fffffff2c80, start_index=..., loop_start=..., loop_exit=..., clause_exit=...)
    at crates/ruff_linter/src/rules/pylint/rules/unreachable.rs:484
#3  0x00005555569722f9 in ruff_linter::rules::pylint::rules::unreachable::loop_block (blocks=0x7fffffff2c80, condition=..., body=..., orelse=..., after=...)
    at crates/ruff_linter/src/rules/pylint/rules/unreachable.rs:414
#4  0x00005555569731ee in ruff_linter::rules::pylint::rules::unreachable::BasicBlocksBuilder::create_blocks (self=<optimized out>, stmts=..., after=...)
    at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/ub_checks.rs:127
#5  0x0000555556972290 in ruff_linter::rules::pylint::rules::unreachable::BasicBlocksBuilder::append_blocks (self=0x7fffffff2c80, stmts=..., after=...)
    at crates/ruff_linter/src/rules/pylint/rules/unreachable.rs:931
#6  ruff_linter::rules::pylint::rules::unreachable::BasicBlocksBuilder::append_blocks_if_not_empty (self=0x7fffffff2c80, stmts=..., after=...)
    at crates/ruff_linter/src/rules/pylint/rules/unreachable.rs:946
#7  ruff_linter::rules::pylint::rules::unreachable::loop_block (blocks=0x7fffffff2c80, condition=..., body=..., orelse=..., after=...)
    at crates/ruff_linter/src/rules/pylint/rules/unreachable.rs:411
#8  0x00005555569731ee in ruff_linter::rules::pylint::rules::unreachable::BasicBlocksBuilder::create_blocks (self=<optimized out>, stmts=..., after=...)
    at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/ub_checks.rs:127
#9  0x0000555556972fab in ruff_linter::rules::pylint::rules::unreachable::BasicBlocksBuilder::append_blocks (self=0x7fffffff2c80, stmts=..., after=...)
    at crates/ruff_linter/src/rules/pylint/rules/unreachable.rs:931
#10 ruff_linter::rules::pylint::rules::unreachable::BasicBlocksBuilder::append_blocks_if_not_empty (self=0x7fffffff2c80, stmts=..., after=...)
    at crates/ruff_linter/src/rules/pylint/rules/unreachable.rs:946
#11 ruff_linter::rules::pylint::rules::unreachable::BasicBlocksBuilder::create_blocks (self=0x7fffffff2c80, stmts=..., after=...)
    at crates/ruff_linter/src/rules/pylint/rules/unreachable.rs:781
#12 0x0000555556971ff6 in ruff_linter::rules::pylint::rules::unreachable::{impl#2}::from (stmts=...) at crates/ruff_linter/src/rules/pylint/rules/unreachable.rs:222
#13 0x00005555569714b8 in ruff_linter::rules::pylint::rules::unreachable::in_function (name=0x7fffeee228f8, body=...) at crates/ruff_linter/src/rules/pylint/rules/unreachable.rs:48
#14 0x00005555568ee670 in ruff_linter::checkers::ast::analyze::statement::statement (stmt=0x7fffeee228c8, checker=0x7fffffff4a10)
    at crates/ruff_linter/src/checkers/ast/analyze/statement.rs:372
#15 0x00005555568f4003 in ruff_linter::checkers::ast::{impl#5}::visit_stmt (self=<optimized out>, stmt=0x7fffeee228c8) at crates/ruff_linter/src/checkers/ast/mod.rs:1064
#16 0x00005555568f9419 in ruff_linter::checkers::ast::{impl#5}::visit_body (self=0x7fffffff4a10, body=...) at crates/ruff_linter/src/checkers/ast/mod.rs:1703
#17 ruff_linter::checkers::ast::check_ast (parsed=0x7fffffff5ba0, locator=<optimized out>, stylist=<optimized out>, indexer=<optimized out>, noqa_line_for=<optimized out>, 
    settings=0x7fffffff9608, noqa=<optimized out>, path=..., package=..., source_type=ruff_python_ast::PySourceType::Python, cell_offsets=..., notebook_index=...)
    at crates/ruff_linter/src/checkers/ast/mod.rs:2638
#18 0x0000555556842892 in ruff_linter::linter::check_path (path=..., package=..., locator=<optimized out>, stylist=<optimized out>, indexer=<optimized out>, directives=<optimized out>, 
    settings=<optimized out>, noqa=<optimized out>, source_kind=<optimized out>, source_type=<optimized out>, parsed=<optimized out>) at crates/ruff_linter/src/linter.rs:148
#19 0x000055555684683a in ruff_linter::linter::lint_fix (path=..., package=..., noqa=<optimized out>, unsafe_fixes=<optimized out>, settings=<optimized out>, source_kind=<optimized out>, 
    source_type=<optimized out>) at crates/ruff_linter/src/linter.rs:513
#20 0x00005555562e1616 in ruff::diagnostics::lint_path (path=..., package=..., settings=0x7fffffff9608, cache=..., noqa=<optimized out>, fix_mode=<optimized out>, 
    unsafe_fixes=<optimized out>) at crates/ruff/src/diagnostics.rs:278
#21 0x0000555556446b0f in ruff::commands::check::lint_path::{closure#0} () at crates/ruff/src/commands/check.rs:195
#22 std::panicking::try::do_call<ruff::commands::check::lint_path::{closure_env#0}, core::result::Result<ruff::diagnostics::Diagnostics, anyhow::Error>> (
    data=<error reading variable: Cannot access memory at address 0x0>)
--Type <RET> for more, q to quit, c to continue without paging--c
    at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panicking.rs:568
#23 std::panicking::try<core::result::Result<ruff::diagnostics::Diagnostics, anyhow::Error>, ruff::commands::check::lint_path::{closure_env#0}> (f=...)
    at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panicking.rs:531
#24 std::panic::catch_unwind<ruff::commands::check::lint_path::{closure_env#0}, core::result::Result<ruff::diagnostics::Diagnostics, anyhow::Error>> (f=...)
    at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panic.rs:358
#25 ruff_db::panic::catch_unwind<ruff::commands::check::lint_path::{closure_env#0}, core::result::Result<ruff::diagnostics::Diagnostics, anyhow::Error>> (f=...)
    at crates/ruff_db/src/panic.rs:40
#26 0x00005555563ec92f in ruff::commands::check::lint_path (path=..., package=<error reading variable: Cannot access memory at address 0x0>, settings=0x4, cache=..., 
    noqa=ruff_linter::settings::flags::Noqa::Enabled, fix_mode=ruff_linter::settings::flags::FixMode::Apply, unsafe_fixes=ruff_linter::settings::types::UnsafeFixes::Enabled)
    at crates/ruff/src/commands/check.rs:194
#27 0x00005555563ad1d1 in ruff::commands::check::check::{closure#0} (resolved_file=<error reading variable: Cannot access memory at address 0x0>) at crates/ruff/src/commands/check.rs:96
#28 rayon::iter::filter_map::{impl#6}::consume<&core::result::Result<ruff_workspace::resolver::ResolvedFile, ignore::Error>, ruff::diagnostics::Diagnostics, rayon::iter::fold::FoldFolder<rayon::iter::reduce::ReduceFolder<ruff::commands::check::check::{closure_env#4}, (ruff::diagnostics::Diagnostics, u64)>, (ruff::diagnostics::Diagnostics, u64), ruff::commands::check::check::{closure_env#2}>, ruff::commands::check::check::{closure_env#0}> (self=..., item=<optimized out>)
    at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/filter_map.rs:123
#29 rayon::iter::plumbing::Folder::consume_iter<rayon::iter::filter_map::FilterMapFolder<rayon::iter::fold::FoldFolder<rayon::iter::reduce::ReduceFolder<ruff::commands::check::check::{closure_env#4}, (ruff::diagnostics::Diagnostics, u64)>, (ruff::diagnostics::Diagnostics, u64), ruff::commands::check::check::{closure_env#2}>, ruff::commands::check::check::{closure_env#0}>, &core::result::Result<ruff_workspace::resolver::ResolvedFile, ignore::Error>, core::slice::iter::Iter<core::result::Result<ruff_workspace::resolver::ResolvedFile, ignore::Error>>> (self=..., 
    iter=...) at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/plumbing/mod.rs:178
#30 rayon::iter::plumbing::Producer::fold_with<rayon::slice::IterProducer<core::result::Result<ruff_workspace::resolver::ResolvedFile, ignore::Error>>, rayon::iter::filter_map::FilterMapFolder<rayon::iter::fold::FoldFolder<rayon::iter::reduce::ReduceFolder<ruff::commands::check::check::{closure_env#4}, (ruff::diagnostics::Diagnostics, u64)>, (ruff::diagnostics::Diagnostics, u64), ruff::commands::check::check::{closure_env#2}>, ruff::commands::check::check::{closure_env#0}>> (self=..., folder=...)
    at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/plumbing/mod.rs:109
#31 0x000055555639ce1e in rayon::iter::plumbing::bridge_producer_consumer::helper<rayon::slice::IterProducer<core::result::Result<ruff_workspace::resolver::ResolvedFile, ignore::Error>>, rayon::iter::filter_map::FilterMapConsumer<rayon::iter::fold::FoldConsumer<rayon::iter::reduce::ReduceConsumer<ruff::commands::check::check::{closure_env#4}, ruff::commands::check::check::{closure_env#3}>, ruff::commands::check::check::{closure_env#1}, ruff::commands::check::check::{closure_env#2}>, ruff::commands::check::check::{closure_env#0}>> (len=1, migrated=<optimized out>, 
    splitter=..., producer=..., consumer=...) at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/plumbing/mod.rs:437
#32 0x00005555563ebd6d in rayon::iter::plumbing::bridge_producer_consumer<rayon::slice::IterProducer<core::result::Result<ruff_workspace::resolver::ResolvedFile, ignore::Error>>, rayon::iter::filter_map::FilterMapConsumer<rayon::iter::fold::FoldConsumer<rayon::iter::reduce::ReduceConsumer<ruff::commands::check::check::{closure_env#4}, ruff::commands::check::check::{closure_env#3}>, ruff::commands::check::check::{closure_env#1}, ruff::commands::check::check::{closure_env#2}>, ruff::commands::check::check::{closure_env#0}>> (len=1, 
    producer=<error reading variable: access outside bounds of object referenced via synthetic pointer>, consumer=...)
    at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/plumbing/mod.rs:396
#33 rayon::iter::plumbing::bridge::{impl#0}::callback<rayon::iter::filter_map::FilterMapConsumer<rayon::iter::fold::FoldConsumer<rayon::iter::reduce::ReduceConsumer<ruff::commands::check::check::{closure_env#4}, ruff::commands::check::check::{closure_env#3}>, ruff::commands::check::check::{closure_env#1}, ruff::commands::check::check::{closure_env#2}>, ruff::commands::check::check::{closure_env#0}>, &core::result::Result<ruff_workspace::resolver::ResolvedFile, ignore::Error>, rayon::slice::IterProducer<core::result::Result<ruff_workspace::resolver::ResolvedFile, ignore::Error>>> (self=<error reading variable: access outside bounds of object referenced via synthetic pointer>, 
    producer=<error reading variable: access outside bounds of object referenced via synthetic pointer>)
    at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/plumbing/mod.rs:372
#34 rayon::slice::{impl#6}::with_producer<core::result::Result<ruff_workspace::resolver::ResolvedFile, ignore::Error>, rayon::iter::plumbing::bridge::Callback<rayon::iter::filter_map::FilterMapConsumer<rayon::iter::fold::FoldConsumer<rayon::iter::reduce::ReduceConsumer<ruff::commands::check::check::{closure_env#4}, ruff::commands::check::check::{closure_env#3}>, ruff::commands::check::check::{closure_env#1}, ruff::commands::check::check::{closure_env#2}>, ruff::commands::check::check::{closure_env#0}>>> (self=..., 
    callback=<error reading variable: access outside bounds of object referenced via synthetic pointer>)
    at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/slice/mod.rs:826
#35 rayon::iter::plumbing::bridge<rayon::slice::Iter<core::result::Result<ruff_workspace::resolver::ResolvedFile, ignore::Error>>, rayon::iter::filter_map::FilterMapConsumer<rayon::iter::fold::FoldConsumer<rayon::iter::reduce::ReduceConsumer<ruff::commands::check::check::{closure_env#4}, ruff::commands::check::check::{closure_env#3}>, ruff::commands::check::check::{closure_env#1}, ruff::commands::check::check::{closure_env#2}>, ruff::commands::check::check::{closure_env#0}>> (par_iter=..., consumer=...)
    at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/plumbing/mod.rs:356
#36 rayon::slice::{impl#5}::drive_unindexed<core::result::Result<ruff_workspace::resolver::ResolvedFile, ignore::Error>, rayon::iter::filter_map::FilterMapConsumer<rayon::iter::fold::FoldConsumer<rayon::iter::reduce::ReduceConsumer<ruff::commands::check::check::{closure_env#4}, ruff::commands::check::check::{closure_env#3}>, ruff::commands::check::check::{closure_env#1}, ruff::commands::check::check::{closure_env#2}>, ruff::commands::check::check::{closure_env#0}>> (self=..., consumer=...)
    at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/slice/mod.rs:802
#37 rayon::iter::filter_map::{impl#2}::drive_unindexed<rayon::slice::Iter<core::result::Result<ruff_workspace::resolver::ResolvedFile, ignore::Error>>, ruff::commands::check::check::{closure_env#0}, ruff::diagnostics::Diagnostics, rayon::iter::fold::FoldConsumer<rayon::iter::reduce::ReduceConsumer<ruff::commands::check::check::{closure_env#4}, ruff::commands::check::check::{closure_env#3}>, ruff::commands::check::check::{closure_env#1}, ruff::commands::check::check::{closure_env#2}>> (self=..., consumer=...)
    at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/filter_map.rs:46
#38 rayon::iter::fold::{impl#2}::drive_unindexed<(ruff::diagnostics::Diagnostics, u64), rayon::iter::filter_map::FilterMap<rayon::slice::Iter<core::result::Result<ruff_workspace::resolver::ResolvedFile, ignore::Error>>, ruff::commands::check::check::{closure_env#0}>, ruff::commands::check::check::{closure_env#1}, ruff::commands::check::check::{closure_env#2}, rayon::iter::reduce::ReduceConsumer<ruff::commands::check::check::{closure_env#4}, ruff::commands::check::check::{closure_env#3}>> (self=..., consumer=...)
    at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/fold.rs:59
#39 rayon::iter::reduce::reduce<rayon::iter::fold::Fold<rayon::iter::filter_map::FilterMap<rayon::slice::Iter<core::result::Result<ruff_workspace::resolver::ResolvedFile, ignore::Error>>, ruff::commands::check::check::{closure_env#0}>, ruff::commands::check::check::{closure_env#1}, ruff::commands::check::check::{closure_env#2}>, ruff::commands::check::check::{closure_env#4}, ruff::commands::check::check::{closure_env#3}, (ruff::diagnostics::Diagnostics, u64)> (pi=..., identity=..., reduce_op=...)
    at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/reduce.rs:15
#40 rayon::iter::ParallelIterator::reduce<rayon::iter::fold::Fold<rayon::iter::filter_map::FilterMap<rayon::slice::Iter<core::result::Result<ruff_workspace::resolver::ResolvedFile, ignore::Error>>, ruff::commands::check::check::{closure_env#0}>, ruff::commands::check::check::{closure_env#1}, ruff::commands::check::check::{closure_env#2}>, ruff::commands::check::check::{closure_env#4}, ruff::commands::check::check::{closure_env#3}> (self=...) at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/mod.rs:998
#41 ruff::commands::check::check (files=..., pyproject_config=<optimized out>, config_arguments=<optimized out>, cache=<optimized out>, noqa=ruff_linter::settings::flags::Noqa::Enabled, 
    fix_mode=ruff_linter::settings::flags::FixMode::Apply, unsafe_fixes=ruff_linter::settings::types::UnsafeFixes::Enabled) at crates/ruff/src/commands/check.rs:166
#42 0x00005555563d18ed in ruff::check (args=..., global_options=...) at crates/ruff/src/lib.rs:423
#43 0x00005555563cf161 in ruff::run () at crates/ruff/src/lib.rs:195
#44 0x000055555645a784 in ruff::main () at crates/ruff/src/main.rs:44

```

[SANITIZER_6-before.zip](https://github.com/user-attachments/files/18306155/SANITIZER_6-before.zip)


---

_Comment by @qarmin on 2025-01-04 08:51_

Minimized
```
def l():
 while T:
       try:
        while():
         if 3:break
       finally:return
```

[a.py.zip](https://github.com/user-attachments/files/18306202/a.py.zip)


---

_Label `bug` added by @AlexWaygood on 2025-01-04 10:48_

---

_Label `fuzzer` added by @AlexWaygood on 2025-01-04 10:48_

---

_Comment by @AlexWaygood on 2025-01-04 10:49_

Cc. @dylwil3 as well

---

_Comment by @MichaReiser on 2025-01-04 11:29_

I demoted the rule to testing only for now, see https://github.com/astral-sh/ruff/pull/15252

Let's do some more fuzzing (we have multiple fuzzer scripts) before promoting the rule to preview

---

_Assigned to @dylwil3 by @MichaReiser on 2025-01-04 11:34_

---

_Closed by @dylwil3 on 2025-01-07 17:26_

---
