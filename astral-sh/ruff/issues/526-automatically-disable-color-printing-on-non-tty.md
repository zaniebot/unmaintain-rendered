```yaml
number: 526
title: Automatically disable color printing on non-tty terminals
type: issue
state: closed
author: charliermarsh
labels:
  - good first issue
assignees: []
created_at: 2022-10-31T13:14:47Z
updated_at: 2022-11-09T14:50:13Z
url: https://github.com/astral-sh/ruff/issues/526
synced_at: 2026-01-12T15:54:40Z
```

# Automatically disable color printing on non-tty terminals

---

_@charliermarsh_

_No description provided._

---

_Label `enhancement` added by @charliermarsh on 2022-10-31 13:14_

---

_Label `good first issue` added by @charliermarsh on 2022-10-31 13:14_

---

_Comment by @squiddy on 2022-11-09 08:19_

@charliermarsh From my testing, `colored` is already taking care of this, even though it's not mentioned specifically. From their README.

> Respect the CLICOLOR/CLICOLOR_FORCE behavior (see [the specs](http://bixense.com/clicolors/))
> Respect the NO_COLOR behavior (see [the specs](https://no-color.org/))

The page for CLICOLOR has an example of how to implement it, and they do check for TTY.

`colored` is doing it here (2.0.0): https://github.com/mackwic/colored/blob/v2.0.0/src/control.rs#L108

```rust
ShouldColorize {
            clicolor: ShouldColorize::normalize_env(env::var("CLICOLOR")).unwrap_or_else(|| true)
>>                && atty::is(atty::Stream::Stdout),
            clicolor_force: ShouldColorize::resolve_clicolor_force(
                env::var("NO_COLOR"),
                env::var("CLICOLOR_FORCE"),
            ),
            ..ShouldColorize::default()
        }
```

Confirmed on my machine as well.

---

_Comment by @charliermarsh on 2022-11-09 14:49_

Oh, perfect, thank you.

---

_Closed by @charliermarsh on 2022-11-09 14:50_

---
