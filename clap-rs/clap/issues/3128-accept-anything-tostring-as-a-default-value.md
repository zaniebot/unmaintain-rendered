---
number: 3128
title: "Accept anything `ToString` as a `default_value`"
type: issue
state: closed
author: epage
labels:
  - A-derive
assignees: []
created_at: 2021-12-09T16:18:43Z
updated_at: 2021-12-09T16:40:47Z
url: https://github.com/clap-rs/clap/issues/3128
synced_at: 2026-01-10T01:27:33Z
---

# Accept anything `ToString` as a `default_value`

---

_Issue opened by @epage on 2021-12-09 16:18_

<a href="https://github.com/CodeSandwich"><img src="https://avatars.githubusercontent.com/u/26183680?v=4" align="left" width="96" height="96" hspace="10"></img></a> **Issue by [CodeSandwich](https://github.com/CodeSandwich)**
_Tuesday Apr 07, 2020 at 07:51 GMT_
_Originally opened as https://github.com/TeXitoi/structopt/issues/371_

----

It would be great if `default_value` could accept anything implementing `ToString`, e.g.
```rust
const ABC_DEFAULT: u32 = 123;
...
#[structopt(long, default_value = ABC_DEFAULT)]
abc: u32,
```

or 

```rust
#[structopt(long, default_value = 50 * 2 + 20 + 3)]
abc: u32,
```

It's already possible to pass nothing to `default_value` and then its `ArgType::default().to_string()` is used. This change wouldn't affect existing code, because all string literals implement `ToString`.


---

_Comment by @epage on 2021-12-09 16:18_

<a href="https://github.com/CreepySkeleton"><img src="https://avatars.githubusercontent.com/u/50968528?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [CreepySkeleton](https://github.com/CreepySkeleton)**
_Tuesday Apr 07, 2020 at 14:56 GMT_

----

The main problem here is that `Arg::default_value` takes `&str` and not `String`. We would have to leak the string which may be undesirable.


---

_Comment by @epage on 2021-12-09 16:18_

<a href="https://github.com/CodeSandwich"><img src="https://avatars.githubusercontent.com/u/26183680?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [CodeSandwich](https://github.com/CodeSandwich)**
_Tuesday Apr 07, 2020 at 17:49 GMT_

----

How is empty `default_value` solved? It uses type's `ToString`, does it cache it anywhere?


---

_Comment by @epage on 2021-12-09 16:18_

<a href="https://github.com/CreepySkeleton"><img src="https://avatars.githubusercontent.com/u/50968528?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [CreepySkeleton](https://github.com/CreepySkeleton)**
_Tuesday Apr 07, 2020 at 18:05 GMT_

----

Exactly as described: we leak the string and make use of `lazy_static` to make sure that using it in loops is safe.

https://github.com/TeXitoi/structopt/blob/15f65db7540dfe6818d440329198d98c0ca49505/structopt-derive/src/attrs.rs#L314-L323

Well yeah, we could use direct call for `"..."` string literals and `(expr).to_string().leak()` for everything else.

@TeXitoi What do you think?


---

_Comment by @epage on 2021-12-09 16:18_

<a href="https://github.com/CreepySkeleton"><img src="https://avatars.githubusercontent.com/u/50968528?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [CreepySkeleton](https://github.com/CreepySkeleton)**
_Wednesday Apr 22, 2020 at 13:52 GMT_

----

@TeXitoi gentle ping


---

_Comment by @epage on 2021-12-09 16:18_

<a href="https://github.com/TeXitoi"><img src="https://avatars.githubusercontent.com/u/5787066?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [TeXitoi](https://github.com/TeXitoi)**
_Saturday Apr 25, 2020 at 12:09 GMT_

----

The problem, is that it's really a special case. A lot of other methods would like such a feature (I think in particular to `env`).

And no leak, but a lazy static is acceptable.


---

_Comment by @epage on 2021-12-09 16:18_

<a href="https://github.com/TeXitoi"><img src="https://avatars.githubusercontent.com/u/5787066?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [TeXitoi](https://github.com/TeXitoi)**
_Saturday Apr 25, 2020 at 12:19 GMT_

----

Maybe just https://play.rust-lang.org/?version=stable&mode=debug&edition=2018&gist=884d76afb1211e02a6ddfd2f176cea6d would do the job? (I didn't try inside structopt attribute, but it should work


---

_Comment by @epage on 2021-12-09 16:18_

<a href="https://github.com/CreepySkeleton"><img src="https://avatars.githubusercontent.com/u/50968528?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [CreepySkeleton](https://github.com/CreepySkeleton)**
_Saturday Apr 25, 2020 at 12:20 GMT_

----

Sure, it would. Can I make a PR?


---

_Comment by @epage on 2021-12-09 16:18_

<a href="https://github.com/TeXitoi"><img src="https://avatars.githubusercontent.com/u/5787066?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [TeXitoi](https://github.com/TeXitoi)**
_Saturday Apr 25, 2020 at 13:27 GMT_

----

Ok


---

_Comment by @epage on 2021-12-09 16:35_

`default_value_t` works with any `Display` type

---

_Closed by @epage on 2021-12-09 16:35_

---

_Label `A-derive` added by @epage on 2021-12-09 16:40_

---
