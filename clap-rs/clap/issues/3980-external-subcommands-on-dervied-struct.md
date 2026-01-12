```yaml
number: 3980
title: External Subcommands on Dervied Struct
type: issue
state: closed
author: Tony-Goat
labels: []
assignees: []
created_at: 2022-07-23T15:39:34Z
updated_at: 2022-09-28T16:16:33Z
url: https://github.com/clap-rs/clap/issues/3980
synced_at: 2026-01-12T16:14:15Z
```

# External Subcommands on Dervied Struct

---

_@Tony-Goat_

Howdy,

I need to parse external subcommands into a struct built using the derive API, and I'm having a hard time figuring out how to do this. I can't find any reference to this kind of functionality in the docs, so is there something I'm missing, or is this just not implemented yet?

---

_Comment by @epage on 2022-07-23 16:50_

At the moment, external subcommands can only be serialzied into a `Vec<String>` or `Vec<OsString>`

See the use of `external_subcommand` in https://docs.rs/clap/latest/clap/_derive/_cookbook/git_derive/index.html

---

_Comment by @epage on 2022-09-28 16:16_

I'm going to assume this is resolved.  If there is anything else remaining for this, let us know!

---

_Closed by @epage on 2022-09-28 16:16_

---
