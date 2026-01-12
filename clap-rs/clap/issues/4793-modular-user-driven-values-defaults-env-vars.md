```yaml
number: 4793
title: "[modular] User-driven values (defaults, env vars, implied values, etc)"
type: issue
state: open
author: epage
labels:
  - C-enhancement
  - A-parsing
  - S-waiting-on-design
assignees: []
created_at: 2023-03-27T18:20:19Z
updated_at: 2023-03-27T18:30:20Z
url: https://github.com/clap-rs/clap/issues/4793
synced_at: 2026-01-12T16:14:16Z
```

# [modular] User-driven values (defaults, env vars, implied values, etc)

---

_@epage_

With clap today, each value source needs to be hard coded, including
- `default_value`
- `default_missing_value`
- [`default_value_if`](https://docs.rs/clap/latest/clap/builder/struct.Arg.html#method.default_value_if)
- [`env`](https://docs.rs/clap/latest/clap/builder/struct.Arg.html#method.env)

When new use cases arise, users either have to
- Hard code the values
- Get a new feature approved and contributed which runs up against #2037 / #1365

It would help if clap had an API for users to provide their own systems for this.  See also #3476.

Example unsupported use cases
- #4788 implied values
  - Implied values should have the same value source as the caused value
  - Ideally we let users leverage an actions default value / default missing value
- #4607: custom env variable source for wasm
- #1634

`default_value` and `default_missing_value` are integral to `Action`s, so I'm thinking we leave those as built-in

Requirements
- Know the source for existing relevant arg values
- Able to set the source for value
- Able to augment the help description of a specific argument with details
- Ideally help uses with `ValueSource` precedence
- Ideally leverage built-in `Action` behavior
- Ideally not so unergonomic to be unuseful

Tools
- Plugin API to allow storing arbitrary data on Command/Arg
- Blanket `impl` of a trait for functions
- `Into` to allow custom plugin data to be converted to the plugin type

---

_Label `C-enhancement` added by @epage on 2023-03-27 18:20_

---

_Label `A-parsing` added by @epage on 2023-03-27 18:20_

---

_Label `S-waiting-on-design` added by @epage on 2023-03-27 18:20_

---

_Comment by @epage on 2023-03-27 18:24_

Idea
```rust
trait ValueSource {
    /// Fill in additional values
    fn fill_values(&self, cmd: &Command, values: &mut ...) {}
    /// Augment an arguments help output with additional information, like defaults, env variables and their values, etc
    fn arg_hint(&self, cmd: &Command, arg: &Arg)  -> Option<Hint> {}
}
```

Ideally, we allow a user to define a source on an `Arg` and automatically adapt it to be for the `Command`, imagine if:
```rust
struct Env {
    name: Str
}

struct CommandEnv {
    id: Id
    value: Env
}

impl From<(&'_ Arg, Env)> for CommandEnv {
    fn from(other: (&'_ Arg, Env)) -> Self {
        Self { id: other.0.get_id(), value: other.1 }
    }
}
```

---

_Referenced in [clap-rs/clap#4607](../../clap-rs/clap/issues/4607.md) on 2023-03-27 20:57_

---

_Referenced in [clap-rs/clap#4605](../../clap-rs/clap/pulls/4605.md) on 2023-03-27 20:57_

---

_Referenced in [clap-rs/clap#4805](../../clap-rs/clap/pulls/4805.md) on 2023-03-28 16:29_

---

_Referenced in [clap-rs/clap#4817](../../clap-rs/clap/issues/4817.md) on 2023-03-31 23:14_

---

_Referenced in [clap-rs/clap#1634](../../clap-rs/clap/issues/1634.md) on 2023-04-07 00:32_

---
