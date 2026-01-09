---
number: 5374
title: Why do all issues asking for prefix are closed?
type: issue
state: closed
author: DimanNe
labels:
  - C-enhancement
assignees: []
created_at: 2024-02-24T21:39:32Z
updated_at: 2024-02-25T03:03:19Z
url: https://github.com/clap-rs/clap/issues/5374
synced_at: 2026-01-07T13:12:20-06:00
---

# Why do all issues asking for prefix are closed?

---

_Issue opened by @DimanNe on 2024-02-24 21:39_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.5.0

### Describe your use case

I know there are: https://github.com/clap-rs/clap/issues/3513, https://github.com/clap-rs/clap/issues/3117, https://github.com/clap-rs/clap/issues/5050, https://github.com/clap-rs/clap/issues/4556, but there are no solutions (in the issues).

I personally came across this limitation too.

I have multiple ways to specify password:

```
#[derive(Debug, clap::Args)]
#[group(required = true, multiple = false)]
struct ReqPassphrase {
   #[arg(long, default_value_t)]
   passphrase: String,

   #[arg(long)]
   pass_stdin: bool,

   pass_fido: bool,

   pass_fido_rp: String,

   #[arg(long)]
   pass_fido_uid: Option<String>,
}
```

and some subcommands need to know old passphrase and new:

```
   AddKey {
      #[command(flatten)]
      new_passphrase: ReqPassphrase,

      #[command(flatten)]
      old_passphrase: ReqPassphrase,
   },
```
which obviously fails to compile.

How can I implement this without copy-paste? Why are all the issues above closed?

### Describe the solution you'd like

The simplest one: is to provide prefix as an attribute, for example in my case I could provide prefix `old_` and `pass_stdin` would become `old_pass_stdin`

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @DimanNe on 2024-02-24 21:39_

---

_Comment by @epage on 2024-02-25 03:03_

2 of the 4 linked issues are open.  The 2 linked issues list their reason for being closed.

If the reason the issue is closed is unclear, the thing to do would be to post on that issue, rather than open a new issue.

Going to close in favor of the linked issues so we can keep the discussion in one place, rather than creating more places.

---

_Closed by @epage on 2024-02-25 03:03_

---
