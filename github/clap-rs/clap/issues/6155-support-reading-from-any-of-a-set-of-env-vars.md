---
number: 6155
title: support reading from any of a set of env vars
type: issue
state: closed
author: programmerjake
labels:
  - C-enhancement
  - S-triage
assignees: []
created_at: 2025-10-16T12:10:07Z
updated_at: 2025-10-16T14:42:39Z
url: https://github.com/clap-rs/clap/issues/6155
synced_at: 2026-01-07T13:12:20-06:00
---

# support reading from any of a set of env vars

---

_Issue opened by @programmerjake on 2025-10-16 12:10_

### Please complete the following tasks

- [x] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.5.48

### Describe your use case

I have a tool I'm writing that needs to get a directory name, for compatibility it needs to read that from `$DB_DIR`, but since that's awfully generic (it can read from several different pre-built databases depending on the command you give it), I want to also have it read that same argument from something like `$PRJXRAY_DB_DIR` so I can have all the different database env vars set by default in the podman container so you don't need to supply them every time.

### Describe the solution you'd like

support setting multiple different `env` vars on a single `Arg`, with prioritization if more than one of the env vars turn out to be set.

### Alternatives, if applicable

manually handle reading env vars after parsing the command line.

### Additional Context

_No response_

---

_Label `C-enhancement` added by @programmerjake on 2025-10-16 12:10_

---

_Label `S-triage` added by @programmerjake on 2025-10-16 12:10_

---

_Comment by @epage on 2025-10-16 14:42_

We have #5447 for env variable aliases.  I feel like the use cases are overlapping enough that I'm going to close in favor of that.  If there is a reason for us to re-evaluate this, let us know!

---

_Closed by @epage on 2025-10-16 14:42_

---
