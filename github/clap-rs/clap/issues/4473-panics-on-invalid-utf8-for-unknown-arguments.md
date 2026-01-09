---
number: 4473
title: Panics on invalid utf8 for unknown arguments
type: issue
state: closed
author: matklad
labels: []
assignees: []
created_at: 2022-11-11T16:03:42Z
updated_at: 2022-11-11T18:46:03Z
url: https://github.com/clap-rs/clap/issues/4473
synced_at: 2026-01-07T13:12:20-06:00
---

# Panics on invalid utf8 for unknown arguments

---

_Issue opened by @matklad on 2022-11-11 16:03_

```rust
fn main() {
    let app = clap::builder::Command::new("test");
    let matches = app.try_get_matches();
    eprintln!("matches = {:?}", matches);
}
```

If I run this "empty" app with an unexpected arg, which is also invalid utf8, I get a panic: 

```console
λ cargo run -- --flag \XFF
    Finished dev [unoptimized + debuginfo] target(s) in 0.01s
     Running `target/debug/clap-clap --flag '�'`
thread 'main' panicked at 'unexpected invalid UTF-8 code point', /home/matklad/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-4.0.22/src/parser/parser.rs:176:53
stack backtrace:
   0: rust_begin_unwind
             at /rustc/e080cc5a659fb760c0bc561b722a790dad35b5e1/library/std/src/panicking.rs:575:5
   1: core::panicking::panic_fmt
             at /rustc/e080cc5a659fb760c0bc561b722a790dad35b5e1/library/core/src/panicking.rs:65:14
   2: core::panicking::panic_display
             at /rustc/e080cc5a659fb760c0bc561b722a790dad35b5e1/library/core/src/panicking.rs:139:5
   3: core::panicking::panic_str
             at /rustc/e080cc5a659fb760c0bc561b722a790dad35b5e1/library/core/src/panicking.rs:123:5
   4: core::option::expect_failed
             at /rustc/e080cc5a659fb760c0bc561b722a790dad35b5e1/library/core/src/option.rs:1879:5
   5: core::option::Option<T>::expect
             at /rustc/e080cc5a659fb760c0bc561b722a790dad35b5e1/library/core/src/option.rs:741:21
   6: clap::parser::parser::Parser::get_matches_with::{{closure}}
             at /home/matklad/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-4.0.22/src/parser/parser.rs:176:42
   7: core::iter::adapters::map::map_fold::{{closure}}
             at /rustc/e080cc5a659fb760c0bc561b722a790dad35b5e1/library/core/src/iter/adapters/map.rs:84:28
   8: core::iter::adapters::map::map_fold::{{closure}}
             at /rustc/e080cc5a659fb760c0bc561b722a790dad35b5e1/library/core/src/iter/adapters/map.rs:84:21
   9: core::iter::traits::iterator::Iterator::fold
             at /rustc/e080cc5a659fb760c0bc561b722a790dad35b5e1/library/core/src/iter/traits/iterator.rs:2414:21
  10: <core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold
             at /rustc/e080cc5a659fb760c0bc561b722a790dad35b5e1/library/core/src/iter/adapters/map.rs:124:9
  11: <core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold
             at /rustc/e080cc5a659fb760c0bc561b722a790dad35b5e1/library/core/src/iter/adapters/map.rs:124:9
  12: core::iter::traits::iterator::Iterator::for_each
             at /rustc/e080cc5a659fb760c0bc561b722a790dad35b5e1/library/core/src/iter/traits/iterator.rs:831:9
  13: <alloc::vec::Vec<T,A> as alloc::vec::spec_extend::SpecExtend<T,I>>::spec_extend
             at /rustc/e080cc5a659fb760c0bc561b722a790dad35b5e1/library/alloc/src/vec/spec_extend.rs:40:17
  14: <alloc::vec::Vec<T> as alloc::vec::spec_from_iter_nested::SpecFromIterNested<T,I>>::from_iter
             at /rustc/e080cc5a659fb760c0bc561b722a790dad35b5e1/library/alloc/src/vec/spec_from_iter_nested.rs:62:9
  15: <alloc::vec::Vec<T> as alloc::vec::spec_from_iter::SpecFromIter<T,I>>::from_iter
             at /rustc/e080cc5a659fb760c0bc561b722a790dad35b5e1/library/alloc/src/vec/spec_from_iter.rs:33:9
  16: <alloc::vec::Vec<T> as core::iter::traits::collect::FromIterator<T>>::from_iter
             at /rustc/e080cc5a659fb760c0bc561b722a790dad35b5e1/library/alloc/src/vec/mod.rs:2757:9
  17: core::iter::traits::iterator::Iterator::collect
             at /rustc/e080cc5a659fb760c0bc561b722a790dad35b5e1/library/core/src/iter/traits/iterator.rs:1836:9
  18: clap::parser::parser::Parser::get_matches_with
             at /home/matklad/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-4.0.22/src/parser/parser.rs:174:58
  19: clap::builder::command::Command::_do_parse
             at /home/matklad/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-4.0.22/src/builder/command.rs:3794:29
  20: clap::builder::command::Command::try_get_matches_from_mut
             at /home/matklad/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-4.0.22/src/builder/command.rs:708:9
  21: clap::builder::command::Command::try_get_matches_from
             at /home/matklad/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-4.0.22/src/builder/command.rs:624:9
  22: clap::builder::command::Command::try_get_matches
             at /home/matklad/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-4.0.22/src/builder/command.rs:548:9
  23: clap_clap::main
             at ./src/main.rs:3:19
  24: core::ops::function::FnOnce::call_once
             at /rustc/e080cc5a659fb760c0bc561b722a790dad35b5e1/library/core/src/ops/function.rs:251:5
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
```


Seems pre-existing since clap 2.0

---

_Referenced in [clap-rs/clap#4474](../../clap-rs/clap/pulls/4474.md) on 2022-11-11 18:26_

---

_Closed by @epage on 2022-11-11 18:46_

---
