```yaml
number: 3880
title: "Implement Pylint's `cyclic-import` rule (`PLR0401`)"
type: pull_request
state: closed
author: chanman3388
labels: []
assignees: []
base: main
head: PLR0401
created_at: 2023-04-05T00:26:07Z
updated_at: 2023-10-20T18:04:07Z
url: https://github.com/astral-sh/ruff/pull/3880
synced_at: 2026-01-12T02:32:41Z
```

# Implement Pylint's `cyclic-import` rule (`PLR0401`)

---

_Pull request opened by @chanman3388 on 2023-04-05 00:26_

Implement https://pylint.pycqa.org/en/v2.13.9/messages/refactor/cyclic-import.html
Issue #970 

The principle here is we utilise the `ImportMap` to traverse the modules which may or may not import each other. When a module has all its imported modules checked, it is counted as 'fully visited'. 
When considering a traversed path, each module imported along the way is kept in a stack, a cycle is detected when the module we're currently inspecting is already contained within this stack. The appropriate part of the stack is copied into a new `Vec` and pushed onto a `Vec` which contains all the cycles currently detected from the current module.

By the nature of the implementation, cycles of other modules traversed are detected, but these won't be reported immediately, instead stored in a `HashMap`. When the cyclic import rule is called, a check of whether we have already the cycles for this module is done to avoid duplicate work.

---

_Comment by @chanman3388 on 2023-04-05 00:26_

Consider this file tree
```
grand
├── __init__.py
├── a.py (from .parent.child import a; from .parent import a; from grand.parent.child import b)
└── parent
    ├── __init__.py
    ├── a.py (from .child import a, b; from .. import a)
    └── child
        ├── __init__.py
        ├── a.py (from .. import a; from ... import a)
        ├── b.py (from . import a)
        └── c.py --empty--
```

---

_Comment by @chanman3388 on 2023-04-05 00:29_

Pylint outputs:
```
************* Module grand.parent.child.b
grand/parent/child/b.py:2:0: W0611: Unused import a (unused-import)
grand/parent/child/b.py:1:0: R0401: Cyclic import (grand.parent.a -> grand.parent.child.b -> grand.parent.child.a) (cyclic-import)
grand/parent/child/b.py:1:0: R0401: Cyclic import (grand.a -> grand.parent.a -> grand.parent.child.b -> grand.parent.child.a) (cyclic-import)
grand/parent/child/b.py:1:0: R0401: Cyclic import (grand.a -> grand.parent.child.b -> grand.parent.child.a) (cyclic-import)
grand/parent/child/b.py:1:0: R0401: Cyclic import (grand.a -> grand.parent.child.a) (cyclic-import)
grand/parent/child/b.py:1:0: R0401: Cyclic import (grand.a -> grand.parent.a) (cyclic-import)
grand/parent/child/b.py:1:0: R0401: Cyclic import (grand.parent.a -> grand.parent.child.a) (cyclic-import)
```

---

_Comment by @chanman3388 on 2023-04-05 00:31_

