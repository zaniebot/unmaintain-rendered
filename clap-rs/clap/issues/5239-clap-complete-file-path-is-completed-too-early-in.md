```yaml
number: 5239
title: "clap_complete: File path is completed too early in bash"
type: issue
state: closed
author: sudotac
labels:
  - C-bug
assignees: []
created_at: 2023-12-03T06:19:50Z
updated_at: 2024-01-22T15:44:13Z
url: https://github.com/clap-rs/clap/issues/5239
synced_at: 2026-01-12T16:14:16Z
```

# clap_complete: File path is completed too early in bash

---

_@sudotac_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.74.0 (79e9716c9 2023-11-13)

### Clap Version

clap 4.4.10 (clap_complete 4.4.4)

### Minimal reproducible code

build.rs

```rust
use clap::{CommandFactory, Parser, ValueHint};
use clap_complete::Shell;
use std::env;
use std::path::PathBuf;

#[derive(Parser)]
struct Args {
    #[arg(long, value_hint = ValueHint::FilePath)]
    file: PathBuf,
}

fn main() {
    let mut cmd = Args::command();
    let out_dir = env::var("OUT_DIR").unwrap();
    clap_complete::generate_to(Shell::Bash, &mut cmd, "clap_path_compl", out_dir).unwrap();
}
```

### Steps to reproduce the bug with the above code

```sh
cargo new clap_path_compl
cd clap_path_compl
cargo add clap --build -F derive
cargo add --build clap_complete
vim build.rs # Enter the code as above
cargo build
source target/debug/build/clap_path_compl-*/out/clap_path_compl.bash
```

Enter `./target/debug/clap_path_compl --file ` and press TAB to start completion.

### Actual Behaviour

```
# If user enters as follows:
./target/debug/clap_path_compl --file /us<TAB>

# `/usr ` is completed and move to next argument
./target/debug/clap_path_compl --file /usr 
```


### Expected Behaviour

```
# If user enters as follows:
./target/debug/clap_path_compl --file /us<TAB>

# `/usr/` should be completed
# because `/usr` is a directory, not a file.
./target/debug/clap_path_compl --file /usr/
```


### Additional Context

A possible solution is to use `compopt`.

This issue can be fixed by patching out the completion script as follows:

```diff
--- target/debug/build/clap_path_compl-55b9c400b2155aaf/out/clap_path_compl.bash.orig
+++ target/debug/build/clap_path_compl-55b9c400b2155aaf/out/clap_path_compl.bash
@@ -27,6 +27,7 @@
             case "${prev}" in
                 --file)
                     COMPREPLY=($(compgen -f "${cur}"))
+                    compopt -o filenames
                     return 0
                     ;;
                 *)
```

### Debug Output

_No response_

---

_Label `C-bug` added by @sudotac on 2023-12-03 06:19_

---

_Comment by @sudotac on 2023-12-03 06:44_

By the way, I think it's possible to improve support of other variants of ValueHint with `compopt`, like `-o plusdirs` for `ValueHint::DirPath` or `-o nospace` for `ValueHint::Other`.

---

_Referenced in [clap-rs/clap#5240](../../clap-rs/clap/pulls/5240.md) on 2023-12-03 08:36_

---

_Closed by @epage on 2024-01-22 15:44_

---

_Referenced in [Anomalocaridid/handlr-regex#89](../../Anomalocaridid/handlr-regex/issues/89.md) on 2024-12-19 02:12_

---

_Referenced in [clap-rs/clap#5981](../../clap-rs/clap/pulls/5981.md) on 2025-04-28 17:05_

---
