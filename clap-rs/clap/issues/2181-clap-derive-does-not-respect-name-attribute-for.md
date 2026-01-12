```yaml
number: 2181
title: clap_derive does not respect name attribute for enums
type: issue
state: closed
author: newAM
labels:
  - C-bug
  - E-easy
  - A-derive
  - ":money_with_wings: $5"
assignees: []
created_at: 2020-10-22T23:08:10Z
updated_at: 2021-02-22T16:08:07Z
url: https://github.com/clap-rs/clap/issues/2181
synced_at: 2026-01-12T16:14:12Z
```

# clap_derive does not respect name attribute for enums

---

_@newAM_

### Code

```rust
use clap::Clap;

#[derive(Clap)]
#[clap(name = "mybin", version = clap::crate_version!())]
enum Opts {}

fn main() {
    Opts::parse();
}
```

Full repository here: https://github.com/newAM/clap-issue

### Steps to reproduce the issue

1. `git clone git@github.com:newAM/clap-issue.git`
2. `cd clap-issue`
3. `cargo run -- -V`

### Version

* **Rust**: `rustc 1.47.0 (18bf6b4f0 2020-10-07)`
* **Clap**: fad3d9632ebde36004455084df2c9052afe40855 (`master`)

### Actual Behavior Summary

```
$ cargo run -- -V                                          
    Finished dev [unoptimized + debuginfo] target(s) in 0.01s
     Running `target/debug/cli -V`
cli 0.1.0
```

### Expected Behavior Summary

I expect `name = "mybin"` to set the name for the application, and the output should then look like this.

```
$ cargo run -- -V                                          
    Finished dev [unoptimized + debuginfo] target(s) in 0.01s
     Running `target/debug/cli -V`
mybin 0.1.0
```

### Additional context

This *only* occurs for enums, when using a struct this matches the expected behaviour:
```rust
use clap::Clap;

#[derive(Clap)]
#[clap(name = "mybin", version = clap::crate_version!())]
struct Opts {}

fn main() {
    Opts::parse();
}
```

```
$ cargo run -- -V                                          
    Finished dev [unoptimized + debuginfo] target(s) in 0.01s
     Running `target/debug/cli -V`
mybin 0.1.0
```

In addition to impacting `-V` and `--version` this also impacts completion scripts generated with `clap`.

### Debug output

<details>
<summary> Debug Output </summary>
<pre>
<code>
[            clap::build::app]  App::_do_parse
[            clap::build::app]  App::_build
[            clap::build::app]  App::_propagate:cli
[            clap::build::app]  App::_derive_display_order:cli
[            clap::build::app]  App::_create_help_and_version
[            clap::build::app]  App::_create_help_and_version: Building --help
[            clap::build::app]  App::_create_help_and_version: Building --version
[clap::build::app::debug_asserts]       App::_debug_asserts
[            clap::build::arg]  Arg::_debug_asserts:help
[            clap::build::arg]  Arg::_debug_asserts:version
[         clap::parse::parser]  Parser::get_matches_with
[         clap::parse::parser]  Parser::_build
[         clap::parse::parser]  Parser::_verify_positionals
[         clap::parse::parser]  Parser::get_matches_with: Begin parsing '"-V"' ([45, 86])
[         clap::parse::parser]  Parser::is_new_arg: "-V":NotFound
[         clap::parse::parser]  Parser::is_new_arg: arg_allows_tac=false
[         clap::parse::parser]  Parser::is_new_arg: - found
[         clap::parse::parser]  Parser::is_new_arg: starts_new_arg=true
[         clap::parse::parser]  Parser::possible_subcommand: arg="-V"
[         clap::parse::parser]  Parser::get_matches_with: possible_sc=false, sc=None
[         clap::parse::parser]  Parser::parse_short_arg: full_arg="-V"
[         clap::parse::parser]  Parser::parse_short_arg:iter:V
[         clap::parse::parser]  Parser::parse_short_arg:iter:V: Found valid opt or flag
[         clap::parse::parser]  Parser::check_for_help_and_version_char
[         clap::parse::parser]  Parser::check_for_help_and_version_char: Checking if -V is help or version...
[         clap::parse::parser]  Version
[         clap::parse::parser]  Parser::version_err
[            clap::build::app]  App::_render_version
[         clap::parse::parser]  Parser::color_help
[           clap::output::fmt]  is_a_tty: stderr=false
cli 0.1.0
</code>
</pre>
</details>


---

_Label `T: bug` added by @newAM on 2020-10-22 23:08_

---

_Label `:money_with_wings: $5` added by @pksunkara on 2020-10-23 09:29_

---

_Label `C: derive macros` added by @pksunkara on 2020-10-23 09:29_

---

_Added to milestone `3.0` by @pksunkara on 2020-10-23 09:29_

---

_Referenced in [clap-rs/clap#2189](../../clap-rs/clap/pulls/2189.md) on 2020-10-30 15:48_

---

_Label `D: easy` added by @pksunkara on 2021-02-08 05:55_

---

_Label `Z: good first issue` added by @pksunkara on 2021-02-08 05:55_

---

_Comment by @pksunkara on 2021-02-22 09:35_

@logansquirel This is a good one.

---

_Referenced in [clap-rs/clap#2358](../../clap-rs/clap/pulls/2358.md) on 2021-02-22 14:19_

---

_Closed by @pksunkara on 2021-02-22 16:08_

---
