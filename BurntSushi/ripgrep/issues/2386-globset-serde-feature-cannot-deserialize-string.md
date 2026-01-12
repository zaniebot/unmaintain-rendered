```yaml
number: 2386
title: "globset: `serde` feature - cannot deserialize `String` into `Glob`"
type: issue
state: closed
author: jeertmans
labels:
  - rollup
assignees: []
created_at: 2022-12-31T17:45:18Z
updated_at: 2023-07-08T22:52:55Z
url: https://github.com/BurntSushi/ripgrep/issues/2386
synced_at: 2026-01-12T16:13:24Z
```

# globset: `serde` feature - cannot deserialize `String` into `Glob`

---

_@jeertmans_

Hi, I hope that posting issues about subcrates is ok.

#### What version of globset are you using?

`globset = { version = "0.4.9", features = ["serde1"] }`.

#### Describe your bug.

Because the implementation of trait `Deserialize` uses `&str`, it does not accept deserializing owned `String`.

https://github.com/BurntSushi/ripgrep/blob/61101289fabc032fd8e90009c41d0b78e6dfc9a2/crates/globset/src/serde_impl.rs#L15-L22

#### What are the steps to reproduce the behavior?

This problem occurred to me when I tried to parse `Glob`s from a `toml::Value` object (already obtained from a file). As `toml::Value` owns strings, it caused a problem.

If needed, I can include a MWE (but quite large). 

#### What is the actual behavior?

When parsing some globs, here is the error obtained:

```
Error: TomlDecode(Error { inner: ErrorInner { kind: Custom, line: None, col: 0, at: None, message: "invalid type: string \"src/**.rs\", expected a borrowed string", key: [] } })
```

#### What is the expected behavior?

Not sure if it is interesting performance-wise, but using a custom `deserialize_with` function, with `String` instead of `&str`, fixes the problem (`glob.as_str()`) has to be used afterward).

I can provide tests and make a PR if you are ok with this solution. Otherwise, I am also open to discussions :-)

---

_Comment by @BurntSushi on 2022-12-31 18:01_

I don't think perf is relevant here, since building a glob from a string is quite expensive. Here are the things that will make a PR easier to merge:

* Small change with tests.
* No breaking changes.
* Have someone with more experience with Serde APIs sign off on this. From your issue, I'm not actually totally sure whether this is a problem with how you're using Serde or if it's truly a problem with how Serde is impled in `globset. 

@LPGhatguy could you please review this? You're the one who originally added Serde support to `globset`.

---

_Comment by @jeertmans on 2023-01-01 12:20_

Thanks for the quick reply!

After comparing with what is done in the [`regex_serde` crate](https://github.com/tailhook/serde-regex/blob/336bb456ecd146ba9e3fcc2fef71870f603d72c5/src/lib.rs#L179-L191), I changed from `String` to `Cow<str>`, in the hope (?) to avoid unnecessary copy.

I created a runnable example that illustrates the error, and how using a different deserialize function can solve it: see https://www.rustexplorer.com/b/qagwmp.

The code is reproduced here:

```rust
/*
[dependencies]
serde = { version = "1", features = ["derive"] }
globset = { version = "0.4.9", features = ["serde1"] }
toml = "0.5.10"
*/

use globset::Glob;
use serde::{Deserialize, Deserializer};

#[derive(Debug, Deserialize)]
struct S {
    // (un)comment the following line to see the error
    //#[serde(deserialize_with = "deserialize_glob")]
    #[allow(unused)]
    glob: Glob,
}

#[allow(unused)]
fn deserialize_glob<'de, D: Deserializer<'de>>(
    deserializer: D,
) -> std::result::Result<Glob, D::Error> {
    use serde::de::Error;
    use std::borrow::Cow;
    let glob = <Cow<str> as Deserialize>::deserialize(deserializer)?;

    Glob::new(&glob).map_err(D::Error::custom)
}

fn main() -> Result<(), Box<dyn std::error::Error>> {
    let string = "glob = '*.md'";

    // Parse toml from borrowed string into S
    let _: S = toml::from_str(string)?;

    // Parse toml from borrowed string into intermediate toml::Value
    let v: toml::Value = toml::from_str(string)?;

    // Convert toml::Value into S
    //   will fail without custom deserialize function
    //   because v owns the String
    let _: S = v.try_into()?;

    Ok(())
}
```

---

_Comment by @LPGhatguy on 2023-01-08 05:19_

Based on how [`serde::Deserialize` is implemented for `Cow<'a, T>`](https://docs.rs/serde/latest/src/serde/de/impls.rs.html#1822-1834), I don't think that using `Cow<str>` will behave any differently than `String`.

I think the correct solution here is to implement our own visitor that handles both the borrowed and owned case. Always using the owned case (using `String` or `Cow<str>`) will always allocate, even when we don't need to.

---

_Comment by @jeertmans on 2023-01-08 10:36_

Hi @LPGhatguy, I have updated the PR accordingly :-)

---

_Label `rollup` added by @BurntSushi on 2023-07-07 18:45_

---

_Closed by @BurntSushi on 2023-07-08 22:52_

---
