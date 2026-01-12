```yaml
number: 3011
title: Flag SubCommands in YAML?
type: issue
state: closed
author: epage
labels:
  - C-bug
assignees: []
created_at: 2021-11-09T16:18:17Z
updated_at: 2021-12-08T20:01:12Z
url: https://github.com/clap-rs/clap/issues/3011
synced_at: 2026-01-12T16:14:14Z
```

# Flag SubCommands in YAML?

---

_@epage_

### Discussed in https://github.com/clap-rs/clap/discussions/3010

<div type='discussions-op-text'>

<sup>Originally posted by **Kibouo** November  9, 2021</sup>
I saw that #1974 added support for sub-commands with a flag-format. However, they don't seem to be available when ingesting YAML?

I tried the following:
```yaml
[...]
subcommands:
    - web:
        about: "web subcommand"
        short_flag: W
        long_flag: web
[...]
```

However, the keys `short_flag` and `long_flag` are silently ignored. Meaning, `cargo run -- --help` shows only `web` as a sub-command.

I expected it to show `-W, --web, web`.</div>

---

_Label `T: bug` added by @epage on 2021-11-09 16:18_

---

_Label `C: yaml parser` added by @epage on 2021-11-09 16:18_

---

_Referenced in [epage/clapng#236](../../epage/clapng/issues/236.md) on 2021-12-06 22:24_

---

_Comment by @epage on 2021-12-08 20:01_

We have decided to deprecate the YAML API.  For more details, see https://github.com/clap-rs/clap/issues/3087.  If there is interest in a YAML API, it will likely be broken out into a `clap_yaml` and developed independent from the core of clap.

---

_Closed by @epage on 2021-12-08 20:01_

---
