```yaml
number: 220
title: Macro undefined when using YAML
type: issue
state: closed
author: milgner
labels: []
assignees: []
created_at: 2015-09-03T22:30:44Z
updated_at: 2020-04-11T15:48:49Z
url: https://github.com/clap-rs/clap/issues/220
synced_at: 2026-01-12T16:14:08Z
```

# Macro undefined when using YAML

---

_@milgner_

`Cargo.toml`:

```
[dependencies]
clap = { git = "https://github.com/kbknapp/clap-rs.git" }
```

Simple `main.rs`:

``` rust
#[macro_use] extern crate clap;

use clap::App;

fn main() {
    let yml = load_yaml!("cli.yml");
    let matches = App::from_yaml(yml).get_matches();
}
```

And `cargo build` produces: `src/main.rs:11:15: 11:24 error: macro undefined: 'load_yaml!`

Using the regular, code-based `App` construction works, only I can't get the YAML loading feature to work.


---

_Comment by @Vinatorul on 2015-09-03 22:39_

Thank you for your report!
To use YAML feature you need to specify it in `Cargo.toml`. You can do it in two ways:

```
[dependencies.clap]
features = ["yaml"]
```

or 

```
[dependencies]
clap = { features = ["yaml"] }
```

Take a look at [yaml example](https://github.com/kbknapp/clap-rs/blob/master/examples/17_yaml.rs)

If you will have further questions, or this advice willn't help you - let us know!


---

_Label `T: RFC / question` added by @Vinatorul on 2015-09-03 22:39_

---

_Comment by @milgner on 2015-09-04 08:12_

Many thanks! Sorry for opening a ticket then: looks like my noobness with regards to Rust was the problem then :)
Also, thanks for a great library, a good option parser is really a boon to have!


---

_Closed by @milgner on 2015-09-04 08:12_

---

_Comment by @Vinatorul on 2015-09-04 11:07_

No problem! 
If you will have futher questions - feel free to ask them. You can use our [gitter channel](https://gitter.im/kbknapp/clap-rs) for it.


---

_Comment by @staszewski on 2020-04-11 15:35_

@Vinatorul @milgner How did you manage to run this code? When i do `cargo run --features yaml` with cargo.toml looking like in this
```
[dependencies]
clap = { features = ["yaml"] }
or 
[dependencies.clap]
features = ["yaml"]
```

i get error 
```
:!cargo run --features yaml
error: Package `fun v0.1.0 (/Users/XXX/Projekty/rust/fun)` does not have these fe
atures: `yaml`

shell returned 101
```

---

_Comment by @pksunkara on 2020-04-11 15:48_

You have to do just `cargo run`.

---
