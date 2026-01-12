```yaml
number: 2968
title: Regenerate expected Black snapshots
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/snapshots
created_at: 2023-02-16T19:36:01Z
updated_at: 2023-02-16T19:39:19Z
url: https://github.com/astral-sh/ruff/pull/2968
synced_at: 2026-01-12T04:39:44Z
```

# Regenerate expected Black snapshots

---

_Pull request opened by @charliermarsh on 2023-02-16 19:36_

The script I used to generate these wasn't idempotent, and I must've run it twice over the "unused" tests -- so most of them contained the wrong expected result, which quickly became clear as I tried to enable them.

This PR just re-runs the generation process, which (for posterity) leverages this script:

```py
import argparse
from pathlib import Path

EMPTY_LINE = "# EMPTY LINE WITH WHITESPACE (this comment will be removed)"


def read_data_from_file(file_name: Path) -> tuple[str, str]:
    with open(file_name, "r", encoding="utf8") as test:
        lines = test.readlines()
    _input: list[str] = []
    _output: list[str] = []
    result = _input
    for line in lines:
        line = line.replace(EMPTY_LINE, "")
        if line.rstrip() == "# output":
            result = _output
            continue

        result.append(line)
    if _input and not _output:
        # If there's no output marker, treat the entire file as already pre-formatted.
        _output = _input[:]
    return "".join(_input).strip() + "\n", "".join(_output).strip() + "\n"


def main() -> None:
    parser = argparse.ArgumentParser()
    parser.add_argument("file", type=Path)
    args = parser.parse_args()

    file_name: Path = args.file
    (input_content, output_content) = read_data_from_file(file_name)

    with open(file_name, "w+", encoding="utf8") as fp:
        fp.write(input_content)

    relative_name = str(
        file_name.relative_to("crates/ruff_python_formatter/resources/test/fixtures/black")
    )
    suffix = relative_name.replace("/", "__")
    snapshot_path = (
        Path("crates/ruff_python_formatter/src/snapshots/expect") / f"ruff_python_formatter__tests__{suffix}.snap.expect"
    )
    with open(snapshot_path, "w+", encoding="utf8") as fp:
        fp.write(
            f"""---
source: src/source_code/mod.rs
assertion_line: 0
expression: formatted
---
{output_content}
"""
        )


if __name__ == "__main__":
    main()
```

---

_Label `autoformatter` added by @charliermarsh on 2023-02-16 19:36_

---

_Merged by @charliermarsh on 2023-02-16 19:39_

---

_Closed by @charliermarsh on 2023-02-16 19:39_

---

_Branch deleted on 2023-02-16 19:39_

---
