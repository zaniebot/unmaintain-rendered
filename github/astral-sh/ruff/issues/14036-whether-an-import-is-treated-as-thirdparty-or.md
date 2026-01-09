---
number: 14036
title: Whether an import is treated as ThirdParty or FirstParty depends on whether the SAME config file is explicitly specified or implicitly discovered
type: issue
state: open
author: tnahm
labels:
  - question
assignees: []
created_at: 2024-11-01T09:56:08Z
updated_at: 2024-11-04T16:20:22Z
url: https://github.com/astral-sh/ruff/issues/14036
synced_at: 2026-01-07T13:12:16-06:00
---

# Whether an import is treated as ThirdParty or FirstParty depends on whether the SAME config file is explicitly specified or implicitly discovered

---

_Issue opened by @tnahm on 2024-11-01 09:56_

I am calling ruff in two different ways, once explicitly specifiying the config file as  /home/torsten/src/service-digital-agent/.ruff.toml, once having it autodiscovered via parent.

`ruff check --config /home/torsten/src/service-digital-agent/.ruff.toml --verbose knowledge_retrieval/__init__.py`

vs.

`ruff check --verbose knowledge_retrieval/__init__.py`

In both cases, exactly the same configuration file is used (see debug logs below).

In the first case, the import of the (local) data module is correctly identified as "FirstParty", in the second case it is incorrectly identified as "ThirdParty" (see debug logs below):

