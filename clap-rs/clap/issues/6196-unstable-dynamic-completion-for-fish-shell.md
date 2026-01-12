```yaml
number: 6196
title: "`unstable-dynamic` completion for fish shell invalid when binary path has a space"
type: issue
state: closed
author: devjgm
labels:
  - C-bug
  - E-medium
  - A-completion
assignees: []
created_at: 2025-12-16T15:25:32Z
updated_at: 2025-12-18T14:28:29Z
url: https://github.com/clap-rs/clap/issues/6196
synced_at: 2026-01-12T16:14:17Z
```

# `unstable-dynamic` completion for fish shell invalid when binary path has a space

---

_@devjgm_

### Please complete the following tasks

- [x] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.91.0 (f8297e351 2025-10-28)

### Clap Version

4.5.53

### Minimal reproducible code

```rust
use clap::CommandFactory;

#[derive(clap::Parser, Debug)]
struct Args;

fn main() {
    clap_complete::env::CompleteEnv::with_factory(|| Args::command()).complete();
    // Nothing
}
```

```toml
[package]
name = "clap-complete-bug"
version = "0.1.0"
edition = "2024"

[dependencies]
clap = { version = "4.5.53", features = ["derive"] }
clap_complete = { version = "4.5.61", features = ["unstable-dynamic"] }
```


### Steps to reproduce the bug with the above code

* Build a project with the code above. `cargo build`
* Run the binary with `COMPLETE=fish ./target/debug/clap-complete-bug`. So far, THIS IS OK
```console
❯ COMPLETE=fish ./target/debug/clap-complete-bug
complete --keep-order --exclusive --command clap-complete-bug --arguments "(COMPLETE=fish "'/Users/greg/clap-complete-bug/./target/debug/clap-complete-bug'" -- (commandline --current-process --tokenize --cut-at-cursor) (commandline --current-token))"
```

# The bug
* Use a symlink to add a space in the path to the binary
```console
ln -s ./target/debug/clap-complete-bug "with space"
```
* Now run the binary with `COMPLETE=fish` using the path with a space
```console
❯ COMPLETE=fish ./with\ space 
complete --keep-order --exclusive --command clap-complete-bug --arguments "(COMPLETE=fish "''/Users/greg/clap-complete-bug/./with space''" -- (commandline --current-process --tokenize --cut-at-cursor) (commandline --current-token))"
```
* Notice how the generated path has too many quotes around the path. Fish is unable to run the command:
```console
❯ complete --keep-order --exclusive --command clap-complete-bug --arguments "(COMPLETE=fish "''/Users/greg/clap-complete-bug/./with space''" -- (commandline --current-process --tokenize --cut-at-cursor) (commandline -
-current-token))"
complete: too many arguments

(Type 'help complete' for related documentation)
```

### Actual Behaviour

`COMPLETE=fish  /path with a space/my-binary` produces invalid `fish` syntax due to invalid quoting around the path with a space.

### Expected Behaviour

I expect the produced paths to be quoted correctly with or without spaces.

### Additional Context

This will be a somewhat common issue because on macOS, paths like `Library/Application Support` have spaces, and binaries in those directories will not be able to use dynamic clap completions.

Note: If you're open to it, I'd be willing to send a PR.

### Debug Output

```
❯ COMPLETE=fish ./with\ space
[clap_builder::builder::command]Command::_build: name="clap-complete-bug"
[clap_builder::builder::command]Command::_propagate:clap-complete-bug
[clap_builder::builder::command]Command::_check_help_and_version:clap-complete-bug expand_help_tree=true
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_propagate_global_args:clap-complete-bug
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::builder::command]Command::_build_bin_names
[ clap_builder::output::usage]Usage::get_required_usage_from: incls=[], matcher=false, incl_last=true
[ clap_builder::output::usage]Usage::get_required_usage_from: unrolled_reqs=[]
[ clap_builder::output::usage]Usage::get_required_usage_from: ret_val=[]
complete --keep-order --exclusive --command clap-complete-bug --arguments "(COMPLETE=fish "''/Users/greg/clap-complete-bug/./with space''" -- (commandline --current-process --tokenize --cut-at-cursor) (commandline --current-token))"
```

---

_Label `C-bug` added by @devjgm on 2025-12-16 15:25_

---

_Label `S-triage` added by @devjgm on 2025-12-16 15:25_

---

_Label `A-completion` added by @epage on 2025-12-16 16:25_

---

_Label `S-triage` removed by @epage on 2025-12-16 16:26_

---

