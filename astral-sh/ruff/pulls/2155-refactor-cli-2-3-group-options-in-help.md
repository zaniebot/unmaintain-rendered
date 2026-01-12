```yaml
number: 2155
title: "refactor CLI (2/3): Group options in `--help`"
type: pull_request
state: merged
author: not-my-profile
labels: []
assignees: []
merged: true
base: main
head: refactor-cli
created_at: 2023-01-25T14:13:35Z
updated_at: 2023-01-26T19:19:07Z
url: https://github.com/astral-sh/ruff/pull/2155
synced_at: 2026-01-12T15:55:07Z
```

# refactor CLI (2/3): Group options in `--help`

---

_@not-my-profile_

This is the followup PR for #2145.

New `ruff --help` output:

```
Ruff: An extremely fast Python linter.

Usage: ruff [OPTIONS] [FILES]...

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
  -V, --version          Print version

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

Subcommands:
      --explain <EXPLAIN>  Explain a rule
      --clean              Clear any caches in the current directory or any subdirectories

Log levels:
  -v, --verbose  Enable verbose logging
  -q, --quiet    Print lint violations, but nothing else
  -s, --silent   Disable all logging (but still exit with status code "1" upon detecting lint violations)
```

---

_Renamed from "refactor CLI (2/2)" to "refactor CLI (2/2): Use subcommands" by @not-my-profile on 2023-01-25 14:32_

---

_Renamed from "refactor CLI (2/2): Use subcommands" to "refactor CLI (2/2): Use clap subcommands" by @not-my-profile on 2023-01-25 14:32_

---

_Comment by @charliermarsh on 2023-01-26 03:13_

This is a significant improvement. And migrating to subcommands opens up a lot of opportunities

 I've been hesitant to do this out of fear of breaking compatibility. But the setup you've achieved here is pretty interesting. I need some time to digest the impact and implications.

I do think we should have clear answers to a few questions that've come up before:

1. Do we expect `ruff .` to _always_ work? That is, should `check` always be the "default" subcommand? Or is this just a temporarily measure, for compatibility?
2. Do we want to move `ruff . --fix` to `ruff fix`? Is there any value, at all, in that? `deno` has `deno check` (to type-check) and `deno fmt` to format, but no clear analog here. (I think the answer is "no", but now is the time to ask it.)


---

_Converted to draft by @not-my-profile on 2023-01-26 04:29_

---

_Renamed from "refactor CLI (2/2): Use clap subcommands" to "rework CLI (2/3): Group options in `--help`" by @not-my-profile on 2023-01-26 05:40_

---

_Marked ready for review by @not-my-profile on 2023-01-26 05:40_

---

_Comment by @not-my-profile on 2023-01-26 05:49_

I have updated this PR to only group the options in `--help` for now so that it can be merged and the subcommands can be implemented in a followup PR :) Please see the latest commit message for the reasoning.

To be clear this PR is now purely a refactor (aside from the changed `--help` output).

> Do we expect `ruff .` to *always* work?

Yes I'd expect it to always work. I think it makes sense that a tool defaults to its main functionality. `ruff` is a linter so it makes sense that invoking it with some files/directories just "does the thing it's supposed to do" (linting).

> Do we want to move `ruff . --fix` to `ruff fix`? Is there any value, at all, in that?

I think subcommands primarily make sense when they have a different synopsis, e.g:

* `--explain` and `--clean` don't support any of the other options (=> they should be subcommands)
* `--fix` and `--add-noqa` act like boolean flags (that ideally should be combinable, => option flags are fine)

Thanks for asking this question ... it made me realize that `--add-noqa` really shouldn't be a subcommand, since it would make sense to be able to combine it with `--fix` (even though that currently doesn't work at all, see #2187).
In the same vein I think `--show-settings` and `--show-files` shouldn't be subcommands since combining them with the other options would be useful, e.g. you should be able to see how `--select` or `--no-respect-gitignore` affects the selected rules / files.

---

_Renamed from "rework CLI (2/3): Group options in `--help`" to "refactor CLI (2/3): Group options in `--help`" by @not-my-profile on 2023-01-26 05:50_

---

_@charliermarsh reviewed on 2023-01-26 17:32_

---

_Review comment by @charliermarsh on `ruff_cli/src/args.rs`:47 on 2023-01-26 17:32_

Should this not be grouped with `show_source`? Same for `no_fix` and `fix`.

---

_Review comment by @not-my-profile on `ruff_cli/src/args.rs`:47 on 2023-01-26 17:41_

Ah yes, good catch ... fixed :)

---

_@not-my-profile reviewed on 2023-01-26 17:41_

---

_@charliermarsh reviewed on 2023-01-26 17:47_

---

_Review comment by @charliermarsh on `src/settings/mod.rs`:134 on 2023-01-26 17:47_

Is the idea here that we now validate when reading the config, rather than when transforming it into `Settings`? I wonder why I didn't do it this way initially. I'm getting Chesterton's fence vibes.

---

_@not-my-profile reviewed on 2023-01-26 17:55_

---

_Review comment by @not-my-profile on `src/settings/mod.rs`:134 on 2023-01-26 17:55_

I am sorry I cannot tell you why you wrote it this way ... looking at it just doesn't make sense to me ... as I explained in the commit message in Rust you ideally manage to make illegal states unrepresentable ... since `Settings` is used all over the codebase it only makes sense that `Settings` denotes valid settings as opposed to settings that may be invalid. So yes I think the Settings constructor is the right place for any settings validation.

---

_Merged by @charliermarsh on 2023-01-26 18:06_

---

_Closed by @charliermarsh on 2023-01-26 18:06_

---

_Review comment by @charliermarsh on `src/settings/mod.rs`:134 on 2023-01-26 18:06_

Are you saying you don't like my code...? (Joking)

---

_@charliermarsh reviewed on 2023-01-26 18:06_

---

_@not-my-profile reviewed on 2023-01-26 18:24_

---

_Review comment by @not-my-profile on `src/settings/mod.rs`:134 on 2023-01-26 18:24_

Haha ... no. I think it's very impressive that you managed to get ruff so far and keep grinding on the issues that matter. I tend to get sidetracked too much while pursuing my own projects ...

---

_@charliermarsh reviewed on 2023-01-26 19:03_

---

_Review comment by @charliermarsh on `src/settings/mod.rs`:134 on 2023-01-26 19:03_

Thank you :) I was only kidding but maybe I'll just say a few quick things:

- I try to stay unattached to my code so, of course, always feel free to suggest improvements.
- To me much of building software is tradeoffs and prioritization... Ruff is a big project and so required making a lot of tradeoffs!
- As a result, there's certainly "bad" code in various places, things that are done poorly that I know I'd like to be done differently, things that are done poorly that I've totally forgotten about, etc.
- ...and as such, I'm really appreciative of all the larger refactors you've taken on! They've made the project much better. Some of them aligned with things I wanted to change, some of them hadn't even occurred to me.

---

_@not-my-profile reviewed on 2023-01-26 19:19_

---

_Review comment by @not-my-profile on `src/settings/mod.rs`:134 on 2023-01-26 19:19_

I know you were kidding^^

Right ... I just often don't like making tradeoffs and try to avoid them and end up going down some rabbit hole.

---
