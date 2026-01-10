```yaml
number: 16743
title: "`uvx`/`uv tool run` install confirmation prompt"
type: pull_request
state: open
author: mikaylathompson
labels: []
assignees: []
draft: true
base: main
head: mikayla/uvx-install-prompt
created_at: 2025-11-14T20:11:43Z
updated_at: 2025-11-18T21:34:45Z
url: https://github.com/astral-sh/uv/pull/16743
synced_at: 2026-01-10T05:58:11Z
```

# `uvx`/`uv tool run` install confirmation prompt

---

_Pull request opened by @mikaylathompson on 2025-11-14 20:11_

## Summary

This is a proof-of-concept for https://github.com/astral-sh/uv/issues/16600, providing installation confirmation prompts.

It adds an opt-in confirmation prompt system that asks users to confirm before installing uncached packages when running `uvx` or `uv tool run`. The feature is gated behind the `tool-install-confirmation` preview flag.

The implementation adds a pre-download hook that gets passed down to the HTTP client to run before downloading any files. Putting it right before the download step ensures that no already-cached packages are prompted. Additionally, this hook can be used by future implementations of the prompt in `uv add` or other commands. To get to that step, the biggest change would be to 

A heuristics mechanism allows certain packages to skip prompts automatically. Right now only the top-packages heuristic is implemented, which skips prompts for a number of popular Python packages determined by analyzing GitHub Code Search results for `uvx` usage patterns. Two other heuristics are stubbed out but not implemented yet: previously-installed and a custom allowlist.

Configuration is handled through `InstallPromptOptions` in the settings, with an `approve-all-tool-installs` flag to skip all prompts (useful for CI/CD) and `approve-all-heuristics` to control which heuristics are enabled. There's also a `--approve-all-tool-installs` CLI flag that overrides config settings. The prompt logic handles TTY detection for non-interactive environments, to minimize the impact on CI and other non-interactive usecases.

----

Config file (`~/.config/uv/uv.toml` or `uv.toml`):

```toml
# Skip all prompts (useful for CI/CD)
approve-all-tool-installs = true

# Or configure heuristics (defaults to ["top-packages"])
approve-all-heuristics = ["top-packages"]

# Disable all heuristics to prompt for everything
approve-all-heuristics = []
```

CLI usage:

```bash
# Enable the feature and get prompted for uncached packages
$ uvx --preview-features tool-install-confirmation some-package

# Skip prompts for this run only
$ uvx --preview-features tool-install-confirmation --approve-all-tool-installs some-package

# Less common packages will prompt
$ uvx --preview-features tool-install-confirmation obscure-package

# with top-packages heuristic disabled
$ uvx --preview ipython
⠙ Resolving dependencies...                                                                                                                                                                              This tool is provided by the following package:

+ ipython

This package is not currently installed.
✔ Would you like to proceed? · yes
Installed 16 packages in 30ms
Python 3.14.0 (main, Oct 14 2025, 21:10:22) [Clang 20.1.4 ]
Type 'copyright', 'credits' or 'license' for more information
IPython 9.7.0 -- An enhanced Interactive Python. Type '?' for help.
Tip: You can find how to type a LaTeX symbol by back-completing it, eg `\θ<tab>` will expand to `\theta`.

In [1]:
```

Catching a typo with top-packages enabled
```
$ uvx --preview rufff check
⠙ Resolving dependencies...                                                                                                                                                                              This tool is provided by the following package:

+ rufff

This package is not currently installed and is not a top python package.
? Would you like to proceed?
✔ Would you like to proceed?
 · no
Installation cancelled by the user.
error: Download cancelled by user
```

## Test Plan

There are unit tests, but they're somewhat limited by the tty-detecting behavior. It's possibly worth mocking that call to block it, but I haven't made it that far.

-------

### A bit on the top-packages.txt file

Zsolt brought up that packages popular for tool use are not necessarily those well-represented in the overall PyPI data. I was curious about the idea of searching GitHub for mentions of `uvx` to figure out what packages _are_ being used as tools.

The manual search results looked promising, so I embarked on the surprisingly-complicated step of collating this data in a repeatable way.

The GitHub code search API is annoying-at-best:
- Rate limited to 10 requests/minute
- Wildcards are not supported
- You can not combine any repository search fields (or user, org, etc.) with code search characteristics
- Maximum two text matches returned per file
- and worst of all: a maximum of 1000 results for any query (and that's on top of the 100 max results per page).
- Plus a bunch of other limitations listed here: https://docs.github.com/en/search-github/searching-on-github/searching-code#considerations-for-code-search

Fiddling with this resulted in the following non-optimized process:

1. Query for "uvx" in Markdown and Shell files, with each query broken up into a number of size blocks. Size blocks have been mostly manually fine-tuned to result in ~1000 (+/- 200) results per query.
2. Execute each query through all 10 pages (to acquire ~1000 results). For each text match returned, match it against a set of regexes to attempt to extract a potential package name. This is only moderately optimized -- I'm sure I'm missing the actual package name in a number of cases and I'm definitely returning a lot of non-package names (e.g. `# run uvx if you need a tool` returns `if` as a package name).
3. Take the potential package names and check if they're _actual_ package names against the PyPI API. In some ways, this reintroduces the very typosquatting risk we're trying to protect against -- if a package is on PyPI, we count it as "real". 

The results of this look reasonable overall, with some caveats. At this point, I'm putting any package with > 5 mentions in at this point.
- Ideally, you also ensure that the mentions are uncorrelated in terms of user/repo, to minimize the risk of a typo squatting attack that includes a gh campaign to increase apparent credibility.
- While all of these are "real" packages, they're definitely not necessarily packages that people were intentionally callign with uvx. The regexes will extract something like "use uvx with your pipeline..." as a `uvx` call for the package `with`. Applying a PyPI popularity filter on top of this would make it somewhat more resilient to that type of example.

------

### Known edge cases
- `--with <package>` is not caught by the prompt. It's slightly unclear how this fits into the design of only prompting on the top-level package. I think the best approach here is to adjust the prompt to mention both packages. However, this adds some complication where the hook has to somehow collect all of the necessary packages, even though neither the top level (the tool run) or the low level (the http client) know that there will be multiple packages that need to be included (some could already be cached). 

---

_@mikaylathompson reviewed on 2025-11-14 20:14_

---

_Review comment by @mikaylathompson on `crates/uv-client/src/registry_client.rs`:51 on 2025-11-14 20:14_

need to re-add a Debug implementation. The PreDownloadHook broke the automatic one.

---

_@mikaylathompson reviewed on 2025-11-14 20:16_

---

_Review comment by @mikaylathompson on `crates/uv/src/commands/project/environment.rs`:170 on 2025-11-14 20:16_

fixing

---

_@mikaylathompson reviewed on 2025-11-14 20:17_

---

_Review comment by @mikaylathompson on `crates/uv/src/commands/project/mod.rs`:1970 on 2025-11-14 20:17_

it seems like I should just be able to do this as part of the builder. I keep banging against the compiler doing so though.

---

_Comment by @samypr100 on 2025-11-15 01:13_

Even though it's opt-in right now (thinking about a future where it's opt-out), do we want a security setting such as `approve-all-tool-installs = true` to be specified in a project-level configuration? This is similar to a previous remark I've made https://github.com/astral-sh/uv/issues/9461#issuecomment-2509801588

---

_Converted to draft by @zanieb on 2025-11-18 21:34_

---
