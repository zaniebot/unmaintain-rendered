```yaml
number: 5099
title: Provide CLI arguments in config file
type: issue
state: closed
author: SamuelMarks
labels:
  - C-enhancement
assignees: []
created_at: 2023-08-29T00:15:17Z
updated_at: 2023-08-29T00:39:30Z
url: https://github.com/clap-rs/clap/issues/5099
synced_at: 2026-01-12T16:14:16Z
```

# Provide CLI arguments in config file

---

_@SamuelMarks_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.4.0

### Describe your use case

Writing version and daemon managers. So you begin by `download`ing, `install`ing, and `start`ing a package. Then you `stop`, `install-service` (daemon).

Either by including all the arguments with you every step of the way, or using a config file.

### Describe the solution you'd like

My use case:
- Store JSON of `version` → all provided CLI arguments (uses ENV if not provided otherwise the default value)

To give a concrete of what the JSON would look like:

```json
{
  "15.3.1": {
    "Port": 5430,
    "Database": "my_15_3_db",
    "Username": "thespp",
    "Password": "cxvsd",
  },
  "15.4.0": {
    "Port": 5432,
    "Database": "my_15_4_db",
    "Username": "cvxvx",
    "Password": "45j45lF",
  }
}
```

### Alternatives, if applicable

Working prototype (in Go, but planning a Rust rewrite): https://github.com/offscale/postgres-version-manager-go

I suppose I could use
https://github.com/aobatact/clap-serde

Or straight up `serde` on the `struct` in question and use it that way. But there seems to be a lot of intricacies to consider, like how do I know when:
- A default value has been overriden by an env var or by a CLI arg or given in a config file

Also how do I deserialise it with a version per JSON key. It was pretty hacky to get working in Go.

### Additional Context

Related: #3113

That way you can either provide this JSON file in a `--config` or it will use the default location (assuming you don't have `--no-config-write` set).

---

_Label `C-enhancement` added by @SamuelMarks on 2023-08-29 00:15_

---

_Comment by @epage on 2023-08-29 00:39_

Closing in favor of #2763

---

_Closed by @epage on 2023-08-29 00:39_

---
