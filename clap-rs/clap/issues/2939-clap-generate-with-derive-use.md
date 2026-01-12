```yaml
number: 2939
title: clap_generate with derive use
type: issue
state: closed
author: ghost
labels:
  - A-docs
assignees: []
created_at: 2021-10-24T19:28:00Z
updated_at: 2021-10-26T21:56:49Z
url: https://github.com/clap-rs/clap/issues/2939
synced_at: 2026-01-12T16:14:14Z
```

# clap_generate with derive use

---

_@ghost_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Clap Version

3.0.0-beta.5

### Where?

https://docs.rs/clap_generate/3.0.0-beta.5/clap_generate/

### What's wrong?

docs only show using :App but coming from structopt am using derive[] instead. how to use generate completions with this pattern instead?

### How to fix?

maybe expand docs with other example or add disclaimer if only work with one pattern not other

---

_Label `C: docs` added by @ghost on 2021-10-24 19:28_

---

_Comment by @epage on 2021-10-25 13:26_

This will look something like:
```rust
use clap::{App, ValueHint, Partser, IntoApp};
use clap_generate::{generate, Generator, Shell};
use std::io;

#[derive(Parser, Debug)]
struct Cli {
    file: Option<std::path::PathBuf>,
    #[clap(long, arg_enum)]
    generate: Option<Shell>,
}

fn print_completions<G: Generator>(gen: G, app: &mut App) {
    generate(gen, app, app.get_name().to_string(), &mut io::stdout());
}

fn main() {
    let cli = Cli::parse();

    if let Some(generator) = cli.generate {
        let mut app = Cli::into_app();
        eprintln!("Generating completion file for {}...", generator);
        print_completions(generator, &mut app);
    }
}
```

The key is the `IntoApp` trait.

---

_Added to milestone `3.0` by @epage on 2021-10-25 13:26_

---

_Comment by @epage on 2021-10-26 13:38_

I forgot, we do have an example, but its in the `clap_derive` crate: https://github.com/clap-rs/clap/blob/master/clap_derive/examples/value_hints_derive.rs

I wonder if we should move that over to `clap_generate`

---

_Referenced in [clap-rs/clap#2948](../../clap-rs/clap/pulls/2948.md) on 2021-10-26 14:35_

---

_Closed by @bors[bot] on 2021-10-26 21:56_

---
