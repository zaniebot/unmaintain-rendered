```yaml
number: 5524
title: "Arg::new(\"set-import\").num_args(3).value_names([\"asdf\"]) are listed 3 times in --help; should only list once"
type: issue
state: closed
author: qknight
labels:
  - C-bug
  - A-help
assignees: []
created_at: 2024-06-10T10:16:06Z
updated_at: 2024-06-10T22:15:41Z
url: https://github.com/clap-rs/clap/issues/5524
synced_at: 2026-01-12T16:14:17Z
```

# Arg::new("set-import").num_args(3).value_names(["asdf"]) are listed 3 times in --help; should only list once

---

_@qknight_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

1.78.0

### Clap Version

4.3.24

### Minimal reproducible code

# issue

the --help for set-import should look like:

    --set-import <file> <from> <to>

but instead it looks like:

    --set-import <asdf> <asdf> <asdf>

It will always just repeat what i put into 

    .num_args(3).value_names(["asdf"])

or

    .num_args(3).value_name("file from to")

I also tried subcommand and browsed through all the examples but I couldn't find a mention for a command line option with 3 arguments.

# the --help run

```
(base) PS C:\Users\joschie\Desktop\Projects\fixPath\target\debug> .\fixPath.exe --help
>>> fixPath to modify FS locations of linked DLLs in an PE executable <<<

Usage: fixPath.exe <--version|--list-imports <FILENAME>|--set-import <asdf> <asdf> <asdf>>

Options:
      --version                          Prints the version
      --list-imports <FILENAME>          Calls the list_imports function with a filename
      --set-import <asdf> <asdf> <asdf>  Updates DLL bindings for <from> so it points to <to>
  -h, --help                             Print help
```

# replit

The code is at: 
https://replit.com/@qknight/test

# the code
```rust
use clap::{value_parser, Arg, ArgAction, Command};
use std::{env, fs, process};

fn main() {
    let matches = Command::new("fixPath")
        .about(">>> fixPath to modify FS locations of linked DLLs in an PE executable <<<")
        .arg(
            Arg::new("version")
                .long("version")
                .help("Prints the version")
                .action(ArgAction::SetTrue),
        )
        .arg(
            Arg::new("list-imports")
                .long("list-imports")
                .help("Calls the list_imports function with a filename")
                .value_name("FILENAME"),
        )
        .arg(
            Arg::new("set-import")
                .long("set-import")
                .help("Updates DLL bindings for <from> so it points to <to>")
                .num_args(3)
                .value_names(["asdf"]) // FIXME is printed 3 times in --help, why?!
                .required(false),
        )
        .group(
            clap::ArgGroup::new("commands")
                .args(&["version", "list-imports", "set-import"])
                .required(true)
                .multiple(false),
        )
        .get_matches();

    if matches.get_flag("version") {
        println!("fixPath version 1.0"); // FIXME read from cargo.toml
    } else if let Some(filename) = matches.get_one::<String>("list-imports") {
        println!("{}", filename);
    } else if let Some(values) = matches.get_many::<String>("set-import") {
        let args: Vec<&str> = values.map(|s| s.as_str()).collect();
        println!("set-import: {}, {}, {}", args[0], args[1], args[2]);
    }
}
```


### Steps to reproduce the bug with the above code

Use replit or simply copy the contents into a main.rs and use:

```toml
[package]
name = "fixPath"
version = "0.1.0"
edition = "2021"

[dependencies]
clap = { version = "4.3.24", features = ["derive"] }
```

### Actual Behaviour

It shows:

--set-import <asdf> <asdf> <asdf>

### Expected Behaviour

Should show:

--set-import <file> <from> <to>

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @qknight on 2024-06-10 10:16_

---

_Comment by @epage on 2024-06-10 14:43_

I'm a bit confused by this issue.  Its not clear how the following argument:
```rust
        .arg(
            Arg::new("set-import")
                .long("set-import")
                .help("Updates DLL bindings for <from> so it points to <to>")
                .num_args(3)
                .value_names(["asdf"]) // FIXME is printed 3 times in --help, why?!
                .required(false),
        )
```
is supposed to produce the following help output
```
--set-import <file> <from> <to>
```

Either (or both)
- The example should be updated to match the expected output
- The expected output should be updated for what is expected from the example

---

_Label `A-help` added by @epage on 2024-06-10 14:43_

---

_Comment by @qknight on 2024-06-10 20:07_

@epage I don't know how I could be more clear.

All I want is that the help page reads:

```
(base) PS C:\Users\joschie\Desktop\Projects\fixPath\target\debug> .\fixPath.exe --help
>>> fixPath to modify FS locations of linked DLLs in an PE executable <<<

Usage: fixPath.exe <--version|--list-imports <FILENAME>|--set-import <asdf> <asdf> <asdf>>

Options:
      --version                          Prints the version
      --list-imports <file>          Calls the list_imports function with a filename
      --set-import <file> <from> <to>  Updates <file> DLL bindings for <from> so it points to <to>
  -h, --help                             Print help
```

---

_Comment by @epage on 2024-06-10 20:57_

Can you provide a reproduction example that attempts that?  As I said, the reproduction case in the issue does not.  `asdf` has nothign to do with `file`, `from`, or `to`

---

_Comment by @qknight on 2024-06-10 21:13_

I think we are done here, thanks anyway.

---

_Closed by @qknight on 2024-06-10 21:13_

---

_Comment by @qknight on 2024-06-10 22:15_

For those who read this issue, my solution is now:

```
        .arg(
            Arg::new("set-import")
                .long("set-import")
                .short('s')
                .help("Updates DLL <file> bindings for <from> so it points to <to>, ./fixPath test.exe foo.dll c:\\foo.dll")
                .value_name("arg")
                .num_args(3)
                .required(false),
        )
```
And the help is then:

```
Usage: fixPath.exe <--version|--list-imports <arg>|--set-import <arg> <arg> <arg>>

Options:
      --version                       Prints the version
  -l, --list-imports <arg>            Lists DLL <file> the list_imports function with a filename
  -s, --set-import <arg> <arg> <arg>  Updates DLL <file> bindings for <from> so it points to <to>, ./fixPath test.exe foo.dll c:\foo.dll
  -h, --help                          Print help
```

---