Ruff outputs
```
grand/a.py:3:21: PLR0401 Cyclic import (grand.a -> grand.parent.a -> grand.parent.child.b -> grand.parent.child.a) (cyclic-import)
grand/a.py:3:21: PLR0401 Cyclic import (grand.a -> grand.parent.child.a -> grand.parent.a) (cyclic-import)
grand/a.py:3:21: PLR0401 Cyclic import (grand.a -> grand.parent.child.a) (cyclic-import)
grand/a.py:3:21: PLR0401 Cyclic import (grand.a -> grand.parent.a -> grand.parent.child.a) (cyclic-import)
grand/a.py:3:21: PLR0401 Cyclic import (grand.a -> grand.parent.child.b -> grand.parent.child.a -> grand.parent.a) (cyclic-import)
grand/a.py:3:21: PLR0401 Cyclic import (grand.a -> grand.parent.a) (cyclic-import)
grand/a.py:3:21: PLR0401 Cyclic import (grand.a -> grand.parent.child.b -> grand.parent.child.a) (cyclic-import)
grand/parent/a.py:2:23: PLR0401 Cyclic import (grand.parent.a -> grand.a) (cyclic-import)
grand/parent/a.py:2:23: PLR0401 Cyclic import (grand.parent.a -> grand.a -> grand.parent.child.b -> grand.parent.child.a) (cyclic-import)
grand/parent/a.py:2:23: PLR0401 Cyclic import (grand.parent.a -> grand.parent.child.a) (cyclic-import)
grand/parent/a.py:2:23: PLR0401 Cyclic import (grand.parent.a -> grand.parent.child.b -> grand.parent.child.a -> grand.a) (cyclic-import)
grand/parent/a.py:2:23: PLR0401 Cyclic import (grand.parent.a -> grand.parent.child.a -> grand.a) (cyclic-import)
grand/parent/a.py:2:23: PLR0401 Cyclic import (grand.parent.a -> grand.parent.child.b -> grand.parent.child.a) (cyclic-import)
grand/parent/a.py:2:23: PLR0401 Cyclic import (grand.parent.a -> grand.a -> grand.parent.child.a) (cyclic-import)
grand/parent/child/a.py:3:17: PLR0401 Cyclic import (grand.parent.child.a -> grand.parent.a) (cyclic-import)
grand/parent/child/a.py:3:17: PLR0401 Cyclic import (grand.parent.child.a -> grand.parent.a -> grand.a -> grand.parent.child.b) (cyclic-import)
grand/parent/child/a.py:3:17: PLR0401 Cyclic import (grand.parent.child.a -> grand.a) (cyclic-import)
grand/parent/child/a.py:3:17: PLR0401 Cyclic import (grand.parent.child.a -> grand.a -> grand.parent.child.b) (cyclic-import)
grand/parent/child/a.py:3:17: PLR0401 Cyclic import (grand.parent.child.a -> grand.a -> grand.parent.a) (cyclic-import)
grand/parent/child/a.py:3:17: PLR0401 Cyclic import (grand.parent.child.a -> grand.parent.a -> grand.parent.child.b) (cyclic-import)
grand/parent/child/a.py:3:17: PLR0401 Cyclic import (grand.parent.child.a -> grand.a -> grand.parent.a -> grand.parent.child.b) (cyclic-import)
grand/parent/child/a.py:3:17: PLR0401 Cyclic import (grand.parent.child.a -> grand.parent.a -> grand.a) (cyclic-import)
grand/parent/child/b.py:2:15: PLR0401 Cyclic import (grand.parent.child.b -> grand.parent.child.a -> grand.a -> grand.parent.a) (cyclic-import)
grand/parent/child/b.py:2:15: PLR0401 Cyclic import (grand.parent.child.b -> grand.parent.child.a -> grand.parent.a -> grand.a) (cyclic-import)
grand/parent/child/b.py:2:15: PLR0401 Cyclic import (grand.parent.child.b -> grand.parent.child.a -> grand.a) (cyclic-import)
grand/parent/child/b.py:2:15: PLR0401 Cyclic import (grand.parent.child.b -> grand.parent.child.a -> grand.parent.a) (cyclic-import)
```

That is to say, that for each module, each cycle it is involved in will generate a message.
The example test data, I hope, is more complicated that one would ever come across in practice.

---

_Converted to draft by @chanman3388 on 2023-04-05 00:31_

---

_Comment by @chanman3388 on 2023-04-05 00:46_

@charliermarsh I was going to add some more unittests, namely to test more complicated cycles and to test `pylint/rules/cyclic_import.rs#cyclic_import`, would we want to consider adding the functionality to add folders as fixtures parallel to this?

---

_Comment by @github-actions[bot] on 2023-04-05 01:16_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.0±0.09ms     2.7 MB/sec    1.01     15.1±0.10ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.00ms     4.6 MB/sec    1.01      3.7±0.01ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.00    374.8±1.82µs     7.9 MB/sec    1.02    381.1±0.82µs     7.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.01ms     4.1 MB/sec    1.01      6.3±0.01ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.4±0.02ms     5.5 MB/sec    1.02      7.6±0.01ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1595.2±5.17µs    10.4 MB/sec    1.01   1616.4±2.33µs    10.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    173.8±1.68µs    17.0 MB/sec    1.03    179.3±0.99µs    16.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.01ms     7.6 MB/sec    1.01      3.4±0.01ms     7.5 MB/sec
parser/large/dataset.py                    1.01      5.8±0.00ms     7.0 MB/sec    1.00      5.7±0.01ms     7.1 MB/sec
parser/numpy/ctypeslib.py                  1.00   1134.2±1.60µs    14.7 MB/sec    1.00   1130.9±2.51µs    14.7 MB/sec
parser/numpy/globals.py                    1.02    117.0±0.78µs    25.2 MB/sec    1.00    115.1±0.23µs    25.6 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.00ms    10.3 MB/sec    1.00      2.5±0.00ms    10.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.03     17.3±0.15ms     2.3 MB/sec    1.00     16.8±0.12ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      4.4±0.03ms     3.8 MB/sec    1.00      4.3±0.03ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.05    448.4±5.08µs     6.6 MB/sec    1.00    428.5±6.04µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.3±0.05ms     3.5 MB/sec    1.00      7.1±0.04ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.01      8.6±0.07ms     4.8 MB/sec    1.00      8.5±0.04ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1807.1±11.71µs     9.2 MB/sec    1.00  1781.5±15.05µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.03    199.2±1.59µs    14.8 MB/sec    1.00    194.0±7.75µs    15.2 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.9±0.02ms     6.6 MB/sec    1.00      3.8±0.07ms     6.7 MB/sec
parser/large/dataset.py                    1.00      6.7±0.03ms     6.1 MB/sec    1.10      7.4±0.03ms     5.5 MB/sec
parser/numpy/ctypeslib.py                  1.00   1249.3±7.96µs    13.3 MB/sec    1.10   1373.9±8.45µs    12.1 MB/sec
parser/numpy/globals.py                    1.00    129.7±0.84µs    22.8 MB/sec    1.08    140.0±0.82µs    21.1 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.02ms     9.1 MB/sec    1.10      3.1±0.01ms     8.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @andersk on 2023-04-05 01:38_

