---
number: 5445
title: Multilingual Help Commands
type: issue
state: closed
author: Pi-Cla
labels:
  - C-enhancement
  - M-breaking-change
  - A-help
  - S-waiting-on-design
assignees: []
created_at: 2024-04-07T21:13:46Z
updated_at: 2024-04-09T15:53:28Z
url: https://github.com/clap-rs/clap/issues/5445
synced_at: 2026-01-10T01:28:12Z
---

# Multilingual Help Commands

---

_Issue opened by @Pi-Cla on 2024-04-07 21:13_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.5.4

### Describe your use case

What if the user of myapp could access a help page in their own language?

### Describe the solution you'd like

For example a user could type in:
```
myapp -h fr
```
to get the French version of the help page.

and developers of myapp would only need to add one more argument in order to add the translation like so:
```Rust
struct Cli {
    #[arg(help = "Does thing", help(fr,"Fait la chose"))]
    thing: bool,
}
```

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @Pi-Cla on 2024-04-07 21:13_

---

_Comment by @ziyouwa on 2024-04-09 03:23_

All functions automatically created by Clap, hope to be able to customize messages,  as blow:
#[clap(next_help_heading = "选项", version_message_zh_CN = "打印版本信息", help_message_zh_CN = "打印帮助信息")]

---

_Label `M-breaking-change` added by @epage on 2024-04-09 15:50_

---

_Label `A-help` added by @epage on 2024-04-09 15:50_

---

_Label `S-waiting-on-design` added by @epage on 2024-04-09 15:50_

---

_Comment by @epage on 2024-04-09 15:53_

We have #380 for allowing clap to be localized.

Switching `--help` to accepting an argument would be a breaking change for applications.  We could limit the scope to just applications that want support for more than one translation but that also means adopting translations is a breaking change.

I also generally view command-lines as something for state that can change run to run but I would expect this to be something to change by the user, rather than by the invocation instance, and seems like something that belongs in config.

This also feels like its verging on clap becoming a bespoke localization framework and I'd rather smooth out the path for people to choose what localization framework works for them.

With all of that, I lean towards closing this in favor of #380.  If there is a reason we should re-evaluate this, let us know!

---

_Closed by @epage on 2024-04-09 15:53_

---
