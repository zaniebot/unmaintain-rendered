```yaml
number: 15603
title: "Implement tab autocomplete for `ruff config`"
type: pull_request
state: merged
author: mishamsk
labels:
  - cli
assignees: []
merged: true
base: main
head: 4551-add-shell-completions-for-config
created_at: 2025-01-20T03:46:47Z
updated_at: 2025-01-27T15:33:08Z
url: https://github.com/astral-sh/ruff/pull/15603
synced_at: 2026-01-12T15:55:51Z
```

# Implement tab autocomplete for `ruff config`

---

_@mishamsk_

## Summary

Not the most important feature, but hey... was marked as the good first issue ;-) fixes #4551

Unfortunately, looks like clap only generates proper completions for zsh, so this would not make any difference for bash/fish.

## Test Plan

- cargo nextest run
- manual test by sourcing completions and then triggering autocomplete:
 
```shell
misha@PandaBook ruff % source <(target/debug/ruff generate-shell-completion zsh)
misha@PandaBook ruff % target/debug/ruff config lin
line-length                                                         -- The line length to use when enforcing long-lines violations
lint                                                                -- Configures how Ruff checks your code.
lint.allowed-confusables                                            -- A list of allowed 'confusable' Unicode characters to ignore
lint.dummy-variable-rgx                                             -- A regular expression used to identify 'dummy' variables, or
lint.exclude                                                        -- A list of file patterns to exclude from linting in addition
lint.explicit-preview-rules                                         -- Whether to require exact codes to select preview rules. Whe
lint.extend-fixable                                                 -- A list of rule codes or prefixes to consider fixable, in ad
lint.extend-ignore                                                  -- A list of rule codes or prefixes to ignore, in addition to
lint.extend-per-file-ignores                                        -- A list of mappings from file pattern to rule codes or prefi
lint.extend-safe-fixes                                              -- A list of rule codes or prefixes for which unsafe fixes sho
lint.extend-select                                                  -- A list of rule codes or prefixes to enable, in addition to
lint.extend-unsafe-fixes                                            -- A list of rule codes or prefixes for which safe fixes shoul
lint.external                                                       -- A list of rule codes or prefixes that are unsupported by Ru
lint.fixable                                                        -- A list of rule codes or prefixes to consider fixable. By de
lint.flake8-annotations                                             -- Print a list of available options
lint.flake8-annotations.allow-star-arg-any                          -- Whether to suppress `ANN401` for dynamically typed `*args`

...
```

- check command help
```shell
❯ target/debug/ruff config -h
List or describe the available configuration options

Usage: ruff config [OPTIONS] [OPTION]

Arguments:
  [OPTION]  Config key to show

Options:
      --output-format <OUTPUT_FORMAT>  Output format [default: text] [possible values: text, json]
  -h, --help                           Print help

Log levels:
  -v, --verbose  Enable verbose logging
  -q, --quiet    Print diagnostics, but nothing else
  -s, --silent   Disable all logging (but still exit with status code "1" upon detecting diagnostics)

Global options:
      --config <CONFIG_OPTION>  Either a path to a TOML configuration file (`pyproject.toml` or `ruff.toml`), or a TOML `<KEY> =
                                <VALUE>` pair (such as you might find in a `ruff.toml` configuration file) overriding a specific
                                configuration option. Overrides of individual settings using this option always take precedence over
                                all configuration files, including configuration files that were also specified using `--config`
      --isolated                Ignore all configuration files
```

- running original command
```shell
❯ target/debug/ruff config
cache-dir
extend
output-format
fix
unsafe-fixes
fix-only
show-fixes
required-version
preview
exclude
extend-exclude
extend-include
force-exclude
include
respect-gitignore
builtins
namespace-packages
target-version
src
line-length
indent-width
lint
format
analyze
```


---

_Comment by @github-actions[bot] on 2025-01-20 03:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @dhruvmanila on `crates/ruff_workspace/src/cli.rs`:150 on 2025-01-20 04:36_

What's the reason for limiting this? This makes the short description a bit awkward to read because it ends in the middle of sentences.

<img width="1284" alt="Screenshot 2025-01-20 at 10 02 14 AM" src="https://github.com/user-attachments/assets/75f5b8a4-344c-4185-98e3-f1ea3ed24d61" />