Simple test that crashes this:

```console
$ echo 'import a' > a.py

$ RUST_BACKTRACE=1 cargo run --bin=ruff -- -n --select=PLR0401 a.py
warning: /home/anders/rust/ruff/crates/ruff_diagnostics/Cargo.toml: `default-features` is ignored for serde, since `default-features` was not specified for `workspace.dependencies.serde`, this could become a hard error in the future
warning: /home/anders/rust/ruff/crates/ruff_text_size/Cargo.toml: `default-features` is ignored for serde, since `default-features` was not specified for `workspace.dependencies.serde`, this could become a hard error in the future
    Finished dev [unoptimized + debuginfo] target(s) in 0.19s
warning: the following packages contain code that will be rejected by a future version of Rust: lalrpop v0.19.8, nom v5.1.2
note: to see what the problems were, use the option `--future-incompat-report`, or run `cargo report future-incompatibilities --id 1`
     Running `/home/anders/.cache/cargo-target/debug/ruff -n --select=PLR0401 a.py`

error: `ruff` crashed. This indicates a bug in `ruff`. If you could open an issue at:

https://github.com/charliermarsh/ruff/issues/new?title=%5BPanic%5D

quoting the executed command, along with the relevant file contents and `pyproject.toml` settings, we'd be very appreciative!

thread 'main' panicked at 'called `Option::unwrap()` on a `None` value', crates/ruff/src/rules/pylint/rules/cyclic_import.rs:87:46
stack backtrace:
   0: rust_begin_unwind
             at /rustc/2036fdd24f77d607dcfaa24c48fbe85d3f785823/library/std/src/panicking.rs:577:5
   1: core::panicking::panic_fmt
             at /rustc/2036fdd24f77d607dcfaa24c48fbe85d3f785823/library/core/src/panicking.rs:67:14
   2: core::panicking::panic
             at /rustc/2036fdd24f77d607dcfaa24c48fbe85d3f785823/library/core/src/panicking.rs:117:5
   3: core::option::Option<T>::unwrap
             at /rustc/2036fdd24f77d607dcfaa24c48fbe85d3f785823/library/core/src/option.rs:952:21
   4: ruff::rules::pylint::rules::cyclic_import::cyclic_import
             at ./crates/ruff/src/rules/pylint/rules/cyclic_import.rs:87:38
   5: ruff_cli::commands::run::run
             at ./crates/ruff_cli/src/commands/run.rs:167:17
   6: ruff_cli::check
             at ./crates/ruff_cli/src/lib.rs:250:13
   7: ruff_cli::run
             at ./crates/ruff_cli/src/lib.rs:89:40
   8: ruff::main
             at ./crates/ruff_cli/src/bin/ruff.rs:45:11
   9: core::ops::function::FnOnce::call_once
             at /rustc/2036fdd24f77d607dcfaa24c48fbe85d3f785823/library/core/src/ops/function.rs:250:5
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
```

---

_Comment by @andersk on 2023-04-21 21:13_

Still easy to crash:

