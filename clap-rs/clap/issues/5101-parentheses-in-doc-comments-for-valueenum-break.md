---
number: 5101
title: "Parentheses in doc comments for `ValueEnum` break generated fish completions"
type: issue
state: closed
author: cstyles
labels:
  - C-bug
  - A-completion
  - E-easy
assignees: []
created_at: 2023-08-29T19:28:41Z
updated_at: 2023-09-07T13:52:08Z
url: https://github.com/clap-rs/clap/issues/5101
synced_at: 2026-01-10T01:28:07Z
---

# Parentheses in doc comments for `ValueEnum` break generated fish completions

---

_Issue opened by @cstyles on 2023-08-29 19:28_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.72.0 (5680fa18f 2023-08-23)

### Clap Version

4.4.1

### Minimal reproducible code

```rust
use clap::CommandFactory;

#[derive(clap::Parser)]
struct MyCommand {
    #[arg(long)]
    color: Color,
}

#[derive(Clone, clap::ValueEnum)]
enum Color {
    /// do something (default)
    Auto,
    /// do not use colorized output
    Never,
}

fn main() {
    clap_complete::generate(
        clap_complete::shells::Fish,
        &mut MyCommand::command(),
        "ls",
        &mut std::io::stdout(),
    );
}
```


### Steps to reproduce the bug with the above code

```fish
$ cargo run | source
$ ls --color=<TAB>
```

### Actual Behaviour

```fish
$ ls --color=<TAB>fish: Unknown command: default
fish: 
default
^~~~~~^
in command substitution
```

### Expected Behaviour

```fish
$ ls --color=<TAB>
--color=auto  (show colors if the output goes to an interactive console (default))
--color=never                                        (do not use colorized output)
```

### Additional Context

The solution might just be to escape parentheses in `clap_complete::shells::Fish::escape_string`. But A) I'm not sure if there are unintended consequences of doing that and B) the docs for that function mention that it's for escaping single quoted strings which the completions don't use. But it's also used in `value_completion` to escape a string inside double quotes so maybe that comment is just out-of-date? I didn't feel confident making the change so I'll defer to someone more knowledgeable.

### Debug Output

_No response_

---

_Label `C-bug` added by @cstyles on 2023-08-29 19:28_

---

_Label `A-completion` added by @epage on 2023-08-29 19:30_

---

_Label `E-easy` added by @epage on 2023-08-29 19:30_

---

_Comment by @jporwal05 on 2023-09-01 17:08_

I would like to work on this @epage. Please assign this to me.

---

_Assigned to @jporwal05 by @epage on 2023-09-01 17:11_

---

_Comment by @jporwal05 on 2023-09-04 13:35_

Below is my analysis so far:
The `complete` command that `clap_complete` generates for @cstyles's example, for fish shell is this:
```bash
complete -c ls -l color -r -f -a "{auto	do something (default)}"
```
If I go through fish shell's [documentation](https://fishshell.com/docs/current/completions.html#completion-own), I don't see any mention of adding description/hint in the `-a` flag itself. The flag `-d` suits the purpose. So, if I compose the following command:
```bash
complete -c ls -l color -r -f -a auto -d "do something (default)"
```
And then try the auto-completion:
```bash
$ ls --color=<TAB>
```
I get the expected output:
```bash
--color=always  (Use colors)  --color=auto  (do something (default))  --color=never  (Use colors)
```
without any errors. I will do some more analysis with multiple parameters and their description. From the face value, it seems like separate `complete` command will be required for parameters that have a description/hint with them. If that is the case, this might be a bigger change/fix than anticipated.

---

_Comment by @cstyles on 2023-09-04 17:41_

I don't think it's _required_ that we use separate `complete` commands, though that might be a good idea. The relevant bit from that documentation page is this:

> Each line of output is used as a separate candidate, and anything after a tab is taken as the description.

Which is what `clap_complete` is using:

https://github.com/clap-rs/clap/blob/dc63cba772ab1d1a2b977acedd08af2b3baa6692/clap_complete/src/shells/fish.rs#L171-L175

The problem is this, emphasis mine:

> The argument to the -a switch is always a single string. At completion time, it will be tokenized on spaces and tabs, and variable expansion, *command substitution* and other forms of parameter expansion will take place.

I tried a couple different approaches but I think the simplest solution is to wrap the description in single quotes and ditch the comma-escaping. For me, that prevents any expansion inside the description and doesn't require any complicated escaping.

```diff
diff --git a/clap_complete/src/shells/fish.rs b/clap_complete/src/shells/fish.rs
index 5a069d35..175ae6aa 100644
--- a/clap_complete/src/shells/fish.rs
+++ b/clap_complete/src/shells/fish.rs
@@ -169,9 +169,9 @@ fn value_completion(option: &Arg) -> String {
                     None
                 } else {
                     Some(format!(
-                        "{}\t{}",
+                        "{}\t'{}'",
                         escape_string(value.get_name(), true).as_str(),
-                        escape_string(&value.get_help().unwrap_or_default().to_string(), true)
+                        escape_string(&value.get_help().unwrap_or_default().to_string(), false)
                     ))
                 })
                 .collect::<Vec<_>>()
```

---

_Comment by @jporwal05 on 2023-09-05 08:06_

Valid point @cstyles. I tried few more variations with your solution. Here are my observations:
✓
```bash
$ complete -c ls -l color -r -f -a "{auto\t'do something (default)',never\t'no color',always\t'always color'}"
$ ls --color=<TAB>
--color=always  (always color)  --color=auto  (do something (default))  --color=never  (no color)
```
✕ - I believe this is expected, so this case is fine?
```bash
$ complete -c ls -l color -r -f -a "{auto\t'do something %$# (default)',never\t'no &* color',always\t'always!! color'}"
$ ls --color=<TAB>
fish: $# is not supported. In fish, please use 'count $argv'.
complete -c ls -l color -r -f -a "{auto\t'do something %$# (default)',never\t'no &* color',always\t'always!! color'}"
```
✕ - Same as the previous one.
```bash
$ complete -c ls -l color -r -f -a "{auto\t'do something %$ (default)',never\t'no &* color',always\t'always!! color'}"
$ ls --color=<TAB>
fish: $  is not a valid variable in fish.
complete -c ls -l color -r -f -a "{auto\t'do something %$ (default)',never\t'no &* color',always\t'always!! color'}"
```
✓
```bash
$ complete -c ls -l color -r -f -a "{auto\t'do something % (default)',never\t'no &* color',always\t'always!! color'}"
$ ls --color=<TAB>
--color=always  (always!! color)  --color=auto  (do something % (default))  --color=never  (no &* color)
```

So, in summary, the solution to enclose the help text in `''` , without `,` escaping, should work! I will fix.

---

_Referenced in [clap-rs/clap#5111](../../clap-rs/clap/pulls/5111.md) on 2023-09-05 12:11_

---

_Referenced in [clap-rs/clap#5114](../../clap-rs/clap/pulls/5114.md) on 2023-09-07 12:27_

---

_Closed by @epage on 2023-09-07 13:52_

---

_Referenced in [sharkdp/fd#1382](../../sharkdp/fd/pulls/1382.md) on 2023-09-09 19:53_

---
