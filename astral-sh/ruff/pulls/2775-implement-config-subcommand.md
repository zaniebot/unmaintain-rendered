```yaml
number: 2775
title: "Implement `config` subcommand"
type: pull_request
state: merged
author: not-my-profile
labels: []
assignees: []
merged: true
base: main
head: config-subcommand
created_at: 2023-02-11T17:53:28Z
updated_at: 2023-02-12T04:43:10Z
url: https://github.com/astral-sh/ruff/pull/2775
synced_at: 2026-01-12T04:52:01Z
```

# Implement `config` subcommand

---

_Pull request opened by @not-my-profile on 2023-02-11 17:53_

The synopsis is as follows.

List all top-level config keys:

    $ ruff config
    allowed-confusables
    builtins
    cache-dir
    ... etc.

List all config keys in a specific section:

    $ ruff config mccabe
    max-complexity

Describe a specific config option:

    $ ruff config mccabe.max-complexity
    The maximum McCabe complexity to allow before triggering `C901` errors.

    Default value: 10
    Type: int
    Example usage:
    ```toml
    # Flag errors (`C901`) whenever the complexity level exceeds 5.
    max-complexity = 5
    ```

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/commands/config.rs`:36 on 2023-02-11 20:01_

Can we wrap the usage in backticks with a `toml` syntax annotation?

---

_@charliermarsh reviewed on 2023-02-11 20:01_

---

_Review comment by @not-my-profile on `crates/ruff_cli/src/commands/config.rs`:36 on 2023-02-11 20:34_

Sure ... added :)

---

_@not-my-profile reviewed on 2023-02-11 20:34_

---

_Comment by @charliermarsh on 2023-02-12 00:39_

Do you have any opinion (and should we have any opinion) on the settings vs. configuration vs. options terminology? They mean different things internally -- I'm not sure what makes most sense to communicate to users, but I feel like you're often able to articulate opinions on these kinds of questions :)

---

_Comment by @charliermarsh on 2023-02-12 02:07_

One reason I ask is that `ruff check --config /path/to/pyproject.toml` exists, and so in that context `--config` means the `pyproject.toml` file.


---

_Converted to draft by @not-my-profile on 2023-02-12 03:34_

---

_Comment by @not-my-profile on 2023-02-12 03:50_

I think as far as users are / should be concerned settings, configuration and options all refer to the same thing ... but we should of course try to be consistent in our usage of these terms.

Some compound nouns are generally more established, e.g:

* configuration file
* command-line option

I think it makes sense for us to use these commonly-used terms. Note that I don't think that using all of these terms is a problem, e.g we may say:

> Your ruff settings consist of several options which can be defined either in `.toml` configuration files or provided via command-line options. You can have multiple configuration sources from which ruff computes your effective settings.

So in that regard we'd use:

* "option" to refer to individual settings
* "settings" to refer to the effective configuration

Regarding this new command, I think the case could be made that it should rather be named `option` since that would be more consistent with our existing `rule` and `linter` commands which are also nouns referring to singular entities. And I think indeed `option` would be a better name for the command as currently implemented in this PR ... however I think in the future the new command could additionally print which current values are defined in which `.toml` files ... in which case the command would do more than just describing the available options and `config` would be very apt because e.g. `git config user.name` prints your configured git username ... and `ruff config <option>` could do the same.

---

_Marked ready for review by @not-my-profile on 2023-02-12 03:50_

---

_Merged by @charliermarsh on 2023-02-12 04:43_

---

_Closed by @charliermarsh on 2023-02-12 04:43_

---
