---
number: 1665
title: How to translate the default clap strings?
type: issue
state: closed
author: silvioprog
labels: []
assignees: []
created_at: 2020-02-03T20:06:16Z
updated_at: 2020-02-04T08:26:18Z
url: https://github.com/clap-rs/clap/issues/1665
synced_at: 2026-01-07T13:12:19-06:00
---

# How to translate the default clap strings?

---

_Issue opened by @silvioprog on 2020-02-03 20:06_

Hi.

As default, clap prints its info in English, e.g.:

```
USAGE:
    program

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information
```

however, I would like to translate it to my country, for example:

```
Uso:
    programa

Flags:
    -h, --help       Imprime ajuda da aplicação.
    -V, --version    Imprime versão da aplicação.
```

(also, notice the translation from `USAGE` to `Uso` and `FLAGS` to `Flags`). But, I would like to keep the default clap argument behavior, for example, the `-v/--version` would keep showing the application version without any extra implementation like this:

```rust
    if cmd_args.is_present("version") {
        println!("{}", config::APP_VERSION);
        return Ok(());
    }
```  

So, is there any clap option to change only the strings for translating but keep the original argument behavior?

TIA

---

_Label `T: duplicate` added by @pksunkara on 2020-02-04 07:45_

---

_Comment by @pksunkara on 2020-02-04 07:47_

If you want to change whole help message, you have to implement the default clap behaviour too. To localize the other strings, we can combine it with already existing issue.

Duplicate of #380

---

_Closed by @pksunkara on 2020-02-04 07:47_

---

_Comment by @CreepySkeleton on 2020-02-04 08:26_

@silvioprog You can replace default messages via `App::help_message` and `App::version_message`

---

_Referenced in [clap-rs/clap#2500](../../clap-rs/clap/issues/2500.md) on 2021-05-26 19:25_

---
