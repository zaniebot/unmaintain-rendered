---
number: 16037
title: "Infinite loop when fixing file with `cast` and format strings"
type: issue
state: closed
author: qarmin
labels:
  - bug
  - rule
  - fixes
  - preview
assignees: []
created_at: 2025-02-08T09:16:34Z
updated_at: 2025-02-09T16:16:30Z
url: https://github.com/astral-sh/ruff/issues/16037
synced_at: 2026-01-07T13:12:16-06:00
---

# Infinite loop when fixing file with `cast` and format strings

---

_Issue opened by @qarmin on 2025-02-08 09:16_

### Description

```
ruff 0.9.5+2240 (a29009e4e 2025-02-07)
```


command
```
ruff_normal check problematic_file.py --select ALL --preview --output-format concise --no-cache --fix --unsafe-fixes --isolated
```
freeze ruff in
```
Downloading source file /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/ruff_python_parser/src/lexer/cursor.rs
0x0000555556d1c611 in ruff_python_parser::lexer::cursor::Cursor::first (self=<optimized out>) at crates/ruff_python_parser/src/lexer/cursor.rs:44                                              
warning: 44	crates/ruff_python_parser/src/lexer/cursor.rs: No such file or directory
(gdb) backtrace
#0  0x0000555556d1c611 in ruff_python_parser::lexer::cursor::Cursor::first (self=<optimized out>) at crates/ruff_python_parser/src/lexer/cursor.rs:44
#1  0x0000555556d1d4c0 in ruff_python_parser::lexer::Lexer::lex_fstring_middle_or_end (self=0x7fffffff31b0) at crates/ruff_python_parser/src/lexer.rs:796
#2  ruff_python_parser::lexer::Lexer::lex_token (self=0x7fffffff31b0) at crates/ruff_python_parser/src/lexer.rs:167
#3  0x0000555556d1cdb5 in ruff_python_parser::lexer::Lexer::next_token (self=0x7fffffff31b0) at crates/ruff_python_parser/src/lexer.rs:156
#4  0x0000555556d25579 in ruff_python_parser::token_source::TokenSource::next_non_trivia_token (self=<error reading variable: Cannot access memory at address 0x0>)
    at crates/ruff_python_parser/src/token_source.rs:142
#5  ruff_python_parser::token_source::TokenSource::peek (self=0x7fffffff31b0) at crates/ruff_python_parser/src/token_source.rs:98
#6  ruff_python_parser::parser::Parser::peek (self=0x7fffffff31b0) at crates/ruff_python_parser/src/parser/mod.rs:295
#7  ruff_python_parser::parser::Parser::parse_binary_expression_or_higher_recursive (self=0x7fffffff31b0, left=..., 
    left_precedence=ruff_python_parser::parser::expression::OperatorPrecedence::Initial, context=..., start=...) at crates/ruff_python_parser/src/parser/expression.rs:264
#8  0x0000555556d2475a in ruff_python_parser::parser::Parser::parse_binary_expression_or_higher (self=0x7fffffff31b0, 
    left_precedence=ruff_python_parser::parser::expression::OperatorPrecedence::Initial, context=...) at crates/ruff_python_parser/src/parser/expression.rs:241
#9  ruff_python_parser::parser::Parser::parse_simple_expression (self=0x7fffffff31b0, context=...) at crates/ruff_python_parser/src/parser/expression.rs:228
#10 ruff_python_parser::parser::Parser::parse_conditional_expression_or_higher_impl (self=0x7fffffff31b0, context=...) at crates/ruff_python_parser/src/parser/expression.rs:205
#11 ruff_python_parser::parser::Parser::parse_expression_list (self=0x7fffffff31b0, context=...) at crates/ruff_python_parser/src/parser/expression.rs:143
#12 0x0000555556d30e68 in ruff_python_parser::parser::Parser::parse_fstring_expression_element (self=0x7fffffff31b0, flags=...) at crates/ruff_python_parser/src/parser/expression.rs:1404
#13 ruff_python_parser::parser::expression::{impl#0}::parse_fstring_elements::{closure#0} (parser=0x7fffffff31b0) at crates/ruff_python_parser/src/parser/expression.rs:1333
#14 ruff_python_parser::parser::Parser::parse_list<ruff_python_parser::parser::expression::{impl#0}::parse_fstring_elements::{closure_env#0}> (self=0x7fffffff31b0, 
    recovery_context_kind=..., parse_element=...) at crates/ruff_python_parser/src/parser/mod.rs:479
#15 ruff_python_parser::parser::Parser::parse_fstring_elements (self=0x7fffffff31b0, flags=..., kind=ruff_python_parser::parser::FStringElementsKind::Regular)
    at crates/ruff_python_parser/src/parser/expression.rs:1330
#16 0x0000555556d2e19f in ruff_python_parser::parser::Parser::parse_fstring (self=0x7fffffff31b0) at crates/ruff_python_parser/src/parser/expression.rs:1307
#17 ruff_python_parser::parser::Parser::parse_strings (self=0x7fffffff31b0) at crates/ruff_python_parser/src/parser/expression.rs:1099
#18 0x0000555556d26e79 in ruff_python_parser::parser::Parser::parse_atom (self=0x7fffffff31b0) at crates/ruff_python_parser/src/parser/expression.rs:581
#19 ruff_python_parser::parser::Parser::parse_lhs_expression (self=0x7fffffff31b0, left_precedence=ruff_python_parser::parser::expression::OperatorPrecedence::Initial, context=...)
    at crates/ruff_python_parser/src/parser/expression.rs:404
#20 0x0000555556d2473f in ruff_python_parser::parser::Parser::parse_binary_expression_or_higher (self=0x7fffffff31b0, 
    left_precedence=ruff_python_parser::parser::expression::OperatorPrecedence::Initial, context=...) at crates/ruff_python_parser/src/parser/expression.rs:240
#21 ruff_python_parser::parser::Parser::parse_simple_expression (self=0x7fffffff31b0, context=...) at crates/ruff_python_parser/src/parser/expression.rs:228
#22 ruff_python_parser::parser::Parser::parse_conditional_expression_or_higher_impl (self=0x7fffffff31b0, context=...) at crates/ruff_python_parser/src/parser/expression.rs:205
#23 ruff_python_parser::parser::Parser::parse_expression_list (self=0x7fffffff31b0, context=...) at crates/ruff_python_parser/src/parser/expression.rs:143
#24 0x0000555556d57266 in ruff_python_parser::parser::Parser::parse_single_expression (self=0x7fffffff31b0) at crates/ruff_python_parser/src/parser/mod.rs:96
#25 ruff_python_parser::parser::Parser::parse (self=...) at crates/ruff_python_parser/src/parser/mod.rs:78
#26 0x00005555567a23e2 in ruff_python_parser::parse_expression (source=...) at crates/ruff_python_parser/src/lib.rs:136
#27 ruff_linter::rules::ruff::rules::missing_fstring_syntax::should_be_fstring (locator=<optimized out>, semantic=0x7fffffff42f0, literal=<optimized out>)
    at crates/ruff_linter/src/rules/ruff/rules/missing_fstring_syntax.rs:177
#28 ruff_linter::rules::ruff::rules::missing_fstring_syntax::missing_fstring_syntax (checker=0x7fffffff42b0, literal=<optimized out>)
    at crates/ruff_linter/src/rules/ruff/rules/missing_fstring_syntax.rs:107
#29 0x000055555694d11c in ruff_linter::checkers::ast::analyze::expression::expression (expr=<optimized out>, checker=0x7fffffff42b0)
    at crates/ruff_linter/src/checkers/ast/analyze/expression.rs:1541
#30 0x000055555676f6fe in ruff_linter::checkers::ast::{impl#5}::visit_expr (self=0x7fffffff42b0, expr=0x7ffff792e000) at crates/ruff_linter/src/checkers/ast/mod.rs:1656
#31 0x000055555676fde7 in ruff_linter::checkers::ast::Checker::visit_type_definition (self=0x7fffffff42b0, expr=0x7ffff792e000) at crates/ruff_linter/src/checkers/ast/mod.rs:1989
#32 ruff_linter::checkers::ast::{impl#5}::visit_expr (self=0x7fffffff42b0, expr=0x7ffff782c180) at crates/ruff_linter/src/checkers/ast/mod.rs:1344
#33 0x000055555676db74 in ruff_linter::checkers::ast::{impl#5}::visit_stmt (self=<optimized out>, stmt=0x7ffff7884e78) at crates/ruff_linter/src/checkers/ast/mod.rs:1082
#34 0x0000555556773c61 in ruff_linter::checkers::ast::{impl#5}::visit_body (self=0x7fffffff42b0, body=...) at crates/ruff_linter/src/checkers/ast/mod.rs:1768
#35 ruff_linter::checkers::ast::Checker::visit_deferred_functions (self=0x7fffffff42b0) at crates/ruff_linter/src/checkers/ast/mod.rs:2506
#36 ruff_linter::checkers::ast::Checker::visit_deferred (self=0x7fffffff42b0) at crates/ruff_linter/src/checkers/ast/mod.rs:2546
#37 ruff_linter::checkers::ast::check_ast (parsed=<optimized out>, locator=<optimized out>, stylist=<optimized out>, indexer=<optimized out>, noqa_line_for=<optimized out>, 
    settings=0x7fffffff8c18, noqa=<optimized out>, path=..., package=..., source_type=ruff_python_ast::PySourceType::Python, cell_offsets=..., notebook_index=...)
    at crates/ruff_linter/src/checkers/ast/mod.rs:2715
#38 0x0000555556778b9c in ruff_linter::linter::check_path (path=..., package=..., locator=<optimized out>, stylist=<optimized out>, indexer=<optimized out>, directives=<optimized out>, 
    settings=<optimized out>, noqa=<optimized out>, source_kind=<optimized out>, source_type=<optimized out>, parsed=<optimized out>) at crates/ruff_linter/src/linter.rs:148
#39 0x000055555677a787 in ruff_linter::linter::lint_fix (path=..., package=..., noqa=<optimized out>, unsafe_fixes=<optimized out>, settings=<optimized out>, source_kind=<optimized out>, 
    source_type=<optimized out>) at crates/ruff_linter/src/linter.rs:513
#40 0x00005555564ea7c4 in ruff::diagnostics::lint_path (path=..., package=..., settings=0x7fffffff8c18, cache=..., noqa=<optimized out>, fix_mode=<optimized out>, 
    unsafe_fixes=<optimized out>) at crates/ruff/src/diagnostics.rs:278
#41 0x00005555564fd705 in ruff::commands::check::lint_path::{closure#0} () at crates/ruff/src/commands/check.rs:195
#42 std::panicking::try::do_call<ruff::commands::check::lint_path::{closure_env#0}, core::result::Result<ruff::diagnostics::Diagnostics, anyhow::Error>> (
    data=<error reading variable: Cannot access memory at address 0x0>)
    at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panicking.rs:587
#43 std::panicking::try<core::result::Result<ruff::diagnostics::Diagnostics, anyhow::Error>, ruff::commands::check::lint_path::{closure_env#0}> (f=...)
    at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panicking.rs:550
#44 std::panic::catch_unwind<ruff::commands::check::lint_path::{closure_env#0}, core::result::Result<ruff::diagnostics::Diagnostics, anyhow::Error>> (f=...)
    at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panic.rs:358
#45 ruff_db::panic::catch_unwind<ruff::commands::check::lint_path::{closure_env#0}, core::result::Result<ruff::diagnostics::Diagnostics, anyhow::Error>> (f=...)
    at crates/ruff_db/src/panic.rs:83
#46 0x000055555640ad65 in ruff::commands::check::lint_path (path=..., package=<error reading variable: Cannot access memory at address 0x5c>, settings=0x7fff12d53f53, cache=..., 
    noqa=ruff_linter::settings::flags::Noqa::Enabled, fix_mode=ruff_linter::settings::flags::FixMode::Apply, unsafe_fixes=ruff_linter::settings::types::UnsafeFixes::Enabled)
    at crates/ruff/src/commands/check.rs:194
#47 0x0000555556541386 in ruff::commands::check::check::{closure#0} (resolved_file=<error reading variable: Cannot access memory at address 0x0>) at crates/ruff/src/commands/check.rs:96
#48 rayon::iter::filter_map::{impl#6}::consume<&core::result::Result<ruff_workspace::resolver::ResolvedFile, ignore::Error>, ruff::diagnostics::Diagnostics, rayon::iter::fold::FoldFolder<rayon::iter::reduce::ReduceFolder<ruff::commands::check::check::{closure_env#4}, (ruff::diagnostics::Diagnostics, u64)>, (ruff::diagnostics::Diagnostics, u64), ruff::commands::check::check::{closure_env#2}>, ruff::commands::check::check::{closure_env#0}> (self=..., item=<optimized out>)
    at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/filter_map.rs:123
#49 rayon::iter::plumbing::Folder::consume_iter<rayon::iter::filter_map::FilterMapFolder<rayon::iter::fold::FoldFolder<rayon::iter::reduce::ReduceFolder<ruff::commands::check::check::{closure_env#4}, (ruff::diagnostics::Diagnostics, u64)>, (ruff::diagnostics::Diagnostics, u64), ruff::commands::check::check::{closure_env#2}>, ruff::commands::check::check::{closure_env#0}>, &core::result::Result<ruff_workspace::resolver::ResolvedFile, ignore::Error>, core::slice::iter::Iter<core::result::Result<ruff_workspace::resolver::ResolvedFile, ignore::Error>>> (self=..., 
    iter=...) at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/plumbing/mod.rs:178
#50 rayon::iter::plumbing::Producer::fold_with<rayon::slice::IterProducer<core::result::Result<ruff_workspace::resolver::ResolvedFile, ignore::Error>>, rayon::iter::filter_map::FilterMapFolder<rayon::iter::fold::FoldFolder<rayon::iter::reduce::ReduceFolder<ruff::commands::check::check::{closure_env#4}, (ruff::diagnostics::Diagnostics, u64)>, (ruff::diagnostics::Diagnostics, u64), ruff::commands::check::check::{closure_env#2}>, ruff::commands::check::check::{closure_env#0}>> (self=..., folder=...)
    at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/plumbing/mod.rs:109
#51 0x000055555651ba9d in rayon::iter::plumbing::bridge_producer_consumer::helper<rayon::slice::IterProducer<core::result::Result<ruff_workspace::resolver::ResolvedFile, ignore::Error>>, rayon::iter::filter_map::FilterMapConsumer<rayon::iter::fold::FoldConsumer<rayon::iter::reduce::ReduceConsumer<ruff::commands::check::check::{closure_env#4}, ruff::commands::check::check::{closure_env#3}>, ruff::commands::check::check::{closure_env#1}, ruff::commands::check::check::{closure_env#2}>, ruff::commands::check::check::{closure_env#0}>> (len=1, migrated=<optimized out>, 
    splitter=..., producer=..., consumer=...) at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/plumbing/mod.rs:437
#52 0x000055555640a1ad in rayon::iter::plumbing::bridge_producer_consumer<rayon::slice::IterProducer<core::result::Result<ruff_workspace::resolver::ResolvedFile, ignore::Error>>, rayon::iter::filter_map::FilterMapConsumer<rayon::iter::fold::FoldConsumer<rayon::iter::reduce::ReduceConsumer<ruff::commands::check::check::{closure_env#4}, ruff::commands::check::check::{closure_env#3}>, ruff::commands::check::check::{closure_env#1}, ruff::commands::check::check::{closure_env#2}>, ruff::commands::check::check::{closure_env#0}>> (len=1, 
    producer=<error reading variable: access outside bounds of object referenced via synthetic pointer>, consumer=...)
    at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/plumbing/mod.rs:396
#53 rayon::iter::plumbing::bridge::{impl#0}::callback<rayon::iter::filter_map::FilterMapConsumer<rayon::iter::fold::FoldConsumer<rayon::iter::reduce::ReduceConsumer<ruff::commands::check::check::{closure_env#4}, ruff::commands::check::check::{closure_env#3}>, ruff::commands::check::check::{closure_env#1}, ruff::commands::check::check::{closure_env#2}>, ruff::commands::check::check::{closure_env#0}>, &core::result::Result<ruff_workspace::resolver::ResolvedFile, ignore::Error>, rayon::slice::IterProducer<core::result::Result<ruff_workspace::resolver::ResolvedFile, ignore::Error>>> (self=<error reading variable: access outside bounds of object referenced via synthetic pointer>, 
    producer=<error reading variable: access outside bounds of object referenced via synthetic pointer>)
    at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/plumbing/mod.rs:372
#54 rayon::slice::{impl#6}::with_producer<core::result::Result<ruff_workspace::resolver::ResolvedFile, ignore::Error>, rayon::iter::plumbing::bridge::Callback<rayon::iter::filter_map::FilterMapConsumer<rayon::iter::fold::FoldConsumer<rayon::iter::reduce::ReduceConsumer<ruff::commands::check::check::{closure_env#4}, ruff::commands::check::check::{closure_env#3}>, ruff::commands::check::check::{closure_env#1}, ruff::commands::check::check::{closure_env#2}>, ruff::commands::check::check::{closure_env#0}>>> (self=..., 
    callback=<error reading variable: access outside bounds of object referenced via synthetic pointer>)
    at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/slice/mod.rs:826
#55 rayon::iter::plumbing::bridge<rayon::slice::Iter<core::result::Result<ruff_workspace::resolver::ResolvedFile, ignore::Error>>, rayon::iter::filter_map::FilterMapConsumer<rayon::iter::fold::FoldConsumer<rayon::iter::reduce::ReduceConsumer<ruff::commands::check::check::{closure_env#4}, ruff::commands::check::check::{closure_env#3}>, ruff::commands::check::check::{closure_env#1}, ruff::commands::check::check::{closure_env#2}>, ruff::commands::check::check::{closure_env#0}>> (par_iter=..., consumer=...)
    at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/plumbing/mod.rs:356
#56 rayon::slice::{impl#5}::drive_unindexed<core::result::Result<ruff_workspace::resolver::ResolvedFile, ignore::Error>, rayon::iter::filter_map::FilterMapConsumer<rayon::iter::fold::FoldConsumer<rayon::iter::reduce::ReduceConsumer<ruff::commands::check::check::{closure_env#4}, ruff::commands::check::check::{closure_env#3}>, ruff::commands::check::check::{closure_env#1}, ruff::commands::check::check::{closure_env#2}>, ruff::commands::check::check::{closure_env#0}>> (self=..., consumer=...)
    at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/slice/mod.rs:802
#57 rayon::iter::filter_map::{impl#2}::drive_unindexed<rayon::slice::Iter<core::result::Result<ruff_workspace::resolver::ResolvedFile, ignore::Error>>, ruff::commands::check::check::{closure_env#0}, ruff::diagnostics::Diagnostics, rayon::iter::fold::FoldConsumer<rayon::iter::reduce::ReduceConsumer<ruff::commands::check::check::{closure_env#4}, ruff::commands::check::check::{closure_env#3}>, ruff::commands::check::check::{closure_env#1}, ruff::commands::check::check::{closure_env#2}>> (self=..., consumer=...)
    at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/filter_map.rs:46
#58 rayon::iter::fold::{impl#2}::drive_unindexed<(ruff::diagnostics::Diagnostics, u64), rayon::iter::filter_map::FilterMap<rayon::slice::Iter<core::result::Result<ruff_workspace::resolver::ResolvedFile, ignore::Error>>, ruff::commands::check::check::{closure_env#0}>, ruff::commands::check::check::{closure_env#1}, ruff::commands::check::check::{closure_env#2}, rayon::iter::reduce::ReduceConsumer<ruff::commands::check::check::{closure_env#4}, ruff::commands::check::check::{closure_env#3}>> (self=..., consumer=...)
    at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/fold.rs:59
#59 rayon::iter::reduce::reduce<rayon::iter::fold::Fold<rayon::iter::filter_map::FilterMap<rayon::slice::Iter<core::result::Result<ruff_workspace::resolver::ResolvedFile, ignore::Error>>, ruff::commands::check::check::{closure_env#0}>, ruff::commands::check::check::{closure_env#1}, ruff::commands::check::check::{closure_env#2}>, ruff::commands::check::check::{closure_env#4}, ruff::commands::check::check::{closure_env#3}, (ruff::diagnostics::Diagnostics, u64)> (pi=..., identity=..., reduce_op=...)
    at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/reduce.rs:15
#60 rayon::iter::ParallelIterator::reduce<rayon::iter::fold::Fold<rayon::iter::filter_map::FilterMap<rayon::slice::Iter<core::result::Result<ruff_workspace::resolver::ResolvedFile, ignore::Error>>, ruff::commands::check::check::{closure_env#0}>, ruff::commands::check::check::{closure_env#1}, ruff::commands::check::check::{closure_env#2}>, ruff::commands::check::check::{closure_env#4}, ruff::commands::check::check::{closure_env#3}> (self=...) at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/mod.rs:998
#61 ruff::commands::check::check (files=..., pyproject_config=<optimized out>, config_arguments=<optimized out>, cache=<optimized out>, noqa=ruff_linter::settings::flags::Noqa::Enabled, 
    fix_mode=ruff_linter::settings::flags::FixMode::Apply, unsafe_fixes=ruff_linter::settings::types::UnsafeFixes::Enabled) at crates/ruff/src/commands/check.rs:166
#62 0x000055555645b9a4 in ruff::check (args=..., global_options=...) at crates/ruff/src/lib.rs:423
#63 0x000055555645843b in ruff::run () at crates/ruff/src/lib.rs:195
#64 0x0000555556592838 in ruff::main () at crates/ruff/src/main.rs:44

```

