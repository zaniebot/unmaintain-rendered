---
number: 4631
title: "Extra blank lines when the argument name is long enough in `--help`"
type: issue
state: closed
author: madchicken
labels:
  - C-bug
  - A-help
assignees: []
created_at: 2023-01-12T13:54:38Z
updated_at: 2023-01-12T15:25:30Z
url: https://github.com/clap-rs/clap/issues/4631
synced_at: 2026-01-07T13:12:20-06:00
---

# Extra blank lines when the argument name is long enough in `--help`

---

_Issue opened by @madchicken on 2023-01-12 13:54_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.64.0 (a55dd71d5 2022-09-19)

### Clap Version

clap = { version = "3.2.23", features = ["derive"] }

### Minimal reproducible code

```rust
use clap::{Parser};

#[derive(Parser, Debug)]
#[clap(author, version, about, long_about = None)]
pub struct Config {
    /// Your username
    #[clap(short, long, value_parser)]
    username: String,
    /// Your password
    #[clap(short, long, value_parser)]
    password: String,
    /// Server URI
    #[clap(long, value_parser, default_value = "http://localhost:8581")]
    uri: String,
    /// Authorization keys file. Default to authorization-keys.yml in the current working directory.
    #[clap(long, value_parser, default_value = "authorization-keys.yml")]
    keyfile_path: String,
    /// Metrics webserver port (service /metrics for Prometheus scraper)
    #[clap(long, value_parser, default_value = "9123")]
    port: u16,
}


fn main() {
    let _config = Config::parse();
}
```


### Steps to reproduce the bug with the above code

Run the command:
```
cargo run -- --help
```


### Actual Behaviour

```
mycli 0.1.0

USAGE:
    mycli [OPTIONS] --username <USERNAME> --password <PASSWORD>

OPTIONS:
    -h, --help
            Print help information

        --keyfile-path <KEYFILE_PATH>
            Authorization keys file. Default to authorization-keys.yml in the current working
            directory [default: authorization-keys.yml]

    -p, --password <PASSWORD>
            Your password

        --port <PORT>
            Metrics webserver port (service /metrics for Prometheus scraper) [default: 9123]

    -u, --username <USERNAME>
            Your username

        --uri <URI>
            Server URI [default: http://localhost:8581]

    -V, --version
            Print version information
```

### Expected Behaviour

```
mycli 0.1.0

USAGE:
    mycli [OPTIONS] --username <USERNAME> --password <PASSWORD>

OPTIONS:
    -h, --help                         Print help information
        --keyfile-path <KEYFILEPATH>    Authorization keys file. Default to authorization-keys.yml in
                                       the current working directory [default:
                                       authorization-keys.yml]
    -p, --password <PASSWORD>          Your password
        --port <PORT>                  Metrics webserver port (service /metrics for Prometheus
                                       scraper) [default: 9123]
    -u, --username <USERNAME>          Your username
        --uri <URI>                    Server URI [default: http://localhost:8581]
    -V, --version                      Print version information
```

### Additional Context

The output shows extra new lines.
By changing the name of the parameter `keyfile_path` to `keyfilepath` the issue goes away.

### Debug Output

_No response_

---

_Label `C-bug` added by @madchicken on 2023-01-12 13:54_

---

_Comment by @epage on 2023-01-12 14:50_

While #3300 is for `-h`, a lot of the conversation is the same.  We can leave this open if you want, to track any `--help` specific aspects.

See also #4550

---

_Label `A-help` added by @epage on 2023-01-12 14:50_

---

_Renamed from "Bad formatting depending on the name of a parameter" to "Extra blank lines when the argument name is long enough in `--help`" by @epage on 2023-01-12 14:51_

---

_Comment by @madchicken on 2023-01-12 15:17_

It's fine for me to mark this as a duplicate. And sorry for this, I searched through the open issues but I could not find my problem (my bad)

---

_Closed by @epage on 2023-01-12 15:25_

---
