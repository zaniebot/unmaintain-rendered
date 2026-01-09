---
number: 4556
title: Running panicked when flatten structs which have same struct field name
type: issue
state: closed
author: yiv
labels:
  - C-bug
assignees: []
created_at: 2022-12-15T12:20:24Z
updated_at: 2023-11-09T16:11:45Z
url: https://github.com/clap-rs/clap/issues/4556
synced_at: 2026-01-07T13:12:20-06:00
---

# Running panicked when flatten structs which have same struct field name

---

_Issue opened by @yiv on 2022-12-15 12:20_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.65.0 (897e37553 2022-11-02)

### Clap Version

4.0.29

### Minimal reproducible code

```rust
use clap::{command, Arg, Parser};

#[derive(Parser, Debug)]
#[command(author, version, about, long_about = None)]
struct AppConfig {
    #[clap(flatten)]
    redis: Option<RedisConfig>,
    #[clap(flatten)]
    database: Option<DatabaseConfig>,
}

#[derive(Parser, Debug)]
struct RedisConfig {
    #[arg(long = "redis.level")]
    name: Option<String>,
}


#[derive(Parser, Debug)]
struct DatabaseConfig {
    #[arg(long = "database.url")]
    name: Option<String>,
}

fn main() {
    let config = AppConfig::parse();
    println!("config {:?}", config);
}

```


### Steps to reproduce the bug with the above code

cargo run

### Actual Behaviour


pannic 

```shell

warning: `tt` (bin "tt") generated 1 warning
    Finished dev [unoptimized + debuginfo] target(s) in 0.11s
     Running `target/debug/tt`
thread 'main' panicked at 'Command tt: Argument names must be unique, but 'name' is in use by more than one argument or group', /Users/bytedance/.cargo/registry/src/rsproxy.cn-8f6827c7555bfaf8/clap-4.0.29/src/builder/debug_asserts.rs:95:13
stack backtrace:
   0: rust_begin_unwind
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/std/src/panicking.rs:584:5
   1: core::panicking::panic_fmt
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/core/src/panicking.rs:142:14
   2: clap::builder::debug_asserts::assert_app
             at /Users/bytedance/.cargo/registry/src/rsproxy.cn-8f6827c7555bfaf8/clap-4.0.29/src/builder/debug_asserts.rs:95:13
   3: clap::builder::command::Command::_build_self
             at /Users/bytedance/.cargo/registry/src/rsproxy.cn-8f6827c7555bfaf8/clap-4.0.29/src/builder/command.rs:3920:13
   4: clap::builder::command::Command::_do_parse
             at /Users/bytedance/.cargo/registry/src/rsproxy.cn-8f6827c7555bfaf8/clap-4.0.29/src/builder/command.rs:3790:9
   5: clap::builder::command::Command::try_get_matches_from_mut
             at /Users/bytedance/.cargo/registry/src/rsproxy.cn-8f6827c7555bfaf8/clap-4.0.29/src/builder/command.rs:708:9
   6: clap::builder::command::Command::get_matches_from
             at /Users/bytedance/.cargo/registry/src/rsproxy.cn-8f6827c7555bfaf8/clap-4.0.29/src/builder/command.rs:578:9
   7: clap::builder::command::Command::get_matches
             at /Users/bytedance/.cargo/registry/src/rsproxy.cn-8f6827c7555bfaf8/clap-4.0.29/src/builder/command.rs:490:9
   8: clap::derive::Parser::parse
             at /Users/bytedance/.cargo/registry/src/rsproxy.cn-8f6827c7555bfaf8/clap-4.0.29/src/derive.rs:82:27
   9: tt::main
             at ./src/main.rs:26:18
  10: core::ops::function::FnOnce::call_once
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/core/src/ops/function.rs:248:5
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.

Process finished with exit code 101
```

### Expected Behaviour

should run successfully

### Additional Context

running in release mode will succeed
```
cargo run --release 
```

### Debug Output

