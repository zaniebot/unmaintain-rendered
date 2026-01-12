```yaml
number: 5119
title: "Add the `--unsafe` option for `--fix`, `--fix-only`, and `--diff`"
type: pull_request
state: closed
author: evanrittenhouse
labels:
  - breaking
assignees: []
draft: true
base: main
head: 4185_cli_respect_applicability
created_at: 2023-06-15T13:01:52Z
updated_at: 2023-10-19T13:59:07Z
url: https://github.com/astral-sh/ruff/pull/5119
synced_at: 2026-01-12T15:55:17Z
```

# Add the `--unsafe` option for `--fix`, `--fix-only`, and `--diff`

---

_@evanrittenhouse_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
Fixes #4185. 

This is a _breaking change_ which will alter the rulesets fixed by `--fix`, among others. I'll update the PR description with new changes as I make them - see the issue link for the entire list.

This PR depends on all `Fix::unspecified` instances being refactored to an applicable (pun intended :P ) `Applicability`. Currently, `ripgrep` says there are occurrences in:
1. `flake8_commas`: fixed in #5127 
2. `flake8_logging_format`: fixed in #5129 
3. `flake8_pytest_style`: fixed in #5389 
4. `flake8_quotes`: fixed in #5130 
5. `flake8_simpilfy`: fixed in #5348 
6. `flake8_tidy_imports`: fixed in #5131
7. `flynt`: fixed in #5160
8. `isort`: fixed in #5161 
9. `pandas_vet`: fixed in #5252 
10. `pycodestyle`: fixed in #5282 
11. `pydocstyle`: fixed in #5390 
12. `pyflakes`: fixed in #5253 
13. `pylint`: fixed in #5251 
14. `pyupgrade`: fixed in #5162
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
`C410` is `Applicability::unspecified`. I made a new file:
```python
list([1, 2, 3])
```
and ran Ruff against it. The file was not fixed. I then changed the rule to have `Applicability::automatic` and ran it again - this time, it was fixed.


---

_Comment by @github-actions[bot] on 2023-06-15 13:33_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.4±0.07ms     4.8 MB/sec    1.06      8.9±0.06ms     4.6 MB/sec
formatter/numpy/ctypeslib.py               1.00  1649.2±25.56µs    10.1 MB/sec    1.05  1728.1±41.31µs     9.6 MB/sec
formatter/numpy/globals.py                 1.00    185.3±2.42µs    15.9 MB/sec    1.02    188.3±1.62µs    15.7 MB/sec
formatter/pydantic/types.py                1.00      3.5±0.11ms     7.3 MB/sec    1.05      3.7±0.07ms     6.9 MB/sec
linter/all-rules/large/dataset.py          1.00     11.0±0.12ms     3.7 MB/sec    1.00     11.0±0.06ms     3.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.9±0.03ms     5.8 MB/sec    1.00      2.9±0.04ms     5.8 MB/sec
linter/all-rules/numpy/globals.py          1.01    402.7±6.75µs     7.3 MB/sec    1.00    399.1±1.13µs     7.4 MB/sec
linter/all-rules/pydantic/types.py         1.02      5.7±0.08ms     4.5 MB/sec    1.00      5.6±0.05ms     4.5 MB/sec
linter/default-rules/large/dataset.py      1.00      5.6±0.03ms     7.3 MB/sec    1.02      5.7±0.03ms     7.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1231.6±19.27µs    13.5 MB/sec    1.01  1242.6±10.99µs    13.4 MB/sec
linter/default-rules/numpy/globals.py      1.01    146.5±4.14µs    20.1 MB/sec    1.00    144.5±2.54µs    20.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.5±0.03ms    10.2 MB/sec    1.02      2.6±0.02ms    10.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.1±0.11ms     4.0 MB/sec    1.00     10.1±0.12ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.00  1937.5±18.08µs     8.6 MB/sec    1.01  1960.5±43.33µs     8.5 MB/sec
formatter/numpy/globals.py                 1.00    218.5±5.41µs    13.5 MB/sec    1.00    218.0±6.51µs    13.5 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.06ms     6.0 MB/sec    1.01      4.2±0.06ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.00     12.8±0.16ms     3.2 MB/sec    1.01     12.9±0.15ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.04ms     4.7 MB/sec    1.01      3.5±0.03ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   443.1±14.05µs     6.7 MB/sec    1.01    447.6±9.91µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.6±0.07ms     3.8 MB/sec    1.01      6.7±0.11ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      6.9±0.06ms     5.9 MB/sec    1.01      7.0±0.08ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1480.3±23.45µs    11.2 MB/sec    1.01  1501.1±23.79µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    176.3±4.32µs    16.7 MB/sec    1.01    178.4±3.74µs    16.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.05ms     8.1 MB/sec    1.00      3.1±0.04ms     8.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @zanieb on 2023-06-15 14:42_

