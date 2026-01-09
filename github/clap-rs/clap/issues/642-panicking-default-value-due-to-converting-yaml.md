---
number: 642
title: Panicking default_value due to converting YAML Real to String
type: issue
state: closed
author: SuperFluffy
labels: []
assignees: []
created_at: 2016-09-01T15:07:17Z
updated_at: 2018-08-02T03:29:53Z
url: https://github.com/clap-rs/clap/issues/642
synced_at: 2026-01-07T13:12:19-06:00
---

# Panicking default_value due to converting YAML Real to String

---

_Issue opened by @SuperFluffy on 2016-09-01 15:07_

Given the minimal example below, I get the following error message:

```
thread 'main' panicked at 'failed to convert YAML Real("0.1") value to a string'
```

Any idea what's going on here?

`main.rs`:

``` rust
#[macro_use] extern crate clap;

use clap::App;

fn main() {
    let yml = load_yaml!("cli.yml");
    let matches = App::from_yaml(yml)
        .get_matches();
}
```

`cli.yml`:

``` yaml
name: Clap Yaml Scrap

args:
    - test:
        takes_value: true
        default_value: 0.1
```

`Cargo.toml`:

``` toml
[package]
name = "clap-yaml-scrap"
version = "0.1.0"
authors = ["SuperFluffy"]

[dependencies]

[dependencies.clap]
version = "2.11"
features = ["yaml"]
```

**EDIT:** Alright, I figure the issue comes from [this macro](https://github.com/kbknapp/clap-rs/blob/b7793a2f4d1132f29b3e7858d4ebbb1027f31d39/src/args/macros.rs#L25). It's calling `as_str()` on the `Yaml::Real`, which is not defined. `as_str()` is only defined for `Yaml::String`, as you can see [here](https://github.com/chyh1990/yaml-rust/blob/f7d2897bc0621ef38d7f6d844c6402d64dcbee23/src/yaml.rs#L232).

`Yaml::Real` only has `as_f64()` defined, which however we cannot a priori invoke as we don't know whether or not `default_value` is a `f64` or something else entirely.

I guess the only way to work around this is by demanding that `default_value` be set to a string in the `Yaml`, i.e. `default_value: "0.1"`.

**EDIT:** Of course, this applies to `Yaml::Integer` as well. Basically, `default_value` _has_ to take a string.


---

_Renamed from "Panicking when converting YAML Real to String" to "Panicking default_value due to converting YAML Real to String" by @SuperFluffy on 2016-09-02 07:12_

---

_Comment by @kbknapp on 2016-09-05 17:49_

This is partly due to that clap deals in strings, i.e. the default value will be the string `"0.1"` not the integer `0.1`. To fix this, use `default_value: "0.1"` and then in the user code convert that string to an integer (via `parse::<f32>()` or whichever is appropriate.

I agree it's confusing, and less than optimal, but logically all command line arguments are strings, and it's the consumer code that does the conversions.

There may be a better way to represent this in the docs/training. Of course I'd love to have `clap` automatically parse out to the correct number type, but at this time that's not quite doable on a large scale and thus gets left to consumer code.


---

_Label `T: RFC / question` added by @kbknapp on 2016-09-05 17:49_

---

_Comment by @SuperFluffy on 2016-09-07 13:01_

Maybe this should be made explicit in one of the docs? At first, I was expecting the `0.1` to be simply parsed as a string just like all the other values in the Yaml.

Since I suspect there is no way of making `yaml-rust` treat all values as strings (due to the spec), maybe this should be made explicit in the docs?


---

_Closed by @kbknapp on 2016-10-21 20:24_

---
