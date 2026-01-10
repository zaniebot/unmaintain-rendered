---
number: 2043
title: specifying default value makes field required
type: issue
state: closed
author: jgarvin
labels:
  - C-bug
  - A-derive
assignees: []
created_at: 2020-07-31T21:22:48Z
updated_at: 2021-10-09T02:09:16Z
url: https://github.com/clap-rs/clap/issues/2043
synced_at: 2026-01-10T01:27:12Z
---

# specifying default value makes field required

---

_Issue opened by @jgarvin on 2020-07-31 21:22_

### How to reproduce

With this declaration passing the argument is not required:

    #[clap(short, long)]
    cores: Vec<u32>,

with this declaration it is:

    #[clap(short, long, default_value = "")]
    cores: Vec<u32>,

I would expect specifying a default value to imply optional, so this doesn't seem like it can be intentional.

### Version

* **Rust**: rustc 1.46.0-nightly (16957bd4d 2020-06-30)
* **Clap**: "3.0.0-beta.1"

---

_Label `T: bug` added by @jgarvin on 2020-07-31 21:22_

---

_Comment by @pksunkara on 2020-07-31 21:56_

I think you are trying to assign your own semantics to clap here. Having a default value makes the arg optional but the value of the arg will not be undefined but will instead be the default value.

---

_Comment by @CreepySkeleton on 2020-08-03 09:34_

`default_value` doesn't work with `Vec`. I don't think it should because it implies "only one value" while Vec is all about multiple values. Ideally, this should be a compile time error. 

But `default_valueS` must work as described - its presence should not affect requiredness. This is a bug in derive.

---

_Assigned to @CreepySkeleton by @CreepySkeleton on 2020-08-03 09:34_

---

_Label `C: derive macros` added by @CreepySkeleton on 2020-08-03 09:35_

---

_Comment by @CreepySkeleton on 2020-08-03 09:36_

@jgarvin The temporary workaround is `#[clap(required = false)]`.

---

_Comment by @pksunkara on 2020-08-11 01:10_

@CreepySkeleton can you explain what exactly the problem is and what the solution should be. I am afraid that I don't understand.

---

_Comment by @CreepySkeleton on 2020-08-12 02:50_

@pksunkara 

```rust
#[derive(Clap)]
struct Opts {
    // this option is required because it's not Option (please excuse the pun)
    a: u32,

    // this option is NOT required, despite the fact it's not Option
    // if absent, the default value will be used
    #[clap(default_value = "1")]
    b: u32,

    // this should be a compile time error because default_value (singular)
    // is kinda meaningless with `Vec`
    //
    // This case is what OP complained about: it turns out the field somehow becomes required (WTF???).
    #[clap(default_value = "1")]
    c: Vec<u32>,

    // this should be a legit - and preferable! - usage
    // the field is not required
    #[clap(default_values = &["1"])]
    d: Vec<u32>
}
```

---

_Added to milestone `3.0` by @pksunkara on 2020-08-12 08:38_

---

_Unassigned @CreepySkeleton by @ldm0 on 2021-03-13 13:19_

---

_Assigned to @ldm0 by @ldm0 on 2021-03-13 13:19_

---

_Comment by @pitschr on 2021-04-23 20:12_

I figured out that `#[clap(required = false)]` is not working when you have like:

```
#[derive(Clap)]
struct Write {
    #[clap(short = 'g', long)]
    group_address: String,

    #[clap(short = 'd', long, required = false)]
    data: String
}
```

as the compiler issues with: 
```
thread 'main' panicked at 'called `Option::unwrap()` on a `None` value', src/main.rs:40:11
```


Resolution:
```
    #[clap(short = 'd', long)]
    data: Option<String>
```

This works and also makes `required = false` obsolete.

My point here is that setting like `#[clap(required = false)]` is obsolete as it is sufficient to use `Option` so rather fixing it perhaps drop the `required` as obsolete?

---

_Comment by @abonander on 2021-08-25 19:53_

I was about to open my own issue for this, but I think it might be a bug when you specify an empty string for `default_value`. 

For example, instead of using `Option<String>` I wanted a value to default to the empty string:

```rust
#[derive(Clap)]
struct Opts {
    #[clap(long, env, default_value = "")]
    some_api_key: String
}
```

This will cause `some_api_key` to be _required_, whereas specifying a non-empty default value does not: 

```rust
#[derive(Clap)]
struct Opts {
    #[clap(long, env, default_value = "blah")]
    some_api_key: String
}
```



---

_Comment by @epage on 2021-08-25 20:10_

@abonander I think I'm missing something.  On `master` I tried
```rust
fn main() {
    use clap::Clap;
    #[derive(Clap, Debug)]
    struct Opts {
        #[clap(long, env, default_value = "")]
        nothing: String,
        #[clap(long, env, default_value = "something")]
        something: String,
    }
    let m = Opts::parse();
    dbg!(m);
}
```

and got
```bash
❯ cargo run
   Compiling clap v3.0.0-beta.4 (/home/epage/src/personal/clap)
   Compiling foo-bin v0.1.0 (/home/epage/src/personal/foo)
    Finished dev [unoptimized + debuginfo] target(s) in 4.27s
     Running `target/debug/foo-bin`
[src/main.rs:67] m = Opts {
    nothing: "",
    something: "something",
}
```

---

_Comment by @abonander on 2021-08-25 20:16_

@epage ah, just checked the `Cargo.lock` of the project, it was specifically `3.0.0-beta.2` that I encountered this issue and I was just now able to reproduce it on that version but it seems to have been fixed by `3.0.0-beta.4`.

---

_Comment by @epage on 2021-10-05 15:06_

With `master`, I can't reproduce this issue
```rust
use clap::Clap;

#[derive(Clap, Debug)]
struct Opts {
    // this option is required because it's not Option (please excuse the pun)
    //a: u32,

    // this option is NOT required, despite the fact it's not Option
    // if absent, the default value will be used
    //#[clap(default_value = "1")]
    //b: u32,

    // this should be a compile time error because default_value (singular)
    // is kinda meaningless with `Vec`
    //
    // This case is what OP complained about: it turns out the field somehow becomes required (WTF???).
    #[clap(default_value = "1")]
    c: Vec<u32>,
    // this should be a legit - and preferable! - usage
    // the field is not required
    //#[clap(default_values = &["1"])]
    //d: Vec<u32>,
}

fn main() {
    let opts = Opts::parse(); // panics
    dbg!(opts);
}
```

```
❯ cargo run
   Compiling test-clap v0.1.0 (/home/epage/src/personal/test-clap)
    Finished dev [unoptimized + debuginfo] target(s) in 0.60s
     Running `target/debug/test-clap`
[src/main.rs:27] opts = Opts {
    c: [
        1,
    ],
}
```
```
❯ cargo run -- --help
    Finished dev [unoptimized + debuginfo] target(s) in 0.01s
     Running `target/debug/test-clap --help`
test-clap 

USAGE:
    test-clap [C]...

ARGS:
    <C>...    [default: 1]

FLAGS:
    -h, --help       Print help information
    -V, --version    Print version information
```

Am I doing something wrong?

---

_Closed by @pksunkara on 2021-10-09 02:09_

---
