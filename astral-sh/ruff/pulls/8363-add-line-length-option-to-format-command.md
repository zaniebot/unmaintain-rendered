```yaml
number: 8363
title: "Add `--line-length` option to `format` command"
type: pull_request
state: merged
author: zanieb
labels:
  - cli
  - formatter
assignees: []
merged: true
base: main
head: zanie/format-ll
created_at: 2023-10-30T17:59:12Z
updated_at: 2023-11-03T12:01:04Z
url: https://github.com/astral-sh/ruff/pull/8363
synced_at: 2026-01-12T15:55:26Z
```

# Add `--line-length` option to `format` command

---

_@zanieb_

Restores the `--line-length` option removed in https://github.com/astral-sh/ruff/pull/8131

Closes #8362
Closes #8352 

We removed this option because we did not feel it made sense for the format CLI to expose a single configuration option but not others.

However, there are a few arguments in favor of restoring this option:

1. It is the most asked for option (nobody has pointed out the other options are missing)
2. It is also available via the `ruff check` CLI
3. It is also available via the Black CLI
4. There is no way for a user to configure the line width without a configuration file yet
5. The line length is a unique formatter option; in that it is the most "restrictive" and not a toggle

While we intend to introduce override of general configuration options via the CLI, we have not built it yet. If we replace `ruff check --line-length=...` with a generic `ruff check --setting line-length=...` we'll need to manage a deprecation period anyway.

There remain some arguments against restoring this option:

1. If/when we deprecate this option in favor of a `--setting` flag, more users will be affected
2. Inconsistencies of the availability of configuration options via the CLI could be confusing
3. Should this be `--line-width` to match our terminology for measuring enforced line length?



---

_Label `cli` added by @zanieb on 2023-10-30 17:59_

---

_Label `formatter` added by @zanieb on 2023-10-30 17:59_

---

_Marked ready for review by @zanieb on 2023-10-30 18:09_

---

_Review requested from @MichaReiser by @zanieb on 2023-10-30 18:10_

---

_Review requested from @charliermarsh by @zanieb on 2023-10-30 18:10_

---

_Comment by @github-actions[bot] on 2023-10-30 18:26_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no linter changes.

✅ ecosystem check detected no format changes.



---

_Comment by @T-256 on 2023-10-30 20:59_

> * If/when we deprecate this option in favor of a `--setting` flag, more users will be affected

IMO, this approach would be neater instead of `--settings {option-name}=value`:
```
...

Format configuration:
      --{option-name} <VALUE>  see formatter settings for possible options

      # (also all settings can be auto generated in this section)

Log levels:
  -v, --verbose  Enable verbose logging
  -q, --quiet    Print diagnostics, but nothing else
  -s, --silent   Disable all logging (but still exit with status code "1" upon detecting diagnostics)
```

Then `--line-length` no need to deprecate in future.

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/args.rs`:414 on 2023-10-30 21:02_

This is actually hidden on `ruff check`... We could consider doing the same here.

---

_@charliermarsh approved on 2023-10-30 21:02_

I know we discussed this synchronously but I support this change for the reasons outlined in the summary. Thank you for writing them up.

---

_Comment by @charliermarsh on 2023-10-30 21:03_

> Should this be --line-width to match our terminology for measuring enforced line length?

I believe we should change this if/when we change the terminology everywhere, and that for now, it's best to be consistent with the rest of the settings.

---

_Comment by @zanieb on 2023-10-30 21:16_

@T-256 I'm not sure that scales well to _all_ of the settings. I've created a separate issue for discussion on that topic https://github.com/astral-sh/ruff/issues/8368

---

_@zanieb reviewed on 2023-10-30 21:16_

---

_Review comment by @zanieb on `crates/ruff_cli/src/args.rs`:414 on 2023-10-30 21:16_

Oh.. hm... I don't think we should hide things from the help menu if they're intended to be used?

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/args.rs`:414 on 2023-10-31 07:07_