```console
$ touch __init__.py

$ echo 'from . import a' > a.py

$ RUST_BACKTRACE=1 cargo run --bin=ruff -- -n --select=PLR0401 a.py
warning: /home/anders/rust/ruff/crates/ruff_text_size/Cargo.toml: `default-features` is ignored for serde, since `default-features` was not specified for `workspace.dependencies.serde`, this could become a hard error in the future
warning: /home/anders/rust/ruff/crates/ruff_diagnostics/Cargo.toml: `default-features` is ignored for serde, since `default-features` was not specified for `workspace.dependencies.serde`, this could become a hard error in the future
    Finished dev [unoptimized + debuginfo] target(s) in 0.19s
warning: the following packages contain code that will be rejected by a future version of Rust: lalrpop v0.19.8, nom v5.1.2
note: to see what the problems were, use the option `--future-incompat-report`, or run `cargo report future-incompatibilities --id 1`
     Running `/home/anders/.cache/cargo-target/debug/ruff -n --select=PLR0401 a.py`

error: `ruff` crashed. This indicates a bug in `ruff`. If you could open an issue at:

https://github.com/charliermarsh/ruff/issues/new?title=%5BPanic%5D

quoting the executed command, along with the relevant file contents and `pyproject.toml` settings, we'd be very appreciative!

thread 'main' panicked at 'called `Option::unwrap()` on a `None` value', crates/ruff/src/rules/pylint/rules/cyclic_import.rs:149:75
stack backtrace:
   0: rust_begin_unwind
             at /rustc/28a29282f6dde2e4aba6e1e4cfea5c9430a00217/library/std/src/panicking.rs:577:5
   1: core::panicking::panic_fmt
             at /rustc/28a29282f6dde2e4aba6e1e4cfea5c9430a00217/library/core/src/panicking.rs:67:14
   2: core::panicking::panic
             at /rustc/28a29282f6dde2e4aba6e1e4cfea5c9430a00217/library/core/src/panicking.rs:117:5
   3: core::option::Option<T>::unwrap
             at /rustc/28a29282f6dde2e4aba6e1e4cfea5c9430a00217/library/core/src/option.rs:952:21
   4: ruff::rules::pylint::rules::cyclic_import::cyclic_import::{{closure}}
             at ./crates/ruff/src/rules/pylint/rules/cyclic_import.rs:149:58
   5: <core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::find
             at /rustc/28a29282f6dde2e4aba6e1e4cfea5c9430a00217/library/core/src/slice/iter/macros.rs:249:24
   6: ruff::rules::pylint::rules::cyclic_import::cyclic_import
             at ./crates/ruff/src/rules/pylint/rules/cyclic_import.rs:147:29
   7: ruff_cli::commands::run::run
             at ./crates/ruff_cli/src/commands/run.rs:169:17
   8: ruff_cli::check
             at ./crates/ruff_cli/src/lib.rs:250:13
   9: ruff_cli::run
             at ./crates/ruff_cli/src/lib.rs:89:40
  10: ruff::main
             at ./crates/ruff_cli/src/bin/ruff.rs:45:11
  11: core::ops::function::FnOnce::call_once
             at /rustc/28a29282f6dde2e4aba6e1e4cfea5c9430a00217/library/core/src/ops/function.rs:250:5
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
```

---

_Comment by @chanman3388 on 2023-04-21 21:47_

> Still easy to crash:
> 
> ```
> $ touch __init__.py
> 
> $ echo 'from . import a' > a.py
> 
> $ RUST_BACKTRACE=1 cargo run --bin=ruff -- -n --select=PLR0401 a.py
> warning: /home/anders/rust/ruff/crates/ruff_text_size/Cargo.toml: `default-features` is ignored for serde, since `default-features` was not specified for `workspace.dependencies.serde`, this could become a hard error in the future
> warning: /home/anders/rust/ruff/crates/ruff_diagnostics/Cargo.toml: `default-features` is ignored for serde, since `default-features` was not specified for `workspace.dependencies.serde`, this could become a hard error in the future
>     Finished dev [unoptimized + debuginfo] target(s) in 0.19s
> warning: the following packages contain code that will be rejected by a future version of Rust: lalrpop v0.19.8, nom v5.1.2
> note: to see what the problems were, use the option `--future-incompat-report`, or run `cargo report future-incompatibilities --id 1`
>      Running `/home/anders/.cache/cargo-target/debug/ruff -n --select=PLR0401 a.py`
> 
> error: `ruff` crashed. This indicates a bug in `ruff`. If you could open an issue at:
> 
> https://github.com/charliermarsh/ruff/issues/new?title=%5BPanic%5D
> 
> quoting the executed command, along with the relevant file contents and `pyproject.toml` settings, we'd be very appreciative!
> 
> thread 'main' panicked at 'called `Option::unwrap()` on a `None` value', crates/ruff/src/rules/pylint/rules/cyclic_import.rs:149:75
> stack backtrace:
>    0: rust_begin_unwind
>              at /rustc/28a29282f6dde2e4aba6e1e4cfea5c9430a00217/library/std/src/panicking.rs:577:5
>    1: core::panicking::panic_fmt
>              at /rustc/28a29282f6dde2e4aba6e1e4cfea5c9430a00217/library/core/src/panicking.rs:67:14
>    2: core::panicking::panic
>              at /rustc/28a29282f6dde2e4aba6e1e4cfea5c9430a00217/library/core/src/panicking.rs:117:5
>    3: core::option::Option<T>::unwrap
>              at /rustc/28a29282f6dde2e4aba6e1e4cfea5c9430a00217/library/core/src/option.rs:952:21
>    4: ruff::rules::pylint::rules::cyclic_import::cyclic_import::{{closure}}
>              at ./crates/ruff/src/rules/pylint/rules/cyclic_import.rs:149:58
>    5: <core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::find
>              at /rustc/28a29282f6dde2e4aba6e1e4cfea5c9430a00217/library/core/src/slice/iter/macros.rs:249:24
>    6: ruff::rules::pylint::rules::cyclic_import::cyclic_import
>              at ./crates/ruff/src/rules/pylint/rules/cyclic_import.rs:147:29
>    7: ruff_cli::commands::run::run
>              at ./crates/ruff_cli/src/commands/run.rs:169:17
>    8: ruff_cli::check
>              at ./crates/ruff_cli/src/lib.rs:250:13
>    9: ruff_cli::run
>              at ./crates/ruff_cli/src/lib.rs:89:40
>   10: ruff::main
>              at ./crates/ruff_cli/src/bin/ruff.rs:45:11
>   11: core::ops::function::FnOnce::call_once
>              at /rustc/28a29282f6dde2e4aba6e1e4cfea5c9430a00217/library/core/src/ops/function.rs:250:5
> note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
> ```