File content
```
def _():
    ne_ite=()
    cast("%s"%ne_ite)
from typing import(cast)
```

File - [a.py.zip](https://github.com/user-attachments/files/18717724/a.py.zip)

---

_Label `bug` added by @AlexWaygood on 2025-02-08 14:00_

---

_Label `parser` added by @MichaReiser on 2025-02-08 15:47_

---

_Label `parser` removed by @dylwil3 on 2025-02-08 23:20_

---

_Label `rule` added by @dylwil3 on 2025-02-08 23:20_

---

_Label `fixes` added by @dylwil3 on 2025-02-08 23:20_

---

_Renamed from "Freeze when fixing file inside `ruff_python_parser`" to "Infinite loop when fixing file with `cast` and format strings" by @dylwil3 on 2025-02-08 23:22_

---

_Label `preview` added by @dylwil3 on 2025-02-08 23:24_

---

_Referenced in [astral-sh/ruff#16047](../../astral-sh/ruff/pulls/16047.md) on 2025-02-08 23:25_

---

_Comment by @dylwil3 on 2025-02-08 23:27_

A fun puzzle!

This turns out to be an infinite loop and unrelated to the parser. The original example has a more minimal reproduction with just enabling `TC006`,`UP031`,`UP032`, and `RUF027`. The `UP` rules just serve to turn the format strings into an f-string, so the real problem is the interaction between `TC006` and `RUF027`: the former adds quotes around the f-string, and the latter turns the string to an f-string... ad infinitum.

---

_Comment by @AlexWaygood on 2025-02-09 13:12_

Brilliant work @dylwil3. How did you narrow that down?!

A minimal repro here, then, is to run `ruff check --no-cache foo.py --select=TC006,RUF027 --diff --unsafe-fixes --isolated --preview`, where `foo.py` has these contents:

```py
from typing import cast

x = 42
cast(f"{x}")
```

---

_Referenced in [astral-sh/ruff#16054](../../astral-sh/ruff/pulls/16054.md) on 2025-02-09 16:09_

---

_Comment by @dylwil3 on 2025-02-09 16:11_

> Brilliant work @dylwil3. How did you narrow that down?!

By an embarrassingly manual binary search 😅 ... it helped that at some point I guessed it was a loop and turned the max iterations down to 10 so I could reproduce more quickly.

---

_Closed by @dylwil3 on 2025-02-09 16:16_

---

_Referenced in [astral-sh/ruff#16637](../../astral-sh/ruff/pulls/16637.md) on 2025-03-11 16:43_

---