Agree. We should either expose and document it or not add it. 

It would be nice if we could add a message saying that we recommend using the configuration but it probably doesn't fit in nicely with the CLI documentation (not enough space, not in line with other CLI options)

---

_@MichaReiser reviewed on 2023-10-31 07:16_

I remain concerned about re-introducing the option but I believe removing it and now re-adding it at least served its purpose: We learned more about the use case where people want this option (e.g. pre-commit without a configuration file, VS Code). 

My main concern is that we should encourage users to commit their formatter configuration to get consistent results across teams and interfaces (VS Code, CLI, ...). Adding this option is a step back in that regard.

More details: 

* > It is also available via the ruff check CLI: But it is hidden and we discussed removing it. 
  
  Adding a non-hidden `line-length` doesn't solve the inconsistency. It's differently inconsistent.
* Using `--line-length` in VS code is rarely the right solution. The main advantage of a formatter is consistent formatting. Setting a user-specific `--line-length` doesn't achieve this goal (except if it is a workplace setting and committed). It further has the disadvantage that running `ruff format` on the CLI and in VS code produces different results. Understanding why VS code doesn't respect the line length (or configures code differently) may result in a lengthy debugging session.
* Adding `line-length` unlocks the most common case, but it's still impossible to set all formatter configurations. 
* We're still not black compatible without exposing more options.

To sum it up. It unlocks some use cases but not all, nor does it fix any inconsistencies, it just changes them.

I fear we'll have a hard time justifying not adding other options. Adding them will, similarly, unlock the mentioned use cases for some users and fit the bill as nicely as `--line-length`. I'm okay with this moving forward but would prefer if we pursue alternatives (or justify why we won't) before exposing all formatter options.

---

_Comment by @zanieb on 2023-10-31 18:43_

> Using --line-length in VS code is rarely the right solution. 

While I agree with most of your points, many users use editors for Python files that do not belong to a real "project" and the usability of a formatter in this case is important.

---

_Comment by @MichaReiser on 2023-10-31 21:37_

> While I agree with most of your points, many users use editors for Python files that do not belong to a real "project" and the usability of a formatter in this case is important.

I agree that this is a real use case and that we don't have any other solution for it yet. What I would prefer is something similar to [Prettier's VS Code extension](https://github.com/prettier/prettier-vscode#prettier-settings), allowing to configure a fallback for projects not using Ruff.

---

_Comment by @zanieb on 2023-11-01 01:15_

Oh interesting. I have a strong preference for that. However, I'm okay with merging this until we implement an alternative.

@charliermarsh I don't have the heart to do it though.

---

_Comment by @danielhollas on 2023-11-01 21:24_

> ...Prettier's VS Code extension, allowing to configure a fallback for projects not using Ruff.

How would that that work for vi users?

---

_Comment by @MichaReiser on 2023-11-01 21:36_

> > ...Prettier's VS Code extension, allowing to configure a fallback for projects not using Ruff.
> 
> How would that that work for vi users?

I'm not that familiar with Vim but it would be an lsp setting that you can set.

---

_Merged by @zanieb on 2023-11-02 01:39_

---

_Closed by @zanieb on 2023-11-02 01:39_

---

_Branch deleted on 2023-11-02 01:39_

---

_Comment by @inigohidalgo on 2023-11-03 11:33_

Sorry if this isn't the right place for this, but I started testing ruff yesterday, and I was looking for this setting for the reason @zanieb descibed above

> many users use editors for Python files that do not belong to a real "project" and the usability of a formatter in this case is important.

I am evaluating ruff personally while working on work projects, where I cannot yet commit a new file or toml settings, so am relying on the CLI config to select the rules I want to apply. This could also be solved through some sort of "global" config, as that is basically what I am implementing using the extension settings for the [Ruff vscode extension](https://marketplace.visualstudio.com/items?itemName=charliermarsh.ruff).

EDIT: solved by using the global  `~/.config/ruff/ruff.toml` file. Sorry for the noise

---
