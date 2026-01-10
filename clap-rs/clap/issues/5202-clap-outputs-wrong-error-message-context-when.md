---
number: 5202
title: Clap outputs wrong error message context when using an environment variable with invalid content
type: issue
state: open
author: inf17101
labels:
  - C-bug
  - A-parsing
  - S-waiting-on-design
assignees: []
created_at: 2023-11-08T14:49:15Z
updated_at: 2025-11-02T14:44:32Z
url: https://github.com/clap-rs/clap/issues/5202
synced_at: 2026-01-10T01:28:08Z
---

# Clap outputs wrong error message context when using an environment variable with invalid content

---

_Issue opened by @inf17101 on 2023-11-08 14:49_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.72.1 (d5c2e9c34 2023-09-13)

### Clap Version

clap: 4.1.4, clap_derive: 4.1.0

### Minimal reproducible code

```rust
use url::Url;
use clap::Parser;

#[derive(Parser, Debug)]
#[clap( author="Author x", 
        version="0.1.0", 
        about="Application that reproduces issue with wrong error context in error message when using environment variables.")
]
struct Arguments {
    /// The server url.
    #[clap(short = 's', long = "server-url", env = "CUSTOM_SERVER_URL")]
    server_url: Url,
}


fn main() {
    let args = Arguments::parse();
    println!("{:?}", args);
}
```
Config.toml
```toml
[package]
name = "clap_bug"
version = "0.1.0"
edition = "2021"

# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

[dependencies]
url = "2.3"
clap = { version = "4.0", features = ["derive", "env"] }
```

### Steps to reproduce the bug with the above code

```console
$ export CUSTOM_SERVER_URL=invalid-url
$ cargo run
```

### Actual Behaviour

When running the above command with an invalid value for the environment variable (parser error),
then clap does not output the correct context of the wrong value. It outputs that the cli argument was set wrongly, 
but the user have used an environment variable not the cli argument. 

```
error: invalid value 'invalid-url' for '--server-url <SERVER_URL>': relative URL without a base

For more information, try '--help'.
```

### Expected Behaviour

For good use-ability it shall output the correct error message context, for example:

```
error: invalid value 'invalid-url' for 'environment variable: CUSTOM_SERVER_URL': relative URL without a base

For more information, try '--help'.
```

### Additional Context

The background is better use-ability. A user might forgot that an environment variable was set manually and tries to execute the program. With the current error message context he gets confused, because he thinks that he did not set this argument.

### Debug Output

```
[clap_builder::builder::command]Command::_do_parse
[clap_builder::builder::command]Command::_build: name="clap_bug"
[clap_builder::builder::command]Command::_propagate:clap_bug
[clap_builder::builder::command]Command::_check_help_and_version:clap_bug expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_check_help_and_version: Building default --version
[clap_builder::builder::command]Command::_propagate_global_args:clap_bug
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:server_url
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:version
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::parser::parser]Parser::get_matches_with
[clap_builder::parser::parser]Parser::add_env
[clap_builder::parser::parser]Parser::add_env: Checking arg `--server-url <SERVER_URL>`
[clap_builder::parser::parser]Parser::add_env: Found an opt with value="invalid-url"
[clap_builder::parser::parser]Parser::react action=Set, identifier=None, source=EnvVariable
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="server_url", source=EnvVariable
[clap_builder::builder::command]Command::groups_for_arg: id="server_url"
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="Arguments", source=EnvVariable
[clap_builder::parser::parser]Parser::push_arg_values: ["invalid-url"]
[clap_builder::parser::parser]Parser::add_single_val_to_arg: cur_idx:=1
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
```

---

_Label `C-bug` added by @inf17101 on 2023-11-08 14:49_

---

_Label `A-parsing` added by @epage on 2023-11-08 14:53_

---

_Label `S-waiting-on-design` added by @epage on 2023-11-08 14:53_

---

_Referenced in [eclipse-ankaios/ankaios#75](../../eclipse-ankaios/ankaios/issues/75.md) on 2023-11-08 15:30_

---

_Comment by @inf17101 on 2023-11-13 21:04_

I am currently searching the place in the code base where the parsers construct the error messages. Is it possible to just add a new `ContextKind` "InvalidEnv" to the enum in case of an inalid env var value and pass this new context kind with the value "environment variable 'SOME_ENV_VAR'" (where SOME_ENV_VAR is the env variable key) to the error?

Then instead of the cli argument name the correct error context is provided.

Could you maybe give a hint, where this code path is?

---

_Comment by @epage on 2023-11-13 21:18_

> I am currently searching the place in the code base where the parsers construct the error messages

The [value parser](https://github.com/clap-rs/clap/blob/master/clap_builder/src/builder/value_parser.rs#L908).  You can update it to use `TypedValueParser::parse_` to get the `ValueSource` to tell if an env was used.

>  Is it possible to just add a new ContextKind "InvalidEnv" to the enum in case of an inalid env var value and pass this new context kind with the value "environment variable 'SOME_ENV_VAR'" (where SOME_ENV_VAR is the env variable key) to the error?

Do we need an `InvalidEnv` or could we just put the env variable in `InvalidArg`?

---

_Comment by @inf17101 on 2023-11-13 21:51_

Ok great thank you for that hint. I will take a look there. I was checking `Parser::add_env` also just to see how the arg name is forwarded when env is used. 
It is first time I read this code base. I must first get familiar with all the parts 😃 

I think we can also use the InvalidArg, but I thought it would be better to not mix cli arg context with env context. But for me it would be also ol to use InvalidArg for the first try. 

---

_Referenced in [eclipse-ankaios/ankaios#81](../../eclipse-ankaios/ankaios/pulls/81.md) on 2023-11-14 07:38_

---

_Referenced in [eclipse-ankaios/ankaios#85](../../eclipse-ankaios/ankaios/issues/85.md) on 2023-11-14 07:46_

---

_Comment by @Jason5Lee on 2025-11-02 14:44_

Is there a workaround for better error messages? This issue has been open for 2 years.

---
