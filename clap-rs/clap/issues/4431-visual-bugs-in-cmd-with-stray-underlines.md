```yaml
number: 4431
title: "Visual bugs in `cmd` with stray underlines"
type: issue
state: closed
author: thatbakamono
labels:
  - C-bug
  - A-help
  - S-blocked
assignees: []
created_at: 2022-10-30T14:36:19Z
updated_at: 2023-03-18T00:14:34Z
url: https://github.com/clap-rs/clap/issues/4431
synced_at: 2026-01-12T16:14:16Z
```

# Visual bugs in `cmd` with stray underlines

---

_@thatbakamono_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

`rustc 1.65.0-nightly (f03ce3096 2022-08-08)`

### Clap Version

`4.0.18`

### Minimal reproducible code

```rs
use clap::Command;

fn main() {
    Command::new("repro").get_matches();
}
```

### Steps to reproduce the bug with the above code

`cargo run -- --help`

### Actual Behaviour

![image](https://user-images.githubusercontent.com/13699966/198883926-98b158b4-a581-40fd-b0bd-c845f19404b5.png)

### Expected Behaviour

![image](https://user-images.githubusercontent.com/13699966/198883870-d65286e6-7ce9-4d28-bb94-9b33ad42b328.png)

### Additional Context

The first screenshot shows cmd, the second screenshot shows windows terminal. I wasn't able to trigger this bug in windows terminal but it's worth noting that even in cmd, it doesn't always happen:
![image](https://user-images.githubusercontent.com/13699966/198883969-7bccb1f1-4c99-4d22-b16e-50bb34394e45.png)

### Debug Output

_No response_

---

_Label `C-bug` added by @thatbakamono on 2022-10-30 14:36_

---

_Comment by @epage on 2022-11-05 03:22_

Tbat most likely means something is going wrong with termcolor's wincon support as that is most likely the main difference between the two terminals.  Whats especially weird is that there is no reason to put an underline after "information" as we only use underline in headers and so we aren't touching it at that point.

If you want to go an extra step, reproducing this in termcolor and creating an upstream issue would be great.  With #1433, we will most likely be moving off of termcolor though, so we'll need to keep in mind to fix this when we do.

---

_Added to milestone `4.x` by @epage on 2022-11-05 03:23_

---

_Label `A-help` added by @epage on 2022-11-05 03:23_

---

_Label `S-blocked` added by @epage on 2022-11-05 03:23_

---

_Comment by @saffaffi on 2022-12-09 22:27_

I'm seeing this happen in Wezterm, but specifically only when the help text is printed when the output in the terminal window has reached the bottom. It happens under that condition with both `TERM=wezterm` and `TERM=xterm-256color`.

---

_Renamed from "Visual bugs in cmd" to "Visual bugs in `cmd` with stray underlines" by @epage on 2022-12-26 14:43_

---

_Referenced in [Yamato-Security/hayabusa#911](../../Yamato-Security/hayabusa/issues/911.md) on 2023-02-09 13:03_

---

_Comment by @YamatoSecurity on 2023-02-18 21:56_

We also have the same problem when running on Windows 10 Command Prompt or Powershell Prompt.
(It does not have this bug in Windows 11)
Rust: 1.67.1
Clap: 4.1.6

At first it looks like this:
![clap-line-1](https://user-images.githubusercontent.com/71482215/219901066-98d4e59a-f244-4ae1-af73-6b542aacbc53.png)

If I resize the window, the line will extend out from the Options group name:
![clap-line-2](https://user-images.githubusercontent.com/71482215/219901071-d6f75eb7-19d4-469a-9d04-3aca947f9dc0.png)


---

_Referenced in [clap-rs/clap#4765](../../clap-rs/clap/pulls/4765.md) on 2023-03-17 15:20_

---

_Referenced in [clap-rs/clap#4767](../../clap-rs/clap/pulls/4767.md) on 2023-03-17 23:58_

---

_Closed by @epage on 2023-03-18 00:14_

---
