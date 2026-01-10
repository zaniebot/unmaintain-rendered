---
number: 4674
title: "`disable_colored_help` doesn't work"
type: issue
state: closed
author: danteissaias
labels: []
assignees: []
created_at: 2023-01-25T00:13:41Z
updated_at: 2023-01-25T04:06:30Z
url: https://github.com/clap-rs/clap/issues/4674
synced_at: 2026-01-10T01:27:59Z
---

# `disable_colored_help` doesn't work

---

_Issue opened by @danteissaias on 2023-01-25 00:13_

Similar to #4671 but in error output

```rust
use clap::Parser;

#[derive(Parser, Debug)]
#[command(version, disable_help_subcommand = true, disable_colored_help = true)]
struct Cli {
    package: String,
}

fn main()  {
    let args = Cli::parse();
}
```

`cargo run`

![image](https://user-images.githubusercontent.com/105590557/214450320-122fd43a-cc65-49ac-ae9f-579b7daaa81e.png)

Discussed in #4672


---

_Comment by @epage on 2023-01-25 01:09_

This is working as intended.  `disable_colored_help` is specifically for help.  If you want to disable colors generally, then you want `color = clap::ColorChoice::Never`.

---

_Closed by @epage on 2023-01-25 01:09_

---

_Comment by @danteissaias on 2023-01-25 02:18_

Is there a way to disable the underline/bold styles of usage without removing the error coloring?

---

_Comment by @epage on 2023-01-25 04:06_

#3234 will allow that which is my top priority when I finish with my current project.

---