> [2024-11-01][10:52:16][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'data' as Known(**FirstParty**) (SourceMatch("/home/torsten/src/service-digital-agent/core"))

vs.

> [2024-11-01][10:52:25][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'data' as Known(**ThirdParty**) (NoMatch)

This leads to a different treatment of the import statement, with an error of "I001 [*] Import block is un-sorted or un-formatted" in the second case.

I am using ruff 0.7.1

```
torsten$ ruff check --config /home/torsten/src/service-digital-agent/.ruff.toml --verbose knowledge_retrieval/__init__.py | head -1
[2024-11-01][10:52:16][ruff::resolve][DEBUG] Using user-specified configuration file at: /home/torsten/src/service-digital-agent/.ruff.toml
[2024-11-01][10:52:16][ruff::commands::check][DEBUG] Identified files to lint in: 4.979152ms
[2024-11-01][10:52:16][ruff::diagnostics][DEBUG] Checking: /home/torsten/src/service-digital-agent/core/knowledge_retrieval/__init__.py
[2024-11-01][10:52:16][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'typing' as Known(StandardLibrary) (KnownStandardLibrary)
[2024-11-01][10:52:16][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'threading' as Known(StandardLibrary) (KnownStandardLibrary)
[2024-11-01][10:52:16][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'pandas' as Known(ThirdParty) (NoMatch)
[2024-11-01][10:52:16][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'tiktoken' as Known(ThirdParty) (NoMatch)
[2024-11-01][10:52:16][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'pathlib' as Known(StandardLibrary) (KnownStandardLibrary)
[2024-11-01][10:52:16][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'os' as Known(StandardLibrary) (KnownStandardLibrary)
[2024-11-01][10:52:16][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'data' as Known(FirstParty) (SourceMatch("/home/torsten/src/service-digital-agent/core"))
[2024-11-01][10:52:16][ruff_linter::rules::isort::categorize][DEBUG] Categorized '.evaluate' as Known(LocalFolder) (NonZeroLevel)
[2024-11-01][10:52:16][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'loguru' as Known(ThirdParty) (NoMatch)
[2024-11-01][10:52:16][ruff_linter::rules::isort::categorize][DEBUG] Categorized '.dense' as Known(LocalFolder) (NonZeroLevel)
[2024-11-01][10:52:16][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'app.utils' as Known(FirstParty) (SourceMatch("/home/torsten/src/service-digital-agent/core"))
[2024-11-01][10:52:16][ruff_linter::rules::isort::categorize][DEBUG] Categorized '.sparse' as Known(LocalFolder) (NonZeroLevel)
[2024-11-01][10:52:16][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'app.settings' as Known(FirstParty) (SourceMatch("/home/torsten/src/service-digital-agent/core"))
[2024-11-01][10:52:16][ruff_linter::rules::isort::categorize][DEBUG] Categorized '.mix' as Known(LocalFolder) (NonZeroLevel)
[2024-11-01][10:52:16][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'da_common.schemas' as Known(ThirdParty) (NoMatch)
[2024-11-01][10:52:16][ruff_linter::rules::isort::categorize][DEBUG] Categorized '.base' as Known(LocalFolder) (NonZeroLevel)
[2024-11-01][10:52:16][ruff_linter::rules::isort::categorize][DEBUG] Categorized '.rrf' as Known(LocalFolder) (NonZeroLevel)
[2024-11-01][10:52:16][ruff::commands::check][DEBUG] Checked 1 files in: 22.135574ms
All checks passed!
torsten$ ruff check --verbose knowledge_retrieval/__init__.py | head -1
[2024-11-01][10:52:25][ruff::resolve][DEBUG] Using configuration file (via parent) at: /home/torsten/src/service-digital-agent/.ruff.toml
[2024-11-01][10:52:25][ruff::commands::check][DEBUG] Identified files to lint in: 3.510834ms
[2024-11-01][10:52:25][ruff::diagnostics][DEBUG] Checking: /home/torsten/src/service-digital-agent/core/knowledge_retrieval/__init__.py
[2024-11-01][10:52:25][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'typing' as Known(StandardLibrary) (KnownStandardLibrary)
[2024-11-01][10:52:25][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'threading' as Known(StandardLibrary) (KnownStandardLibrary)
[2024-11-01][10:52:25][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'pandas' as Known(ThirdParty) (NoMatch)
[2024-11-01][10:52:25][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'tiktoken' as Known(ThirdParty) (NoMatch)
[2024-11-01][10:52:25][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'pathlib' as Known(StandardLibrary) (KnownStandardLibrary)
[2024-11-01][10:52:25][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'os' as Known(StandardLibrary) (KnownStandardLibrary)
[2024-11-01][10:52:25][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'data' as Known(ThirdParty) (NoMatch)
[2024-11-01][10:52:25][ruff_linter::rules::isort::categorize][DEBUG] Categorized '.evaluate' as Known(LocalFolder) (NonZeroLevel)
[2024-11-01][10:52:25][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'loguru' as Known(ThirdParty) (NoMatch)
[2024-11-01][10:52:25][ruff_linter::rules::isort::categorize][DEBUG] Categorized '.dense' as Known(LocalFolder) (NonZeroLevel)
[2024-11-01][10:52:25][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'app.utils' as Known(ThirdParty) (NoMatch)
[2024-11-01][10:52:25][ruff_linter::rules::isort::categorize][DEBUG] Categorized '.sparse' as Known(LocalFolder) (NonZeroLevel)
[2024-11-01][10:52:25][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'app.settings' as Known(ThirdParty) (NoMatch)
[2024-11-01][10:52:25][ruff_linter::rules::isort::categorize][DEBUG] Categorized '.mix' as Known(LocalFolder) (NonZeroLevel)
[2024-11-01][10:52:25][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'da_common.schemas' as Known(ThirdParty) (NoMatch)
[2024-11-01][10:52:25][ruff_linter::rules::isort::categorize][DEBUG] Categorized '.base' as Known(LocalFolder) (NonZeroLevel)
[2024-11-01][10:52:25][ruff_linter::rules::isort::categorize][DEBUG] Categorized '.rrf' as Known(LocalFolder) (NonZeroLevel)
[2024-11-01][10:52:25][ruff::commands::check][DEBUG] Checked 1 files in: 16.135841ms
knowledge_retrieval/__init__.py:1:1: I001 [*] Import block is un-sorted or un-formatted
```

---

_Comment by @charliermarsh on 2024-11-02 20:29_

If you use `--config`, then Ruff resolves configuration relative to the current working directory. If you don't use `--config`, then Ruff resolves configuration relative to the "project root". I suspect that's the source of the difference it's intentional. (Ruff will then look in `{project_root}/src` and `{project_root}` for a module named `data`.)

This is all documented here: https://docs.astral.sh/ruff/faq/#how-does-ruff-determine-which-of-my-imports-are-first-party-third-party-etc.

---

_Label `question` added by @charliermarsh on 2024-11-02 20:29_

---

_Comment by @tnahm on 2024-11-04 16:20_

Hi Charlie, thanks for your comment! I looked at the documentation and indeed this looks intentional. That being said, it does feel more like a bug than a feature, and I (and several colleagues) found it quite unintuitive that specifying a particular config file via --config changes how ruff selects the project root as a side effect. Also the help string for `--config`
```
--config <CONFIG_OPTION>  Either a path to a TOML configuration file (`pyproject.toml` or `ruff.toml`), or a TOML `<KEY> = <VALUE>`
                          pair (such as you might find in a `ruff.toml` configuration file) overriding a specific configuration
                          option. Overrides of individual settings using this option always take precedence over all configuration
                          files, including configuration files that were also specified using `--config`
```
doesn't mention anything that might lead a user to suspect this behavior.

Therefore I am leaving this open with the hope that the maintainers reconsider this unintuitive side effect, or at least make it clearer in the documentation and console output. Even a log message to the effect of which project root is used would have helped in this case.

---

_Referenced in [astral-sh/ruff#14497](../../astral-sh/ruff/issues/14497.md) on 2024-11-20 19:49_

---