Yeah I figured this might still be the case, I'll sort it out later.

---

_Marked ready for review by @chanman3388 on 2023-04-22 02:50_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pylint/rules/cyclic_import.rs`:54 on 2023-04-23 03:57_

Using `strings` in hash sets is expensive because hasing requires iterating over the whole string (and it may be necessary to do so more than once if the hash map needs to grow). Could we instead have a single map that stores a `name` to `id` mapping, where `id` is a zero type wrapper around e.g. a `u32` type? It would allow us to only hash the string once rather than once per map.

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pylint/rules/cyclic_import.rs`:52 on 2023-04-23 03:58_

What's the reason for using `Arc` vs eg. `Box`?

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pylint/rules/cyclic_import.rs`:64 on 2023-04-23 03:59_

Nit: It could be useful to put all these related data structures into a single struct that you can than pass around (and can expose semantically meaningful methods)

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pylint/rules/cyclic_import.rs`:102 on 2023-04-23 04:00_

Can we document why calling `unwrap` is safe or avoid calling `unwrap`?

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/commands/run.rs`:165 on 2023-04-23 04:04_

Nit: I first assumed that this is the body of the `for` loop because of how it is nested. It could make sense to assign the result of this expression to a variable and then use it in the for loop

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/imports.rs`:124 on 2023-04-23 04:06_

It's unclear to me why it's necessary to implement `AsRef<ModuleImport`. Could you give me an example of how it is used?

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/commands/run.rs`:177 on 2023-04-23 04:09_

Can you test if ruff correctly renders the diagnostic using `--show-source`. You may need to set the source code if this is not the case.

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pylint/rules/cyclic_import.rs`:155 on 2023-04-23 04:10_

Can we document (here and in other places where we call `unwrap`) why calling `unwrap` is safe or, even better, restructure the code so that calling `unwrap` is unnecessary?

---

_@MichaReiser reviewed on 2023-04-23 04:12_

Impressive. 

Can we add some integration tests that test the generated diagnostics? 

How does that work in ruff-lsp when the user has unsaved changes in multiple files?

---

_@chanman3388 reviewed on 2023-04-23 04:55_

---

_Review comment by @chanman3388 on `crates/ruff/src/rules/pylint/rules/cyclic_import.rs`:54 on 2023-04-23 04:55_

This is a good idea, I'll look into it!

---

_@chanman3388 reviewed on 2023-04-23 05:01_

---

_Review comment by @chanman3388 on `crates/ruff/src/rules/pylint/rules/cyclic_import.rs`:52 on 2023-04-23 05:01_

I'm still reasonably new to this so do correct me if I'm wrong. The way I've done it I believe that we should have the same `str` potentially owned by multiple owners, though this might also not always be the case and I may indeed have a different `Arc<str>` which does contain the same value as another. For a `Box<T>` only one thing can own `T`. Cloning a `Box<T>` requires cloning the contents of the `Box` which isn't what I want.

---

_@chanman3388 reviewed on 2023-04-23 05:03_

---

_Review comment by @chanman3388 on `crates/ruff/src/rules/pylint/rules/cyclic_import.rs`:102 on 2023-04-23 05:03_

Sure, I'll need to double check, but I think that at this point, because this function was called to do the exact same thing one the same arguments when creating the `ImportMap`, then it must be safe here. I will need to check that is unequivocally true.

---

_Review comment by @chanman3388 on `crates/ruff/src/rules/pylint/rules/cyclic_import.rs`:155 on 2023-04-23 05:05_

Sure, I think here though I should just be able to use `the_rest[0]`, would that be more idiomatic?

---