@evanrittenhouse I suspect there are probably too many outstanding `Applicability::unspecified` fixes for us to stop fixing those automatically yet — although I have not checked in a bit.

I think we may want `Applicability::suggested` fixes to require a separate opt-in?

---

_Comment by @evanrittenhouse on 2023-06-15 14:56_

Yeah, so by default we'll apply `Applicability::Automatic` (`suggested` will in fact require a separate opt-in).. After getting this done (the code part), I'm going to go through and categorize the rest of the fixes. Once the fixes are done, I'll rebase this and then mark it as complete. 

---

_Comment by @evanrittenhouse on 2023-06-15 19:51_

I've begun categorizing some of the rules (#5129, #5130, #5127). I plan on doing some more later tonight or tomorrow. This PR depends on all `Fix::unspecified` instances being refactored

---

_Comment by @evanrittenhouse on 2023-06-21 13:18_

What's the consensus on opting into suggested fixes? I was thinking we could either:
1. Have it be an option in the relevant `.toml` file
2. Have it be a CLI option (e.g. `--fix` would only implement `Applicability::Automatic` fixes, while `--fix --include-suggested` would implement `Applicability::Automatic|Suggested`.

Or both!

cc @zanieb @charliermarsh 

---

_Comment by @MichaReiser on 2023-06-22 06:03_

I don't think we have a consensus yet and is something we should discuss:

Adding a new CLI option seems reasonable to me. I would suggest to prefix the option with `--fix`, e.g. `--fix-suggested`, to make its relation clear. We may want to be more explicit about their potential *unsafe* nature by naming the option `--fix-unsafe`. However, this has the downside that the wording in the diagnostic and the CLI option do not align. Clippy uses a `YOLO` environment (not the exact name) to enable all fixes. An alternative is that `--fix` optionally accepts `automatic|yolo` (no idea what to call the value xD)

I'm unsure how to support this in the configuration best. I would expect that adding a rule to *fixable* promotes it to  *automatic* (only when applying, not for rendering diagnostics). Adding a rule to *unfixable* demotes it to manual. The challenge with this approach is that promoting a single *suggested* fix requires adding *all* automatic fixes to *fixable*, which is rather annoying. We could deprecate the existing *fixable* and *unfixable* in favor of adding new options `fix.automatic` (fixable), `fix.suggested`, and `fix.manual` (unfixable). 

We should also think about what changes are necessary in the VS code extension and I'm probably missing a few details here. 

---

_Comment by @evanrittenhouse on 2023-06-22 14:55_

>However, this has the downside that the wording in the diagnostic and the CLI option do not align

Why did we move away from `safe/unsafe`, etc. to the current model? Could/should we switch back?

> An alternative is that --fix optionally accepts automatic|yolo (no idea what to call the value xD)

I like this! We could default to `automatic`, then allow for `suggested` and `manual` (or `yolo` :P) options to be passed which would adjust the "base" applicability that gets fixed.

> would expect that adding a rule to fixable promotes it to automatic (only when applying, not for rendering diagnostics). Adding a rule to unfixable demotes it to manual. The challenge with this approach is that promoting a single suggested fix requires adding all automatic fixes to fixable, which is rather annoying. 

I like this approach, though I think we could smooth the implementation by providing an `applicability_override` field on `Fix` that gets populated **only if** the fix is in `fixable` or `unfixable`. Then, regardless of applicability level, we could check if the fix's safety level should be overridden. That would allow us to pass a "`suggested`" diagnostic to `fixable` and, when we generate a fix for it, check the override and include it in the default fixable set regardless. Does that sound fair?

---

_Comment by @evanrittenhouse on 2023-06-26 21:11_

cc @MichaReiser wondering if the above design makes sense - I plan on wrapping up the remaining linters tonight, then I'll be able to come back to this and implement the rest of the selection logic.

---

_Comment by @charliermarsh on 2023-06-26 21:21_

@evanrittenhouse - Micha's gonna be out tomorrow so feel free to ping again on Wednesday if he doesn't get back to you.

---

_Comment by @zanieb on 2023-06-26 23:04_

@evanrittenhouse you can see the discussion about the names in https://github.com/astral-sh/ruff/issues/4183

I think since the display is "Fix" and "Suggested fix" (as implemented in #4303 but we could change this) users would understand `--fix-suggested`. `--fix-unsafe` would not make it clear to me that the "Manual fixes" would not be applied. I also don't mind the idea of `--fix [applicability]` but I'm not sure how discoverable it is — seems like the way to go if we want to add more applicability levels but that doesn't seem particularly likely.

---

_Comment by @evanrittenhouse on 2023-06-27 12:22_

@zanieb Interesting. So if we were to go with `--fix` only doing automatic and `--fix-suggested` doing suggested + automatic, how would the user see manual fixes? Just via `--diff`?

Also, I'm curious how we want to handle the configuration file for this. In my opinion, adding a rule (regardless of applicability level) to `fixable` should make it get fixed. Adding a rule (again, regardless of applicability) to `unfixable` should leave it alone. To avoid changing the `Applicability` enum, I think that it makes sense to add an `applicability_ovverride` field to `Fix` which: 
1. Gets parsed at runtime from the appropriate .TOML file
2. Supersedes the `Applicability` level assigned to the fix
3. Joins/excludes it from the fixable set

What do you think?

---

_Comment by @MichaReiser on 2023-06-28 12:14_

> I like this approach, though I think we could smooth the implementation by providing an applicability_override field on Fix that gets populated only if the fix is in fixable or unfixable. Then, regardless of applicability level, we could check if the fix's safety level should be overridden. That would allow us to pass a "suggested" diagnostic to fixable and, when we generate a fix for it, check the override and include it in the default fixable set regardless. Does that sound fair?

I'm a bit hesitant from adding new fields to `Fix` because:
* It increases the memory consumption of every fix. 
* Fixes are currently constructed manually by linters. Passing and initializing a field requires changing every single `Fix` call, increasing the complexity for authoring lint rules. 

I'm unsure if I understand your motivation for adding the override field. Is it to avoid generating unnecessary fixes? 

> how would the user see manual fixes? Just via --diff?

That's correct. Manual fixes should not be applied automatically. I wonder if it even makes sense allowing users to promote manual fixes because it is intentional that manual fixes can be incomplete, always or sometimes resulting in invalid syntax. 

>  To avoid changing the Applicability enum, I think that it makes sense to add an applicability_ovverride field to Fix which:

Would this be initialized depending on whether the rule is in the `fixable` or `unfixable`? What are your thoughts on renaming the configuration options to use a single terminology?

> Gets parsed at runtime from the appropriate .TOML file

What do you mean by appropriate toml file. Reading it from the closest pyproject toml or is this another configuration file?




---

_Comment by @evanrittenhouse on 2023-06-28 12:33_

> I'm unsure if I understand your motivation for adding the override field. Is it to avoid generating unnecessary fixes?

Yes, an attempt to keep the current configuration option setup with `fixable`/`unfixable`. If the user passes `--fix` (e.g., selecting `Automatic` fixes), but a `Suggested` rule is in `fixable`, we need a way to show that the suggested rule should be fixed. As far as the creation goes, I don't actually think it's that bad. The `override` field would be private and defaulted to whatever `Applicability` the Fix's construtor specifies. My proposal about the `override` field was really only in case we wanted to keep the existing setup. For example, how do we handle the case where the user selects `fix.automatic` but additionally wants to fix only one `Applicability::Suggested` rule? Do we even want to?

I believe above you also mentioned: 
> We could deprecate the existing fixable and unfixable in favor of adding new options fix.automatic (fixable), fix.suggested, and fix.manual (unfixable).

Which would also definitely work (and I think is better), but I'm not sure how we'd handle the above case.

> That's correct. Manual fixes should not be applied automatically. I wonder if it even makes sense allowing users to promote manual fixes because it is intentional that manual fixes can be incomplete, always or sometimes resulting in invalid syntax.

That's fine with me. I think the important thing is that we make it clear that `Manual` isn't a typical Fix level, and that we can only show them, never implement them. That's easily done though. :)

> What do you mean by appropriate toml file. Reading it from the closest pyproject toml or is this another configuration file?

Yeah, closest `pyproject.toml` (or `ruff.toml`, if applicable I guess).

@MichaReiser let me know what you think!

---

_Comment by @MichaReiser on 2023-06-30 08:33_

> Yes, an attempt to keep the current configuration option setup with fixable/unfixable. If the user passes --fix (e.g., selecting Automatic fixes), but a Suggested rule is in fixable, we need a way to show that the suggested rule should be fixed. As far as the creation goes, I don't actually think it's that bad. The override field would be private and defaulted to whatever Applicability the Fix's construtor specifies. 

Which code would be responsible for changing/setting the `override` field? 

> My proposal about the override field was really only in case we wanted to keep the existing setup. For example, how do we handle the case where the user selects fix.automatic but additionally wants to fix only one Applicability::Suggested rule? Do we even want to?

Can you elaborate how this would work with the `override` field?


What I had in mind is that ruff generates all fixes and then filter out the fixes where the Applicability doesn't match. This has the downside that we'll generate more fixes than what's strictly necessary but I don't think I'm too concerned about that. An alternative would be to change `Checker.patch` to accept an `Applicability` but we don't always know the applicability ahead of time and it would be easy to change the applicability when generating the fix but not when checking whether a patch needs to be created. 



---

_Comment by @evanrittenhouse on 2023-06-30 13:18_

@MichaReiser This is basically how I'm imagining the entire feature.

## CLI
1. `--fix` - changed to fix only `Automatic` fixes and rules in the `fixable` set
2. `--fix-unsafe` - new option, will fix `Automatic` and `Suggested` fixes. While it differs from the implementation's naming conventions, as a user "safe" and "unsafe" make more sense, IMO. From the *fixer's* perspective, an "automatic" or "suggested" fix makes sense. I think it's clearer for users to consider fixes as "this is safe to implement" or "this is unsafe". `Manual` changes will never be fixed
3. `--diff` - will output only `Automatic` fixes and rules in the `fixable` set
4. `--fix-only` - will only fix `Automatic` fixes and rules in the `fixable` set

Rules with a `Manual` applicability will never be fixed, nor will rules in the `unfixable` set.

## Configuration File
`fixable`/`unfixable` remain as normal, preserving backwards compatibility.

## Implementation
Consider the example where we're enforcing default rules (e.g., `E`, `F`) and F841 (unused variable).

The `Fix` gets generated [here](https://github.com/astral-sh/ruff/blob/main/crates/ruff/src/rules/pyflakes/rules/unused_variable.rs#L191) - assume that we're going with the `override` field as well. The `Fix::suggested()` constructor would assign an `override: None`. The user then runs `--fix`. Ruff goes out and fixes all `Automatic` fixes, leaving F841 untouched.

Now assume that the user wants to fix all Automatic fixes, with F841 added. They F841 into the `fixable` set in their `pyproject.toml` file. The `fixable`/`unfixable` tables would essentially operate as ways to say "I always want this fixed" or "I never want this fixed".

We need to somehow tell F841 that its `Applicability` has to be overridden. This can be done in a few ways (and thanks for pushing back, I think the override field is worse):
1. An `override` field. We could add a `set_override()` method to `Fix` which allows you to pass an `Applicability`, setting `override: Some(new_applicability)`. The downside here is going through and adding `set_override()` calls everywhere, and it adds memory, like you said. 
2. **preferred**: `Checker` handles all fixing logic itself, now based on the CLI command and the contents of `fixable`/`unfixable`. The upside here is no change is required on a per-rule basis - `checker.patch(Rule::UnusedVariable)` would remain true and generate the fix. 

If we go with the second option, `Checker` would see that the "base applicability" (determined by seeing that the user ran `--fix` and not `--fix-unsafe`) is `Automatic`, and that `fixable` contains F841. Regardless of applicability, `patch()` would therefore return true, and we'd generate the fix. All fix generation would be gated through `Checker.patch()`.

What do you think?

---

_Comment by @MichaReiser on 2023-07-03 09:14_

Thanks for the in-depth elaboration. This helped. 
I think it would be good to create a discussion for the feature. What we're discussing is unrelated to the code changes and 
the lack of topics makes the discussion less efficient. 

Inputs/Questions from my side:
* `--fix-unsafe`: I think its good to have the option. We can align on the final naming later.
* ` --fix-only`: Should `--fix-unsafe` enable fixing the *suggested* fixes too? 
*  `--diff`: Should `--fix-unsafe` enable the suggested fixes? I think it should be aligned with `--fix-only`, considering that `--diff` implies `--fix-only`.
* `--diff`: We could show diffs for manual fixes. I'm not recommending that we should do this. We may decide to only show the fixes as part of the `MessageEmitter`s in the future. 
* Configuration: How would I promote a single suggested rule? 




---

_Comment by @evanrittenhouse on 2023-07-03 13:21_

@MichaReiser [Done!](https://github.com/astral-sh/ruff/discussions/5476) I'll be out most of today but will be able to get to your feedback by tonight or tomorrow morning probably.

---

_Renamed from "Autofix Automatic/Suggested fixes with --fix" to "Add the `--unsafe` option for `--fix`, `--fix-only`, and `--diff`" by @evanrittenhouse on 2023-07-13 21:26_

---

_Comment by @evanrittenhouse on 2023-07-13 21:49_

@MichaReiser @charliermarsh Opening this up for review, though I expect we'll have to iterate a bit

---

_Marked ready for review by @evanrittenhouse on 2023-07-13 21:49_

---

_Comment by @MichaReiser on 2023-07-14 06:11_

Thx for working on this. I don't think I'll get to review this before the weekend, but hope to get to it on Monday.

---

_Label `breaking` added by @MichaReiser on 2023-07-17 11:33_

---

_Review comment by @MichaReiser on `crates/ruff/src/autofix/mod.rs`:47 on 2023-07-17 11:40_

Nit: I think it would be good if we add this as a method to `Fix`: `fix.applies(applicability)` (I'm not super happy with the name, maybe you can come up with something better). 



```suggestion
    let required_applicability = if fix_unsafe ? Applicability::Suggested : Applicability::Automatic;
    let mut with_fixes = diagnostics
        .iter()
        .filter(|diagnostic| {
            diagnostic.fix.map_or(false, |fix| fix.applies(required_applicability))
        })
```


---

_Review comment by @MichaReiser on `crates/ruff/src/settings/configuration.rs`:57 on 2023-07-17 11:41_

Nit: Whether we want to add `unsafe` is still open for debate. An alternatie would be to have a `suggested` and `automatic` settings (see discussion)

---

_Review comment by @MichaReiser on `crates/ruff/src/settings/flags.rs`:5 on 2023-07-17 11:45_

The `bool` field is difficult to understand. I think it would be good to add a meaningful enum instead (and pass to e.g. `fix`)

```rust
enum SuggestedFixes {
    Apply,
    Disable,
}

impl SuggestedFixes {
    fn required_applicability(self) -> Applicability {
        match self {
            ...
        }
    }
}
```

---

_Review comment by @MichaReiser on `crates/ruff/src/settings/flags.rs`:3 on 2023-07-17 11:46_

Can you explain why the `suggested` is required for `Generate`? Aren't we always generating all diagnostics?

---

_Review comment by @MichaReiser on `crates/ruff/src/settings/options.rs`:214 on 2023-07-17 11:47_


```suggestion
    /// by the `--fix` and `--no-fix` command-line flags). This will only apply "safe" fixes
```

---

_Review comment by @MichaReiser on `crates/ruff/src/settings/options.rs`:216 on 2023-07-17 11:47_


```suggestion
    /// existing code.
```

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/printer.rs`:192 on 2023-07-17 11:52_

My expectations would be that the emitters always show all diagnostics, regardless of the fix level specified. Mainly because they are unrelated from applying fixes. Diff shows only the fixes with the specified applicability because it is used as a preview mode for `--fix`

---

_Review comment by @MichaReiser on `crates/ruff_diagnostics/src/fix.rs`:28 on 2023-07-17 11:56_

Is it now possible to remove `Unspecified` (and all other related code) in a separate PR?

If not, make sure you test what happens with unspecfieid fixes.

---

_Review comment by @MichaReiser on `crates/ruff_diagnostics/src/fix.rs`:24 on 2023-07-17 12:01_

Nit: `-1` feels weird and I wonder if Rust change's the underlying type because of it. You can change your tests to use less than instead of greater than instead of assigning explicit values. 

Would you, in turn, document the ordering of the values



---

_Review comment by @MichaReiser on `crates/ruff_cli/src/printer.rs`:123 on 2023-07-17 12:04_

I think it would be good to compute the diagnostic counts once instead of once in `write_summary_text` and once in `write_once` to avoid iterating the diagnostics twice (could be good to run the cpython benchmark before/after your change)

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/printer.rs`:392 on 2023-07-17 12:06_

Hmm... I'm not convinced by `unsafe` because it now results in mixed up terminology (should you use "unsafe" or suggested). I think we should either explicitly use suggested or unsafe (also externally to avoid confusion). @zanie what do you think?

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/printer.rs`:398 on 2023-07-17 12:11_

I think it would help readability and reusability if you move the logic into its own struct and split into smaller functions (again, naming is hard)

```rust
struct FixableStatistics {
    automatic: u32,
    suggested: u32,
}

impl FixableStatistics {
    fn from_slice(diagnostic: &[Diagnostic]) -> Self {
        // count
    }

    fn has_applicable_fixes(suggested_fixes: SuggestedFixes) -> bool {
        // returns true if there are any applicable fixes
    }
}
```

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/printer.rs`:409 on 2023-07-17 12:12_

Nit: I think a simple for loop is easier to read, except if you use filter first


```suggestion
    for message in diagnsotics.messages {
        if let Some(fix) = &msg.fix {
            if fix.applicability() == Applicability::Suggested {
                unsafe_remaining += 1;
            } else if fix.applicability() == Applicability::Automatic {
                safe_remaining += 1;
            }
        }
    }

    // OR
    diagnostics.messages.iter().filter_map(|msg| &msg.fix).for_each(|fix| {
        if fix.applicability() == Applicability::Suggested {
            unsafe_remaining += 1;
        } else if fix.applicability() == Applicability::Automatic {
            safe_remaining += 1;
        }
    });
```

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/printer.rs`:487 on 2023-07-17 12:13_

The count cannot be negative.

```suggestion
fn display_violations(safe_remaining: u32, unsafe_remaining: u32) -> String {
```

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/printer.rs`:468 on 2023-07-17 12:14_


```suggestion
    if safe_remaining > 0 {
        format!("{prefix} {safe_remaining} potentially fixable with the --fix option.")
    }
    if unsafe_remaining > 0 {
        let (line_break, extra_prefix) = if safe_remaining > 0 {
            // Avoid re-allocating prefix.
            ("\n", format!("[{}]", "*".cyan()))
        } else {
            ("", String::new())
        };

        let total = safe_remaining + unsafe_remaining;
        format!(
            "{prefix}{line_break}{extra_prefix} {total} potentially fixable with the --fix --unsafe options."
        )
    }
```

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/printer.rs`:497 on 2023-07-17 12:14_

Can you explain what you mean by *avoid re-allocating prefix*?

---

_@MichaReiser reviewed on 2023-07-17 12:18_

This looks pretty good to me and surprisingly few changes. I've a few nit comments. The main outstanding questions for me are:

* `unsafe` setting: I would still prefer another way of promoting / demoting rules instead. Let's continue on the discussion 
* `unsafe` vs `suggested` wording. I think we should decide to use either wording. Using both in our code base will ultimately spread into user documentation and cause confusion. I'm more leaning towards suggested, although I like the  cautionary effect that`unsafe` has.
* Please extend your test plan, ideally by including screen shots of the different commands.

Fast follow up: We also need to update the documentation.

---

_Review requested from @charliermarsh by @MichaReiser on 2023-07-17 12:19_

---

_Review requested from @zanieb by @MichaReiser on 2023-07-17 12:19_

---

_@evanrittenhouse reviewed on 2023-07-17 14:59_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/settings/flags.rs`:3 on 2023-07-17 14:59_

Since Apply and Diff take a single argument, the compiler requires that Generate does as well. After implementing some of the comments you've suggested that hack (ish) should go away though 

---

_Review comment by @evanrittenhouse on `crates/ruff_cli/src/printer.rs`:497 on 2023-07-17 15:04_

Sorry, should be a TODO there - I want to see if there's a way that I can construct the status such that I can avoid extra allocations. I was looking into .push_str() but I'm pretty sure that won't work. 

---

_@evanrittenhouse reviewed on 2023-07-17 15:04_

---

_@MichaReiser reviewed on 2023-07-17 20:40_

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/printer.rs`:192 on 2023-07-17 20:40_

I now understand your change. Nice touch with showing the different numbers.

I would probably go with a single message that is the same regardless of the specified --unsafe option (I'm on my phone and looking up the original message is too cumbersome. But something along the following lines):

- 10 automatic fixes
- 5 automatic and 5 suggested fixes
- 10 suggested fixes


Or as a single message

10 (5 out of 10 suggested) fixes

This has the advantage that the normal lint phase is entirely independent of the new cli options (we could even land this change independently)

---

_@zanieb reviewed on 2023-07-18 04:40_

---

_Review comment by @zanieb on `crates/ruff_cli/src/printer.rs`:392 on 2023-07-18 04:40_

I'm leaning towards "suggested" for overall coherence. I'll try to take some time tomorrow (Tuesday) to play with it and see how different options feel.

---

_@evanrittenhouse reviewed on 2023-07-18 04:51_

---

_Review comment by @evanrittenhouse on `crates/ruff_cli/src/printer.rs`:392 on 2023-07-18 04:51_

Cool! Excited to hear your thoughts

---

_Converted to draft by @evanrittenhouse on 2023-07-19 04:42_

---

_Review comment by @evanrittenhouse on `crates/ruff_cli/src/printer.rs`:392 on 2023-07-31 03:14_

@MichaReiser did you guys decide on a way forward here?

---

_@evanrittenhouse reviewed on 2023-07-31 03:14_

---

_@evanrittenhouse reviewed on 2023-08-01 03:04_

---

_Review comment by @evanrittenhouse on `crates/ruff_diagnostics/src/fix.rs`:28 on 2023-08-01 03:04_

That's the plan as a follow-up to this one

---

_Comment by @zanieb on 2023-08-03 15:17_

Hey @evanrittenhouse 

I just want to let you know that I'm working on our CLI design right now and I expect it to overlap a bit with our plans for this feature. For that reason, I expect it to be a bit before we're ready to merge this breaking change. I'll be back with some guidance soon hopefully.

Let me know if you have any questions or concerns!

---

_Comment by @evanrittenhouse on 2023-08-03 16:44_

@zanieb no worries, thanks for the heads up! I'll just work on implementing the changes mentioned and leave the CLI option stuff for later. Should be able to get the underlying implementation down at least. 

I might go back and see what I can do about the Markdown parser...

---

_@evanrittenhouse reviewed on 2023-08-04 03:14_

---

_Review comment by @evanrittenhouse on `crates/ruff_cli/src/printer.rs`:392 on 2023-08-04 03:14_

Resolving, waiting for @zanieb's clarification

---

_@evanrittenhouse reviewed on 2023-08-04 03:15_

---

_Review comment by @evanrittenhouse on `crates/ruff_cli/src/printer.rs`:468 on 2023-08-04 03:15_

I don't think this will work - we have to allocate `fix_status` so that we can carry through the first option if there are fixes fixable by both `--fix` and `--fix-unsafe` . If we go with one message instead of 2, then this would work.

---

_Comment by @codspeed-hq[bot] on 2023-09-24 14:35_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/evanrittenhouse:4185_cli_respect_applicability)

### Merging #5119 will **not alter performance**

<sub>Comparing <code>evanrittenhouse:4185_cli_respect_applicability</code> (05462ce) with <code>main</code> (f32b0ee)</sub>



### Summary

`✅ 25` untouched benchmarks






---

_Comment by @evanrittenhouse on 2023-09-24 15:02_

Finally getting around to rebasing this, hoping to have it done either early this week.

---

_Comment by @zanieb on 2023-10-19 13:59_

Replaced by #7769 which used the commits from here. Thanks so much Evan for driving this forward!

---

_Closed by @zanieb on 2023-10-19 13:59_

---
