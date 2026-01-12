```yaml
number: 1390
title: "--version output should go to the clap::Error instead of directly to stdout"
type: issue
state: closed
author: m-ou-se
labels: []
assignees: []
created_at: 2018-12-02T17:13:04Z
updated_at: 2019-11-26T17:39:12Z
url: https://github.com/clap-rs/clap/issues/1390
synced_at: 2026-01-12T16:14:10Z
```

# --version output should go to the clap::Error instead of directly to stdout

---

_@m-ou-se_

### Affected Version of clap

2.32.0

### Bug or Feature Request Summary

I'm using `get_matches_from_safe` to avoid exits and writing to stdout and stderr, because I'm using Clap not to parse arguments to the program itself, but somewhere inside the program (say, for commands submitted by connecting clients, or an internal command line interface in a gui):

```rust
let foo = app.get_matches_from_safe(client_args).map_err(|e| writeln!(client_output, "{}", e).unwrap())?;
```

This works fine, except that the output of `--version` ends up in stdout, instead of in `client_output`. It works fine for `--help`.

### Expected Behaviour Summary

I would expect the output from `--version` to not be written directly to stdout, but to end up in a `clap::Error`. This looks like the originally intended behaviour, seeing that `clap::Error::use_stderr` returns true for `ErrorKind::VersionDisplayed`.

### Actual Behaviour Summary

The version is not printed to the stream that I print the `clap::Error` to, but is always sent to stdout.

### Sample Code

```rust
extern crate clap;

use std::fmt::Write;

fn main() {
	let mut buf = String::new();

	let _ = clap::App::new("test").version("1")
		.get_matches_from_safe(&["test", "--version"])
		.map_err(|e| writeln!(&mut buf, "{}", e).unwrap());

	assert_eq!(buf, "test 1\n");
}
```

### Debug output

<details>
<summary> Debug Output </summary>

```
DEBUG:clap:Parser::propagate_settings: self=test, g_settings=AppFlags(
    (empty)
)
DEBUG:clap:Parser::get_matches_with;
DEBUG:clap:Parser::create_help_and_version;
DEBUG:clap:Parser::create_help_and_version: Building --help
DEBUG:clap:Parser::create_help_and_version: Building --version
DEBUG:clap:Parser::get_matches_with: Begin parsing '"--version"' ([45, 45, 118, 101, 114, 115, 105, 111, 110])
DEBUG:clap:Parser::is_new_arg:"--version":NotFound
DEBUG:clap:Parser::is_new_arg: arg_allows_tac=false
DEBUG:clap:Parser::is_new_arg: -- found
DEBUG:clap:Parser::is_new_arg: starts_new_arg=true
DEBUG:clap:Parser::possible_subcommand: arg="--version"
DEBUG:clap:Parser::get_matches_with: possible_sc=false, sc=None
DEBUG:clap:ArgMatcher::process_arg_overrides:None;
DEBUG:clap:Parser::parse_long_arg;
DEBUG:clap:Parser::parse_long_arg: Does it contain '='...No
DEBUG:clap:Parser::parse_long_arg: Found valid flag '--version'
DEBUG:clap:Parser::check_for_help_and_version_str;
DEBUG:clap:Parser::check_for_help_and_version_str: Checking if --version is help or version...Version
DEBUG:clap:Parser::_version: 
test 1thread 'main' panicked at 'assertion failed: `(left == right)`
  left: `"\n\n"`,
 right: `"test 1\n"`', src/main.rs:12:2
```
</details>


---

_Referenced in [watchexec/cargo-watch#114](../../watchexec/cargo-watch/pulls/114.md) on 2019-02-14 22:25_

---

_Referenced in [clap-rs/clap#1477](../../clap-rs/clap/pulls/1477.md) on 2019-05-25 15:24_

---

_Referenced in [clap-rs/clap#1602](../../clap-rs/clap/pulls/1602.md) on 2019-11-26 16:34_

---

_Closed by @Dylan-DPC-zz on 2019-11-26 17:39_

---

_Referenced in [d-e-s-o/nitrocli#97](../../d-e-s-o/nitrocli/pulls/97.md) on 2020-01-08 12:29_

---

_Referenced in [d-e-s-o/nitrocli#114](../../d-e-s-o/nitrocli/issues/114.md) on 2020-08-31 09:23_

---
