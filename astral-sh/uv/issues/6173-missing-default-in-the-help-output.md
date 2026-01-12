```yaml
number: 6173
title: "missing default in the `--help` output."
type: issue
state: open
author: ChannyClaus
labels:
  - documentation
  - cli
assignees: []
created_at: 2024-08-17T19:08:29Z
updated_at: 2024-08-18T13:19:08Z
url: https://github.com/astral-sh/uv/issues/6173
synced_at: 2026-01-12T15:59:01Z
```

# missing default in the `--help` output.

---

_@ChannyClaus_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
from `uv pip install --help`:
```
      --link-mode <LINK_MODE>
          The method to use when installing packages from the global cache [env: UV_LINK_MODE=] [possible
          values: clone, copy, hardlink, symlink]
```
it is not immediately obvious what the default is for `--link-mode`. it's not prohibitively difficult to figure out the default from looking at the code but it would be even nicer to be able to not have to look at the code? https://github.com/astral-sh/uv/blob/ad8e3a2c323d97c83acf541453ac65725c7c2edc/crates/install-wheel-rs/src/linker.rs#L251-L259

there seem to be some other flags like `--resolution` which doesn't show the default in the `--help` output, while `--index-url` does have the default included:
```
  -i, --index-url <INDEX_URL>
          The URL of the Python package index (by default: <https://pypi.org/simple>) [env: UV_INDEX_URL=]
      --extra-index-url <EXTRA_INDEX_URL>
```
(similar issue for other commands `uv pip compile --annotation-style`, etc)

---

_Comment by @zanieb on 2024-08-17 21:04_

Agree we should say what the default value is — though sometimes it differs based on runtime information. Is there a way we can get Clap to include this for us?

---

_Label `enhancement` added by @zanieb on 2024-08-17 21:04_

---

_Label `cli` added by @zanieb on 2024-08-17 21:04_

---

_Label `enhancement` removed by @zanieb on 2024-08-17 21:04_

---

_Label `documentation` added by @zanieb on 2024-08-17 21:04_

---

_Comment by @ChannyClaus on 2024-08-17 23:28_

seems like `default_value_t` (or something similar) should do the trick?

```
$ cat src/main.rs 
use clap::Parser;

/// Simple program to greet a person
#[derive(Parser, Debug)]
#[command(version, about, long_about = None)]
struct Args {
    /// Name of the person to greet
    #[arg(short, long)]
    name: String,

    /// Number of times to greet
    #[arg(short, long, default_value_t = 1)]
    count: u8,
}

fn main() {
    let args = Args::parse();

    for _ in 0..args.count {
        println!("Hello {}!", args.name);
    }
```

```
$ cargo run --quiet -- --help
Simple program to greet a person

Usage: clap-test [OPTIONS] --name <NAME>

Options:
  -n, --name <NAME>    Name of the person to greet
  -c, --count <COUNT>  Number of times to greet [default: 1]
  -h, --help           Print help
  -V, --version        Print version
```
let me see if this is easily adaptable for `uv`.

---

_Comment by @eth3lbert on 2024-08-18 11:30_

You should be able to make it work by changing `Option<LinkMode>` to `LinkMode` and setting `default_value_t` to `LinkMode::default()`.

```
      --link-mode <LINK_MODE>                  The method to use when installing packages from the global cache [env: UV_LINK_MODE=] [default: clone] [possible values: clone, copy, hardlink, symlink]
```

---

_Comment by @zanieb on 2024-08-18 13:19_

Oh interesting. So we'll need to audit all of our uses of `Option` for CLI options? Fun. I'm willing to review these — preferably one at a time? not sure.

---
