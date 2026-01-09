---
number: 2090
title: "global_setting(AppSettings::DisableVersion) does not disable autocomplete for subcommands."
type: issue
state: closed
author: newAM
labels:
  - C-bug
  - A-completion
assignees: []
created_at: 2020-08-21T01:17:03Z
updated_at: 2020-08-24T06:45:41Z
url: https://github.com/clap-rs/clap/issues/2090
synced_at: 2026-01-07T13:12:19-06:00
---

# global_setting(AppSettings::DisableVersion) does not disable autocomplete for subcommands.

---

_Issue opened by @newAM on 2020-08-21 01:17_

### Make sure you completed the following tasks

- [x] Searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] Searched the closed issues

### Code
This is a modification of the `bash_completion.rs` example in the `clap_generate` workspace.

```rust
use clap::{App, AppSettings};
use clap_generate::{generate, generators::Bash};
use std::io;

fn main() {
    let mut app = App::new("myapp")
        .global_setting(AppSettings::DisableVersion)
        .subcommand(App::new("test").subcommand(App::new("config")))
        .subcommand(App::new("hello"));

    generate::<Bash, _>(&mut app, "myapp", &mut io::stdout());
}
```

### Steps to reproduce the issue

1. Run `cargo run --example bash_completion` (with above modifications)
2. Observe that there are auto-completion options for `-V --version` in the sub-commands, despite not being valid commands.

```bash
        myapp__test)
            opts=" -h -V  --help --version  config"  # <----- -V and --version should not be here
            if [[ ${cur} == -* || ${COMP_CWORD} -eq 2 ]] ; then
                COMPREPLY=( $(compgen -W "${opts}" -- "${cur}") )
                return 0
            fi
            case "${prev}" in
                
                *)
                    COMPREPLY=()
                    ;;
            esac
            COMPREPLY=( $(compgen -W "${opts}" -- "${cur}") )
            return 0
            ;;
```

### Version

* **Rust**: rustc 1.45.2 (d3fb005a3 2020-07-31)
* **Clap**:  `8454136126b48bd69d9218e19680082cc28fa5ef` (master)

### Actual Behavior Summary

When I disable version globally it disables `-V` and `--version` for subcommands; but these are still auto-completion options for subcommands, and I do not think there should be.

### Expected Behavior Summary

I think that using `AppSettings::DisableVersion` as a `global_setting` should also disable the autocomplete for subcommands.

### Debug output

```
[            clap::build::app]  App::_build
[            clap::build::app]  App::_derive_display_order:myapp
[            clap::build::app]  App::_derive_display_order:test
[            clap::build::app]  App::_derive_display_order:config
[            clap::build::app]  App::_derive_display_order:hello
[            clap::build::app]  App::_create_help_and_version
[            clap::build::app]  App::_create_help_and_version: Building --help
[            clap::build::app]  App::_create_help_and_version: Building help
[clap::build::app::debug_asserts]       App::_debug_asserts
[            clap::build::arg]  Arg::_debug_asserts:help
[            clap::build::app]  App::_build_bin_names
[            clap::build::app]  App::_build_bin_names:iter: bin_name set...
[            clap::build::app]  No
[            clap::build::app]  App::_build_bin_names:iter: Setting bin_name of myapp to myapp test
[            clap::build::app]  App::_build_bin_names:iter: Calling build_bin_names from...test
[            clap::build::app]  App::_build_bin_names
[            clap::build::app]  App::_build_bin_names:iter: bin_name set...
[            clap::build::app]  No
[            clap::build::app]  App::_build_bin_names:iter: Setting bin_name of test to myapp test config
[            clap::build::app]  App::_build_bin_names:iter: Calling build_bin_names from...config
[            clap::build::app]  App::_build_bin_names
[            clap::build::app]  App::_build_bin_names:iter: bin_name set...
[            clap::build::app]  No
[            clap::build::app]  App::_build_bin_names:iter: Setting bin_name of myapp to myapp hello
[            clap::build::app]  App::_build_bin_names:iter: Calling build_bin_names from...hello
[            clap::build::app]  App::_build_bin_names
[            clap::build::app]  App::_build_bin_names:iter: bin_name set...
[            clap::build::app]  No
[            clap::build::app]  App::_build_bin_names:iter: Setting bin_name of myapp to myapp help
[            clap::build::app]  App::_build_bin_names:iter: Calling build_bin_names from...help
[            clap::build::app]  App::_build_bin_names
```


---

_Label `T: bug` added by @newAM on 2020-08-21 01:17_

---

_Comment by @newAM on 2020-08-21 05:42_

I did some triage on this.

`clap_generate/src/generators/mod.rs` uses this block to determine if the version is visible for a sub-command.
```rust
        if !p.is_set(AppSettings::DisableVersion)
            && !shorts_and_visible_aliases.iter().any(|&x| x == 'V')
        {
            shorts_and_visible_aliases.push('V');
        }
```

The expectation of that code is that `subapp.is_set(AppSettings::DisableVersion)` will be `true` when a parent `App` sets it as a global option.  This expectation does not hold true; sub-command global settings do not inherit from the parent.

I tried a quick hack to see what would happen if that expectation were true by changing `App::subcommand` to propagate all parent `g_settings` to children `g_settings`.

```rust
    #[inline]
    pub fn subcommand(mut self, subcmd: App<'help>) -> Self {
        let mut subcmd = subcmd.clone();
        subcmd.g_settings = self.g_settings;
        // this would need to be recursive, but you get the idea.
        for sub in subcmd.get_subcommands_mut() {
            sub.g_settings = self.g_settings;
        }
        self.subcommands.push(subcmd);
        self
    }
```

That mostly fixed the issue, all the sub-commands except for `help` had `-V` and `--version` auto-completion options removed.

Is this the correct place to be applying the fix (in `src/build/app/mod.rs`)?

---

_Comment by @CreepySkeleton on 2020-08-21 06:57_

I think the correct place is `fn _propagate` in `src/build/app/mod.rs`. 

And you should use OR (`|=`) instead of `=`.

---

_Label `C: generator` added by @CreepySkeleton on 2020-08-21 07:00_

---

_Label `P2: need to have` added by @CreepySkeleton on 2020-08-21 07:00_

---

_Added to milestone `3.1` by @CreepySkeleton on 2020-08-21 07:00_

---

_Label `P2: need to have` removed by @CreepySkeleton on 2020-08-21 07:19_

---

_Referenced in [clap-rs/clap#2095](../../clap-rs/clap/pulls/2095.md) on 2020-08-22 15:11_

---

_Referenced in [clap-rs/clap#2102](../../clap-rs/clap/pulls/2102.md) on 2020-08-23 18:02_

---

_Closed by @bors[bot] on 2020-08-24 06:45_

---
