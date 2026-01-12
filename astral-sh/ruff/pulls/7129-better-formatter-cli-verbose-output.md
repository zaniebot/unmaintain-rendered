```yaml
number: 7129
title: Better formatter CLI verbose output
type: pull_request
state: merged
author: konstin
labels:
  - formatter
assignees: []
merged: true
base: main
head: format-cli-debug-output
created_at: 2023-09-04T11:49:23Z
updated_at: 2023-09-04T22:25:17Z
url: https://github.com/astral-sh/ruff/pull/7129
synced_at: 2026-01-12T02:45:38Z
```

# Better formatter CLI verbose output

---

_Pull request opened by @konstin on 2023-09-04 11:49_

**Summary** A bit of polishing for the formatter CLI verbose output. I'm trying to use more tracing in the main CLI without having to rewrite everything. I included the options for each file because we allow hierarchical configuration, for the formatter at least in theory.

```console
$ target/debug/ruff format -q scripts/
```

```console
$ target/debug/ruff format scripts/
warning: `ruff format` is a work-in-progress, subject to change at any time, and intended only for experimentation.
11 files left unchanged
```

```console
$ target/debug/ruff format -v scripts/
warning: `ruff format` is a work-in-progress, subject to change at any time, and intended only for experimentation.
[2023-09-04][13:37:26][ruff_cli::resolve][DEBUG] Using pyproject.toml (parent) at /home/konsti/ruff/pyproject.toml
[2023-09-04][13:37:26][ignore::walk][DEBUG] ignoring /home/konsti/ruff/scripts/__pycache__: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/home/konsti/ruff/.gitignore"), original: "__pycache__/", actual: "**/__pycache__", is_whitelist: false, is_only_dir: true })))
[2023-09-04][13:37:26][ruff_workspace::resolver][DEBUG] Included path via `include`: "/home/konsti/ruff/scripts/update_schemastore.py"
[2023-09-04][13:37:26][ruff_workspace::resolver][DEBUG] Included path via `include`: "/home/konsti/ruff/scripts/check_ecosystem.py"
[2023-09-04][13:37:26][ruff_workspace::resolver][DEBUG] Included path via `include`: "/home/konsti/ruff/scripts/transform_readme.py"
[2023-09-04][13:37:26][ruff_workspace::resolver][DEBUG] Included path via `include`: "/home/konsti/ruff/scripts/generate_known_standard_library.py"
[2023-09-04][13:37:26][ruff_workspace::resolver][DEBUG] Included path via `include`: "/home/konsti/ruff/scripts/ecosystem_all_check.py"
[2023-09-04][13:37:26][ruff_workspace::resolver][DEBUG] Included path via `include`: "/home/konsti/ruff/scripts/check_docs_formatted.py"
[2023-09-04][13:37:26][ruff_workspace::resolver][DEBUG] Included path via `include`: "/home/konsti/ruff/scripts/update_ambiguous_characters.py"
[2023-09-04][13:37:26][ruff_workspace::resolver][DEBUG] Included path via `include`: "/home/konsti/ruff/scripts/pyproject.toml"
[2023-09-04][13:37:26][ruff_workspace::resolver][DEBUG] Included path via `include`: "/home/konsti/ruff/scripts/generate_mkdocs.py"
[2023-09-04][13:37:26][ruff_workspace::resolver][DEBUG] Included path via `include`: "/home/konsti/ruff/scripts/_utils.py"
[2023-09-04][13:37:26][ruff_workspace::resolver][DEBUG] Ignored path via `exclude`: "/home/konsti/ruff/scripts/.ruff_cache"
[2023-09-04][13:37:26][ruff_workspace::resolver][DEBUG] Included path via `include`: "/home/konsti/ruff/scripts/add_rule.py"
[2023-09-04][13:37:26][ruff_workspace::resolver][DEBUG] Included path via `include`: "/home/konsti/ruff/scripts/add_plugin.py"
[2023-09-04][13:37:26][ruff_workspace::resolver][DEBUG] Included path via `include`: "/home/konsti/ruff/scripts/benchmarks/pyproject.toml"
[2023-09-04][13:37:26][ruff_cli::commands::format][DEBUG] Formatting /home/konsti/ruff/scripts/update_ambiguous_characters.py with PyFormatOptions { source_type: Python, indent_style: Space(4), line_width: LineWidth(88), tab_width: TabWidth(4), line_ending: LineFeed, quote_style: Double, magic_trailing_comma: Respect, source_map_generation: Disabled }
[2023-09-04][13:37:26][ruff_cli::commands::format][DEBUG] Formatting /home/konsti/ruff/scripts/check_ecosystem.py with PyFormatOptions { source_type: Python, indent_style: Space(4), line_width: LineWidth(88), tab_width: TabWidth(4), line_ending: LineFeed, quote_style: Double, magic_trailing_comma: Respect, source_map_generation: Disabled }
[2023-09-04][13:37:26][ruff_cli::commands::format][DEBUG] Formatting /home/konsti/ruff/scripts/_utils.py with PyFormatOptions { source_type: Python, indent_style: Space(4), line_width: LineWidth(88), tab_width: TabWidth(4), line_ending: LineFeed, quote_style: Double, magic_trailing_comma: Respect, source_map_generation: Disabled }
[2023-09-04][13:37:26][ruff_cli::commands::format][DEBUG] Formatting /home/konsti/ruff/scripts/update_schemastore.py with PyFormatOptions { source_type: Python, indent_style: Space(4), line_width: LineWidth(88), tab_width: TabWidth(4), line_ending: LineFeed, quote_style: Double, magic_trailing_comma: Respect, source_map_generation: Disabled }
[2023-09-04][13:37:26][ruff_cli::commands::format][DEBUG] Formatting /home/konsti/ruff/scripts/transform_readme.py with PyFormatOptions { source_type: Python, indent_style: Space(4), line_width: LineWidth(88), tab_width: TabWidth(4), line_ending: LineFeed, quote_style: Double, magic_trailing_comma: Respect, source_map_generation: Disabled }
[2023-09-04][13:37:26][ruff_cli::commands::format][DEBUG] Formatting /home/konsti/ruff/scripts/add_rule.py with PyFormatOptions { source_type: Python, indent_style: Space(4), line_width: LineWidth(88), tab_width: TabWidth(4), line_ending: LineFeed, quote_style: Double, magic_trailing_comma: Respect, source_map_generation: Disabled }
[2023-09-04][13:37:26][ruff_cli::commands::format][DEBUG] Formatting /home/konsti/ruff/scripts/ecosystem_all_check.py with PyFormatOptions { source_type: Python, indent_style: Space(4), line_width: LineWidth(88), tab_width: TabWidth(4), line_ending: LineFeed, quote_style: Double, magic_trailing_comma: Respect, source_map_generation: Disabled }
[2023-09-04][13:37:26][ruff_cli::commands::format][DEBUG] Formatting /home/konsti/ruff/scripts/generate_known_standard_library.py with PyFormatOptions { source_type: Python, indent_style: Space(4), line_width: LineWidth(88), tab_width: TabWidth(4), line_ending: LineFeed, quote_style: Double, magic_trailing_comma: Respect, source_map_generation: Disabled }
[2023-09-04][13:37:26][ruff_cli::commands::format][DEBUG] Formatting /home/konsti/ruff/scripts/check_docs_formatted.py with PyFormatOptions { source_type: Python, indent_style: Space(4), line_width: LineWidth(88), tab_width: TabWidth(4), line_ending: LineFeed, quote_style: Double, magic_trailing_comma: Respect, source_map_generation: Disabled }
[2023-09-04][13:37:26][ruff_cli::commands::format][DEBUG] Formatting /home/konsti/ruff/scripts/generate_mkdocs.py with PyFormatOptions { source_type: Python, indent_style: Space(4), line_width: LineWidth(88), tab_width: TabWidth(4), line_ending: LineFeed, quote_style: Double, magic_trailing_comma: Respect, source_map_generation: Disabled }
[2023-09-04][13:37:26][ruff_cli::commands::format][DEBUG] Formatting /home/konsti/ruff/scripts/add_plugin.py with PyFormatOptions { source_type: Python, indent_style: Space(4), line_width: LineWidth(88), tab_width: TabWidth(4), line_ending: LineFeed, quote_style: Double, magic_trailing_comma: Respect, source_map_generation: Disabled }
[2023-09-04][13:37:26][ruff_cli::commands::format][DEBUG] Formatted 11 files in 29.62ms
11 files left unchanged
```

**Test Plan** I don't think it makes sense to add tests for this


---

_Label `formatter` added by @konstin on 2023-09-04 11:49_

---

_Comment by @github-actions[bot] on 2023-09-04 12:05_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_@MichaReiser approved on 2023-09-04 12:06_

What's your opinion on using [`tracing_subscriber`](https://docs.rs/tracing-subscriber/latest/tracing_subscriber/) with [tracing-tree](https://github.com/davidbarsky/tracing-tree) instead of using `log` directly. 

Using subscribers has the advantage that the traces can either be forwarded to a file or the console depending on the subscriber setup without requiring any code changes.

---

_Comment by @konstin on 2023-09-04 12:17_

i'd like to switch out fern for tracing-subscriber and it's related tools (i really like tracing-indicatif so far and tracing-tree looks similar), but that's an unrelated bigger change

---

_@charliermarsh approved on 2023-09-04 21:58_

---

_Merged by @konstin on 2023-09-04 22:25_

---

_Closed by @konstin on 2023-09-04 22:25_

---

_Branch deleted on 2023-09-04 22:25_

---
