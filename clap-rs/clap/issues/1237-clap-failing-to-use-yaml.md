```yaml
number: 1237
title: Clap failing to use YAML 
type: issue
state: closed
author: r8d8
labels: []
assignees: []
created_at: 2018-03-29T12:05:01Z
updated_at: 2018-09-06T23:24:47Z
url: https://github.com/clap-rs/clap/issues/1237
synced_at: 2026-01-12T16:14:10Z
```

# Clap failing to use YAML 

---

_@r8d8_

<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

### Rust Version
rustc 1.26.0-nightly (482a913fb 2018-03-25)

### Affected Version of clap
`2.31.2`

### Expected Behavior Summary
Produce proper `App` using YAML config

### Actual Behavior Summary
Stack trace:
```shell
$  RUST_BACKTRACE=1 ./target/debug/emerald -h
thread 'main' panicked at 'called `Option::unwrap()` on a `None` value', libcore/option.rs:335:21
stack backtrace:
   0: std::sys::unix::backtrace::tracing::imp::unwind_backtrace
             at libstd/sys/unix/backtrace/tracing/gcc_s.rs:49
   1: std::sys_common::backtrace::print
             at libstd/sys_common/backtrace.rs:71
             at libstd/sys_common/backtrace.rs:59
   2: std::panicking::default_hook::{{closure}}
             at libstd/panicking.rs:207
   3: std::panicking::default_hook
             at libstd/panicking.rs:223
   4: std::panicking::rust_panic_with_hook
             at libstd/panicking.rs:402
   5: std::panicking::begin_panic_fmt
             at libstd/panicking.rs:349
   6: rust_begin_unwind
             at libstd/panicking.rs:325
   7: core::panicking::panic_fmt
             at libcore/panicking.rs:72
   8: core::panicking::panic
             at libcore/panicking.rs:51
   9: <core::option::Option<T>>::unwrap
             at /checkout/src/libcore/macros.rs:20
  10: clap::args::arg::Arg::from_yaml
             at /home/k2/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.31.2/src/args/arg.rs:99
  11: <clap::app::App<'a, 'a> as core::convert::From<&'a yaml_rust::yaml::Yaml>>::from
             at /home/k2/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.31.2/src/app/mod.rs:1767
  12: clap::app::App::from_yaml
             at /home/k2/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.31.2/src/app/mod.rs:156
  13: emerald::main
             at src/main.rs:48
  14: std::rt::lang_start::{{closure}}
             at /checkout/src/libstd/rt.rs:74
  15: std::panicking::try::do_call
             at libstd/rt.rs:59
             at libstd/panicking.rs:306
  16: __rust_maybe_catch_panic
             at libpanic_unwind/lib.rs:102
  17: std::rt::lang_start_internal
             at libstd/panicking.rs:285
             at libstd/panic.rs:361
             at libstd/rt.rs:58
  18: std::rt::lang_start
             at /checkout/src/libstd/rt.rs:74
  19: main
  20: __libc_start_main
  21: _start
```

### Steps to Reproduce the issue
Using this [cli.yaml](https://gist.github.com/r8d8/08d7785e578ed76ef43d9b0b62829afd) config to create app.
Rust code as follows:
```rust
fn main() {
    let yaml = load_yaml!("../cli.yml");
    let matches = App::from_yaml(yaml).get_matches();
}
```

---

_Closed by @r8d8 on 2018-03-29 12:20_

---

_Comment by @r8d8 on 2018-03-29 12:20_

Was an yaml error

---

_Comment by @Hellseher on 2018-09-06 23:24_

I've got the same on `version = "2.31.2"`

cli option from cli.yml are not generated.

```
#[macro_use]
extern crate clap;

use clap::App;

fn main() {
    let cli_yaml = load_yaml!("cli.yml");
    let matches = App::from_yaml(cli_yaml)
        .about(crate_description!())
        .name(crate_name!())
        .author(crate_authors!())
        .version(crate_version!())
        .get_matches();

    let config = matches.value_of("config").unwrap_or("default.conf");
    println!("Value for config: {}", config);
}
// End of main.rs
```

---
