```yaml
number: 1011
title: "Clap doesn't allow a short last argument"
type: issue
state: closed
author: marmistrz
labels: []
assignees: []
created_at: 2017-07-24T08:32:57Z
updated_at: 2018-08-02T03:30:09Z
url: https://github.com/clap-rs/clap/issues/1011
synced_at: 2026-01-12T16:14:10Z
```

# Clap doesn't allow a short last argument

---

_@marmistrz_

### Rust Version

1.19.0

### Affected Version of clap

2.25.1

### Expected Behavior Summary
I want to accept one of the following command lines:
```
./my_app -l foo -l bar -- pacman -S rust
```
```
./my_app -l foo -l bar pacman -S rust
```
There may be 0,1,2,... `-l` arguments.

So I did
```
    let matches = App::new(APP)
        .version(VERSION)
        .author(AUTHOR)
        .about("about")
        .arg(
            Arg::with_name("library")
                .short("l")
                .required(false)
                .takes_value(true)
                .multiple(true)
                .last(true),
        )
        .get_matches();
```

### Actual Behavior Summary
This panicks with
```
thread 'main' panicked at 'Flags or Options may not have last(true) set. library has both a short and last(true) set.', /home/marcin/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.25.1/src/app/parser.rs:210
```
It's unclear to me why the library dislikes flags having a `last` option. I could not find another way of achieving the command line I want to parse.

When I found out how to do this, I'll contribute documentation. :)

### Sample Code or Link to Sample Code
https://play.rust-lang.org/?gist=659376caba48dcc6eae0cb3f7fb3854e&version=stable
(couldn't try compiling it, playground is currently out of memory)

### Debug output

No new output appeared.


---

_Comment by @marmistrz on 2017-07-24 10:40_

I got it. I'll contribute an example so that other can avoid that pitfall.

---

_Closed by @marmistrz on 2017-07-24 10:40_

---

_Comment by @snowe2010 on 2017-08-24 16:43_

I'm seeing this issue but with no `last(true)` setting

```
   Compiling clap v2.26.0
   Compiling pt v0.2.2 (file:///Users/tylerthrailkill/Dropbox/rustup/cli)
    Finished dev [unoptimized + debuginfo] target(s) in 31.36 secs
     Running `target/debug/pt start -g -i -s -b`
error: Found argument '-b' which wasn't expected, or isn't valid in this context

USAGE:
    pt start --gateway <BRANCH> --iam-service <BRANCH> --server <BRANCH>

For more information try --help
```

```
cargo run start -- -g -i -s --blah
    Finished dev [unoptimized + debuginfo] target(s) in 0.0 secs
     Running `target/debug/pt start -g -i -s --blah`
works here....✔
```


---