_@chanman3388 reviewed on 2023-04-23 05:05_

---

_@chanman3388 reviewed on 2023-04-23 05:11_

---

_Review comment by @chanman3388 on `crates/ruff_python_ast/src/imports.rs`:124 on 2023-04-23 05:11_

                       `imports.module_to_imports[module_name][0].as_ref().into(),`
Line 125 of `cylic_import.rs`
Could I have avoided it?
`imports.module_to_imports[module_name]` yields a `ModuleImport`, I made `impl From<&ModuleImport> for Range` only so I need a reference to a `ModuleImport`.

---

_Comment by @chanman3388 on 2023-04-23 05:13_

> Impressive. 
> 
> Can we add some integration tests that test the generated diagnostics? 
> 
> How does that work in ruff-lsp when the user has unsaved changes in multiple files?

If I understand you correctly, I _think_ at the moment we can't do the former as the integration tests only work on individual files, whereas to check I get the correct cycles I need to lint a package.

Short answer for the latter is, I don't know, I'll find that out.

Thanks for the thorough review!

---

_@andersk reviewed on 2023-04-23 06:09_

---

_Review comment by @andersk on `crates/ruff_python_ast/src/imports.rs`:124 on 2023-04-23 06:09_

You can write `(&imports.module_to_imports[module_name][0]).into()`.

---

_@chanman3388 reviewed on 2023-04-23 14:02_

---

_Review comment by @chanman3388 on `crates/ruff_python_ast/src/imports.rs`:124 on 2023-04-23 14:02_

@andersk, thanks! I thought I tried this, but I must've been mistaken. Thanks for the review/bug finding btw.

---

_@MichaReiser reviewed on 2023-04-23 16:22_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pylint/rules/cyclic_import.rs`:52 on 2023-04-23 16:22_

That's a good point! Using refcounting here does make sense to avoid allocating the same module name many times. Would it be possible to use an `Rc` instead of `Arc` or is this data structure shared across thread boundaries?

---

_@chanman3388 reviewed on 2023-04-23 16:52_

---

_Review comment by @chanman3388 on `crates/ruff_cli/src/commands/run.rs`:177 on 2023-04-23 16:52_

The answer is that it doesn't render it correctly so I'll need to sort this out.

---

_@chanman3388 reviewed on 2023-04-23 17:18_

---

_Review comment by @chanman3388 on `crates/ruff_cli/src/commands/run.rs`:177 on 2023-04-23 17:18_

I've got this working, but I'm not sure I've done this in the expected fashion. If I want to use `ruff::linter::diagnostics_to_messages` I have to re-tokenize the source file which seems inefficient. I would need to do that to populate the `noqa` flags correctly, should I do that? For now I haven't as it _appears_ simpler no to.

---

_@chanman3388 reviewed on 2023-04-23 17:53_

---

_Review comment by @chanman3388 on `crates/ruff_cli/src/commands/run.rs`:165 on 2023-04-23 17:53_

Hm, I had to format it that way due to `cargo fmt` I think, the variables that are to be used in the actual for loop are in the line where the for loop starts?

---

_@chanman3388 reviewed on 2023-04-23 20:29_

---

_Review comment by @chanman3388 on `crates/ruff/src/rules/pylint/rules/cyclic_import.rs`:52 on 2023-04-23 20:29_

So the `ImportMap` is created in a way that is 'threaded' due to using `par_iter` which is where the first use of `Arc<str>` comes in. With the changes for referring to module names by id instead, because I create a name to id and id to name mapping...these just clone the `Arc` so unless I deliberately construct new `Rc`s from these `Arc`s I don't _think_ I can just use an `Rc` even after this point. So I don't think I can trivially use `Rc` in favour of `Arc`.
It's awkward, because I don't think we actually share these across thread boundaries, but the compiler doesn't like it.

---

_@chanman3388 reviewed on 2023-04-24 00:11_

---

_Review comment by @chanman3388 on `crates/ruff/src/rules/pylint/rules/cyclic_import.rs`:54 on 2023-04-24 00:11_

Ok, I've done this, but I think it's a bit ugly imho, at least I think it's kinda ugly.
At least on the noddy example I provided above, it doesn't appear to provide any gains, but it is a small example.

---

_@chanman3388 reviewed on 2023-04-24 00:12_

---

_Review comment by @chanman3388 on `crates/ruff/src/rules/pylint/rules/cyclic_import.rs`:102 on 2023-04-24 00:12_

I don't _believe_ it's unsafe to call `unwrap` here, however to be certain, I've avoided it now.

---

_Comment by @chanman3388 on 2023-04-24 00:37_

> > Impressive.
> > Can we add some integration tests that test the generated diagnostics?
> > How does that work in ruff-lsp when the user has unsaved changes in multiple files?
> 
> If I understand you correctly, I _think_ at the moment we can't do the former as the integration tests only work on individual files, whereas to check I get the correct cycles I need to lint a package.
> 
> Short answer for the latter is, I don't know, I'll find that out.
> 
> Thanks for the thorough review!

So at the moment at least I don't know how to test ruff-lsp with my own version of ruff. Is there someone that could help with that?

---

_@MichaReiser reviewed on 2023-04-26 00:56_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pylint/rules/cyclic_import.rs`:54 on 2023-04-26 00:56_