_Label `E-medium` added by @epage on 2025-12-16 16:26_

---

_Comment by @epage on 2025-12-16 16:30_

Help would be appreciated! 

We should probably have a shell integration test for this.  If we don't already have a test for this, it would be good to mirror the test over to the other shells as this is this is a common corner case across shells, though you aren't responsible for fixing the other shells.

We've found it helpful to split PRs up to be as small as possible while still being atomic (i.e. no dead code, tests pass in all commits).  Particularly, we've found it helpful to have tests added in their own commit, showing the current behavior (the bug), then the fix commit would update the tests to show the new behavior.  The diff of the tests between these commits then helps highlight how the behavior changed.

---

_Comment by @devjgm on 2025-12-17 00:53_

I'm pretty sure the fix is simply

```diff
diff --git a/clap_complete/src/env/shells.rs b/clap_complete/src/env/shells.rs
index 526c8e8b88..1ee0d091fb 100644
--- a/clap_complete/src/env/shells.rs
+++ b/clap_complete/src/env/shells.rs
@@ -224,7 +224,7 @@
 
         writeln!(
             buf,
-            r#"complete --keep-order --exclusive --command {bin} --arguments "({var}=fish "'{completer}'" -- (commandline --current-process --tokenize --cut-at-cursor) (commandline --current-token))""#
+            r#"complete --keep-order --exclusive --command {bin} --arguments "({var}=fish {completer} -- (commandline --current-process --tokenize --cut-at-cursor) (commandline --current-token))""#
         )
     }
     fn write_complete(
```
... because the `completer` comes from `shlex::try_quote()` and should have the necessary quoting already. The original issue was caused by `"'{completer}'"`, because when `{completer}` itself expands to a single-quoted string, its single quotes will close the format string's single quotes and leave the resulting command unquoted, e.g., `"''/path with a space''"`

I informally tested this in the repo (and in my main code base) with ad-hoc invocations like:

No space, unquoted, works.
```console
❯ COMPLETE=fish cargo run --example "dynamic" -F unstable-dynamic
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.24s
     Running `/Users/greg/github/devjgm/clap/target/debug/examples/dynamic`
complete --keep-order --exclusive --command dynamic --arguments "(COMPLETE=fish /Users/greg/github/devjgm/clap/target/debug/examples/dynamic -- (commandline --current-process --tokenize --cut-at-cursor) (commandline --current-token))"
```

With space, quoted, works.
```console
❯ ln -s ../target/debug/examples/dynamic "dynamic with a space"
❯ COMPLETE=fish ./dynamic\ with\ a\ space
complete --keep-order --exclusive --command dynamic --arguments "(COMPLETE=fish '/Users/greg/github/devjgm/clap/clap_complete/./dynamic with a space' -- (commandline --current-process --tokenize --cut-at-cursor) (commandline --current-token))"
```

....

So I think the fix is not too bad, but I haven't figured out how to tweak the test infra to cover the case of a space somewhere in the command's path. Any pointers here would be appreciated.

---

_Comment by @epage on 2025-12-17 00:58_

We already have a test for spaces.
https://github.com/clap-rs/clap/blob/1a6c0de0d761482a6e2c6e6705a4d9fa0668e842/clap_complete/tests/testsuite/fish.rs#L299

---

_Comment by @devjgm on 2025-12-17 01:25_

Thank  you. But AFAICT, that's not testing for spaces in the _command's path_. I don't see any tests covering that currently. The completions invoke the command like `COMPLETE=fish /path/to/foo`, but it should also work if there's a space in the path, like `COMPLETE=fish '/path/with a space/foo'`. It's this latter case that breaks today due to the format string mentioned above like `"'{completer}'"`

---

_Comment by @epage on 2025-12-17 20:16_

Eh, let's skip a test for now.

---

_Referenced in [clap-rs/clap#6197](../../clap-rs/clap/pulls/6197.md) on 2025-12-17 20:42_

---

_Comment by @devjgm on 2025-12-17 20:43_

PTAL at this PR: https://github.com/clap-rs/clap/pull/6197 It adds a unit test, not an integration test, but I think it demonstrates the _current_ behavior.

Happy to make changes if I'm heading in the wrong direction here. Thanks for your help so far.

---

_Referenced in [clap-rs/clap#6198](../../clap-rs/clap/pulls/6198.md) on 2025-12-18 01:00_

---

_Closed by @epage on 2025-12-18 13:50_

---

_Comment by @devjgm on 2025-12-18 14:28_

Thanks for the guidance on this fix and the quick release, @epage 👍 

---
