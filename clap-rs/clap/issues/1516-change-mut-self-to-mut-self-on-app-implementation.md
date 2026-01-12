```yaml
number: 1516
title: "Change `mut self` to `&mut self` on `App` Implementation and Possible Variants"
type: issue
state: closed
author: erayerdin
labels: []
assignees: []
created_at: 2019-07-05T12:25:05Z
updated_at: 2022-09-02T12:44:51Z
url: https://github.com/clap-rs/clap/issues/1516
synced_at: 2026-01-12T16:14:11Z
```

# Change `mut self` to `&mut self` on `App` Implementation and Possible Variants

---

_@erayerdin_

<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

### Rust Version

* rustc 1.34.0 (91856ed52 2019-04-10)

### Affected Version of clap

```
name = "clap"
 "clap 2.33.0 (registry+https://github.com/rust-lang/crates.io-index)",
"checksum clap 2.33.0 (registry+https://github.com/rust-lang/crates.io-index)" = "5067f5bb2d80ef5d68b4c87db81601f0b75bca627bc2ef76b141d7b846a3c6d9"
```

### Feature Request Summary

`&self` instead of `self` in `Arg` implementation.

### Expected Behavior Summary

```rust
// assuming I have an instance called `app`
// and I have two `Arg` instance called `arg1` and `arg2`
app.arg(arg1)
app.arg(arg2) // I expect it to succeed
```

### Actual Behavior Summary

Compiler yells that I have moved `app` in `app.arg(arg1)` and fails on `app.arg(arg2)`.

### Steps to Reproduce the issue

Told above.

### Sample Code or Link to Sample Code

I actually discussed this [on a thread in r/rust](https://www.reddit.com/r/rust/comments/c7ujoq/hey_rustaceans_got_an_easy_question_ask_here/eswcw86?utm_medium=android_app&utm_source=share) and provided sample codes there.

### Further Elaboration

I do not know if this is a design choice due to enforcing *builder pattern*, but I find it convenient to define my `Arg`s and `SubCommand`s as a variable beforehand and then put it into builder pattern. What I find convenient is:

```rust
// assuming I have `app`

let sc1 = SubCommand::with_name("sc1");
let foo_arg = Arg::with_name("foo");
sc1.arg(foo_arg);

let sc2 = SubCommand::with_name("sc2");
let bar_arg = Arg::with_name("bar");
sc2.arg(bar_arg);

app.subcommand(sc1);
app.subcommand(sc2); // fails here because app moved in the line above
// because `subcommand` signature contains `mut self`, not `&mut self`
```

I am aware there are methods (i) that I can provide `SubCommand`s and `Arg`s as array as in `subcommands` and `args` methods that accept a reference to an array and (ii) that I can use a builder kind-of pattern when constructing subcommands and args and adding them into `App`.

However, don't you think it would be better to separate the definition of `Arg`s and/or `SubCommand`s beforehand due to the pros below:

 - **It would be more convenient** for programmer's mind.
 - **It would be more programmatic** while constructing `Arg`s and `SubCommand`s.
 - **It would be way more readable** if we seperated the definitions of `Arg`s and `SubCommand`s. Instead of nesting them each other, separating them would make it more easier to read and debug in mind.

On the other hand, I, kinda, understand why the current approach is chosen:

 - **You might like to enforce builder pattern**
 - Or **defining the app once and keeping it immutable during the lifetime of a program** would make it safer so nobody could change the `Arg`s or `SubCommand`s while the program runs.

I could not find or define a possible variant of this issue so I have wanted to create myself. If this is duplicate or is not considered to be implemented, core developers can close the issue. Thanks in advance.

---

_Comment by @comfortablynick on 2019-07-18 21:42_

As a workaround, this will work:

```rust
// assuming I have `app`
let app = app.subcommand(sc1);
let app = app.subcommand(sc2); 
```
I'm not sure if there are any performance penalties, but that's what I've done when I needed to get around the builder pattern (e.g., conditionally calling a method on `app`).

---

_Comment by @CreepySkeleton on 2020-02-01 13:12_

> I do not know if this is a design choice due to enforcing builder pattern

Yes, this is intentional design choice. `&mut self` and `mut self` are different types of builders, this is described [there](https://docs.rs/derive_builder/0.7.2/derive_builder/#builder-patterns) in detail, both have their own pros and cons. `clap` made it's choice.

---

_Closed by @CreepySkeleton on 2020-02-01 13:12_

---

_Label `T: RFC / question` added by @pksunkara on 2020-02-01 19:53_

---

_Label `W: wont do` added by @pksunkara on 2020-02-01 19:53_

---

_Comment by @ModProg on 2022-09-01 21:23_

Can this decision be reconsidered?

At least [`derive_builder` claims](https://docs.rs/derive_builder/latest/derive_builder/index.html#-performance-considerations):

> Luckily Rust is clever enough to optimize these clone-calls away in release builds for your every-day use cases. Thats quite a safe bet - we checked this for you. ;-) Switching to consuming signatures (=self) is unlikely to give you any performance gain, but very likely to restrict your API for non-chained use cases.

The restrictiveness is very noticeable when dynamically generating a command and ending up needing a mutable variable you need to reassign for each conditional setting etc. or when having interactions in loops.

Changing it to `&mut` would either require any function consuming an e.g. Command to allow a reference, or it would need to have a build() method cloning it. So I can understand the hesitation changing this. On top of the obvious breaking change.

Just wanted to voice my opinion, running into this problem immediately when using the builder pattern as I used the derive macro until now.


My problem would probably be a lot less noticeable when all the builder methods would take options, but e.g. version takes the `&str` directly, meaning the only way to conditionally set it, is like so:
```rs
if let Some(version) = version {
  command = command.version(version);
}
// or (non mut variable)
let command = if let Some(version) = version {
  command.version(version)
} else {
  command
}
```
As many of my uses look like that, enabling support for passing options into e.g. `version()` would improve this greatly.



---

_Comment by @epage on 2022-09-01 22:30_

Personally, the reason I dislike `&mut self` builders is
````rust
let cmd = std::process::Command::new("foo").arg("bar");
```
fails to compile because `cmd` is capturing a `&mut` and instead you need to do
```rust
let mut cmd = std::process::Command::new("foo").;
cmd.arg("bar");
```

> Luckily Rust is clever enough to optimize these clone-calls away in release builds for your every-day use cases. Thats quite a safe bet - we checked this for you. ;-) Switching to consuming signatures (=self) is unlikely to give you any performance gain, but very likely to restrict your API for non-chained use cases.

I would be very cautious about relying on the compiler optimization as its dependent on inlining.  The function probably needs a `#[inline(always)]`.

---

_Comment by @ModProg on 2022-09-02 09:37_

But maybe we could add `set_*` methods that can be used on `&mut`?

---

_Comment by @epage on 2022-09-02 12:23_

Not really thrilled with having both builders in the API
- I don't want to keep up two different sets and it will be easy for contributors to do so
- This puts the dilemma of choice on users which we should be cautious with
- We already have a problem with having too much in rustdoc, this will make it worse
- This will be more for the compiler to chew on and we are looking to drop compile times.

---

_Comment by @ModProg on 2022-09-02 12:44_

Makes sense. Also, I noticed looking at the most recent changes, with the new implementation using `Resettable` it allows using the builder pattern even for conditional `version` etc., so at least for my use case that solves this inconvenience mostly.

---
