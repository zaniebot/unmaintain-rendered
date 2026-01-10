---
number: 6036
title: Conformation Argument
type: issue
state: closed
author: snowfoxsh
labels:
  - C-enhancement
assignees: []
created_at: 2025-06-16T18:29:01Z
updated_at: 2025-06-16T20:01:27Z
url: https://github.com/clap-rs/clap/issues/6036
synced_at: 2026-01-10T01:28:21Z
---

# Conformation Argument

---

_Issue opened by @snowfoxsh on 2025-06-16 18:29_

### Please complete the following tasks

- [x] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.5.39

### Describe your use case

sometimes you want to ask the user “are you sure?” before doing something destructive. it’d be nice if clap had a built-in way to mark a flag or subcommand as requiring confirmation, instead of rolling that logic by hand every time. maybe something like `#[arg(confirm)]` or a requires_confirmation(true) builder method. 

`#[arg(confirm(default=bool))]` could be used to set if by default (on enter press).
`#[arg(confirm(prompt="this action is destructive, would you like to continue?"))]` could be used to set a custom prompt

### Describe the solution you'd like

before executing the command read from standard input. when the colored feature is enabled then maybe color codes could be added.
```rust
// hook run before command
// basic psudocode
fn confirm(prompt: Option<&str>, default_no: Option<bool>) {
   let prompt_tail = if default_no.unwrap_or(true) {
      "[y/N]"
   } else {
      "[Y/n]"   
   };
  
   let prompt = prompt.unwrap_or("continue");
   print!("{prompt} {prompt_tail}");
   std::io::flush().unwrap();
   
   let mut input = String::new();
   std::io::stdin().read_line(&mut input)).unwrap();
   
   if !matches!(input.trim.to_lowercase().as_str(), "y" | "yes") {
      println!("aborting");
      std::process:exit(1);
   }
}
```

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @snowfoxsh on 2025-06-16 18:29_

---

_Comment by @epage on 2025-06-16 20:01_

This sounds like it is a duplicate of #1634, so closing in favor of that.  If there is a reason to keep this open, let us know!

---

_Closed by @epage on 2025-06-16 20:01_

---
