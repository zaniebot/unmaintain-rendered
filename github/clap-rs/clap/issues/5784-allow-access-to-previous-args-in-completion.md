---
number: 5784
title: Allow access to previous args in completion function for context-sensitive completions
type: issue
state: open
author: LucasPickering
labels:
  - C-enhancement
  - A-completion
  - S-waiting-on-design
assignees: []
created_at: 2024-10-21T12:10:02Z
updated_at: 2025-01-20T15:28:46Z
url: https://github.com/clap-rs/clap/issues/5784
synced_at: 2026-01-07T13:12:20-06:00
---

# Allow access to previous args in completion function for context-sensitive completions

---

_Issue opened by @LucasPickering on 2024-10-21 12:10_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

clap 4.4.2, clap_complete 4.5.29

### Describe your use case

In a [ArgValueCompleter](https://docs.rs/clap_complete/latest/clap_complete/engine/struct.ArgValueCompleter.html) function, it would be nice to have access to what has been parsed so far when generating completions. My use case is that I'm loading the arguments from a YAML file, but the file to load from can be overridden by a prior `-f` argument. Another identical example is [docker-compose run](https://docs.docker.com/reference/cli/docker/compose/run/): You can run `docker-compose run <service>` in which case it will load services from `docker-compose.yml`, or you can do `docker-compose -f different/compose/file.yml run <service>`, in which case it should load from `different/compose/file.yml`.

### Describe the solution you'd like

Access to what has already been parsed as an argument to the `ArgValueCompleter` function. This would most likely be a breaking change because it requires adding an argument to [ValueCompleter::complete](https://docs.rs/clap_complete/latest/clap_complete/engine/trait.ValueCompleter.html). The user impact could be mitigated though by keeping the blanket impl on `fn(&OsStr) -> Vec<CompletionCandidate>`. I imagine most users are not implementing `ValueCompleter` themselves.

The best I can think of is the additional argument is just a `&[&OsStr]` containing what's already been parsed. The completer would then be responsible for iterating over that and figuring out the semantic meaning. It'd be great if we could get the prior arguments in a more structured form, but doing that without the full command present is probably not possible. So an example completion function for something like `docker-compose run` would look like:

```rust
fn complete_service(current: &OsStr, previous: &[&OsStr]) -> Vec<CompletionCandidate> {
    let file_arg_index = previous.iter().position(|arg| arg == "-f" || arg == "--file");
    let path = if let Some(file_index) = file_arg_index {
        previous[file_arg_index + 1]
    } else {
        "docker-compose.yml"
    };
    let Ok(config) = load_config(path) else {return vec![]};
    config
        .services
        .into_iter()
        .map(|service| CompletionCandidate::new(service.name))
        .collect()
}
```

(this is pseudocode, I'm sure it doesn't actually compile but it should get the idea across)

### Alternatives, if applicable

- Manually parse `std::env::args` and look for `-f` or `--file`. This requires you to manually skip over the first few items to get to where the arguments start, which is fragile
- Use my app's `Command` struct to parse `std::env::args`, but this requires everything already present to constitute a valid command, i.e. it can't be used to complete a required argument (unless at least one character has already been typed)

### Additional Context

- Related to #3166 
- My completion functions are [here](https://github.com/LucasPickering/slumber/blob/adcca444904a64ffd097ff38b5d1d0883314f1bc/crates/cli/src/completions.rs)

---

_Label `C-enhancement` added by @LucasPickering on 2024-10-21 12:10_

---

_Label `A-completion` added by @epage on 2024-10-21 23:50_

---

_Comment by @epage on 2024-10-21 23:57_

Huh, I thought we had an issue for this but can't find one. Passing in an `OsStr` slice wouldn't be ideal as people would have to hack-in their own parsing. We'd likely want ‘ArgMatches` or something like it. The challenge with `ArgMatches` is we've not exposed the API for making one. More generally, #5515 is a good intermediate step.

---

_Label `S-waiting-on-design` added by @epage on 2024-10-21 23:58_

---

_Comment by @LucasPickering on 2024-10-22 00:46_

Yes `ArgMatches` seems perfect. Is there a need to expose the construction of it? I would think the completion engine could construct the `ArgMatches` then pass it into the user's completer functions.

---

_Comment by @epage on 2024-10-22 15:46_

`ArgMatches` lives in `clap` and so the constructor would need to be exposed so `clap_complete` could make one and pass it to the users completer.

---

_Comment by @LucasPickering on 2024-10-23 01:48_

Ah of course. What can I do to help with this? I don't have a ton of time to dedicate to it but I'd like to help if possible.

---

_Comment by @epage on 2024-10-23 01:57_

This is one of the lower priority tasks as we are focusing on
- feature parity with existing completion generator
- feature parity with cargo's hand written completions as that is our primary test case for the design

As such, I'm not going to be setting aside the time to help drive the design of this forward for this to be resolved atm.

Previously, I said that #5784 might be initial step but I just realized that there is some extra complexity to that that I had overlooked.

---

_Referenced in [jj-vcs/jj#4823](../../jj-vcs/jj/pulls/4823.md) on 2024-11-12 01:40_

---

_Referenced in [jj-vcs/jj#4887](../../jj-vcs/jj/pulls/4887.md) on 2024-11-21 08:52_

---

_Renamed from "clap_complete: Allow access to previous args in completion function" to "Allow access to previous args in completion function for context-sensitive completions" by @epage on 2025-01-20 15:28_

---

_Referenced in [clap-rs/clap#5884](../../clap-rs/clap/pulls/5884.md) on 2025-01-20 15:31_

---
