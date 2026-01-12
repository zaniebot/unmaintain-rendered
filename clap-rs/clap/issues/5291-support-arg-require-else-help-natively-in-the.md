```yaml
number: 5291
title: "Support `arg_require_else_help` natively in the derive"
type: issue
state: open
author: epage
labels:
  - C-enhancement
  - A-derive
  - S-waiting-on-design
assignees: []
created_at: 2024-01-08T17:35:29Z
updated_at: 2024-01-08T17:36:16Z
url: https://github.com/clap-rs/clap/issues/5291
synced_at: 2026-01-12T16:14:16Z
```

# Support `arg_require_else_help` natively in the derive

---

_@epage_

Right now, users of `arg_required_else_help` must define their args as being overrideable by the subcommand, even if generally required:
```rust
#[derive(Parser)]
struct Cli {
    #[arg(long, required = true)]
    input: Option<PathBuf>,
    #[command(subcommand)]
    subcommand: Option<Subcommand>,
}
```

In the derive, we could instead express this as
```rust
#[derive(Parser)]
enum Cli {
    #[command(these_are_top_level_args)]
    Args {
        #[arg(long)]
        input: PathBuf,
    },
    // ...
}
```

The main question is what to call `these_are_top_level_args` attribute. 

---

_Label `A-derive` added by @epage on 2024-01-08 17:35_

---

_Label `S-waiting-on-design` added by @epage on 2024-01-08 17:35_

---

_Comment by @epage on 2024-01-08 17:36_

Could we just use `flatten` and rely on it being a variant-struct to distinguish it from other `flatten`?  

Probably not because someone might want to define those args in a different struct and then the syntax would be ambiguous and we wouldn't know what code to generate.

---

_Label `C-enhancement` added by @epage on 2024-01-08 17:36_

---
