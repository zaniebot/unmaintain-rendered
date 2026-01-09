---
number: 5901
title: "Resolution order conflict between env arguments and `value_parser`"
type: issue
state: closed
author: asibahi
labels:
  - C-bug
assignees: []
created_at: 2025-02-05T01:21:04Z
updated_at: 2025-02-05T09:55:49Z
url: https://github.com/clap-rs/clap/issues/5901
synced_at: 2026-01-07T13:12:20-06:00
---

# Resolution order conflict between env arguments and `value_parser`

---

_Issue opened by @asibahi on 2025-02-05 01:21_

### Please complete the following tasks

- [x] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.84.1 (e71f9a9a9 2025-01-27)

### Clap Version

version = "4.5.27"

### Minimal reproducible code

Even a minimal example would be a lot of code. The summary of it is that I have a `value_parser` that makes an API call that depends on credentials.

```rust
pub enum MinionType {
    BloodElf, Draenei,   Dwarf,  Gnome, // etc
}

impl FromStr for MinionType {
    type Err = anyhow::Error;

    fn from_str(s: &str) -> Result<Self, Self::Err> {
        get_metadata() // <-- api call depends on credentials
            .iterator_stuff()
    }
}
```

The Credentials in the library are loaded thus

```rust
static BLIZZARD_CLIENT_AUTH: RwLock<Option<(String, String)>> = RwLock::new(None);

pub fn set_blizzard_client_auth(id: String, secret: String) {
    _ = BLIZZARD_CLIENT_AUTH.write().insert((id, secret));
}
```

The call parts are essentially this:
```rust
#[derive(Args)]
pub struct BGArgs {
    #[arg(short = 'T', long = "type", value_parser = str::parse::<MinionType>)]
    minion_type: Option<MinionType>,

}

#[derive(Parser)]
#[command(author, version)]
struct Cli {
    // credentials in question being loaded
    #[arg(env(mimiron::BLIZZARD_CLIENT_ID), hide_env_values(true))]
    id: String,

    #[arg(env(mimiron::BLIZZARD_CLIENT_SECRET), hide_env_values(true))]
    secret: String,

    #[command(subcommand)]
    command: Commands,
}

#[derive(Subcommand)]
enum Commands {
    BG(bg::BGArgs),
}

pub fn run() -> Result<()> {
    let args = Cli::parse();

    // loading said credentials
    mimiron::set_blizzard_client_auth(args.id, args.secret);

    match args.command {
        // here code is delegated to places
        Commands::BG(args) => bg::run(args, locale)?,
    }

    Ok(())
}
```

The issue essentially is that clap is attempting to parse `MinionType` before loading the API calls. I have absolutely no idea how to resolve this issue.

### Steps to reproduce the bug with the above code

Explained all in there

### Actual Behaviour

As earlier

### Expected Behaviour

Code execution order isn't clear to me.

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @asibahi on 2025-02-05 01:21_

---

_Comment by @asibahi on 2025-02-05 09:55_

I have decided to invoke/validate the parsing manually instead of relying on clap to do it, which is probably the best way to handle this anyway.

---

_Closed by @asibahi on 2025-02-05 09:55_

---
