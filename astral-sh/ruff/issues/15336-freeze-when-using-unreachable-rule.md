```yaml
number: 15336
title: Freeze when using unreachable rule
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2025-01-08T05:44:59Z
updated_at: 2025-01-08T15:45:06Z
url: https://github.com/astral-sh/ruff/issues/15336
synced_at: 2026-01-12T15:54:54Z
```

# Freeze when using unreachable rule

---

_@qarmin_

Command
```
timeout -v 2 ruff_normal check {} --select ALL --preview --output-format concise --no-cache --fix --unsafe-fixes --isolated
```

```
Downloading source file /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/ruff_linter/src/rules/pylint/rules/unreachable.rs
0x0000555556729096 in ruff_linter::rules::pylint::rules::unreachable::post_process_loop (blocks=0x7fffffff27c0,         
    start_index=..., loop_start=..., loop_exit=..., clause_exit=...)
    at crates/ruff_linter/src/rules/pylint/rules/unreachable.rs:449
warning: 449	crates/ruff_linter/src/rules/pylint/rules/unreachable.rs: No such file or directory
(gdb) backtrace
#0  0x0000555556729096 in ruff_linter::rules::pylint::rules::unreachable::post_process_loop (blocks=0x7fffffff27c0, 
    start_index=..., loop_start=..., loop_exit=..., clause_exit=...)
    at crates/ruff_linter/src/rules/pylint/rules/unreachable.rs:449
#1  0x00005555567291b2 in ruff_linter::rules::pylint::rules::unreachable::post_process_loop (blocks=0x7fffffff27c0, 
    start_index=..., loop_start=..., loop_exit=..., clause_exit=...)
    at crates/ruff_linter/src/rules/pylint/rules/unreachable.rs:484
#2  0x0000555556728ee9 in ruff_linter::rules::pylint::rules::unreachable::loop_block (blocks=0x7fffffff27c0, 
    condition=..., body=..., orelse=..., after=...) at crates/ruff_linter/src/rules/pylint/rules/unreachable.rs:414
#3  0x0000555556729dee in ruff_linter::rules::pylint::rules::unreachable::BasicBlocksBuilder::create_blocks (
    self=<optimized out>, stmts=..., after=...)
    at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/ub_checks.rs:127
#4  0x0000555556728be6 in ruff_linter::rules::pylint::rules::unreachable::{impl#2}::from (stmts=...)
    at crates/ruff_linter/src/rules/pylint/rules/unreachable.rs:222
#5  0x0000555556728198 in ruff_linter::rules::pylint::rules::unreachable::in_function (name=0x7ffff7922378, body=...)
    at crates/ruff_linter/src/rules/pylint/rules/unreachable.rs:48
#6  0x00005555568b1096 in ruff_linter::checkers::ast::analyze::statement::statement (stmt=0x7ffff7922348, 
    checker=0x7fffffff47a0) at crates/ruff_linter/src/checkers/ast/analyze/statement.rs:373
#7  0x00005555568b97d3 in ruff_linter::checkers::ast::{impl#5}::visit_stmt (self=<optimized out>, stmt=0x7ffff7922348)
    at crates/ruff_linter/src/checkers/ast/mod.rs:1064
#8  0x00005555568b8c0d in ruff_linter::checkers::ast::{impl#5}::visit_body (self=0x7fffffff47a0, body=...)
    at crates/ruff_linter/src/checkers/ast/mod.rs:1703
#9  ruff_linter::checkers::ast::{impl#5}::visit_stmt (self=0x7fffffff47a0, stmt=0x7ffff7924ac8)
--Type <RET> for more, q to quit, c to continue without paging--c
    at crates/ruff_linter/src/checkers/ast/mod.rs:875
#10 0x00005555568bece9 in ruff_linter::checkers::ast::{impl#5}::visit_body (self=0x7fffffff47a0, body=...)
    at crates/ruff_linter/src/checkers/ast/mod.rs:1703
#11 ruff_linter::checkers::ast::check_ast (parsed=0x7fffffff5cf0, locator=<optimized out>, stylist=<optimized out>, 
    indexer=<optimized out>, noqa_line_for=<optimized out>, settings=0x7fffffff96c8, noqa=<optimized out>, path=..., 
    package=..., source_type=ruff_python_ast::PySourceType::Python, cell_offsets=..., notebook_index=...)
    at crates/ruff_linter/src/checkers/ast/mod.rs:2638
#12 0x00005555566a6e18 in ruff_linter::linter::check_path (path=..., package=..., locator=<optimized out>, 
    stylist=<optimized out>, indexer=<optimized out>, directives=<optimized out>, settings=<optimized out>, 
    noqa=<optimized out>, source_kind=<optimized out>, source_type=<optimized out>, parsed=<optimized out>)
    at crates/ruff_linter/src/linter.rs:148
#13 0x00005555566add24 in ruff_linter::linter::lint_fix (path=..., package=..., noqa=<optimized out>, 
    unsafe_fixes=<optimized out>, settings=<optimized out>, source_kind=<optimized out>, source_type=<optimized out>)
    at crates/ruff_linter/src/linter.rs:513
#14 0x0000555556308c54 in ruff::diagnostics::lint_path (path=..., package=..., settings=0x7fffffff96c8, cache=..., 
    noqa=<optimized out>, fix_mode=<optimized out>, unsafe_fixes=<optimized out>) at crates/ruff/src/diagnostics.rs:278
#15 0x000055555644e85f in ruff::commands::check::lint_path::{closure#0} () at crates/ruff/src/commands/check.rs:195
#16 std::panicking::try::do_call<ruff::commands::check::lint_path::{closure_env#0}, core::result::Result<ruff::diagnostics::Diagnostics, anyhow::Error>> (data=<error reading variable: Cannot access memory at address 0x0>)
    at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panicking.rs:584
#17 std::panicking::try<core::result::Result<ruff::diagnostics::Diagnostics, anyhow::Error>, ruff::commands::check::lint_path::{closure_env#0}> (f=...)
    at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panicking.rs:547
#18 std::panic::catch_unwind<ruff::commands::check::lint_path::{closure_env#0}, core::result::Result<ruff::diagnostics::Diagnostics, anyhow::Error>> (f=...)
    at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panic.rs:358
#19 ruff_db::panic::catch_unwind<ruff::commands::check::lint_path::{closure_env#0}, core::result::Result<ruff::diagnostics::Diagnostics, anyhow::Error>> (f=...) at crates/ruff_db/src/panic.rs:40
#20 0x000055555643a61f in ruff::commands::check::lint_path (path=..., 
    package=<error reading variable: Cannot access memory at address 0x0>, settings=0x3, cache=..., 
    noqa=ruff_linter::settings::flags::Noqa::Enabled, fix_mode=ruff_linter::settings::flags::FixMode::Apply, 
    unsafe_fixes=ruff_linter::settings::types::UnsafeFixes::Enabled) at crates/ruff/src/commands/check.rs:194
#21 0x00005555563ff4c8 in ruff::commands::check::check::{closure#0} (
    resolved_file=<error reading variable: Cannot access memory at address 0x0>)
    at crates/ruff/src/commands/check.rs:96
#22 rayon::iter::filter_map::{impl#6}::consume<&core::result::Result<ruff_workspace::resolver::ResolvedFile, ignore::Error>, ruff::diagnostics::Diagnostics, rayon::iter::fold::FoldFolder<rayon::iter::reduce::ReduceFolder<ruff::commands::check::check::{closure_env#4}, (ruff::diagnostics::Diagnostics, u64)>, (ruff::diagnostics::Diagnostics, u64), ruff::commands::check::check::{closure_env#2}>, ruff::commands::check::check::{closure_env#0}> (self=..., item=<optimized out>)
    at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/filter_map.rs:123
#23 rayon::iter::plumbing::Folder::consume_iter<rayon::iter::filter_map::FilterMapFolder<rayon::iter::fold::FoldFolder<rayon::iter::reduce::ReduceFolder<ruff::commands::check::check::{closure_env#4}, (ruff::diagnostics::Diagnostics, u64)>, (ruff::diagnostics::Diagnostics, u64), ruff::commands::check::check::{closure_env#2}>, ruff::commands::check::check::{closure_env#0}>, &core::result::Result<ruff_workspace::resolver::ResolvedFile, ignore::Error>, core::slice::iter::Iter<core::result::Result<ruff_workspace::resolver::ResolvedFile, ignore::Error>>> (self=..., iter=...)
    at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/plumbing/mod.rs:178
#24 0x000055555639935d in rayon::iter::plumbing::Producer::fold_with<rayon::slice::IterProducer<core::result::Result<ruff_workspace::resolver::ResolvedFile, ignore::Error>>, rayon::iter::filter_map::FilterMapFolder<rayon::iter::fold::FoldFolder<rayon::iter::reduce::ReduceFolder<ruff::commands::check::check::{closure_env#4}, (ruff::diagnostics::Diagnostics, u64)>, (ruff::diagnostics::Diagnostics, u64), ruff::commands::check::check::{closure_env#2}>, ruff::commands::check::check::{closure_env#0}>> (self=..., 
    folder=<error reading variable: access outside bounds of object referenced via synthetic pointer>)
    at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/plumbing/mod.rs:109
#25 rayon::iter::plumbing::bridge_producer_consumer::helper<rayon::slice::IterProducer<core::result::Result<ruff_workspace::resolver::ResolvedFile, ignore::Error>>, rayon::iter::filter_map::FilterMapConsumer<rayon::iter::fold::FoldConsumer<rayon::iter::reduce::ReduceConsumer<ruff::commands::check::check::{closure_env#4}, ruff::commands::check::check::{closure_env#3}>, ruff::commands::check::check::{closure_env#1}, ruff::commands::check::check::{closure_env#2}>, ruff::commands::check::check::{closure_env#0}>> (len=1, migrated=<optimized out>, splitter=..., producer=..., consumer=...)
    at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/plumbing/mod.rs:437
#26 0x0000555556439ac4 in rayon::iter::plumbing::bridge_producer_consumer<rayon::slice::IterProducer<core::result::Result<ruff_workspace::resolver::ResolvedFile, ignore::Error>>, rayon::iter::filter_map::FilterMapConsumer<rayon::iter::fold::FoldConsumer<rayon::iter::reduce::ReduceConsumer<ruff::commands::check::check::{closure_env#4}, ruff::commands::check::check::{closure_env#3}>, ruff::commands::check::check::{closure_env#1}, ruff::commands::check::check::{closure_env#2}>, ruff::commands::check::check::{closure_env#0}>> (len=1, producer=..., consumer=...)
    at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/plumbing/mod.rs:396
#27 rayon::iter::plumbing::bridge::{impl#0}::callback<rayon::iter::filter_map::FilterMapConsumer<rayon::iter::fold::FoldConsumer<rayon::iter::reduce::ReduceConsumer<ruff::commands::check::check::{closure_env#4}, ruff::commands::check::check::{closure_env#3}>, ruff::commands::check::check::{closure_env#1}, ruff::commands::check::check::{closure_env#2}>, ruff::commands::check::check::{closure_env#0}>, &core::result::Result<ruff_workspace::resolver::ResolvedFile, ignore::Error>, rayon::slice::IterProducer<core::result::Result<ruff_workspace::resolver::ResolvedFile, ignore::Error>>> (self=..., 
    producer=...)
    at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/plumbing/mod.rs:372
#28 rayon::slice::{impl#6}::with_producer<core::result::Result<ruff_workspace::resolver::ResolvedFile, ignore::Error>, rayon::iter::plumbing::bridge::Callback<rayon::iter::filter_map::FilterMapConsumer<rayon::iter::fold::FoldConsumer<rayon::iter::reduce::ReduceConsumer<ruff::commands::check::check::{closure_env#4}, ruff::commands::check::check::{closure_env#3}>, ruff::commands::check::check::{closure_env#1}, ruff::commands::check::check::{closure_env#2}>, ruff::commands::check::check::{closure_env#0}>>> (self=..., callback=...)
    at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/slice/mod.rs:826
#29 rayon::iter::plumbing::bridge<rayon::slice::Iter<core::result::Result<ruff_workspace::resolver::ResolvedFile, ignore::Error>>, rayon::iter::filter_map::FilterMapConsumer<rayon::iter::fold::FoldConsumer<rayon::iter::reduce::ReduceConsumer<ruff::commands::check::check::{closure_env#4}, ruff::commands::check::check::{closure_env#3}>, ruff::commands::check::check::{closure_env#1}, ruff::commands::check::check::{closure_env#2}>, ruff::commands::check::check::{closure_env#0}>>
    (par_iter=..., consumer=...)
    at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/plumbing/mod.rs:356
#30 rayon::slice::{impl#5}::drive_unindexed<core::result::Result<ruff_workspace::resolver::ResolvedFile, ignore::Error>, rayon::iter::filter_map::FilterMapConsumer<rayon::iter::fold::FoldConsumer<rayon::iter::reduce::ReduceConsumer<ruff::commands::check::check::{closure_env#4}, ruff::commands::check::check::{closure_env#3}>, ruff::commands::check::check::{closure_env#1}, ruff::commands::check::check::{closure_env#2}>, ruff::commands::check::check::{closure_env#0}>> (
    self=..., consumer=...)
    at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/slice/mod.rs:802
#31 rayon::iter::filter_map::{impl#2}::drive_unindexed<rayon::slice::Iter<core::result::Result<ruff_workspace::resolver::ResolvedFile, ignore::Error>>, ruff::commands::check::check::{closure_env#0}, ruff::diagnostics::Diagnostics, rayon::iter::fold::FoldConsumer<rayon::iter::reduce::ReduceConsumer<ruff::commands::check::check::{closure_env#4}, ruff::commands::check::check::{closure_env#3}>, ruff::commands::check::check::{closure_env#1}, ruff::commands::check::check::{closure_env#2}>> (self=..., consumer=...)
    at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/filter_map.rs:46
#32 rayon::iter::fold::{impl#2}::drive_unindexed<(ruff::diagnostics::Diagnostics, u64), rayon::iter::filter_map::FilterMap<rayon::slice::Iter<core::result::Result<ruff_workspace::resolver::ResolvedFile, ignore::Error>>, ruff::commands::check::check::{closure_env#0}>, ruff::commands::check::check::{closure_env#1}, ruff::commands::check::check::{closure_env#2}, rayon::iter::reduce::ReduceConsumer<ruff::commands::check::check::{closure_env#4}, ruff::commands::check::check::{closure_env#3}>> (self=..., consumer=...)
    at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/fold.rs:59
#33 rayon::iter::reduce::reduce<rayon::iter::fold::Fold<rayon::iter::filter_map::FilterMap<rayon::slice::Iter<core::result::Result<ruff_workspace::resolver::ResolvedFile, ignore::Error>>, ruff::commands::check::check::{closure_env#0}>, ruff::commands::check::check::{closure_env#1}, ruff::commands::check::check::{closure_env#2}>, ruff::commands::check::check::{closure_env#4}, ruff::commands::check::check::{closure_env#3}, (ruff::diagnostics::Diagnostics, u64)> (pi=..., 
    identity=..., reduce_op=...)
    at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/reduce.rs:15
#34 rayon::iter::ParallelIterator::reduce<rayon::iter::fold::Fold<rayon::iter::filter_map::FilterMap<rayon::slice::Iter<core::result::Result<ruff_workspace::resolver::ResolvedFile, ignore::Error>>, ruff::commands::check::check::{closure_env#0}>, ruff::commands::check::check::{closure_env#1}, ruff::commands::check::check::{closure_env#2}>, ruff::commands::check::check::{closure_env#4}, ruff::commands::check::check::{closure_env#3}> (self=...)
    at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/mod.rs:998
#35 ruff::commands::check::check (files=..., pyproject_config=<optimized out>, config_arguments=<optimized out>, 
    cache=<optimized out>, noqa=ruff_linter::settings::flags::Noqa::Enabled, 
    fix_mode=ruff_linter::settings::flags::FixMode::Apply, 
    unsafe_fixes=ruff_linter::settings::types::UnsafeFixes::Enabled) at crates/ruff/src/commands/check.rs:166
#36 0x000055555639496d in ruff::check (args=..., global_options=...) at crates/ruff/src/lib.rs:423
#37 0x0000555556392791 in ruff::run () at crates/ruff/src/lib.rs:195
#38 0x0000555556463d44 in ruff::main () at crates/ruff/src/main.rs:44

```

File minimized
```
def _():
 for i in():
    try:
        try:
         while r:
          if t:break
        finally:()
        return
    except:l
```

[a.py.zip](https://github.com/user-attachments/files/18342372/a.py.zip)


---

_Label `bug` added by @MichaReiser on 2025-01-08 07:05_

---

_Label `fuzzer` added by @dylwil3 on 2025-01-08 09:53_

---

_Closed by @dylwil3 on 2025-01-08 15:45_

---

_Closed by @dylwil3 on 2025-01-08 15:45_

---
