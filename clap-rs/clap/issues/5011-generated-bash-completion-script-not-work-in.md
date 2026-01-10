---
number: 5011
title: Generated bash completion script not work in POSIX mode
type: issue
state: open
author: hehaoqian
labels:
  - C-bug
  - E-medium
  - A-completion
assignees: []
created_at: 2023-07-15T09:50:25Z
updated_at: 2023-07-18T02:19:39Z
url: https://github.com/clap-rs/clap/issues/5011
synced_at: 2026-01-10T01:28:05Z
---

# Generated bash completion script not work in POSIX mode

---

_Issue opened by @hehaoqian on 2023-07-15 09:50_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

1.70.0

### Clap Version

4.3.11

### Minimal reproducible code

```rust
// build.rs

use clap::{CommandFactory, Parser};
use clap_complete::{generate_to, Shell};

#[derive(Parser)]
pub struct Args {
    pub input_file: String,
}

fn main() {
    let mut cmd = Args::command();
    let out_dir = std::env::var("OUT_DIR").unwrap();
    generate_to(Shell::Bash, &mut cmd, "hello-world", out_dir).unwrap();
}
```


### Steps to reproduce the bug with the above code

1. cd into output directory
2. run the generated script in bash posix mode
```
bash --posix -c "source ./hello-world.bash"
```

### Actual Behaviour

The shell errors with the following message, with non-zero return code

./hello-world.bash: line 36: `_hello-world': not a valid identifier

### Expected Behaviour

The script should work in bash posix mode.

I want to emphasize that this issue report, 
does NOT request to make the script work in non-bash, POSIX only "sh" shell,
such as `/bin/sh` in ubuntu.
Only requesting to make the script work in POSIX mode of `/bin/bash`

Bash doc <https://www.gnu.org/software/bash/manual/html_node/Bash-POSIX-Mode.html> says

> Function names must be valid shell names. 
That is, they may not contain characters other than letters, digits, and underscores, and may not start with a digit. 
Declaring a function with an invalid name causes a fatal syntax error in non-interactive shells.

In my example, the function name of generated script is `_hello-world`, which is invalid in POSIX mode

This should be simple to fix, if dash character is properly escaped

The simplest way would be replacing `-` by `_`, 
but it would cause conflict if generating completion script for both `hello-world` and `hello_world`
Maybe a more complex escape is needed for robustness.
Also, additional escape needed to satisfy `they may not contain characters other than letters, digits, and underscores, and may not start with a digit` requirement
 

### Additional Context

There are several ways to enter bash in posix mode:
1. If bash is compiled with `--enable-strict-posix-default`, then posix mode is the default
2. Environment variable POSIXLY_CORRECT is set when bash is invoked
3. Shell variable POSIXLY_CORRECT  is set in bash script
4. bash is invoked with "bash --posix"
5. bash is invoked with "sh", such as invoking `/bin/sh` in CentOS7, which symlinks `/bin/sh` to `/bin/bash`

### Debug Output

_No response_

---

_Label `C-bug` added by @hehaoqian on 2023-07-15 09:50_

---

_Comment by @epage on 2023-07-18 02:19_

My first question is how much do we care about posix mode but then I saw

> bash is invoked with "sh", such as invoking /bin/sh in CentOS7, which symlinks /bin/sh to /bin/bash

I'm fine with someone going in and fixing this with a reasonable level of care taken to avoid name conflicts.

---

_Label `E-medium` added by @epage on 2023-07-18 02:19_

---

_Label `A-completion` added by @epage on 2023-07-18 02:19_

---

_Referenced in [clap-rs/clap#5240](../../clap-rs/clap/pulls/5240.md) on 2024-01-04 12:47_

---
