---
number: 2044
title: "May Clap has ability to define `short` have more than one letter in #[clap] macro?"
type: issue
state: closed
author: ccqpein
labels: []
assignees: []
created_at: 2020-08-04T04:17:59Z
updated_at: 2020-08-04T08:31:41Z
url: https://github.com/clap-rs/clap/issues/2044
synced_at: 2026-01-07T13:12:19-06:00
---

# May Clap has ability to define `short` have more than one letter in #[clap] macro?

---

_Issue opened by @ccqpein on 2020-08-04 04:17_

### Make sure you completed the following tasks

- [x] Searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] Searched the closed issues

### Describe your use case

I am trying to define my struct with `#[clap]` macro with Clap 3.

Like: 
```rust
#[derive(Default, Clap, Debug)]
#[clap(version = "0.1.0")]
struct Test {
    #[clap(short, long)]
    dd:String,
    #[clap(short = "dx", long)]
    dir:String,
}
```

`dd` and `dir` have a conflict when I compiling it. 

### Describe the solution you'd like

I expand code and figure out `dir` expand to `.short("dx".chars().nth(0).unwrap())`. So the dir is `-d`, has conflict with `dd`

### Alternatives, if applicable

Just change `dx` to another letter.



---

_Label `T: new feature` added by @ccqpein on 2020-08-04 04:17_

---

_Comment by @CreepySkeleton on 2020-08-04 08:31_

> Just change dx to another letter.

That would be it. 

```rust
#[derive(Default, Clap, Debug)]
#[clap(version = "0.1.0")]
struct Test {
    #[clap(short, long)]
    dd:String,
    #[clap(short = "x", long)]
    dir:String,
}
```

Anyway, I don't think we want to commit to something like that. I saw only so many (very few) CLIs that use something resembling sevearl-letter short flags and it never looked good.

---

_Closed by @CreepySkeleton on 2020-08-04 08:31_

---
