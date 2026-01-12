```yaml
number: 4013
title: Set the value of an argument from a file, based on an environment variable
type: issue
state: closed
author: nitnelave
labels:
  - C-enhancement
  - A-help
  - A-parsing
  - S-wont-fix
assignees: []
created_at: 2022-08-01T12:18:24Z
updated_at: 2022-08-01T14:13:52Z
url: https://github.com/clap-rs/clap/issues/4013
synced_at: 2026-01-12T16:14:15Z
```

# Set the value of an argument from a file, based on an environment variable

---

_@nitnelave_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

3.1.15

### Describe your use case

I'm writing an application that will be run in a docker container. The application takes some secrets which should be possible to pass as a file, rather than as direct values on the command line or environment.

### Describe the solution you'd like

Currently, for a flag `--password`, it's possible to get an env variable `PASSWORD` and load the value from there. I'd like to be able to have `PASSWORD_FILE` that would point to a file that would be read to get the value of the flag.

E.g.:
```
$ ./my_app --password=abc
Got password: abc
$ PASSWORD=abc ./my_app
Got password: abc
$ PASSWORD_FILE=/tmp/password ./my_app
Got password: abc
$ cat /tmp/password
abc
```

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @nitnelave on 2022-08-01 12:18_

---

_Referenced in [SergioBenitez/Figment#51](../../SergioBenitez/Figment/issues/51.md) on 2022-08-01 13:13_

---

_Comment by @nitnelave on 2022-08-01 13:14_

An alternative would be to use a custom provider with e.g. Figment to load these values: https://github.com/SergioBenitez/Figment/issues/51

The disadvantage of this is that the option doesn't show up in the usage/help.

---

_Label `A-help` added by @epage on 2022-08-01 14:13_

---

_Label `A-parsing` added by @epage on 2022-08-01 14:13_

---

_Label `S-wont-fix` added by @epage on 2022-08-01 14:13_

---

_Comment by @epage on 2022-08-01 14:13_

At the moment, thos seems too specialized of a use case to include in clap.  One workaround  is to document `PASSWORD_FILE` in the `Arg::help` and read it directly if `Arg::help` is not set.  In the future, I hope to make all env support a plugin.  The way I'm thinking of this, it would allow other people to implement their own variation on the same theme.  Again, this would be the user supplying the logic for this, rather than clap directly.

As I feel this is out of scope, I'm closing this issue.  If there are concerns about this, feel free to let us know!

---

_Closed by @epage on 2022-08-01 14:13_

---
