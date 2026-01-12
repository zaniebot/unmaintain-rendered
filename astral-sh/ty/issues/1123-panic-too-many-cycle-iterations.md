```yaml
number: 1123
title: "[panic] too many cycle iterations"
type: issue
state: closed
author: furiousteabag
labels: []
assignees: []
created_at: 2025-09-03T16:58:32Z
updated_at: 2025-09-03T17:06:21Z
url: https://github.com/astral-sh/ty/issues/1123
synced_at: 2026-01-12T15:54:24Z
```

# [panic] too many cycle iterations

---

_@furiousteabag_

First of all, thanks for creating such a great tool!

I am getting a panic error when running type checking on my project. Specifically, it fails on a file where I create a custom pytest logging message.

Command I run:

```
RUST_BACKTRACE=1 uv run ty check
```

Error I get:

```
uts/salsa-e6f3bb7c2a062968/a3ffa22/src/function/execute.rs:228:25 when checking `<REDACTED_PATH>/tests/conftest.py`: `infer_definition_types(Id(a40a)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: linux x86_64
info: Version: 0.0.1-alpha.20
info: Args: ["ty", "check"]
info: Backtrace:
   0: <unknown>
   1: <unknown>
   2: <unknown>
   3: <unknown>
   4: <unknown>
   5: <unknown>
   6: <unknown>
   7: <unknown>
   8: <unknown>
   9: <unknown>
  10: <unknown>
  11: <unknown>
  12: <unknown>
  13: <unknown>
  14: <unknown>
  15: <unknown>
  16: <unknown>
  17: <unknown>
  18: <unknown>
  19: <unknown>
  20: <unknown>
  21: <unknown>
  22: <unknown>
  23: <unknown>
  24: <unknown>
  25: <unknown>
  26: <unknown>
  27: <unknown>
  28: <unknown>
  29: <unknown>
  30: <unknown>
  31: <unknown>
  32: <unknown>
  33: <unknown>

info: query stacktrace:
   0: infer_scope_types(Id(9800))
             at crates/ty_python_semantic/src/types/infer.rs:145
   1: check_file_impl(Id(c02))
             at crates/ty_project/src/lib.rs:522
```

File that triggers the error:

```python
from collections import defaultdict
from typing import TypedDict, TypeGuard, Union, cast

import pytest


class TestFileStats(TypedDict):
    passed: int
    failed: int
    scenarios: dict[str, str]


# Tree can contain either subdirectories (Dict) or file stats (TestFileStats)
TreeNode = dict[str, Union["TreeNode", TestFileStats]]


def is_file_stats(content: TreeNode | TestFileStats) -> TypeGuard[TestFileStats]:
    """Type guard to check if content is TestFileStats"""
    return isinstance(content, dict) and "passed" in content and "failed" in content


def get_color_for_percentage(pct: float) -> str:
    """Get ANSI color code based on percentage - red to green via orange/yellow"""
    if pct == 0:
        return "\033[91m"  # Bright red
    elif pct <= 10:
        return "\033[31m"  # Red
    elif pct <= 30:
        return "\033[31;1m"  # Dark red/maroon
    elif pct <= 50:
        return "\033[38;5;208m"  # Orange
    elif pct <= 70:
        return "\033[33m"  # Yellow
    elif pct <= 90:
        return "\033[93m"  # Bright yellow/yellow-green
    elif pct < 100:
        return "\033[32m"  # Green
    else:  # 100%
        return "\033[92m"  # Bright green


class TestResultCollector:
    def __init__(self) -> None:
        # Structure: {file_path: {"passed": int, "failed": int, "scenarios": {scenario_name: result}}}
        self.results: defaultdict[str, TestFileStats] = defaultdict(lambda: {"passed": 0, "failed": 0, "scenarios": {}})

    def add_result(self, source_path: str, scenario_name: str, passed: bool) -> None:
        """Add a test result for a specific scenario"""
        self.results[source_path]["scenarios"][scenario_name] = "PASSED" if passed else "FAILED"
        if passed:
            self.results[source_path]["passed"] += 1
        else:
            self.results[source_path]["failed"] += 1

    def get_directory_stats(self, dir_path: str) -> tuple[int, int]:
        """Get aggregated stats for a directory"""
        passed = 0
        failed = 0
        for file_path, stats in self.results.items():
            if file_path.startswith(dir_path):
                passed += stats["passed"]
                failed += stats["failed"]
        return passed, failed

    def build_tree_structure(self) -> TreeNode:
        """Build hierarchical tree structure from flat results"""
        tree: TreeNode = {}

        # Get all unique directory paths
        all_paths = set()
        for file_path in self.results:
            parts = file_path.split("/")
            for i in range(len(parts)):
                all_paths.add("/".join(parts[: i + 1]))

        # Build tree with directories and files
        for path in sorted(all_paths):
            parts = path.split("/")
            current = tree

            for part in parts[:-1]:
                if part not in current:
                    current[part] = {}
                current = current[part]

            final_part = parts[-1]
            if path in self.results:
                # This is a file
                current[final_part] = self.results[path]
            else:
                # This is a directory
                if final_part not in current:
                    current[final_part] = {}

        return tree

    def print_tree(
        self, terminalreporter: pytest.TerminalReporter, tree: TreeNode, prefix: str = "", base_path: str = ""
    ) -> None:
        """Print the hierarchical tree with statistics"""
        items = list(tree.items())
        for i, (name, content) in enumerate(items):
            current_path = f"{base_path}/{name}" if base_path else name
            is_last = i == len(items) - 1
            connector = "└── " if is_last else "├── "

            if is_file_stats(content):
                # This is a file
                total = content["passed"] + content["failed"]
                if total > 0:
                    pass_pct = (content["passed"] / total) * 100
                    color_code = get_color_for_percentage(pass_pct)
                    stats_text = f"({content['passed']}/{total} passed, {pass_pct:.1f}%)"
                    colored_stats = f"{color_code}{stats_text}\033[0m"
                    terminalreporter.write(f"{prefix}{connector}{name}.json ")
                    terminalreporter.write_line(colored_stats)
                else:
                    terminalreporter.write_line(f"{prefix}{connector}{name}.json (0 tests)")
            else:
                # This is a directory - calculate aggregated stats
                passed, failed = self.get_directory_stats(current_path)
                total = passed + failed
                if total > 0:
                    pass_pct = (passed / total) * 100
                    color_code = get_color_for_percentage(pass_pct)
                    stats_text = f"({passed}/{total} passed, {pass_pct:.1f}%)"
                    colored_stats = f"{color_code}{stats_text}\033[0m"
                    terminalreporter.write(f"{prefix}{connector}{name}/ ")
                    terminalreporter.write_line(colored_stats)
                else:
                    terminalreporter.write_line(f"{prefix}{connector}{name}/ (0 tests)")

                # Recursively print subdirectories/files
                if not is_file_stats(content) and content:
                    new_prefix = prefix + ("    " if is_last else "│   ")
                    self.print_tree(terminalreporter, cast(TreeNode, content), new_prefix, current_path)


# Global collector instance
collector = TestResultCollector()


@pytest.hookimpl(tryfirst=True)
def pytest_runtest_logreport(report: pytest.TestReport) -> None:
    """Hook to capture test results"""
    if (
        report.when == "call" and "test_scenario[" in report.nodeid
    ):  # Only capture the actual test call, not setup/teardown
        try:
            # Extract the parametrized part
            param_part = report.nodeid.split("test_scenario[")[1].rstrip("]")

            # Split by " | " to get source and scenario name
            if " | " in param_part:
                source_path, scenario_name = param_part.split(" | ", 1)
                collector.add_result(source_path, scenario_name, report.outcome == "passed")
        except Exception:
            # If parsing fails, skip this result
            pass


def pytest_unconfigure(config: pytest.Config) -> None:
    """Display custom summary at the absolute end"""
    if not collector.results:
        return

    terminalreporter = config.pluginmanager.getplugin("terminalreporter")
    if not terminalreporter:
        return

    # Cast to proper type since we know it's a TerminalReporter
    tr = cast(pytest.TerminalReporter, terminalreporter)

    # Force a newline and separator
    tr.write_line("")
    tr.write_sep("=", "Test Results by Scenario File", bold=True)
    tr.write_line("")

    # Build and print the tree structure
    tree = collector.build_tree_structure()
    collector.print_tree(tr, tree)

    # Print overall summary
    total_passed = sum(stats["passed"] for stats in collector.results.values())
    total_failed = sum(stats["failed"] for stats in collector.results.values())
    total_tests = total_passed + total_failed

    if total_tests > 0:
        overall_pass_pct = (total_passed / total_tests) * 100
        tr.write_line("")
        color_code = get_color_for_percentage(overall_pass_pct)
        overall_stats = f"{total_passed}/{total_tests} passed ({overall_pass_pct:.1f}%)"
        colored_overall = f"{color_code}{overall_stats}\033[0m"
        tr.write("Overall: ")
        tr.write_line(colored_overall)

```

---

_Comment by @AlexWaygood on 2025-09-03 17:06_

Thanks for the clear report! Your `TreeNode` type alias is recursive, so this is a duplicate of #256

---

_Closed by @AlexWaygood on 2025-09-03 17:06_

---
