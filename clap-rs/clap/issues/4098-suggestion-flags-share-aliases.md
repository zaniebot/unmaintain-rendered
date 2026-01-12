```yaml
number: 4098
title: "Suggestion: flags share aliases"
type: issue
state: closed
author: InCogNiTo124
labels:
  - C-enhancement
assignees: []
created_at: 2022-08-19T17:21:39Z
updated_at: 2022-08-19T17:54:09Z
url: https://github.com/clap-rs/clap/issues/4098
synced_at: 2026-01-12T16:14:15Z
```

# Suggestion: flags share aliases

---

_@InCogNiTo124_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

3.2.17

### Describe your use case

My use case is reproducing something along of [this feature](https://www.gnu.org/software/datamash/manual/datamash.html#index-_002d_002dheaders):

- `-H`
    - `Same as ‘--header-in --header-out’. A short option indicating the input file has a header line, and the output should contain a header line as well`.

So basically, an boolean flag which enables **two other flags**. Currently, clap does not permit this:

`'short option names must be unique, but '-H' is in use by both 'headers-in' and 'headers-out''`

### Describe the solution you'd like

Ideally, the code should be allowed to look like this:

```rs
Command::new()
.arg(
    Arg::new("headers-in")
        .long("headers-in")
        .short_visible_alias('H')
        .action(ArgAction::SetTrue))
.arg(
    Arg::new("headers-out")
        .long("headers-out")
        .short_visible_alias('H')
        .action(ArgAction::SetTrue))
```

and the invocation would then look something like:
```sh
$ my_program -H x.txt
Headers in detected!
Headers out detected!
```

### Alternatives, if applicable

Right now, the alternative is to have a complicated condition in `if`:

Code right now:
```rs
if *matches.get_one::<bool>("headers-in").unwrap_or(&false) || *matches.get_one::<bool>("headers").unwrap_or(&false) {
.... something ...
}
```

Ideally, this should look like
```
if *matches.get_one::<bool>("headers-in").unwrap_or(&false) {
.... something ...
}
```

### Additional Context

I'm struggling with the source code a bit, however I imagine a hashmap of aliases to an iterable of args. The value of the alias would be the value which is given to the original arguments.

---

_Label `C-enhancement` added by @InCogNiTo124 on 2022-08-19 17:21_

---

_Comment by @epage on 2022-08-19 17:29_

We have the unstable feature [Command::replace](https://docs.rs/clap/latest/clap/struct.App.html#method.replace) that is meant to help with cases like this.  The tracking issue is https://github.com/clap-rs/clap/issues/2836.  No one has stepped up to see it completed though.

Another option is to use `default_value_if`.  It would look something like
```rust
Command::new()
.arg(
    Arg::new("all-headers")
        .short('H')
        .action(ArgAction::SetTrue))
.arg(
    Arg::new("headers-in")
        .long("headers-in")
        .default_value_if("all-headers", Some("true"), Some("true"))
        .action(ArgAction::SetTrue))
.arg(
    Arg::new("headers-out")
        .long("headers-out")
        .short_visible_alias('H')
        .default_value_if("all-headers", Some("true"), Some("true"))
        .action(ArgAction::SetTrue))
```

---

_Comment by @InCogNiTo124 on 2022-08-19 17:48_

The second suggestion seems better than what I've come up with so I'll use this from now on

---

_Closed by @epage on 2022-08-19 17:54_

---
