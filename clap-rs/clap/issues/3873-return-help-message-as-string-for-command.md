---
number: 3873
title: Return help message as String for Command
type: issue
state: closed
author: ychiguer
labels:
  - C-enhancement
assignees: []
created_at: 2022-06-27T12:22:02Z
updated_at: 2022-09-22T15:16:40Z
url: https://github.com/clap-rs/clap/issues/3873
synced_at: 2026-01-10T01:27:48Z
---

# Return help message as String for Command

---

_Issue opened by @ychiguer on 2022-06-27 12:22_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

3.1.18

### Describe your use case

The only way I could find to use the help message is to print out it to `Stdout`. In my use case I would like to get the help message as a String as I intend to not print it to `Stdout` directly.

### Describe the solution you'd like

A function that returns the help message as a String.

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @ychiguer on 2022-06-27 12:22_

---

_Referenced in [clap-rs/clap#3874](../../clap-rs/clap/pulls/3874.md) on 2022-06-27 12:25_

---

_Comment by @epage on 2022-06-27 19:19_

We do have [Command::write_help](https://docs.rs/clap/latest/clap/builder/struct.App.html#method.write_help) so its possible to do this today.

The interesting part to me is that for help, we have a `write` function and for usage/version we have a `render` function. We should be more consistent but to what is the question
- only `render` functions
- only `write` functions
- both

---

_Comment by @ychiguer on 2022-06-28 07:26_

Well the question is about `render` functions and especially rendering help.

>  We do have [Command::write_help](https://docs.rs/clap/latest/clap/builder/struct.App.html#method.write_help) so its possible to do this today.
#### I think this function might work but would require a lot of missing around in my opinion, and for my use case I would like it simply rendered as a String. The thing is, I think there should be a `render` function for help as well.

---

_Comment by @epage on 2022-06-29 03:33_

To be clear, my point about `write_help` was in response to the Issue stating that the user can't write to an in-memory structure.  It is possible.  It can still be valid to ask what the API should more generally look like though.

btw a quick example for `write_help`:
```rust
fn main() {
    let mut cmd = clap::Command::new("myprog");

    let mut out: Vec<u8> = Vec::new();
    cmd.write_help(&mut out).expect("failed to write to buffer");
    let out = String::from_utf8(out).expect("help is utf-8");

    dbg!(&out);
}
```
Beyond a `render` variant, this requires:
- Initializing the buffer
- Converting the buffer to a `String`

Random thoughts on the proposal to simplify things for people who just want a `String`:
- The gap between `write` and `render` is small, making this a lower priority for me
- `write` reason for existing is more likely an optimization to avoid allocating a medium sized buffer just for someone writing to stdout/stderr.  That optimization is unlikely to have a noticeable impact on any application using it.
- We currently offer more `render` functions so being at least consistent with that for now seems reasonable
- We will likely be revisiting all of these API calls when I re-examine colored output in clap 4
  - I try to avoid user churn where we add a function just to remove it afterwards.  We do it at times (`ArgSetting::MultipleOccurrences` -> `Arg::multiple_occurrences` -> `ArgAction::Append`) but we didn't predict that.  In this case, we have an expected area of investigation that could have a dramatic impact on the API
  - However, the cost is the same for someone using one way vs the other when we come out with that new API.  The bigger concern then is if we deprecate a previous API variant but we are unlikely to be doing that here

Thinking through that, I think I am fine with moving forward with this as long as the review churn ends up being low.


---

_Closed by @ychiguer on 2022-07-01 07:49_

---

_Comment by @epage on 2022-07-01 14:41_

@ychiguer how come this is closed?  The related PR isn't merged yet and generally Issues are closed as completed when their PR is merged and github will do this automatically if you use the right syntax.  See https://docs.github.com/en/issues/tracking-your-work-with-issues/linking-a-pull-request-to-an-issue#linking-a-pull-request-to-an-issue-using-a-keyword

---

_Reopened by @epage on 2022-07-01 14:41_

---

_Comment by @ychiguer on 2022-07-01 14:45_

Must have done it by mistake.

---

_Referenced in [clap-rs/clap#4248](../../clap-rs/clap/pulls/4248.md) on 2022-09-22 14:57_

---

_Closed by @epage on 2022-09-22 15:16_

---
