---
number: 2575
title: "Custom exit status on clap::Error"
type: issue
state: closed
author: ceca69ec
labels: []
assignees: []
created_at: 2021-07-08T16:09:20Z
updated_at: 2021-07-26T14:54:01Z
url: https://github.com/clap-rs/clap/issues/2575
synced_at: 2026-01-07T13:12:19-06:00
---

# Custom exit status on clap::Error

---

_Issue opened by @ceca69ec on 2021-07-08 16:09_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Clap Version

2.33.3

### Describe your use case

Give the option to the user (of the crate, obviously) to show custom error status according to internal errors of their implementation.

### Describe the solution you'd like

Addition of this function on `src/errors.rs`
```rust
pub fn exit_with_status(&self, status: i32) -> ! {
    if status != 0 {
        wlnerr!("{}", self.message);
        process::exit(status);
    }
    let out = io::stdout();
    writeln!(&mut out.lock(), "{}", self.message).expect("Error writing Error to stdout");
    process::exit(0);
}
```
Usage example
```rust
fn main() {
    handle_arguments(&init_clap().get_matches()).unwrap_or_else(|err| {
        clap::Error::with_description(
            &err.message(),
            clap::ErrorKind::InvalidValue
        ).exit_with_status(err.status());
    });
}
```

### Alternatives, if applicable

The above function is just a draft. Any better idea/implementation is welcome.

### Additional Context

I have used the suggested function the way I described and works well.
I don't know if it's the better place to ask but I would like to become a contributor of `clap`.
Thanks for your time and attention.

---

_Label `T: new feature` added by @ceca69ec on 2021-07-08 16:09_

---

_Comment by @pksunkara on 2021-07-16 21:32_

This already exists, https://docs.rs/clap/2.33.3/clap/struct.Error.html#method.exit

---

_Comment by @ceca69ec on 2021-07-17 02:46_

Exists with the exit status prefixed to 1. My suggestion and function example are to enable the customization of the number (code/status) of the error. Here are the function (exit_with_status) I've suggested again:
```rust
pub fn exit_with_status(&self, status: i32) -> ! {
    if status != 0 {
        wlnerr!("{}", self.message);
        process::exit(status);
    }
    let out = io::stdout();
    writeln!(&mut out.lock(), "{}", self.message).expect("Error writing Error to stdout");
    process::exit(0);
}
```

---

_Comment by @pksunkara on 2021-07-17 03:56_

Well, all the errors have status code 0 or 1, which are then correctly used in that method

---

_Comment by @ldm0 on 2021-07-20 18:59_

Have you tried to use [`try_get_matches`](https://docs.rs/clap/3.0.0-beta.2/clap/struct.App.html#method.try_get_matches) and do the customization on the Error type?

---

_Comment by @ceca69ec on 2021-07-20 20:42_

The customization I'm suggesting is exactly on the status codes. Simply returning 0 or 1 it's not always enough. Some times cli tools inform specific status code number for operating with other tools and just to inform the user.

Customize the status of each error is a good suggestion too.
As I said: It's just a suggestion but I think it's really handy in some situations.

---

_Comment by @pksunkara on 2021-07-25 13:40_

> to enable the customization of the number (code/status) of the error

I am unable to find a good reason to do this. I am closing this issue and will reconsider if more people request something like this.

As an alternative, you do have [`ErrorKind`](https://docs.rs/clap/2.33.3/clap/enum.ErrorKind.html) which you can use to `match` and then provide custom errors yourself.

---

_Closed by @pksunkara on 2021-07-25 13:40_

---

_Comment by @epage on 2021-07-26 14:54_

With clap2 using exit_code=1, I customized the exit code in my application.  I chose USAGE_ERR rather than 2, which is what clap3 has settled on.

For an example of a single exit code, see https://github.com/crate-ci/typos/blob/master/src/bin/typos-cli/main.rs#L23

It is a bit messier than I would like and reimplements some details but I think the importance is less now that clap3 is using exit_code=2.

---
