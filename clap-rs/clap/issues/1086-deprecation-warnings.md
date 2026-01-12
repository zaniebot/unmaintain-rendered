```yaml
number: 1086
title: Deprecation warnings
type: issue
state: closed
author: durka
labels: []
assignees: []
created_at: 2017-10-27T16:36:25Z
updated_at: 2018-08-02T03:30:13Z
url: https://github.com/clap-rs/clap/issues/1086
synced_at: 2026-01-12T16:14:10Z
```

# Deprecation warnings

---

_@durka_

When compiling clap from git master, I get a bunch of warnings from within clap:

```
   Compiling clap v2.27.1 (file:///home/haptics/proton/nri/overrides/clap)
   Compiling nri v0.1.0 (file:///home/haptics/proton/nri)
warning: use of deprecated item: No longer required to propagate values
   --> overrides/clap/src/macros.rs:745:19
    |
745 |                   $($n::$v => self.0.insert($c)),+
    |                     ^^^^^^
    |
   ::: overrides/clap/src/app/settings.rs
    |
69  | /     impl_settings! { AppSettings,
70  | |         ArgRequiredElseHelp => A_REQUIRED_ELSE_HELP,
71  | |         ArgsNegateSubcommands => ARGS_NEGATE_SCS,
72  | |         AllowExternalSubcommands => ALLOW_UNK_SC,
...   |
109 | |         ContainsLast => CONTAINS_LAST
110 | |     }
    | |_____- in this macro invocation
    |
    = note: #[warn(deprecated)] on by default
```

[The rest are included here.](https://gist.github.com/durka/e687271cc6b04d3af90f67565275453a)

---

_Referenced in [clap-rs/clap#1087](../../clap-rs/clap/pulls/1087.md) on 2017-10-27 16:58_

---

_Closed by @kbknapp on 2017-10-28 14:56_

---
