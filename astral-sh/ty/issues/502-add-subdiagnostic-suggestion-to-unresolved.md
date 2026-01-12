```yaml
number: 502
title: "Add subdiagnostic suggestion to `unresolved-reference` diagnostic if it's a method scope and the variable exists on `self`"
type: issue
state: closed
author: AlexWaygood
labels:
  - help wanted
  - diagnostics
assignees: []
created_at: 2025-05-23T23:28:35Z
updated_at: 2025-06-04T15:13:52Z
url: https://github.com/astral-sh/ty/issues/502
synced_at: 2026-01-12T15:54:23Z
```

# Add subdiagnostic suggestion to `unresolved-reference` diagnostic if it's a method scope and the variable exists on `self`

---

_@AlexWaygood_

If you do something like this in Rust:

```rs
struct Foo {
    x: u8
}

impl Foo {
    fn method(&self) {
        let y = x;
    }
}
```

Then rustc gives a really nice diagnostic that tells you you probably meant to access `self.x`:

```
error[E0425]: cannot find value `x` in this scope
 --> src/main.rs:7:17
  |
7 |         let y = x;
  |                 ^
  |
help: you might have meant to use the available field
  |
7 |         let y = self.x;
  |                 +++++
```

https://play.rust-lang.org/?version=stable&mode=debug&edition=2024&gist=00fc5f337e983eeedfdcaab2d70df558

We should add a similar suggestion to our `unresolved-reference` diagnostic in situations like this:

```py
class Foo:
    x: int

    def method(self):
        y = x
```

---

_Label `help wanted` added by @AlexWaygood on 2025-05-23 23:28_

---

_Label `diagnostics` added by @AlexWaygood on 2025-05-23 23:28_

---

_Comment by @lipefree on 2025-06-03 13:36_

In the case where the user defined the class the following way

```py
class Foo:
    def method(self):
        y = x
    def method2(self):
        self.x: int = 1
```
Should we emit the suggestion at all ? Or do we emit only if `method2` was called before `method` ? Or do we emit in all cases where a `self.x` is defined somewhere in the class ?  

---

_Comment by @AlexWaygood on 2025-06-03 13:43_

> Should we emit the suggestion at all ? Or do we emit only if `method2` was called before `method` ? Or do we emit in all cases where a `self.x` is defined somewhere in the class ?

(replied on Discord)

---

_Closed by @carljm on 2025-06-04 15:13_

---