```shell

warning: `tt` (bin "tt") generated 1 warning
    Finished dev [unoptimized + debuginfo] target(s) in 0.16s
     Running `target/debug/tt`
[      clap::builder::command] 	Command::_do_parse
[      clap::builder::command] 	Command::_build: name="tt"
[      clap::builder::command] 	Command::_propagate:tt
[      clap::builder::command] 	Command::_check_help_and_version:tt expand_help_tree=false
[      clap::builder::command] 	Command::long_help_exists
[      clap::builder::command] 	Command::_check_help_and_version: Building default --help
[      clap::builder::command] 	Command::_check_help_and_version: Building default --version
[      clap::builder::command] 	Command::_propagate_global_args:tt
[clap::builder::debug_asserts] 	Command::_debug_asserts
[clap::builder::debug_asserts] 	Arg::_debug_asserts:name
thread 'main' panicked at 'Command tt: Argument names must be unique, but 'name' is in use by more than one argument or group', /Users/bytedance/.cargo/registry/src/rsproxy.cn-8f6827c7555bfaf8/clap-4.0.29/src/builder/debug_asserts.rs:95:13
stack backtrace:
   0: rust_begin_unwind
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/std/src/panicking.rs:584:5
   1: core::panicking::panic_fmt
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/core/src/panicking.rs:142:14
   2: clap::builder::debug_asserts::assert_app
             at /Users/bytedance/.cargo/registry/src/rsproxy.cn-8f6827c7555bfaf8/clap-4.0.29/src/builder/debug_asserts.rs:95:13
   3: clap::builder::command::Command::_build_self
             at /Users/bytedance/.cargo/registry/src/rsproxy.cn-8f6827c7555bfaf8/clap-4.0.29/src/builder/command.rs:3920:13
   4: clap::builder::command::Command::_do_parse
             at /Users/bytedance/.cargo/registry/src/rsproxy.cn-8f6827c7555bfaf8/clap-4.0.29/src/builder/command.rs:3790:9
   5: clap::builder::command::Command::try_get_matches_from_mut
             at /Users/bytedance/.cargo/registry/src/rsproxy.cn-8f6827c7555bfaf8/clap-4.0.29/src/builder/command.rs:708:9
   6: clap::builder::command::Command::get_matches_from
             at /Users/bytedance/.cargo/registry/src/rsproxy.cn-8f6827c7555bfaf8/clap-4.0.29/src/builder/command.rs:578:9
   7: clap::builder::command::Command::get_matches
             at /Users/bytedance/.cargo/registry/src/rsproxy.cn-8f6827c7555bfaf8/clap-4.0.29/src/builder/command.rs:490:9
   8: clap::derive::Parser::parse
             at /Users/bytedance/.cargo/registry/src/rsproxy.cn-8f6827c7555bfaf8/clap-4.0.29/src/derive.rs:82:27
   9: tt::main
             at ./src/main.rs:26:18
  10: core::ops::function::FnOnce::call_once
             at /rustc/897e37553bba8b42751c67658967889d11ecd120/library/core/src/ops/function.rs:248:5
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.

Process finished with exit code 101
```

---

_Label `C-bug` added by @yiv on 2022-12-15 12:20_

---

