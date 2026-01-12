```yaml
number: 17697
title: "[ty] Add --config CLI arg"
type: pull_request
state: merged
author: thejchap
labels:
  - cli
  - ty
assignees: []
merged: true
base: main
head: feature/red-knot-config-arg
created_at: 2025-04-29T03:09:47Z
updated_at: 2025-05-09T06:38:38Z
url: https://github.com/astral-sh/ruff/pull/17697
synced_at: 2026-01-12T15:56:04Z
```

# [ty] Add --config CLI arg

---

_@thejchap_

## Summary
astral-sh/ty#218
- Add a `--config` CLI arg. Accepts many, where each passed value is a TOML string

## Test Plan
Snapshot tests

---

_Comment by @github-actions[bot] on 2025-04-29 03:12_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @thejchap on 2025-04-29 03:22_

@MichaReiser pausing here for some early feedback. in particular:
- can you clarify what you meant by structural representation [here](https://github.com/astral-sh/ty/issues/218)? i see the ordered list of (rule, level) pairs - do you mean we end up with a list of (setting, value) pairs or something else?
- because we already have `Options::from_toml_str` with `ValueSource` for either CLI/file, my thought had been to have clap return a list of parsed `Options` objects from [from_arg_matches](https://github.com/thejchap/ruff/blob/cceba489d4c7f5f64a1cfaee07731fb783c932ec/crates/red_knot/src/args.rs#L312), which then get applied in `ProjectMetadata::apply_cli_options` - related to the first bullet, let me know if you had something else in mind

---

_Comment by @MichaReiser on 2025-04-29 06:00_

@thejchap what you have here looks good to me. I wasn't aware that Ruff already uses `FromArgs`

> because we already have Options::from_toml_str with ValueSource for either CLI/file, my thought had been to have clap return a list of parsed Options

We could try to fold those `Options` objects eagerly by calling `combine` instead of collecting a `Vec` but that's a minor detail. That, overall, sounds good to me

---

_Label `cli` added by @MichaReiser on 2025-04-29 06:02_

---

_Label `red-knot` added by @MichaReiser on 2025-04-29 06:02_

---

_Comment by @github-actions[bot] on 2025-05-01 04:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/mesonbuild/meson-python">mesonbuild/meson-python</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read tests/packages/symlinks/baz.py: No such file or directory (os error 2)
error: Failed to read tests/packages/symlinks/qux.py: No such file or directory (os error 2)
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/mesonbuild/meson-python">mesonbuild/meson-python</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read tests/packages/symlinks/baz.py: No such file or directory (os error 2)
error: Failed to read tests/packages/symlinks/qux.py: No such file or directory (os error 2)
```

</p>
</details>




---

_@thejchap reviewed on 2025-05-03 04:47_

---

_Review comment by @thejchap on `crates/red_knot/src/args.rs`:24 on 2025-05-03 04:47_

appears to be [allowed in ruff as well](https://github.com/thejchap/ruff/blob/cc78133632470eff8a2e4f208291a4981480aa79/crates/ruff/src/args.rs#L96)

---

_Marked ready for review by @thejchap on 2025-05-03 05:09_

---

_Review requested from @carljm by @thejchap on 2025-05-03 05:09_

---

_Review requested from @MichaReiser by @thejchap on 2025-05-03 05:09_

---

_Review requested from @AlexWaygood by @thejchap on 2025-05-03 05:09_

---

_Review requested from @sharkdp by @thejchap on 2025-05-03 05:09_

---

_Review requested from @dcreager by @thejchap on 2025-05-03 05:09_

---

_Review comment by @MichaReiser on `crates/red_knot/src/args.rs`:314 on 2025-05-03 13:29_

`--config` should only accept configuration arguments. We'll use `--config-file` to allow overriding a configuration file.

---

_Review comment by @MichaReiser on `crates/red_knot/src/args.rs`:324 on 2025-05-03 13:30_

Hmm. It's a bit awkward that we have to create a `system` out of the blue but I see why it's necessary. The only alternative would be is if we expose it using a thread local. 

I think this is fine for now

---

_Review comment by @MichaReiser on `crates/red_knot/src/args.rs`:317 on 2025-05-03 13:31_

Nit
```suggestion
pub(crate) struct ConfigsArg(Option<Options>);

impl clap::FromArgMatches for ConfigsArg {
```

---

_Review comment by @MichaReiser on `crates/red_knot/src/args.rs`:382 on 2025-05-03 13:32_

Nit: Some blank lines between different functions and impl blocks can help readers to parse the code more easily
```suggestion
    }
    
    fn update_from_arg_matches(&mut self, matches: &ArgMatches) -> Result<(), Error> {
        self.0 = Self::from_arg_matches(matches)?.0;
        Ok(())
    }
}

impl clap::Args for ConfigsArg {
    fn augment_args(cmd: clap::Command) -> clap::Command {
        cmd.arg(
            clap::Arg::new("config")
                .short('c')
                .long("config")
                .value_name("CONFIG_OPTION")
                .help("Either a path to a configuration file or a TOML `<KEY> = <VALUE>` pair")
                .action(ArgAction::Append),
        )
    }
    
    fn augment_args_for_update(cmd: clap::Command) -> clap::Command {
        Self::augment_args(cmd)
    }
}

impl ConfigsArg {
    pub(crate) fn into_options(self) -> Option<Options> {
        self.0
    }
    
    /// Attempts to load the configuration argument, first as a file, then as a TOML string
    fn load_single_config(arg: &str, system: &dyn System) -> Result<Options, Error> {
        if let Some(options) = Self::load_single_config_from_file(arg, system)? {
            return Ok(options);
        }
        Options::from_toml_str(arg, ValueSource::Cli)
            .map_err(|err| Error::raw(ErrorKind::InvalidValue, err.to_string()))
    }
    
    /// If the argument is a valid path, we attempt to load config from it and error if we can't.
    /// Otherwise, return None
    fn load_single_config_from_file(
        arg: &str,
        system: &dyn System,
    ) -> Result<Option<Options>, Error> {
        let Ok(path) = SystemPathBuf::from_path_buf(arg.into()) else {
            return Ok(None);
        };
        if !system.is_file(path.as_path()) {
            return Ok(None);
        }
        let config_file = ConfigurationFile::from_path(path, system)
            .map_err(|err| Error::raw(ErrorKind::Io, err.to_string()))?;
        Ok(Some(config_file.options))
    }
}
```

---

_Review comment by @MichaReiser on `crates/red_knot/src/main.rs`:129 on 2025-05-03 13:35_

We can only have one `cli_options` struct because it gets passed down to the `MainLoop` which applies the same options again when reloading the configuration in watch mode. 

I'd suggest to combine the `Options` inside `args.into_options` (which should be easily possible because `args` has access to `args.config`. 

---

_@MichaReiser requested changes on 2025-05-03 13:36_

Nice. I should have been more clear that we don't want the "overloaded" semantics from Ruff where `--config` can be used to supply both a configuration file and configuration options. For ty, we want to have two options `--config <value>` and `--config-file <path>`. 

I'll leave it up to you if you want to split the PR or implement both options in this one PR

---

_Review request for @carljm removed by @carljm on 2025-05-03 15:35_

---

_Comment by @MichaReiser on 2025-05-03 17:51_

@thejchap I'll rebase your changes ontop of my renaming PR. Just make sure to pull before making new commits

---

_Review comment by @thejchap on `crates/red_knot/src/args.rs`:324 on 2025-05-03 21:30_

@MichaReiser had a similar thought on instantiating `system` a second time there - agree it's awkward. one thought that crossed my mind was to separate out parsing the passed arguments from actually reading/parsing the config files so that could happen a bit further downstream and we could use the existing `system` instance. leaning towards leaving it as is like you say but if you have any strong feelings the other way happy to refactor

---

_@thejchap reviewed on 2025-05-03 21:30_

---

_@thejchap reviewed on 2025-05-03 21:40_

---

_Review comment by @thejchap on `crates/red_knot/src/main.rs`:129 on 2025-05-03 21:40_

Helpful context - thanks!

---

_@MichaReiser reviewed on 2025-05-03 21:41_

---

_Review comment by @MichaReiser on `crates/red_knot/src/args.rs`:324 on 2025-05-03 21:41_

There's a good chance this gets solved if we introduce the --config-file option because Args could then simply store a string and the loading and processing can happen later 

---

_Renamed from "[red-knot] Add --config CLI arg" to "[red-knot] Add --config and --config-file CLI args" by @thejchap on 2025-05-04 02:33_

---

_@thejchap reviewed on 2025-05-06 02:29_

---

_Review comment by @thejchap on `crates/ty_project/src/db/changes.rs`:179 on 2025-05-06 02:29_

@MichaReiser one thing this leaves undone is supporting config files passed in the cli via `--config-file` in watch mode - it seems like this would either require
- adding a `cli_config_files` property on `MainLoop` that gets passed through to `ProjectDatabase::apply_changes` then `ProjectMetadata::apply_configuration_files`
  - this made the most sense to me but looks like it requires a larger refactor
- reading the configuration files in `Options::into_options`

could use some design input there - also let me know your thoughts on potentially doing that in a separate pr

---

_Review requested from @MichaReiser by @thejchap on 2025-05-06 02:29_

---

_@MichaReiser reviewed on 2025-05-06 06:38_

---

_Review comment by @MichaReiser on `crates/ty_project/src/db/changes.rs`:179 on 2025-05-06 06:38_

> could use some design input there - also let me know your thoughts on potentially doing that in a separate pr

I think it would make sense to split the options into different PRs, considering that `--config` is done (as far as I can tell) and `--config-file `requires quiet some more work.

> adding a cli_config_files property on MainLoop that gets passed through to ProjectDatabase::apply_changes then ProjectMetadata::apply_configuration_files

I'm more inclined to have a wrapper struct that wraps `Options` and an optional configuration path. Simply to reduce the number of floating arguments). 

Reading thef file could still happen outside of `ProjectMetadata::apply_configuration_files`. I think the challenge with this is that we also need to pass the path to the file watcher somehow. This makes me wonder if we should store the path on `ProjectMetadata` instead where it has an `Option<SystemPathBuf>` for when the configuration should be overriden because `--config-file` has some more semantic differences:

> path to a knot.toml configuration file (pyproject.toml files are not accepted). When provided, this file will be used in place of any discovered configuration (including user-level configuration). Red Knot will skip project discovery and default to the current working directory. Paths are anchored at the current working directory.

(this was written from before the ty rename)

And I'm sorry, I should probably have shared this earlier. I sort of assumed this semantics but I can see how this wasn't obvious.

---

_@thejchap reviewed on 2025-05-07 19:56_

---

_Review comment by @thejchap on `crates/ty_project/src/db/changes.rs`:179 on 2025-05-07 19:56_

@MichaReiser shall I reopen this against the new ty repo?

---

_@MichaReiser reviewed on 2025-05-07 20:01_

---

_Review comment by @MichaReiser on `crates/ty_project/src/db/changes.rs`:179 on 2025-05-07 20:01_

No. We still use this repo for all the code. It makes it easier to share different components 

---

_Renamed from "[red-knot] Add --config and --config-file CLI args" to "[red-knot] Add --config CLI arg" by @thejchap on 2025-05-09 01:33_

---

_@thejchap reviewed on 2025-05-09 01:42_

---

_Review comment by @thejchap on `crates/ty_project/src/db/changes.rs`:179 on 2025-05-09 01:42_

> I think it would make sense to split the options into different PRs

sounds good, i agree

> I'm more inclined to have a wrapper struct that wraps Options

also makes sense

> ...path to a knot.toml configuration file (pyproject.toml files are not accepted)....

ah interesting - i think i missed this documentation somewhere initially - where was this from? and that sounds good - so more of an "override" for a single `ty.toml` that gets used, rather than an arbitrary number of config files that get treated like the `--config` option

i'll spin up a separate issue for `--config-file` and link back to this discussion - may work on some other things before coming back to that

---

_Renamed from "[red-knot] Add --config CLI arg" to "[ty] Add --config CLI arg" by @thejchap on 2025-05-09 03:04_

---

_@MichaReiser approved on 2025-05-09 06:38_

This is great. Thank you!

---

_Merged by @MichaReiser on 2025-05-09 06:38_

---

_Closed by @MichaReiser on 2025-05-09 06:38_

---