>At least on the noddy example I provided above,

What example is this? 

You would probably want to hide this behind a custom data structure that takes care of performing the `str -> id` lookups. So that it appears to users the same as when they to the `HashMap.get(str)` lookup directly.

---

_@chanman3388 reviewed on 2023-04-26 15:11_

---

_Review comment by @chanman3388 on `crates/ruff/src/rules/pylint/rules/cyclic_import.rs`:54 on 2023-04-26 15:11_

> Consider this file tree
> 
> ```
> grand
> ├── __init__.py
> ├── a.py (from .parent.child import a; from .parent import a; from grand.parent.child import b)
> └── parent
>     ├── __init__.py
>     ├── a.py (from .child import a, b; from .. import a)
>     └── child
>         ├── __init__.py
>         ├── a.py (from .. import a; from ... import a)
>         ├── b.py (from . import a)
>         └── c.py --empty--
> ```

This one.
Ok, I'll give `.get` implementation a go.

---

_@MichaReiser reviewed on 2023-04-26 16:02_

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/commands/run.rs`:177 on 2023-04-26 16:02_

I plan to have a closer look and provide you with an answer by Friday or early next week (I'm travelling today/tomorrow)

---

_Review comment by @chanman3388 on `crates/ruff/src/rules/pylint/rules/cyclic_import.rs`:54 on 2023-04-26 21:26_

So I decided to do some noddy 'benchmarking'. I cloned the cpython repo, checked out the tag v.3.11.3 and ran ruff on it with --select=E,F,PLR0401 on it with varying commits from this branch using `cargo build --release`, by my estimation this is about 1M lines of python. I have an AMD 5900X. Off the current tip ([f819262](https://github.com/charliermarsh/ruff/pull/3880/commits/f819262695a0cf10c8ab5feb9fd6b5cb1d6dc6cd)), it takes about 15-17s. Without performing a `name` to `id` mapping, that is to say leaving all the module names as `Arc<str>`, [c878a78](https://github.com/charliermarsh/ruff/pull/3880/commits/c878a78960cf42b6ae3d1f00b39e75bd07f1e835), it seems to reasonably consistently take 15s.

---

_@chanman3388 reviewed on 2023-04-26 21:26_

---

_Review comment by @chanman3388 on `crates/ruff/src/rules/pylint/rules/cyclic_import.rs`:54 on 2023-04-29 21:17_

Wrt ruff-lsp...I _believe_ I'm using my own build of ruff with vscode...but I don't think it picks up on the cyclic imports. Does it just lint individual files at a time?

---

_@chanman3388 reviewed on 2023-04-29 21:17_

---

_Comment by @chanman3388 on 2023-04-30 21:25_

I've got another branch that uses `par_iter` instead and places `CyclicImportHelper` in an `Arc<RwLock<T>`. At least from my benchmarking it's ever-so-slightly faster.

---

_Comment by @chanman3388 on 2023-04-30 21:37_

> I've got another branch that uses `par_iter` instead and places `CyclicImportHelper` in an `Arc<RwLock<T>`. At least from my benchmarking it's ever-so-slightly faster.

Scratch that, seems I was trying something else.

---

_Comment by @MichaReiser on 2023-05-01 15:21_

This is a very impressive PR for an extremely valuable lint rule. Thanks for keeping working on it and rebasing it. 

I have taken a closer look today and what it would mean. I have a few technical concerns for which I don't have any immediate answers right now and I recommend solving before merging. We can make some other improvements to the data structures to improve performance and reduce memory consumption.

## Open Questions

### LSP and unsaved changes

I haven't verified this but I fear that ruff will start reporting false positives (or negatives) if users have multiple open unsaved documents in their editor. Ruff's LSP passes the content of the active file in the editor per stdin instead of reading it from disk because there are likely unsaved changes. 

The problem is, users can have multiple open documents with unsaved changes. We would need to pass all of them via stdin to ensure ruff operates on the latest content rather than on the stale content on disk. A scenario where I could see this be especially problematic is if the user has a cyclic import, makes changes two both files that fix the issue, but ruff continues to report it because it uses stale content from disk. 

Properly supporting this use case most likely require significant changes to the LSP. We may want to decide that we're okay with this limitation. I rather prefer not to disable the rule in editors only because one of ruffs value propositions is that it works the same in editors and the CLI. 

### Caching

Ruff caches all diagnostics per file to avoid repeating the same analysis for unchanged files. I don't know how to cache the project-wide analysis yet and/or how cache invalidation should work. But I think that's something that we potentially can defer to later. 

Related to this: We need to have the source code to emit diagnostics. It's unclear to me how to best retrieve the source code. We may need to first refactor our code to extract the logic that reads the source for a file. 

## Architecture

I would like to build an architecture that supports other use-cases like checking if a module only imports valid symbols from a module. Ideally, most of the computation happens per-file to use the CPUs' multi-core architecture best. 

The way I think about this, and it is to a large extent implemented this way in this PR, is that we need to extract additional metadata in the per-file lint phase, cache it, aggregate that information, and then run additional checks on the aggregated data. 

### Lint Phase

Extract the name of the imported modules (and future, exported symbols). We can do this by computing a set of all imports in the lint phase where `ModuleImport` is defined as 

```rust
struct ModuleImports {
	imports: FxHashSet<ModuleImport>
}

