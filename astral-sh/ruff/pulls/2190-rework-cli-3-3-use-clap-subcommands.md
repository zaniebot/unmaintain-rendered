```yaml
number: 2190
title: "rework CLI (3/3): Use clap subcommands"
type: pull_request
state: merged
author: not-my-profile
labels: []
assignees: []
merged: true
base: main
head: subcommands
created_at: 2023-01-26T07:46:20Z
updated_at: 2023-01-28T03:21:24Z
url: https://github.com/astral-sh/ruff/pull/2190
synced_at: 2026-01-12T15:55:07Z
```

# rework CLI (3/3): Use clap subcommands

---

_@not-my-profile_

Followup PR for #2155.

This greatly simplifies the implementation, as well as improves the UX of the CLI (by no longer listing all options at once even though many of them are in fact incompatible).

To preserve backwards compatibility as much as possible aliases have been added (so e.g. `ruff --explain <rule>` still works) and the implicit default subcommand is also preserved (so e.g. `ruff .` is equivalent to `ruff check .`).

New `ruff --help` output:

```
Ruff: An extremely fast Python linter.

Usage: ruff [OPTIONS] <COMMAND>

Commands:
  check    Run ruff on the given files or directories (this command is used by default and may be omitted)
  explain  Explain a rule
  clean    Clear any caches in the current directory or any subdirectories
  help     Print this message or the help of the given subcommand(s)

Options:
  -h, --help     Print help
  -V, --version  Print version

Log levels:
  -v, --verbose  Enable verbose logging
  -q, --quiet    Print lint violations, but nothing else
  -s, --silent   Disable all logging (but still exit with status code "1" upon detecting lint violations)

To get help about a specific command, see 'ruff help <command>'.
```

<details>
<summary>
New <code>ruff help check</code> output
</summary>

```
Run ruff on the given files or directories (this command is used by default and may be omitted)

Usage: ruff check [OPTIONS] [FILES]...

Arguments:
  [FILES]...  

Options:
      --fix              Attempt to automatically fix lint violations
      --show-source      Show violations with source code
      --diff             Avoid writing any fixed files back; instead, output a diff for each changed file to stdout
  -w, --watch            Run in watch mode by re-running whenever files change
      --fix-only         Fix any fixable lint violations, but don't report on leftover violations. Implies `--fix`
      --format <FORMAT>  Output serialization format for violations [env: RUFF_FORMAT=] [possible values: text, json, junit, grouped, github, gitlab, pylint]
      --config <CONFIG>  Path to the `pyproject.toml` or `ruff.toml` file to use for configuration
      --add-noqa         Enable automatic additions of `noqa` directives to failing lines
      --show-files       See the files Ruff will be run against with the current settings
      --show-settings    See the settings Ruff will use to lint a given Python file
  -h, --help             Print help

Rule selection:
      --select <RULE_CODE>
          Comma-separated list of rule codes to enable (or ALL, to enable all rules)
      --ignore <RULE_CODE>
          Comma-separated list of rule codes to disable
      --extend-select <RULE_CODE>
          Like --select, but adds additional rule codes on top of the selected ones
      --extend-ignore <RULE_CODE>
          Like --ignore, but adds additional rule codes on top of the ignored ones
      --per-file-ignores <PER_FILE_IGNORES>
          List of mappings from file pattern to code to exclude
      --fixable <RULE_CODE>
          List of rule codes to treat as eligible for autofix. Only applicable when autofix itself is enabled (e.g., via `--fix`)
      --unfixable <RULE_CODE>
          List of rule codes to treat as ineligible for autofix. Only applicable when autofix itself is enabled (e.g., via `--fix`)

File selection:
      --exclude <FILE_PATTERN>         List of paths, used to omit files and/or directories from analysis
      --extend-exclude <FILE_PATTERN>  Like --exclude, but adds additional files and directories on top of those already excluded
      --respect-gitignore              Respect file exclusions via `.gitignore` and other standard ignore files
      --force-exclude                  Enforce exclusions, even for paths passed to Ruff directly on the command-line

Rule configuration:
      --target-version <TARGET_VERSION>
          The minimum Python version that should be supported
      --line-length <LINE_LENGTH>
          Set the line-length for length-associated rules and automatic formatting
      --dummy-variable-rgx <DUMMY_VARIABLE_RGX>
          Regular expression matching the name of dummy variables

Miscellaneous:
  -n, --no-cache
          Disable cache reads
      --isolated
          Ignore all configuration files
      --cache-dir <CACHE_DIR>
          Path to the cache directory [env: RUFF_CACHE_DIR=]
      --stdin-filename <STDIN_FILENAME>
          The name of the file when passing it through stdin
  -e, --exit-zero
          Exit with status code "0", even upon detecting lint violations
      --update-check
          Enable or disable automatic update checks

Log levels:
  -v, --verbose  Enable verbose logging
  -q, --quiet    Print lint violations, but nothing else
  -s, --silent   Disable all logging (but still exit with status code "1" upon detecting lint violations)
```
</details>

Resolves #455.

---

_Comment by @not-my-profile on 2023-01-27 04:06_

>  I need some time to digest the impact and implications.

For what it's worth I think now that the PR is keeping `--add-noqa`, `--show-settings`  and `--show-files` as option-only flags, the likelihood that this breaks existing commands has gone down significantly.

As I tried to explain in `BREAKING_CHANGES.md` I think there are only four reasons why this change might break a command:

1. `--explain`, `--clean` or `--generate-shell-completion` are used in a position other than 1st argument.
2. `--explain`, `--clean` or `--generate-shell-completion` are used with unsupported options that were previously silently ignored.
3. You have a directory/file named after a new subcommand (i.e. `check`, `explain`, `clean`, `help` or `generate-shell-completion`) and run `ruff <name>` ... you now have to use either `ruff check <name>` or `ruff -- <name>`.
4. You used `--explain` with `--format grouped` (which didn't do anything other than `--format text` so I removed it in this PR).

I think all of these are very niche cases. I think for the majority of users this change shouldn't break anything but rather have them benefit from a more intuitive CLI (where the synopsis of subcommands is clear and supplying an option that doesn't do anything results in an error rather than being silently ignored).

---

_Comment by @charliermarsh on 2023-01-27 13:01_

I'm hoping to merge this today, but it might happen over the weekend.

---

_Comment by @charliermarsh on 2023-01-27 13:01_

(I want to do some testing myself since it's a significant change.)

---

_Merged by @charliermarsh on 2023-01-28 00:38_

---

_Closed by @charliermarsh on 2023-01-28 00:38_

---

_Comment by @not-my-profile on 2023-01-28 03:08_

Just FYI I have some followup changes coming up that we probably want to merge before making a new release.

In particular I think we should rename the `explain` command to `rule` because I think we'll end up wanting multiple "explain" commands and overloading one command to explain everything seems like a bad idea.

In particular my idea is:

* `ruff rule` lists all rules supported by ruff
* `ruff rule <code>` explains a specific rule
* `ruff linter` lists all linters supported by ruff
* `ruff linter <name>` lists all rules/options supported by a specific linter

(opened #2288)

---