Should we instead take the first paragraph from the documentation? For example, for the [`lint.task-tags`](https://docs.astral.sh/ruff/settings/#lint_task-tags) setting, we would consider "A list of task tags to recognize (e.g., "TODO", "FIXME", "XXX")." as the description. If so, you'll need to split the documentation at the first double newline occurence (`\n\n`) and take the first part of it.

---

_@dhruvmanila reviewed on 2025-01-20 04:37_

Thank you for working on this!

I'm seeing this entry at the end of the completion list after I press <kbd>Tab</kbd> at the end of `ruff config `:

<img width="1262" alt="Screenshot 2025-01-20 at 9 57 00 AM" src="https://github.com/user-attachments/assets/ad86a968-ebed-4e55-aad5-116a445dd5d8" />



---

_Label `cli` added by @dhruvmanila on 2025-01-20 04:37_

---

_@mishamsk reviewed on 2025-01-20 14:39_

---

_Review comment by @mishamsk on `crates/ruff_workspace/src/cli.rs`:150 on 2025-01-20 14:39_

@dhruvmanila thanks for looking into this. I decided to truncate based on clap's doc suggestion:
![CleanShot 2025-01-20 at 09 36 04@2x](https://github.com/user-attachments/assets/6516d352-fd90-426c-b33c-92f6866bc907)

I've just made a local change to expand to the entire paragraph (also replacing any single newlines with a space). See below:
![CleanShot 2025-01-20 at 09 34 54@2x](https://github.com/user-attachments/assets/6d2e927f-f7f0-4b18-a05b-6f41de8e025b)

You can see that completions now include the full first paragraph, but at least in my terminal (ghostty, "stock" zsh 5.9.0) on MacOS - they are still truncated...

I'll probably push the change regardless, since the truncated string will then depend on the terminal width, but overall I am not sure if there is a way to "solve" this and show entire paragraphs.

---

_@dhruvmanila reviewed on 2025-01-20 14:43_

---

_Review comment by @dhruvmanila on `crates/ruff_workspace/src/cli.rs`:150 on 2025-01-20 14:43_

> I'll probably push the change regardless, since the truncated string will then depend on the terminal width, but overall I am not sure if there is a way to "solve" this and show entire paragraphs.

Yeah, this is fine, thanks you. I only meant to say that Ruff shouldn't handle this as terminals are probably going to truncate them as per screen width. This is useful because users with more screen width should be able to see more content regarding the option but at the same time someone on a laptop shouldn't be affected either.

---

_Comment by @mishamsk on 2025-01-20 14:59_

@dhruvmanila 

updates:

- fixes the erroneous strings at the end of completions (apparently clap has issues with double quotes in help strings, fails to properly escape them => I replaced with single quotes)
- removed sets from completions that do not have doc. Even though it would be valid to call config with them, I do not see additional value in polluting the completion. In real-life zsh would "stop" at them anyway, albeit with a trailing dot
- as described above, I've switched to passing the entire first paragraph as help string. Doesn't fix the fact that zsh truncates the help to screen width, but probably better than hard-truncation I had before

---

_Comment by @dhruvmanila on 2025-01-20 15:06_

> * removed sets from completions that do not have doc. Even though it would be valid to call config with them, I do not see additional value in polluting the completion. In real-life zsh would "stop" at them anyway, albeit with a trailing dot

Are you referring to the header like `lint.pyupgrade`, `lint.ruff`, etc. as is in my screenshot above? If so, we should include them even though they don't have documentation because `ruff config lint.ruff` will list out all the config options:

```console
$ ruff config lint.ruff
parenthesize-tuple-in-subscript
extend-markup-names
allowed-markup-calls
```

---

_Comment by @mishamsk on 2025-01-20 18:03_

> > * removed sets from completions that do not have doc. Even though it would be valid to call config with them, I do not see additional value in polluting the completion. In real-life zsh would "stop" at them anyway, albeit with a trailing dot
> 
> Are you referring to the header like `lint.pyupgrade`, `lint.ruff`, etc. as is in my screenshot above? If so, we should include them even though they don't have documentation because `ruff config lint.ruff` will list out all the config options:
> 
> ```
> $ ruff config lint.ruff
> parenthesize-tuple-in-subscript
> extend-markup-names
> allowed-markup-calls
> ```

Yes. Here is why I think there is no point in including them. The whole purpose of completion is to allow a user to get to detailed configuration field description without guessing its name.

Configuration field groups, like `lint.isort` do not have any associated documentation, and `ruff config` just falls back to showing list of possible subkeys/fields. But with auto-completion, there is now no need to run `ruff config lint.isort`, pressing the tab gives even better output than running the command.

See, how the flow looks at the current commit. I first run `ruff config lint.isort`, and then do double-tab instead:
![CleanShot 2025-01-20 at 12 51 18](https://github.com/user-attachments/assets/0b47a752-f757-4e38-b5c5-f5aa278ea758)

Now, here is how tab completion will look if I remove the filter:
![CleanShot 2025-01-20 at 12 55 24@2x](https://github.com/user-attachments/assets/5505b791-288a-49a2-bb41-ab21496ede2d)

I am not sure if the first line is helpful. Even if I add something like `Lists all available config fields or subconfig keys` - why would anyone care auto-completing to that name if they already have the full list with available on screen?

An important note - if `lint.isort` would get a doc of its own, it will appear in the list. So not all of the config groups are skipped.

tl;dr; I am happy to remove the filter if you think this is how it is supposed to be, but imho the current behavior gets users to where they actually want with slightly less confusion.


---

_Comment by @dhruvmanila on 2025-01-21 03:37_

Thanks for providing your thoughts on this. I agree that it might be quicker for a user to get to a specific config key if we skip the plugin header but those are the possible values that can be passed to the `ruff config` command and I don't think completion list should filter out certain values based on usages. This logic also seems to rely on the fact that the plugin headers doesn't have documentation but it might in the future which means that the suggestions would start including them. It might be that you and I don't use or find it useful but there might be valid use cases that we aren't seeing and users might find it confusing that a possible value is not being suggested. Does that make sense?

---

_Comment by @MichaReiser on 2025-01-21 07:55_

I think it's fine to ignore tables as long as we ignore all table entries because you can't set the table itself (I guess you can, but it's somewhat useless). 

---

_Comment by @dhruvmanila on 2025-01-21 12:05_

> I think it's fine to ignore tables as long as we ignore all table entries because you can't set the table itself (I guess you can, but it's somewhat useless).

I see, I'm not sure if that kind of heuristics exists, maybe it does within the OptionsMetadata? Regardless, I'm not sure I follow what you mean by "you can't set the table itself" as, from what I understand, `ruff config` is mainly to display the documentation of the option or list down all available options for a plugin. Are you referring to a possible future extension of `ruff config set`?

---

_@dhruvmanila reviewed on 2025-01-21 12:07_

---

_Review comment by @dhruvmanila on `crates/ruff_workspace/src/cli.rs`:2 on 2025-01-21 12:07_

What's the reason for having a nested module within the module itself? Should we instead rename this file to `completion.rs` instead? This will involve updating the `lib.rs` and `args.rs` to use the correct name as well.

---

_@mishamsk reviewed on 2025-01-21 13:22_

---

_Review comment by @mishamsk on `crates/ruff_workspace/src/cli.rs`:2 on 2025-01-21 13:22_

oh, yes, my bad. I originally copied the inner module from RuleSelector, but you are totally right this doesn't make sense here as there is nothing else in the module. I'll fix that alongside the OptionsSet completions as soon as we'll align on what we are doing

---

_Comment by @mishamsk on 2025-01-21 13:25_

@MichaReiser as per my logic above - when an OptionSet has some documentation, it is kinda helpful to see it among completions...but tbh only when the set is small and this option even fits on the screen.

So there are two viable alternatives to resolve this:

1. As @dhruvmanila suggested, show all valid values for the command. In that case, I suggest keeping doc for OptionSet's that have them AND adding "Show all available subconfig keys" for those that do not

2. As @MichaReiser suggest filter all OptionSet's from the result and keep only the fields

I'll follow your lead folks, let me know.

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/cli.rs`:13 on 2025-01-23 07:24_

Could we move this entire file to the `ruff` crate or what's the reason that it has to be in the `ruff_workspace` crate?

The `ruff` crate is ruff's CLI interface whereas `ruff_workspace` is (mostly) interface independent (can be WASM, CLI, LSP, ...)

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/cli.rs`:29 on 2025-01-23 07:26_

Can we use a more self explanatory name than `fqn`. I've no idea what it stands for (maybe full qualified name)? How about just `full_option_name`?

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/cli.rs`:15 on 2025-01-23 07:27_

Not too important but we can avoid some allocations by using `&'static str`
```suggestion
        values: Vec<(String, &'static str)>,
        parents: Vec<&'static str>,
```

This does require changing the signature of the `Visit` methods to `&'static str` but I think that should be easily possible.

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/cli.rs`:19 on 2025-01-23 07:28_

Nit
```suggestion
        type IntoIter = std::vec::IntoIter<Self::Item>;
```

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/cli.rs`:150 on 2025-01-23 07:32_

Can you explain why replacing all `"` with `'` is necessary? It does seem dangerous to me in the case the doc contains any code examples

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/cli.rs`:133 on 2025-01-23 07:34_

Doesn't this result in two errors with we have an argument? Should this be a `match`? Can you extend your test plan with an example that exercises this code path

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/cli.rs`:60 on 2025-01-23 07:35_

I think it would be helpful to mention that this type mainly exists to provide autocompletion by implementing `PossibleValues`. 

Can you add the help output for the `config` command to your test plan. I want to make sure  that clap doesn't render all possible values

---

_@MichaReiser requested changes on 2025-01-23 07:37_

@dhruvmanila convinced me. I think showing all values is a great starting point. 

Can you add an integration test similar to the tests we have in https://github.com/astral-sh/ruff/blob/c847cad389f202edae868765e690cb7042e88264/crates/ruff/tests/config.rs#L5-L4

---

_Review comment by @mishamsk on `crates/ruff_workspace/src/cli.rs`:150 on 2025-01-24 01:43_

@MichaReiser first of all thanks a LOT for such a thorough review. I almost feel sorry that such a minor change caused so much trouble.

Back to your question: I added it because apparently clap doesn't escape double quotes when generating zsh completions, which breaks them. @dhruvmanila commented about it: https://github.com/astral-sh/ruff/pull/15603#pullrequestreview-2561307123 

Also, found clap PR that fixed other escapes: https://github.com/clap-rs/clap/pull/4848 but if you look into the change here: https://github.com/clap-rs/clap/pull/4848/files - you'll see that (a) they effectively do the same, just replace characters (b) PR author forgot double quotes

I do not see any risks, neither security no user friendliness. This would only affect completion help, which most likely be truncated anyway + it is unlikely that the first paragraph of the doc contains code snippets.

---

_@mishamsk reviewed on 2025-01-24 01:43_

---

_@mishamsk reviewed on 2025-01-24 01:48_

---

_Review comment by @mishamsk on `crates/ruff_workspace/src/cli.rs`:133 on 2025-01-24 01:48_

this code was copy-pasted verbatim from the existing completion for rules... it gives this:
![CleanShot 2025-01-23 at 20 46 55@2x](https://github.com/user-attachments/assets/14f94870-7f0e-4ac3-b027-af5b91c8aebe)

I believe without the arg, the `for '[OPTION]'` will be missing.

I am not a fan of this specific output, but I was trying to stick to whatever was in the code base already.

---

_@mishamsk reviewed on 2025-01-24 02:36_

---

_Review comment by @mishamsk on `crates/ruff_workspace/src/cli.rs`:2 on 2025-01-24 02:36_

this is fixed, I'll close this one as there is a more recent comment below

---

_@mishamsk reviewed on 2025-01-24 02:37_

---

_Review comment by @mishamsk on `crates/ruff_workspace/src/cli.rs`:13 on 2025-01-24 02:37_

I followed the way it was done for `RuleSelector`. I moved it into a new module inside ruff crate. I'd argue, that RuleSelector should move there too, as the same argument is applicable to it.

---

_@mishamsk reviewed on 2025-01-24 02:37_

---

_Review comment by @mishamsk on `crates/ruff_workspace/src/cli.rs`:29 on 2025-01-24 02:37_

yes, that's `fully_qualified_name`. I expanded the acronym, but kept "qualified", I believe that's the appropriate term here.

---

_Review comment by @mishamsk on `crates/ruff_workspace/src/cli.rs`:15 on 2025-01-24 02:39_

There are 4 structs implementing this Trait, so I didn't make the change to limit the scope of this PR. If you'd like me to go ahead and change all of them I can. But, tbh, this code is ran once per ruff install/update, I am pretty sure the difference is negligible

---

_@mishamsk reviewed on 2025-01-24 02:39_

---

_@mishamsk reviewed on 2025-01-24 02:43_

---

_Review comment by @mishamsk on `crates/ruff_workspace/src/cli.rs`:60 on 2025-01-24 02:43_

added!

---

_Comment by @mishamsk on 2025-01-24 02:44_

> @dhruvmanila convinced me. I think showing all values is a great starting point.

Made the change

> Can you add an integration test similar to the tests we have

doing a true completion integration test seems cumbersome, it would involve generating the completion script, sourcing it, and then executing with some magic env vars inside zsh. I can add such a test, but sounds overcomplicated for the task.

A somewhat easier way is to commit the completion script itself as a snapshot. That's easy.

But whatever path we choose will cause snapshot changes every time anyone changes any option... As a contributor I would have been surprised by this ;-)

Fwiw the other args with autocompletion do not have snapshot tests if I am not mistaken

---

_Review requested from @MichaReiser by @mishamsk on 2025-01-24 03:20_

---

_Comment by @dhruvmanila on 2025-01-24 04:58_

> Unfortunately, looks like clap only generates proper completions for zsh, so this would not make any difference for bash/fish.

Sorry I missed this. Can you tell me why is that so?

In my testing, I can at least see that the Bash completions are being produced correctly.

---

_@dhruvmanila reviewed on 2025-01-24 05:07_

---

_Review comment by @dhruvmanila on `crates/ruff/src/commands/completions/config.rs`:41 on 2025-01-24 05:07_

I don't think this is the correct fix. The fix would be to update the actual documentation in the codebase. For example, the `ruff config lint` option has the following as documentation:

https://github.com/astral-sh/ruff/blob/98fccec2e7c20be9a5b99907e912a678a8f29900/crates/ruff_workspace/src/options.rs#L436-L446

The first line will be used as the description for the completion. This is what should be done as well for other options and I'd recommend that this be done in a follow-up PR and not in this PR.

So, for example, considering `lint.pydoclint`, the following comment:

https://github.com/astral-sh/ruff/blob/98fccec2e7c20be9a5b99907e912a678a8f29900/crates/ruff_workspace/src/options.rs#L472-L474

should be copied on top of `PydoclintOptions` which will then be used as the completion description.

---

_@dhruvmanila reviewed on 2025-01-24 05:08_

---

_Review comment by @dhruvmanila on `crates/ruff/src/commands/completions/config.rs`:76 on 2025-01-24 05:08_

nit: `o` as a single letter is confusing
```suggestion
    fn from(value: OptionString) -> Self {
        value.0
    }
```

---

_@dhruvmanila reviewed on 2025-01-24 05:08_

---

_Review comment by @dhruvmanila on `crates/ruff/src/commands/completions/config.rs`:151 on 2025-01-24 05:08_

```suggestion
                .take_while(|line| !line.trim_end().is_empty())
```

---

_@dhruvmanila approved on 2025-01-24 05:09_

Looks good, thanks for working on this. I'll let @MichaReiser do the final review.

We should revert the default documentation value and use empty string for now. A follow-up work would be to copy the documentation to the relevant struct.

---

_@MichaReiser reviewed on 2025-01-24 08:23_

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/cli.rs`:150 on 2025-01-24 08:23_

It would be great if you could add an inline comment that summarises what you wrote in this comment. It will help future readers to understand (without guessing) of what's happening here. 

---

_@MichaReiser reviewed on 2025-01-24 08:24_

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/cli.rs`:133 on 2025-01-24 08:24_

I see. I'm still not sure if it is correct because we'll end up inserting two errors in case `arg` is some. So maybe this should just be an `if...else`?

---

_@MichaReiser approved on 2025-01-24 08:25_

This is great, thank you

---

_Review comment by @mishamsk on `crates/ruff_workspace/src/cli.rs`:133 on 2025-01-25 15:03_

No, I believe this code (which btw Charlie wrote originally) is correct. Clap uses each piece to create one single error. Notice that one part pushes `clap::error::ContextKind::InvalidArg` and another `clap::error::ContextKind::InvalidValue`, hence we get one error:
```
error: invalid value 'blabla' for '[OPTION]'
                      ^^^^^ <--- clap::error::ContextKind::InvalidValue
                                   ^^^^^^^^ <--- clap::error::ContextKind::InvalidArg
```

---

_@mishamsk reviewed on 2025-01-25 15:03_

---

_Comment by @mishamsk on 2025-01-25 20:45_

@MichaReiser 

* removed default doc text per @dhruvmanila ask, I will open a follow-up and fill in the missing docs as soon as this one is merged
* answered your question rgd error message
* applied two nit's from @dhruvmanila 

should be ok to merge

---

_@dhruvmanila approved on 2025-01-27 15:08_

Thank you! 

---

_Merged by @dhruvmanila on 2025-01-27 15:09_

---

_Closed by @dhruvmanila on 2025-01-27 15:09_

---

_Branch deleted on 2025-01-27 15:33_

---
