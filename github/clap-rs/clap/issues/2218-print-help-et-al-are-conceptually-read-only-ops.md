---
number: 2218
title: "print_help() et al. are conceptually read-only ops and should not take &mut self"
type: issue
state: closed
author: djeedai
labels:
  - C-bug
  - A-builder
  - S-duplicate
assignees: []
created_at: 2020-11-21T13:23:52Z
updated_at: 2022-01-11T18:29:05Z
url: https://github.com/clap-rs/clap/issues/2218
synced_at: 2026-01-07T13:12:19-06:00
---

# print_help() et al. are conceptually read-only ops and should not take &mut self

---

_Issue opened by @djeedai on 2020-11-21 13:23_

`print_help()` and other similar variants conceptually just print information to some output based on the already-configured `App`, and therefore it is quite unexpected for them to be mutating the `App` and take a `&mut self` reference. For a typical command-line tool which might want to print-help-and-exit deeper inside the program due to other errors not directly handled by `clap`'s argument validation, this forces `App` to be passed by `&mut self` everywhere for no good apparent reason, and thereby makes other unrelated parts of the code more complex.

I looked at the implementation, and it seems this is due to some "propagation of globals and settings", which I understand to be lazy operations which need to be done once and could have been done at another time and/or because they are conceptually not mutating the app should be hidden behind a `&self` read-only reference. I might be wrong here, as I am no `clap` expert, but I see no good argument from a user standpoint as to why a print function would mutate anything.

### Version

* **Rust**: rustc 1.48.0 (7eac88abb 2020-11-16)
* **Clap**: 2.33.3

### Actual Behavior Summary

```rust
pub fn print_help(&mut self) -> Result<()>
```

### Expected Behavior Summary

```rust
pub fn print_help(&self) -> Result<()>
```


---

_Label `T: bug` added by @djeedai on 2020-11-21 13:23_

---

_Comment by @pksunkara on 2020-11-21 13:25_

For better performance. There should be an issue like this already.

---

_Comment by @djeedai on 2020-11-21 14:04_

I couldn't find any other issue sorry. I've searched for `print_help` but nothing seems to match.

I understand that how the implementation is lazily doing some ops may be a performance reason, but in my opinion that doesn't warrant making the public interface mutable. Isn't that precisely what interior mutability is for? I can't find the official reference I took that from, but here's another one : https://ricardomartins.cc/2016/06/08/interior-mutability
> There are a few general cases that call for interior mutability, such as:
> 3. Implementation details of logically immutable methods

---

_Label `W: maybe` added by @pksunkara on 2020-11-21 14:05_

---

_Referenced in [epage/clapng#169](../../epage/clapng/issues/169.md) on 2021-12-06 20:19_

---

_Comment by @epage on 2021-12-13 15:10_

Closing this in favor of https://github.com/clap-rs/clap/issues/2911 which has more details on the plan going forward.

---

_Closed by @epage on 2021-12-13 15:10_

---

_Label `S-waiting-on-decision` removed by @epage on 2022-01-11 18:29_

---

_Label `A-builder` added by @epage on 2022-01-11 18:29_

---

_Label `S-duplicate` added by @epage on 2022-01-11 18:29_

---
