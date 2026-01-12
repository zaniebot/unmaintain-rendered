```yaml
number: 2319
title: Cryptic unwrap error when using wrong format of yaml
type: issue
state: closed
author: booleancoercion
labels:
  - C-bug
  - ":money_with_wings: $10"
assignees: []
created_at: 2021-01-30T16:46:37Z
updated_at: 2021-10-26T20:22:06Z
url: https://github.com/clap-rs/clap/issues/2319
synced_at: 2026-01-12T16:14:13Z
```

# Cryptic unwrap error when using wrong format of yaml

---

_@booleancoercion_

Given `Cargo.toml`:
```toml
[package]
name = "testing_testing"
version = "0.1.0"
authors = ["boolean_coercion <booleancoercion@gmail.com>"]
edition = "2018"

[dependencies]
clap = { version = "3.0.0-beta.2", features = ["yaml"] }
```

`main.rs`:
```rust
use clap::{load_yaml, App};

fn main() {
    let yaml = load_yaml!("cli.yaml");
    let matches = App::from(yaml).get_matches();
}

```
And `cli.yaml`:
```yaml
name: yaml_app
version: "1.0"
about: an example for a bug
author: boolean_coercion <booleancoercion@gmail.com>

args:
  - arg:
    short: a
```
The program compiles, however it panics immediately with the following message:
```
thread 'main' panicked at 'called `Option::unwrap()` on a `None` value', C:\Users\bool\.cargo\registry\src\github.com-1ecc6299db9ec823\clap-3.0.0-beta.2\src\build\arg\mod.rs:4353:64
```

When the yaml file is fixed to have 4 spaces of indentation, the error disappears and it works correctly.
The problem isn't with enforcing 4 spaces per se (if that was the intention), although the default indentation style that yaml recommends is 2 spaces which would cause most editors to adopt this style when editing yaml files (for example, VS Code).

Either way, an appropriate error message is necessary here, and the docs and examples don't mention the 4 spaces issue either and just use them silently.

### Version

* **Rust**: `rustc 1.51.0-nightly (44e3daf5e 2020-12-31)`
* **Clap**: `3.0.0-beta.2`

### Debug output

None? I couldn't get a different output when compiling with the debug feature on


---

_Label `T: bug` added by @booleancoercion on 2021-01-30 16:46_

---

_Comment by @pksunkara on 2021-01-30 16:50_

It's not about 4 spaces. You have `arg:` and `short:` starting on the same column. 

---

_Comment by @booleancoercion on 2021-01-30 16:58_

Ah, well, I still believe that's a bug... I also think Github Actions uses the same syntax

---

_Comment by @pksunkara on 2021-01-30 17:03_

That's not a bug, because you wrote it wrong. Them starting on the same column means it's the same object. Equivalent JSON:

```json
{
  "args": [
    {
      "arg": "",
      "short": "a"
    }
  ]
}
```

But what clap needs is `short` being more indented which means the following equivalent JSON:


```json
{
  "args": [
    {
      "arg": {
        "short": "a"
      }
    }
  ]
}
```

I am not sure if we can provide a better error message in this case or not.

---

_Closed by @pksunkara on 2021-01-30 17:03_

---

_Comment by @booleancoercion on 2021-01-30 17:08_

The bug I was referring to was in fact the error message itself.
It's difficult to understand what caused the error when it's an unwrap on a `None` value.
(I understand the syntax was an error on my end though, whoops)

---

_Reopened by @pksunkara on 2021-01-30 17:15_

---

_Added to milestone `3.1` by @pksunkara on 2021-01-30 17:15_

---

_Renamed from "Cryptic unwrap error when using yaml with 2 spaces of indentation" to "Cryptic unwrap error when using wrong format of yaml" by @booleancoercion on 2021-01-30 18:53_

---

_Label `C: yaml parser` added by @pksunkara on 2021-02-22 09:32_

---

_Comment by @pksunkara on 2021-05-31 23:44_

Taking care of `unwraps` in `from(&Yaml)` methods should fix this issue.

---

_Label `:money_with_wings: $10` added by @pksunkara on 2021-05-31 23:44_

---

_Referenced in [clap-rs/clap#2622](../../clap-rs/clap/pulls/2622.md) on 2021-07-25 22:52_

---

_Closed by @pksunkara on 2021-07-27 09:31_

---

_Comment by @Knuckl3head on 2021-10-26 20:22_

Sorry for necroposting, however, I encountered this error as well for formatting issues. In my case, it was forgetting the newline at the end of the yaml file.

This crashed with the error:
![image](https://user-images.githubusercontent.com/68247991/138954874-5ca57fcb-6a4e-4c67-a730-0c4b59b6f5bc.png)

This worked perfectly:
![image](https://user-images.githubusercontent.com/68247991/138954968-9bf21820-5623-4974-b730-e21305aff5cb.png)

The more you know


---
