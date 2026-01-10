```yaml
number: 12111
title: Command for Initializing Ruff Configuration
type: issue
state: open
author: erickguan
labels:
  - cli
  - needs-decision
assignees: []
created_at: 2024-06-30T13:14:50Z
updated_at: 2024-07-01T09:01:55Z
url: https://github.com/astral-sh/ruff/issues/12111
synced_at: 2026-01-10T11:09:54Z
```

# Command for Initializing Ruff Configuration

---

_Issue opened by @erickguan on 2024-06-30 13:14_

Hey,

I am proposing a new command, `ruff init`, to facilitate configuration setup for Python projects. This command aims to simplify the configuration process for both beginners and new projects, without replacing Ruff's default settings. Ruff will continue to function without any configurations, but this feature is intended for advanced users and teams who require more tailored settings.

## Proposal
The ruff init command will create or update the `pyproject.toml` file with a default configuration block. This block can be customized further, but will build on [Ruff's defaults](https://docs.astral.sh/ruff/configuration/). The command should:

1. Detect Python Version: Automatically detect the Python version in the environment.
2. Extend Linting Rules: Add a few more linting rules by default. For example:
  ```python
  select = [
      "E",  # pycodestyle
      "F",  # Pyflakes rules
      "UP", # pyupgrade
      "PERF", # Perflint
      # "SIM", "I"
  ]
  ```
3. Support Configuration Styles (Optional): Allow users to specify "styles" and host Ruff configuration styles via GitHub. For example, Ruff could download configurations from a git repository and use them to initialize the project settings.
4. Avoid Default Configuration Upgrades: This command is for advanced users who understand how to configure Ruff, so it will not support automatic upgrades to default configurations.

## Command Functionality
When running `ruff init [--path <pyproject.toml>]`, the command should:

* Append the configuration block to the end of the file without overwriting existing configurations.
* Print out the changes made to the repository.
* Output the initialization operations.
* Print out the rules Ruff is using.

## Future Enhancements
1. Hierarchical Configuration Initialization: Support for hierarchical configuration. For example, if Ruff detects a configuration in a parent pyproject.toml, it should extend the settings automatically and write the differential configuration to the current directory.
2. Subcommand Naming Consistency: Consider renaming subcommands for clarity. While `ruff init` suggests a necessary step before using Ruff, alternatives like `ruff config` might also be considered, although this feels somewhat ambiguous. I expect Ruff 1.0 could make subcommands more harmorized.
3. Default Configuration Based on Environment: When setting up configuration or different versions of Python, I could imagine different default configurations based on what community generally expects. Python 3.8 was released a long time ago. The coding standard could be quite different to what people expects in a Python 3.12 project.

---

_Comment by @eli-schwartz on 2024-06-30 17:20_

I would normally expect an "init" command for a tool to help new users onboard themselves by stubbing out boilerplate and setting good defaults for greenfield development. It's not obvious to me how this ties into the specific use case of:

> intended for advanced users and teams who require more tailored settings

---

_Comment by @erickguan on 2024-06-30 17:30_

I agree with you. I have a few alternatives:
- `ruff init-config`
- `ruff init-setup`
- `ruff scaffold`
- `ruff advanced-setup`
- `ruff advanced-config`

I know Ruff doesn't use subsub-commands. But these might be interesting:
- `ruff configure setup`
- `ruff generate config`

I could imagine you want to have a plan to organize command hierarchy before comming to a design. In that case, you can freely make changes to this proposal.

---

_Label `cli` added by @dhruvmanila on 2024-07-01 04:24_

---

_Label `needs-decision` added by @dhruvmanila on 2024-07-01 04:24_

---

_Comment by @MichaReiser on 2024-07-01 09:01_

I can see how this would be very useful to set up a new project. 

---
