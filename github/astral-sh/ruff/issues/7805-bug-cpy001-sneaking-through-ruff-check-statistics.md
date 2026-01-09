---
number: 7805
title: "Bug: `CPY001` sneaking through `ruff check --statistics`"
type: issue
state: closed
author: jamesbraza
labels:
  - question
  - needs-info
assignees: []
created_at: 2023-10-04T07:05:41Z
updated_at: 2023-10-04T19:49:25Z
url: https://github.com/astral-sh/ruff/issues/7805
synced_at: 2026-01-07T13:12:15-06:00
---

# Bug: `CPY001` sneaking through `ruff check --statistics`

---

_Issue opened by @jamesbraza on 2023-10-04 07:05_

With `ruff==0.0.292`, check the below Python script designed to make a `select` with no work:

```python
"""Hack script to output ruff codes for select, using Python 3.8+."""

from __future__ import annotations

import collections
import json
import shutil
import string
import subprocess
from collections.abc import Collection
from typing import Any

RUFF_PATH = shutil.which("ruff")


def decompose_code(code: str) -> tuple[str, int]:
    """Convert a code like ABC123 to a tuple of ("ABC", 123)."""
    code_text = code.rstrip(string.digits)
    return code_text, int(code[len(code_text) :])


def group_code_text(
    codes: Collection[str],
    text_using_first_digit: bool = False,
) -> dict[str, list[int]]:
    code_text_to_numbers = collections.defaultdict(list)
    for code in sorted(codes):
        text, number = decompose_code(code)
        if text_using_first_digit:
            text = code[: len(text) + 1]
            number = int(code[len(text) :])
        code_text_to_numbers[text].append(number)
    return code_text_to_numbers


def stringify(
    all_rules: list[dict[str, Any]],
    statistics_rules: list[dict[str, Any]],
    autofix_only: bool = True,
) -> str:
    """Convert `ruff rule` and `ruff check --statistics` outputs to a string."""
    # 1. Compute raw delta
    all_codes = {
        blob["code"]
        for blob in all_rules
    }
    nonpreview_codes = {
        blob["code"]
        for blob in all_rules
        if not blob["preview"]
        if ((autofix_only and "always" in blob["fix"]) or not autofix_only)
    }
    erroring_nonpreview_codes = {
        blob["code"]
        for blob in statistics_rules
        if blob["code"] in nonpreview_codes and blob["count"] > 0
    }
    difference = nonpreview_codes - erroring_nonpreview_codes

    # 2. Group on text
    all_code_text_to_numbers = group_code_text(all_codes)
    all_code_text_to_fill = {
        text: max(*(len(str(n)) for n in nums), 3)
        for text, nums in all_code_text_to_numbers.items()
    }
    nonpreview_text_to_numbers = {
        text: nums
        for text, nums in group_code_text(nonpreview_codes).items()
        if nums == all_code_text_to_numbers[text]
    }
    for text, nums in group_code_text(codes=difference).items():
        if nums != nonpreview_text_to_numbers.get(text, []):
            continue
        fill_size = all_code_text_to_fill[text]
        for num in nums:
            difference.remove(text + str(num).zfill(fill_size))
        difference.add(text)

    # 3. Group on text + first digit
    all_code_text_to_numbers = group_code_text(all_codes, text_using_first_digit=True)
    nonpreview_text_to_numbers = {
        text: nums
        for text, nums in group_code_text(
            nonpreview_codes,
            text_using_first_digit=True,
        ).items()
        if nums == all_code_text_to_numbers[text]
    }
    for text, nums in group_code_text(
        codes={code for code in difference if not code.isalpha()},
        text_using_first_digit=True,
    ).items():
        if nums != nonpreview_text_to_numbers.get(text, []):
            continue
        fill_size = all_code_text_to_fill[text[:-1]] - 1
        for num in nums:
            difference.remove(text + str(num).zfill(fill_size))
        difference.add(text)

    # 4. Stringify
    stringified = '",\n    "'.join(sorted(difference))
    return f"""select = [\n    "{stringified}",\n]"""


def main() -> None:
    all_rules = json.loads(
        subprocess.check_output((RUFF_PATH, "rule", "--format=json", "--all")).decode(),
    )
    subprocess.check_call((RUFF_PATH, "check", "--select=ALL", "--fix-only", "."))
    out = subprocess.run(
        (
            RUFF_PATH,
            "check",
            "--select=ALL",
            "--statistics",
            "--output-format=json",
            ".",
        ),
        capture_output=True,
    )
    if out.stderr or out.returncode != 1:
        raise NotImplementedError(
            f"Unhandled return code {out.returncode} or stderr {out.stderr.decode()!r}."
        )
    print(stringify(all_rules, statistics_rules=json.loads(out.stdout.decode())))


if __name__ == "__main__":
    main()
```

However,`CPY001` (and multiple others) are somehow sneaking through the `ruff check --statistics`, because they're not showing up as having errors (e.g. it's neglected in the output).  Any ideas why?

---

_Comment by @charliermarsh on 2023-10-04 13:25_

I think you need to add `--preview` to the `ruff check` call?

---

_Label `question` added by @charliermarsh on 2023-10-04 14:37_

---

_Label `waiting-on-author` added by @charliermarsh on 2023-10-04 14:37_

---

_Comment by @jamesbraza on 2023-10-04 17:04_

This was somehow fixed by https://github.com/astral-sh/ruff/pull/7812, so going to close this out

---

_Closed by @jamesbraza on 2023-10-04 17:04_

---

_Referenced in [run-llama/llama_index#7970](../../run-llama/llama_index/pulls/7970.md) on 2023-10-04 19:12_

---
