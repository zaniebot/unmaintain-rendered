---
number: 3121
title: "How to express \"either ... or ...\" relationship"
type: issue
state: closed
author: epage
labels:
  - A-derive
assignees: []
created_at: 2021-12-09T16:14:56Z
updated_at: 2021-12-09T16:40:58Z
url: https://github.com/clap-rs/clap/issues/3121
synced_at: 2026-01-07T13:12:19-06:00
---

# How to express "either ... or ..." relationship

---

_Issue opened by @epage on 2021-12-09 16:14_

<a href="https://github.com/vbrandl"><img src="https://avatars.githubusercontent.com/u/20639051?v=4" align="left" width="96" height="96" hspace="10"></img></a> **Issue by [vbrandl](https://github.com/vbrandl)**
_Monday Oct 08, 2018 at 14:58 GMT_
_Originally opened as https://github.com/TeXitoi/structopt/issues/141_

----

I want to express the following relationship using structopt: The positional parameter should either be two strings (username and password) or a PathBuf (from where the credentials will be read).

I tried the following:

```rust
#[derive(Debug, StructOpt)]
enum Opt {
    #[structopt(name = "login")]
    Login { param: LoginCredentials },
    #[structopt(name = "logout")]
    Logout,
}
#[derive(Debug, StructOpt)]
enum LoginCredentials {
    UserPass { loginid: String, password: String },
    File { path: PathBuf },
}
``` 

But this will create another subcommand for the `login` subcommand. What would be the right way to express this relationship? Is there even a way to do so?


---

_Comment by @epage on 2021-12-09 16:14_

<a href="https://github.com/TeXitoi"><img src="https://avatars.githubusercontent.com/u/5787066?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [TeXitoi](https://github.com/TeXitoi)**
_Thursday Oct 11, 2018 at 14:38 GMT_

----

You might want to look at https://github.com/TeXitoi/structopt/blob/master/examples/group.rs and https://github.com/TeXitoi/structopt/issues/126


---

_Comment by @epage on 2021-12-09 16:14_

<a href="https://github.com/vbrandl"><img src="https://avatars.githubusercontent.com/u/20639051?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [vbrandl](https://github.com/vbrandl)**
_Thursday Oct 11, 2018 at 14:59 GMT_

----

I found that one already but I think it would be cool if we were able to describe groups using enums. That way it is impossible to represent an illegal state and you could simply match over the group variants.

Is it possible to implement this for structopt?


---

_Comment by @epage on 2021-12-09 16:14_

<a href="https://github.com/TeXitoi"><img src="https://avatars.githubusercontent.com/u/5787066?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [TeXitoi](https://github.com/TeXitoi)**
_Thursday Oct 11, 2018 at 15:14 GMT_

----

That's a new feature, that's why I've tagged this issue enhancement. Then, we can design the details here, and anyone can implement that and do a PR to add it.

I will not implement this feature in short term, as I have others priorities, in this repo and in others. But I encourage anyone interested to contribute, and I'll try to answer any questions as fast as possible.


---

_Comment by @epage on 2021-12-09 16:15_

<a href="https://github.com/vbrandl"><img src="https://avatars.githubusercontent.com/u/20639051?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [vbrandl](https://github.com/vbrandl)**
_Thursday Oct 11, 2018 at 21:42 GMT_

----

Just a few thoughts: 

* How this might be implemented
```rust
#[derive(Debug, StructOpt)]
#[structopt(type = "subcommand")] // this should be implicit
enum Opt {
    #[structopt(name = "login")]
    Login { param: LoginCredentials },
    #[structopt(name = "logout")]
    Logout,
}
#[derive(Debug, StructOpt)]
#[structopt(type = "group")]
enum LoginCredentials {
    UserPass { loginid: String, password: String },
    File { path: PathBuf },
}
```

* Problem: Enums with variants that contain the same types and should be parsed as positional parameters cannot be distinguished and these cases should be compile errors

```rust
#[derive(Debug, StructOpt)]
#[structopt(type = "group")]
enum Group1 {
    Var1 { a: String, b: String },
    Var2 { c: String, d: String }, // this should fail
}

#[derive(Debug, StructOpt)]
#[structopt(type = "group")]
enum Group2 {
    Var1 { 
        #[structopt(short = "a")]
        a: String,
        b: String
    },
    Var2 { c: String, d: String },
} // this should work since the variants are distingushable
```


---

_Comment by @epage on 2021-12-09 16:15_

<a href="https://github.com/TeXitoi"><img src="https://avatars.githubusercontent.com/u/5787066?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [TeXitoi](https://github.com/TeXitoi)**
_Friday Oct 12, 2018 at 10:08 GMT_

----

This is similar to #104 


---

_Comment by @epage on 2021-12-09 16:23_

I believe this is covered by #2621 

If there is something I'm missing though, let me know!

---

_Closed by @epage on 2021-12-09 16:23_

---

_Label `A-derive` added by @epage on 2021-12-09 16:40_

---
