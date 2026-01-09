---
number: 7282
title: Separate stable and preview benchmarks
type: issue
state: closed
author: zanieb
labels:
  - internal
  - help wanted
assignees: []
created_at: 2023-09-11T19:11:40Z
updated_at: 2024-08-28T06:04:52Z
url: https://github.com/astral-sh/ruff/issues/7282
synced_at: 2026-01-07T13:12:15-06:00
---

# Separate stable and preview benchmarks

---

_Issue opened by @zanieb on 2023-09-11 19:11_

Prompted by discussion at https://github.com/astral-sh/ruff/issues/7208#issuecomment-1709263037

Currently, benchmarks are always run including preview rules. However, it'd be good to split benchmarks into separate `preview` and `stable` sections so we can be more strict about performance regressions in stable.

---

_Label `internal` added by @zanieb on 2023-09-11 19:11_

---

_Label `help wanted` added by @zanieb on 2023-09-11 19:11_

---

_Comment by @calumy on 2024-08-26 23:00_

Having checked a [recent CI run](https://github.com/astral-sh/ruff/actions/runs/10567930679/job/29277948725), codspeed appears to run for default rules, all rules and all rules with preview. Would it be possible to clarify if this issue is still relevant, and if so, could you point me in the right direction of how to get started? 

```
______            __ _____                         __
  / ____/____   ____/ // ___/ ____   ___   ___   ____/ /
 / /    / __ \ / __  / \__ \ / __ \ / _ \ / _ \ / __  /
/ /___ / /_/ // /_/ / ___/ // /_/ //  __//  __// /_/ /
\____/ \____/ \__,_/ /____// .___/ \___/ \___/ \__,_/
  https://codspeed.io/     /_/          runner v3.0.0

Preparing the environment
  Environment ready
Running the benchmarks
     Collected 5 benchmark suite(s) to run
       Running ruff_benchmark linter
  Harness: codspeed-criterion-compat v2.6.0
  Measured: crates/ruff_benchmark/benches/linter.rs::default_rules::benchmark_default_rules::linter/default-rules[numpy/globals.py]
  Measured: crates/ruff_benchmark/benches/linter.rs::default_rules::benchmark_default_rules::linter/default-rules[unicode/pypinyin.py]
  Measured: crates/ruff_benchmark/benches/linter.rs::default_rules::benchmark_default_rules::linter/default-rules[pydantic/types.py]
  Measured: crates/ruff_benchmark/benches/linter.rs::default_rules::benchmark_default_rules::linter/default-rules[numpy/ctypeslib.py]
  Measured: crates/ruff_benchmark/benches/linter.rs::default_rules::benchmark_default_rules::linter/default-rules[large/dataset.py]
  Measured: crates/ruff_benchmark/benches/linter.rs::all_rules::benchmark_all_rules::linter/all-rules[numpy/globals.py]
  Measured: crates/ruff_benchmark/benches/linter.rs::all_rules::benchmark_all_rules::linter/all-rules[unicode/pypinyin.py]
  Measured: crates/ruff_benchmark/benches/linter.rs::all_rules::benchmark_all_rules::linter/all-rules[pydantic/types.py]
  Measured: crates/ruff_benchmark/benches/linter.rs::all_rules::benchmark_all_rules::linter/all-rules[numpy/ctypeslib.py]
  Measured: crates/ruff_benchmark/benches/linter.rs::all_rules::benchmark_all_rules::linter/all-rules[large/dataset.py]
  Measured: crates/ruff_benchmark/benches/linter.rs::preview_rules::benchmark_preview_rules::linter/all-with-preview-rules[numpy/globals.py]
  Measured: crates/ruff_benchmark/benches/linter.rs::preview_rules::benchmark_preview_rules::linter/all-with-preview-rules[unicode/pypinyin.py]
  Measured: crates/ruff_benchmark/benches/linter.rs::preview_rules::benchmark_preview_rules::linter/all-with-preview-rules[pydantic/types.py]
  Measured: crates/ruff_benchmark/benches/linter.rs::preview_rules::benchmark_preview_rules::linter/all-with-preview-rules[numpy/ctypeslib.py]
  Measured: crates/ruff_benchmark/benches/linter.rs::preview_rules::benchmark_preview_rules::linter/all-with-preview-rules[large/dataset.py]
          Done running linter
       Running ruff_benchmark lexer
  Harness: codspeed-criterion-compat v2.6.0
  Measured: crates/ruff_benchmark/benches/lexer.rs::lexer::benchmark_lexer::lexer[numpy/globals.py]
  Measured: crates/ruff_benchmark/benches/lexer.rs::lexer::benchmark_lexer::lexer[unicode/pypinyin.py]
  Measured: crates/ruff_benchmark/benches/lexer.rs::lexer::benchmark_lexer::lexer[pydantic/types.py]
  Measured: crates/ruff_benchmark/benches/lexer.rs::lexer::benchmark_lexer::lexer[numpy/ctypeslib.py]
  Measured: crates/ruff_benchmark/benches/lexer.rs::lexer::benchmark_lexer::lexer[large/dataset.py]
          Done running lexer
       Running ruff_benchmark formatter
  Harness: codspeed-criterion-compat v2.6.0
  Measured: crates/ruff_benchmark/benches/formatter.rs::formatter::benchmark_formatter::formatter[numpy/globals.py]
  Measured: crates/ruff_benchmark/benches/formatter.rs::formatter::benchmark_formatter::formatter[unicode/pypinyin.py]
  Measured: crates/ruff_benchmark/benches/formatter.rs::formatter::benchmark_formatter::formatter[pydantic/types.py]
  Measured: crates/ruff_benchmark/benches/formatter.rs::formatter::benchmark_formatter::formatter[numpy/ctypeslib.py]
  Measured: crates/ruff_benchmark/benches/formatter.rs::formatter::benchmark_formatter::formatter[large/dataset.py]
          Done running formatter
       Running ruff_benchmark parser
  Harness: codspeed-criterion-compat v2.6.0
  Measured: crates/ruff_benchmark/benches/parser.rs::parser::benchmark_parser::parser[numpy/globals.py]
  Measured: crates/ruff_benchmark/benches/parser.rs::parser::benchmark_parser::parser[unicode/pypinyin.py]
  Measured: crates/ruff_benchmark/benches/parser.rs::parser::benchmark_parser::parser[pydantic/types.py]
  Measured: crates/ruff_benchmark/benches/parser.rs::parser::benchmark_parser::parser[numpy/ctypeslib.py]
  Measured: crates/ruff_benchmark/benches/parser.rs::parser::benchmark_parser::parser[large/dataset.py]
          Done running parser
       Running ruff_benchmark red_knot
  Harness: codspeed-criterion-compat v2.6.0
  Measured: crates/ruff_benchmark/benches/red_knot.rs::check_file::benchmark_cold::red_knot_check_file[cold]
  Measured: crates/ruff_benchmark/benches/red_knot.rs::check_file::benchmark_incremental::red_knot_check_file[incremental]
          Done running red_knot
      Finished running 5 benchmark suite(s)
```

---

_Comment by @dhruvmanila on 2024-08-28 06:04_

Yeah, I think this is resolved in https://github.com/astral-sh/ruff/pull/8865. Thanks for the heads up.

---

_Closed by @dhruvmanila on 2024-08-28 06:04_

---
