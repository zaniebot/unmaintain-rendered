---
number: 3534
title: "Builder API example 1 does not mention `command!()`"
type: issue
state: closed
author: allan2
labels:
  - C-bug
assignees: []
created_at: 2022-03-04T21:18:23Z
updated_at: 2022-03-04T22:12:30Z
url: https://github.com/clap-rs/clap/issues/3534
synced_at: 2026-01-10T01:27:42Z
---

# Builder API example 1 does not mention `command!()`

---

_Issue opened by @allan2 on 2022-03-04 21:18_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

 N/A

### Clap Version

3.15

### Minimal reproducible code

 N/A

### Steps to reproduce the bug with the above code

  N/A

### Actual Behaviour

 N/A

### Expected Behaviour

 N/A

### Additional Context

This example is the first one you see when looking up the Builder API.
The reader may be confused why their example is not working because the `cargo` feature is not mentioned.

### Debug Output

_No response_

---

_Label `C-bug` added by @allan2 on 2022-03-04 21:18_

---

_Referenced in [clap-rs/clap#3535](../../clap-rs/clap/pulls/3535.md) on 2022-03-04 21:19_

---

_Comment by @epage on 2022-03-04 21:21_

Note that
- We do note this in the code itself
- `command!` now tells you to use the macro( might not be in a release yet)

---

_Comment by @allan2 on 2022-03-04 21:57_

I see it!
![docs_command](https://user-images.githubusercontent.com/6740989/156847119-a26246c5-c915-4043-bddf-7400d5ff7093.png)


A new user navigating from the README might click the Builder API, click on example 1, and get stuck unless they go to docs.rs. I think it'd be helpful to mention the `cargo` flag requirement like in Ex. 3.


---

_Comment by @epage on 2022-03-04 22:00_

Let me be more explicit
- The example has a code comment specifying the needed feature
- Calling `command!` without `cargo` activated will produce a compiler error telling the user to activate it (in `master`, not released yet).

In addition to the screenshot you pointed out.

---

_Comment by @allan2 on 2022-03-04 22:10_

I see what you mean now. I missed the comment and I wasn't on master.

Thanks for explaining. I don't think the PR is needed then.

---

_Closed by @allan2 on 2022-03-04 22:12_

---