_Referenced in [clap-rs/clap#4557](../../clap-rs/clap/issues/4557.md) on 2022-12-15 12:52_

---

_Comment by @epage on 2022-12-15 13:21_

Yes, at the moment, the user is expected to keep all of the names unique as those names translate are references used when parsing and allow the user to reference them for defining different forms of validation (e.g. `#[arg(requires = "name")]`

Doing this at compile-time is being tracked in #3133

---

_Closed by @epage on 2022-12-15 13:21_

---

_Comment by @AndreyMZ on 2023-09-09 15:55_

> at the moment, the user is expected to keep all of the names unique as those names translate are references used when parsing and allow the user to reference them for defining different forms of validation (e.g. #[arg(requires = "name")]

Is there plan to change this weird behavior? I would expect these fields to be referenced by `"redis.name"` and `"database.name"` similar to how they are referenced in Rust code (`config.redis?.name` and `config.database?.name`).

Proper naming is important part of programming. Names like `config.database?.name2` or `config.database?.database_name` are not proper. So, I propose reopening this issue as an enhancement to a future (maybe major) version of Clap.


---

_Comment by @AndreyMZ on 2023-09-09 16:16_

I have found a workaround using [`Arg::id`](https://docs.rs/clap/latest/clap/struct.Arg.html#method.id) and [`Arg::value_name`](https://docs.rs/clap/latest/clap/struct.Arg.html#method.value_name):

```rust
#[derive(Parser, Debug)]
#[command(author, version, about, long_about = None)]
struct AppConfig {
    #[clap(flatten)] redis: Option<RedisConfig>,
    #[clap(flatten)] database: Option<DatabaseConfig>,
}

#[derive(Parser, Debug)]
struct RedisConfig {
    #[arg(long = "redis-name", value_name = "NAME", id = "redis.name")]
    name: Option<String>,
}

#[derive(Parser, Debug)]
struct DatabaseConfig {
    #[arg(long = "database-name", value_name = "NAME", id = "database.name")]
    name: Option<String>,
}
```

---

_Comment by @epage on 2023-09-10 00:10_

> Is there plan to change this weird behavior? I would expect these fields to be referenced by "redis.name" and "database.name" similar to how they are referenced in Rust code (config.redis?.name and config.database?.name).

Are you asking for us to use the `long` for the `Arg::id`?  The problem with that is not everything has a long and we end up in the same situation except guessing the `Arg::id` is more difficult because its inconsistent.

As an alternative, we do have #4701

---

_Comment by @AndreyMZ on 2023-09-29 18:02_

No, I would propose to use names of the variables joined with `.` for the `Arg::id` by default. However, I don't know if this is possible or not with the current macro system in Rust.

Anyway, given the ability to specify `Arg::id` explicitly, this would be a minor issue.


---

_Comment by @AndreyMZ on 2023-09-29 18:03_

Just in case, below is my example demonstrating this issue.

```rs
use std::fs;
use std::path::PathBuf;

use clap::{Args, Parser};


#[derive(Parser, Default)]
#[command(author, version, about, long_about = None)]
struct Cli {
	#[command(flatten)] foo: CliFoo,
	#[command(flatten)] bar: CliBar,
}

#[derive(Args, Default)]
#[group(required = true, multiple = false)]
struct CliFoo{
	#[arg(long = "foo",      /* value_name = "TEXT", id = "foo.text" */)] text: Option<String>,
	#[arg(long = "foo-file", /* value_name = "PATH", id = "foo.path" */)] path: Option<PathBuf>,
}

#[derive(Args, Default)]
#[group(required = true, multiple = false)]
struct CliBar{
	#[arg(long = "bar",      /* value_name = "TEXT", id = "bar.text" */)] text: Option<String>,
	#[arg(long = "bar-file", /* value_name = "PATH", id = "bar.path" */)] path: Option<PathBuf>,
}


fn main() {
	let args = Cli::parse();
	
	let foo = match args.foo {
		CliFoo {text: Some(text), path: None} => text,
		CliFoo {text: None, path: Some(path)} => fs::read_to_string(&path).unwrap(),
		_ => unreachable!(),
	};

	let bar = match args.bar {
		CliBar {text: Some(text), path: None} => text,
		CliBar {text: None, path: Some(path)} => fs::read_to_string(&path).unwrap(),
		_ => unreachable!(),
	};
	
	println!("{foo}");
	println!("{bar}");
}
```

It panics while `value_name` and `id` are commented out: <https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=8e64c6d2dca6a780fa0b9d33beda2406>

And it works OK if you uncomment them: https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=e82170868ce4e34f2d8a0331a4f20e49


---

_Comment by @epage on 2023-09-29 18:32_

> No, I would propose to use names of the variables joined with . for the Arg::id by default. However, I don't know if this is possible or not with the current macro system in Rust.

Oh, you want the name of the field that is used for flattening to be joined with the argument's field's name.

The derive limitation is that we can only process the information for our own derive.  This would require us to deal with this at runtime, to have the one struct pass the field name to the other struct and have it join the field names together at runtime.
- This runtime processing would slow things down when most cases don't need this
- This is one of several problems where solve a derive problem by moving it to runtime but that means we are constantly revving the traits, making them unstable and complicated to deal with.  I've instead been taking the stance that we need very strong justifications for why a feature needs to be shifted to runtime.  Deriving `ArgGroup` has been one of them.

---

_Comment by @Sytten on 2023-11-09 16:03_

I came here for the same problem. I don't mind specifying the `id` manually though to differentiate the two fields.
You don't need to change to Parser, the Args derive also support the `id`.

```rust
#[derive(Args, Clone, Debug)]
pub struct OpenAIConfiguration {
    #[clap(
        long = "openai-base-url",
        env = "OPENAI_BASE_URL",
        id = "openai_base_url"
    )]
    pub base_url: Option<String>,
}
```

I think what I would actually prefer is a derive arg to add a prefix to all, something like:
```rust
#[derive(Args, Clone, Debug)]
#[clap(prefix = "openai")]
pub struct OpenAIConfiguration {
    pub base_url: Option<String>,
}
```

---

_Referenced in [clap-rs/clap#3117](../../clap-rs/clap/issues/3117.md) on 2023-11-09 16:06_

---

_Comment by @epage on 2023-11-09 16:11_

#4701 would automatically add "a" prefix to the `id` but not to other fields.

People have requested prefixes for more than id, like in #3513 and #3221

---

_Referenced in [clap-rs/clap#5374](../../clap-rs/clap/issues/5374.md) on 2024-02-24 21:39_

---