struct ModuleImport {
	/// The range of the import statement, used for diagnostics
	range: TextRange,
	/// Or can we use a `TextRange` slicing into the source to avoid allocations?
	name: String,
}
```

We need to persist the `ModuleImports` when writing to the cache in the `cache.rs` and restore them when reading from the cache. This is similar to what you have today. My main recommendation is to keep this as slim as possible to make it cheap to cache. 

### Orchestration (CLI)

Build up an aggregated view. Ideally, as little as possible of that code lives in the CLI. The CLI should only be calling into the corresponding code living in ruff. For example, we could create a `Modules` or `Project` struct in ruff that aggregates the data:

```rust
struct Modules {
	index: hashbrowns::HashMap<ModuleId, ()> // Use the raw_entry_mut method to manually compute the hash key to avoid storing the name multiple time. https://docs.rs/hashbrown/latest/hashbrown/struct.HashMap.html#method.raw_entry
	modules: Vec<Module> // the index defines the `ModuleId`. Use `self.modules.len()` to compute the next id.
}

impl Modules {
  pub fn add_module(name: String, imports: ModuleImports) {...}
}

#[derive(Copy, Clone, Eq, PartialEq, Hash, Debug)]
struct ModuleId(u32);

struct Module {
	name: String,
	imports: Vec<ResolvedImport>,
	source: String, // (or lazy load?)
}

struct ResolvedImport {
	id: ModuleId,
	range: TextRange
}
```

We can avoid the need for locking on `Modules` by only using it from a single thread. Ideally, we could start building it while the analysis of other modules is still running rather than having to wait until all analysis ended (use a separate thread with a Channel?)

### Project-wide analysis
Create a new `lint_project(modules: &Modules)` or similar method in ruff that does the cross-file linting. 

What's unclear is how to retrieve the source text for cached files without loading every single file. 


## Next Steps

I think we should try to answer if we want to solve the LSP and caching issues today and we can then decide what other technical improvements we want to make (again, your PR is already super close and some of my suggestions may not work, you will know better). @charliermarsh what's your take on the LSP and caching problematic?

---

_Comment by @chanman3388 on 2023-05-01 23:37_

With regards to say, checking that we import valid symbols, I think in the per-file lint phase, we could construct an `ExportMap` of sorts. I suppose the main issue is that such a map is going to require more memory, but it would allow for checking if a member in a module does exist, at least for Ruff, it would be nice to have if we could at least do this on first-party packages.
I've not checked the memory usage, the version of perf I had to compile to work with WSL2 doesn't support memory events. I did play around with some other implementations of cyclic import detection, and it does seem that if you don't store the cycles and only consider cycles applicable to the current module that it is slightly faster than the approach in this PR as you can do it in a parallel fashion.  Would this be of interest? It feels inefficient, but it is faster, and it would allow for the running of other rules much like we do now, but using the `ImportMap` and the `ExportMap` concept that I mentioned.

---

_Comment by @chanman3388 on 2023-05-04 22:26_

The [PR](https://github.com/chanman3388/ruff/pull/4) with checking only the cycles relevant to the module being checked, if it is of any interest.

---

_Comment by @charliermarsh on 2023-10-20 18:04_

I'm going to close this for now in the interest of bookkeeping active PRs. I'm bummed that we haven't been able to merge it (and sorry for all the work you put in, only to lead to this disappointing outcome), but it starts to move us into multi-file analysis and so we need to solve some other problems before we'd be comfortable shipping it (namely, around caching and LSP support, as Micha described above in more detail). When the time comes, we can always reopen.

---

_Closed by @charliermarsh on 2023-10-20 18:04_

---
