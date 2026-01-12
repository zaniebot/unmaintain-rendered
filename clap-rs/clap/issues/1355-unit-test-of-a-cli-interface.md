```yaml
number: 1355
title: Unit test of a CLI interface
type: issue
state: closed
author: matthieugouel
labels: []
assignees: []
created_at: 2018-10-08T21:10:22Z
updated_at: 2019-09-17T15:41:53Z
url: https://github.com/clap-rs/clap/issues/1355
synced_at: 2026-01-12T16:14:10Z
```

# Unit test of a CLI interface

---

_@matthieugouel_

<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

### Rust Version

* Use the output of `rustc -V`

rustc 1.28.0 (9634041f0 2018-07-30)

### Affected Version of clap

* Can be found in Cargo.lock of your project (i.e. `grep clap Cargo.lock`)

2.32.0

### Question

Hi !
I just started to use `clap` and I was wondering if there were an easy way to do unit tests of the CLI interface.
For instance, I have a `main` function like this :  

```rust
fn main() -> io::Result<()> {
    let matches = App::new("test")
        .version(crate_version!())
        .author(crate_authors!())
        .about(crate_description!())
        .arg(Arg::with_name("FILEPATH")
                 .required(true)
                 .takes_value(true)
                 .index(1)
                 .help("File path of the source code to interpret."))
        .get_matches();

...
```
And I would like to test the error if the user doesn't provide the argument `FILEPATH`. 

I tried something like that but it doesn't work : 

```rust
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn entrypoint_with_no_arguments() {
        assert!(main().is_err())
    }
}
```
It would also be nice to be able to simulate arguments to see how the CLI respond.

Hope it's not a duplicate question.
Have a good day,
Matthieu.




---

_Comment by @killercup on 2019-01-03 16:40_

Hey, for unit tests you probably want to use [`get_matches_safe`](https://docs.rs/clap/2.32.0/clap/struct.App.html#method.get_matches_safe), which returns a result instead of exiting the program. You'll have to deal with printing `--help` and `--version` yourself, though.

If you're not dependent of this being a unit test, you might also want to check out the assert_cmd crate.

---

_Comment by @Dylan-DPC-zz on 2019-09-17 15:41_

I'm assuming this is resolved 

---

_Closed by @Dylan-DPC-zz on 2019-09-17 15:41_

---

_Referenced in [informalsystems/hermes#2275](../../informalsystems/hermes/pulls/2275.md) on 2022-06-14 15:59_

---
