```yaml
number: 22231
title: "[ty] Add a conformance script to compare ty diagnostics with expected errors"
type: pull_request
state: open
author: WillDuke
labels:
  - ci
  - ty
assignees: []
base: main
head: wld/improve-conformance-suite
created_at: 2025-12-28T01:15:26Z
updated_at: 2026-01-13T11:29:01Z
url: https://github.com/astral-sh/ruff/pull/22231
synced_at: 2026-01-13T12:25:13Z
```

# [ty] Add a conformance script to compare ty diagnostics with expected errors

---

_@WillDuke_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

I had a go at improving the typing conformance workflow. I compared the path and beginning line from `ty`'s `gitlab` output to the path and line numbers matching "# E" in the conformance suite. Based on the presence of a corresponding error in the conformance tests, I broke out the diagnostics on the current branch into groups (true positives, false positives, etc.) that changed compared to the merge base. I've used Github's diff syntax to make new positives appear in green and new negatives appear in red (displaying the old diagnostic). I've also added a summary with counts for each group and a sentence indicating whether performance improved overall.

Here's a standalone version of the script comparing `0.0.1a35` to `0.0.7`.

<details>
  <summary>conformance.sh</summary>

```console

#!/bin/bash

git clone git@github.com:python/typing.git || true

uvx 'ty@@0.0.1a35' check \
  typing/conformance/tests \
  --output-format gitlab \
  | jq '.[] |
    .key = .location?.path? + ":" + (.location?.positions?.begin?.line? | tostring) |
    .source = "old"
  ' \
  > old.jsonl

uvx 'ty@0.0.7' check \
  typing/conformance/tests \
  --output-format gitlab \
  | jq '.[] |
    .key = .location?.path? + ":" + (.location?.positions?.begin?.line? | tostring) |
    .source = "new"
  ' \
  > new.jsonl

rg '# E\:?' typing/conformance/tests \
  --json \
  | jq '
    select(.type == "match") |
    {
      key: (.data?.path?.text? + ":" + (.data?.line_number? | tostring)),
      source: "expected"
    }' \
  > expected.jsonl

jq -s -r '
  def xor($a; $b): ($a or $b) and (($a and $b) | not);
  def has($a; $b): $a | index($b) != null;
  def missing($a; $b): $a | index($b) == null;
  def severity_to_level:
    {
      "major": "error",
      "minor": "warning",
      "info": "note"
    }[.] // "error";
    def to_concise($d): "\($d.location.path | gsub("^.*typing"; "typing")):" +
      "\($d.location.positions.begin.line):\($d.location.positions.begin.column): " +
      "\( $d.severity | severity_to_level)[\($d.check_name)]" +
      "\($d.description | gsub("^[a-z-]*:"; ""))";
  def ascii_titlecase:
      split(" ") | map((.[0:1] | ascii_upcase) + .[1:]) | join(" ");
  def sign($s):
    if $s | contains("positive") then "+ " else "- " end;
  group_by(.key)
  | map({
      key: .[0].key,
      sources: (map(.source) | unique),
      changed: xor(
        has(map(.source); "new");
        has(map(.source); "old")
      ),
      classification: (
        if has(map(.source); "new") and has(map(.source); "expected") then "true_positive"
        elif has(map(.source); "new") and missing(map(.source); "expected") then "false_positive"
        elif missing(map(.source); "new") and has(map(.source); "expected") then "false_negative"
        elif missing(map(.source); "new") and missing(map(.source); "expected") then "true_negative"
        else "unknown"
        end
      ),
      old: map(select(.source == "old")) | first,
      new: map(select(.source == "new")) | first,
      expected: map(select(.source == "expected")) | first
    })
  | map(select(.changed))
  | map({
      classification: .classification?,
      concise: (
        if .classification == "true_positive" then to_concise(.new)
        elif .classification == "false_positive" then to_concise(.new)
        elif .classification == "false_negative" then to_concise(.old)
        elif .classification == "true_negative" then to_concise(.old)
        else "unknown"
        end
      )
    })
  | group_by(.classification)
  | .[]
  | "### \(. [0].classification | gsub("_"; " ") | ascii_titlecase)s\n"
    + "\n```diff\n"
    + (map(sign(.classification) + .concise) | join("\n"))
    + "\n```\n"
' old.jsonl \
  new.jsonl \
  expected.jsonl \
  > changed.md

jq -s -r '
  def has($a; $b): $a | index($b) != null;
  def stats(fp_source):
    map(select(has(.source; fp_source) or has(.source; "expected")))
    | group_by(.key)
    | map(map(.source) | unique)
    | reduce .[] as $s (
        { tp: 0, fp: 0, fn: 0 };
        if      $s == [fp_source]  then .fp += 1
        elif    $s == ["expected"] then .fn += 1
        elif    ($s | length == 2) then .tp += 1
        else .
        end
      )
    | .precision = (if (.tp + .fp) > 0 then .tp / (.tp + .fp) else 1 end)
    | .recall    = (if (.tp + .fn) > 0 then .tp / (.tp + .fn) else 1 end);
  def sign(x):
    if x > 0 then "+"
    elif x < 0 then "-"
    else "±0"
    end;
  def trend(x):
    if x > 0 then "improves"
    elif x < 0 then "regresses"
    else "does not change"
    end;
  def pct(x):
    (x * 100 | tostring | .[0:5]) + "%";
  def summary:
    [
      "## Typing Conformance",
      "\n",
      "### Summary",
      "\n",
      "| Metric     | Old | New | Δ |",
      "|------------|-----|-----|---|",
      "| True Positives | \(.old.tp) | \(.new.tp) | \(.new.tp - .old.tp) |",
      "| False Positives | \(.old.fp) | \(.new.fp) | \(.new.fp - .old.fp) |",
      "| False Negatives | \(.old.fn) | \(.new.fn) | \(.new.fn - .old.fn) |",
      "| Precision  | \(pct(.old.precision)) | \(pct(.new.precision)) | \(sign(.comparison.precision_delta))\(pct(.comparison.precision_delta)) |",
      "| Recall     | \(pct(.old.recall)) | \(pct(.new.recall)) | \(sign(.comparison.recall_delta))\(pct(.comparison.recall_delta)) |",
      "",
      (
        "Compared to the current merge base, this PR "
        + trend(.comparison.precision_delta)
        + " precision and "
        + trend(.comparison.recall_delta)
        + " recall "
        + "(TP: \(.new.tp - .old.tp), "
        + "FP: \(.new.fp - .old.fp), "
        + "FN: \(.new.fn - .old.fn))."
      )
    ]
    | join("\n");
  {
    new: stats("new"),
    old: stats("old")
  }
  | .comparison = {
      precision_delta: (.new.precision - .old.precision),
      recall_delta:    (.new.recall    - .old.recall)
    }
  | summary
' \
new.jsonl \
old.jsonl \
expected.jsonl \
> stats.md

{
  cat stats.md
  echo
  cat changed.md
} > comment.md


```
</details>

Which produces the following markdown output:

<details>
  <summary>comment.md</summary>

## Typing Conformance


### Summary


| Metric     | Old | New | Δ |
|------------|-----|-----|---|
| True Positives | 647 | 654 | 7 |
| False Positives | 227 | 227 | 0 |
| False Negatives | 539 | 532 | -7 |
| Precision  | 74.02% | 74.23% | +0.206% |
| Recall     | 54.55% | 55.14% | +0.590% |

Compared to the current merge base, this PR improves precision and improves recall (TP: 7, FP: 0, FN: -7).

### False Positives

```diff
+ typing/conformance/tests/constructors_callable.py:143:27: error[invalid-argument-type] Argument to function `accepts_callable` is incorrect: Expected `() -> Any | Class6Any`, found `<class 'Class6Any'>`
+ typing/conformance/tests/constructors_callable.py:145:1: error[type-assertion-failure] Type `Any` does not match asserted type `Any | Class6Any`
+ typing/conformance/tests/dataclasses_transform_converter.py:102:42: error[invalid-argument-type] Argument to function `model_field` is incorrect: Expected `(str | bytes, /) -> ConverterClass`, found `<class 'ConverterClass'>`
+ typing/conformance/tests/dataclasses_transform_converter.py:103:31: error[invalid-argument-type] Argument to function `model_field` is incorrect: Expected `(str | list[str], /) -> int`, found `Overload[(s: str) -> int, (s: list[str]) -> int]`
+ typing/conformance/tests/dataclasses_transform_converter.py:104:30: error[invalid-assignment] Object of type `dict[str, str] | dict[bytes, bytes]` is not assignable to `dict[str, str]`
```

### True Negatives

```diff
- typing/conformance/tests/constructors_callable.py:128:1: error[type-assertion-failure] Argument does not have asserted type `Class6Proxy`
- typing/conformance/tests/constructors_callable.py:37:1: error[type-assertion-failure] Argument does not have asserted type `Class1`
- typing/conformance/tests/constructors_callable.py:50:1: error[type-assertion-failure] Argument does not have asserted type `Class2`
- typing/conformance/tests/constructors_callable.py:65:1: error[type-assertion-failure] Argument does not have asserted type `Class3`
- typing/conformance/tests/constructors_callable.py:80:1: error[type-assertion-failure] Argument does not have asserted type `int`
```

### True Positives

```diff
+ typing/conformance/tests/constructors_callable.py:146:8: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
+ typing/conformance/tests/constructors_callable.py:186:4: error[invalid-argument-type] Argument is incorrect: Expected `list[T@Class8]`, found `list[Unknown | int]`
+ typing/conformance/tests/dataclasses_transform_converter.py:133:31: error[invalid-argument-type] Argument to function `model_field` is incorrect: Expected `(str | int, /) -> int`, found `def converter_simple(s: str) -> int`
+ typing/conformance/tests/dataclasses_usage.py:88:14: error[invalid-assignment] Object of type `dataclasses.Field[str | int]` is not assignable to `int`
+ typing/conformance/tests/namedtuples_usage.py:43:5: error[non-subscriptable] Cannot delete subscript on object of type `Point` with no `__delitem__` method
+ typing/conformance/tests/typeddicts_extra_items.py:128:15: error[invalid-argument-type] Cannot delete required key "name" from TypedDict `MovieEI`
+ typing/conformance/tests/typeddicts_operations.py:49:11: error[invalid-argument-type] Cannot delete required key "name" from TypedDict `Movie`
```

</details>

Partially addresses [#2205](https://github.com/astral-sh/ty/issues/2205).



## Test Plan

<!-- How was it tested? -->

I have only tested the standalone version of this. I think I'd need some guidance on how to test the action if you think this is worth merging. For example, I'm just piping the markdown content into the original diff file.

-------

Edit: I've replaced the inline `jq` script with a python version at `scripts/conformance.py`. Invoking that script with e.g. 

```
uv run scripts/conformance.py --target ../typing/conformance/tests/
```

produces the same output assuming the `typing` repository has been cloned into the same parent as `ruff`. I also noticed in the interim that some errors in the conformance suite are tagged as accepted in one or more of a set of lines or are marked as optional, which I haven't yet implemented here. Instead, every line with a "# E" is currently matched to a single diagnostic on the same line in the old and new `ty` versions.

---

_Label `ci` added by @AlexWaygood on 2025-12-28 07:49_

---

_Label `ty` added by @AlexWaygood on 2025-12-28 07:49_

---

_Review comment by @MichaReiser on `.github/workflows/typing_conformance.yaml`:194 on 2025-12-29 10:13_

This is very impressive that you were able to do all of this with jq. 

I don't know jq well and would find it difficult to make changes to this script in the future. Have you considered writing this as a Python script instead?

---

_@MichaReiser reviewed on 2025-12-29 10:16_

This is great! 

We only recently talked about that having something like this would make life so much easier. Thanks for working on this

> . I think I'd need some guidance on how to test the action if you think this is worth merging. For example, I'm just piping the markdown content into the original diff file.


I think that should just work. We might need to limit the diff's length but I can look into this. 



---

_@WillDuke reviewed on 2025-12-29 14:10_

---

_Review comment by @WillDuke on `.github/workflows/typing_conformance.yaml`:194 on 2025-12-29 14:10_

Sure, this started partially as an exercise to learn jq that ended up taking more code than I anticipated. 😂 I can work on porting it to Python.

---

_@AlexWaygood reviewed on 2026-01-09 17:31_

---

_Review comment by @AlexWaygood on `.github/workflows/typing_conformance.yaml`:194 on 2026-01-09 17:31_

I'd also find a Python script much more maintainable! But thanks so much for taking this on, this is awesome!!

---

_@WillDuke reviewed on 2026-01-09 17:58_

---

_Review comment by @WillDuke on `.github/workflows/typing_conformance.yaml`:194 on 2026-01-09 17:58_

Thanks, I've ported it over! I should find time to update this PR this weekend

---

_Comment by @astral-sh-bot[bot] on 2026-01-11 16:09_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_Review requested from @MichaReiser by @WillDuke on 2026-01-11 16:37_

---

_Review requested from @AlexWaygood by @WillDuke on 2026-01-11 16:37_

---

_Comment by @AlexWaygood on 2026-01-11 17:04_

Sorry about the ty false positive on your Python script! That's the "support `enum.Flag`" bullet point in https://github.com/astral-sh/ty/issues/876. Feel free to just add a `ty: ignore` comment to that line for now.

I'm guessing the conformance workflow is failing because it tries to run the script on `main` in order to compare the results on `main` with the results on this PR, but the script doesn't exist on `main` yet?

---

_Comment by @WillDuke on 2026-01-11 17:23_

> Sorry about the ty false positive on your Python script! That's the "support `enum.Flag`" bullet point in [astral-sh/ty#876](https://github.com/astral-sh/ty/issues/876). Feel free to just add a `ty: ignore` comment to that line for now.
> 
> I'm guessing the conformance workflow is failing because it tries to run the script on `main` in order to compare the results on `main` with the results on this PR, but the script doesn't exist on `main` yet?

Ah, I missed the `ty` failure! Yes, that's exactly the issue. Should I split this into two PRs?

---

_Comment by @AlexWaygood on 2026-01-11 17:33_

Yeah, splitting it into two PRs would be great! We could have one PR to add the script and then another to actually run the script as part of a GitHub workflow.

---

_Renamed from "[ty] Update conformance workflow to compare ty diagnostics with expected errors " to "[ty] Add a conformance script to compare ty diagnostics with expected errors" by @WillDuke on 2026-01-11 17:58_

---

_Comment by @WillDuke on 2026-01-11 17:59_

> Yeah, splitting it into two PRs would be great! We could have one PR to add the script and then another to actually run the script as part of a GitHub workflow.

Sure thing. I've updated this PR to include only the script change, and I've opened #22504 to make the workflow change.

---

_Review comment by @AlexWaygood on `scripts/conformance.py`:22 on 2026-01-11 18:15_

Could you add a module docstring to the top here that describes what this script does and (briefly) how it works?

---

_Review comment by @AlexWaygood on `scripts/conformance.py`:4 on 2026-01-11 18:15_

hmm, this isn't an alias I've seen before... I think I'd prefer either `import itertools` or `from itertools import groupby` :-)

---

_Review comment by @AlexWaygood on `scripts/conformance.py`:17 on 2026-01-11 18:17_

Could you add a comment that documents what this regex intends to capture? You could consider using the `re.VERBOSE` flag: https://docs.python.org/3/library/re.html#re.VERBOSE

---

_Review comment by @AlexWaygood on `scripts/conformance.py`:33 on 2026-01-11 18:17_

```suggestion
@dataclass(kw_only=True, slots=True)
```

---

_Review comment by @AlexWaygood on `scripts/conformance.py`:39 on 2026-01-11 18:17_

```suggestion
@dataclass(kw_only=True, slots=True)
```

---

_Review comment by @AlexWaygood on `scripts/conformance.py`:45 on 2026-01-11 18:17_

```suggestion
@dataclass(kw_only=True, slots=True)
```

---

_Review comment by @AlexWaygood on `scripts/conformance.py`:51 on 2026-01-11 18:18_

```suggestion
@dataclass(kw_only=True, slots=True)
```

---

_Review comment by @AlexWaygood on `scripts/conformance.py`:88 on 2026-01-11 18:18_

```suggestion
    def key(self) -> str:
```

---

_Review comment by @AlexWaygood on `scripts/conformance.py`:104 on 2026-01-11 18:19_

Could this be a custom `__str__` method?

---

_Review comment by @AlexWaygood on `scripts/conformance.py`:107 on 2026-01-11 18:19_

```suggestion
@dataclass(kw_only=True, slots=True)
```

---

_Review comment by @AlexWaygood on `scripts/conformance.py`:149 on 2026-01-11 18:25_

```suggestion
@dataclass(kw_only=True, slots=True)
```

---

_Review comment by @AlexWaygood on `scripts/conformance.py`:153 on 2026-01-11 18:26_

Apologies, I don't have a maths background -- what do these stand for? Could we give them more descriptive names for folks who don't know their statistics jargon that well?

---

_Review comment by @AlexWaygood on `scripts/conformance.py`:271 on 2026-01-11 18:28_

```suggestion
        num_errors = sum(
            1 for g in grouped_diagnostics if source.EXPECTED in g.sources  # ty:ignore[unsupported-operator]
        )
```

also, could you possibly add a comment that says we're ignoring the error because it's a false positive, and links to the ty issue?

---

_Review comment by @AlexWaygood on `scripts/conformance.py`:374 on 2026-01-11 18:34_

A trick I often like to pull is to put this kind of usage information in the module docstring at the top of the file, and then put `description=__doc__` in the ArgumentParser()` call, so that you don't have to write out the documentation more than once but it's still in both logical places a user/reader of the script might look for it:

```diff
diff --git a/scripts/conformance.py b/scripts/conformance.py
index 3771ae2928..992cc76440 100644
--- a/scripts/conformance.py
+++ b/scripts/conformance.py
@@ -1,3 +1,20 @@
+"""
+Run typing conformance tests and compare results between two ty versions.
+
+Examples:
+    # Compare two specific ty versions
+    %(prog)s --old-ty uvx ty@0.0.1a35 --new-ty uvx ty@0.0.7
+
+    # Use local ty builds
+    %(prog)s --old-ty ./target/debug/ty-old --new-ty ./target/debug/ty-new
+
+    # Custom test directory
+    %(prog)s --target-path custom/tests --old-ty uvx ty@0.0.1a35 --new-ty uvx ty@0.0.7
+
+    # Show all diagnostics (not just changed ones)
+    %(prog)s --all --old-ty uvx ty@0.0.1a35 --new-ty uvx ty@0.0.7
+"""
+
 from __future__ import annotations
 
 import argparse
@@ -355,22 +372,8 @@ def render_summary(grouped_diagnostics: list[GroupedDiagnostics]):
 
 def parse_args():
     parser = argparse.ArgumentParser(
-        description="Run typing conformance tests and compare results between two ty versions",
+        description=__doc__,
         formatter_class=argparse.RawDescriptionHelpFormatter,
-        epilog=dedent("""
-            Examples:
-              # Compare two specific ty versions
-              %(prog)s --old-ty uvx ty@0.0.1a35 --new-ty uvx ty@0.0.7
-
-              # Use local ty builds
-              %(prog)s --old-ty ./target/debug/ty-old --new-ty ./target/debug/ty-new
-
-              # Custom test directory
-              %(prog)s --target-path custom/tests --old-ty uvx ty@0.0.1a35 --new-ty uvx ty@0.0.7
-
-              # Show all diagnostics (not just changed ones)
-              %(prog)s --all --old-ty uvx ty@0.0.1a35 --new-ty uvx ty@0.0.7
-        """),
     )
 
     parser.add_argument(
```

---

_@AlexWaygood reviewed on 2026-01-11 18:34_

Nice! A few nitpicks:

---

_Assigned to @AlexWaygood by @AlexWaygood on 2026-01-11 18:34_

---

_@WillDuke reviewed on 2026-01-11 18:36_

---

_Review comment by @WillDuke on `scripts/conformance.py`:153 on 2026-01-11 18:36_

No problem. These just stand for tp -> true positive, fp -> false positive, fn -> false negative. I can update this to spell them out.

---

_@WillDuke reviewed on 2026-01-11 21:46_

---

_Review comment by @WillDuke on `scripts/conformance.py`:374 on 2026-01-11 21:46_

That's a great tip, thanks! 

---

_Review comment by @AlexWaygood on `scripts/conformance.py`:448 on 2026-01-12 14:58_

What do the terms "precision" and "recall" mean in this context? Also, I'd again prefer it if we spelled out "False positives", "Ture positives" and "False negatives" here

---

_Review comment by @AlexWaygood on `scripts/conformance.py`:410 on 2026-01-12 15:00_

We should state clearly in the module docstring that the script assumes you have uv installed (nearly all contributors to Ruff/ty will have it installed, of course, but it's still worth stating explicitly)

---

_Review comment by @AlexWaygood on `scripts/conformance.py`:422 on 2026-01-12 15:01_

Maybe `--conformance-tests-path`? We could also make it so that you can specify the argument with `-c` if that's getting on the wordy side

---

_Review comment by @AlexWaygood on `scripts/conformance.py`:381 on 2026-01-12 15:02_

```suggestion
        | Metric     | Old | New | Delta |
```

---

_Review comment by @AlexWaygood on `scripts/conformance.py`:370 on 2026-01-12 15:19_

I ran this script locally to compare the results on https://github.com/astral-sh/ruff/pull/22317 with the results on `main`, and found the "True negatives" heading a bit confusing here:

<details>
<summary>Screenshot</summary>

<img width="2014" height="1446" alt="image" src="https://github.com/user-attachments/assets/08cec938-6dd8-4a48-931a-8abf0aa7f7a9" />

</details>

I think a "new true negative" is the same as a "removed false positive"? What if we had three sections with these titles (I'm not a huge fan of emojis in general but here I think they'd help make it clear whether each section indicates good news or bad news):
- "True positives added 🎉" 
- "False positives removed 🎉"
- "False positives added 🫤"

---

_@AlexWaygood reviewed on 2026-01-12 15:21_

This is great!!

I think for use in CI, another great feature would be if the in-depth sections below the summary table could link directly to each line in the upstream repo where a diagnostic was added or removed. That would make it very easy for us to debug exactly why a diagnostic was being added or removed as the result of a PR. But that could always be done as a followup (either by you or me!)

---

_@WillDuke reviewed on 2026-01-12 16:47_

---

_Review comment by @WillDuke on `scripts/conformance.py`:448 on 2026-01-12 16:47_

More jargon! Precision is the fraction of diagnostics which are true errors (the number of true positives divided by the total number of diagnostics), while recall is the fraction of true errors for which ty raises a diagnostic. My goal was to give you a sense of how accurate ty is when it raises a diagnostic (precision) and how comprehensive its ability to detect errors is (recall).

I can certainly remove the abbreviations. 

---

_@WillDuke reviewed on 2026-01-12 16:49_

---

_Review comment by @WillDuke on `scripts/conformance.py`:448 on 2026-01-12 16:49_

If you'd like to keep these stats, I could include a sentence explaining what they mean in the output. 

---

_@AlexWaygood reviewed on 2026-01-12 17:00_

---

_Review comment by @AlexWaygood on `scripts/conformance.py`:448 on 2026-01-12 17:00_

Yeah, these seem like useful statistics for sure! But rather than including them in the table, I think I'd prefer it if the script wrote out a descriptive sentence below the table: "The percentage of diagnostic emitted that were true positives increased from 49% to 51%", "The percentage of diagnostics emitted that were true positives held steady at 63%" or "The percentage of diagnostics emitted that were true positives fell from 78% to 74%", etc.

---

_Review comment by @MichaReiser on `scripts/conformance.py`:410 on 2026-01-12 17:12_

The default here feels a bit arbitrary. I'm leaning towards removing it

---

_Review comment by @MichaReiser on `scripts/conformance.py`:417 on 2026-01-12 17:12_

I think defaulting to the latest version is a more sensible default than some arbitrary old version

```suggestion
        default=["uvx", "ty@latest"],
```

---

_Review comment by @MichaReiser on `scripts/conformance.py`:422 on 2026-01-12 17:13_

Maybe just `--tests-path`?

---

_Review comment by @MichaReiser on `scripts/conformance.py`:448 on 2026-01-12 17:14_

I find sentences much harder to parse. It's what I like about codspeed. I can just look at the last column to see the result. 

Adding some documentation at the end of the output makes sense to me

---

_@MichaReiser approved on 2026-01-12 17:17_

This is great. Thank you

---

_@AlexWaygood reviewed on 2026-01-12 17:39_

---

_Review comment by @AlexWaygood on `scripts/conformance.py`:448 on 2026-01-12 17:39_

I'm fine with a jargony table and some documentation below it that explains the jargon :-)

though I wonder if we could add a visual indicator somehow that makes it clear whether a number in the table going up or down is a good or bad thing without having to read a separate documentation paragraph somewhere else. Maybe something similar to the emoji suggestion I had in https://github.com/astral-sh/ruff/pull/22231#discussion_r2682732308?

---

_@AlexWaygood reviewed on 2026-01-12 17:40_

---

_Review comment by @AlexWaygood on `scripts/conformance.py`:448 on 2026-01-12 17:40_

> I'm fine with a jargony table and some documentation below it that explains the jargon :-)

we can always do this for now and iterate on it

---

_@MichaReiser reviewed on 2026-01-12 18:04_

---

_Review comment by @MichaReiser on `scripts/conformance.py`:448 on 2026-01-12 18:04_

This is what biomejs does https://github.com/biomejs/biome/pull/8361#issuecomment-3613423566

and it does add nice emoji's for every row based on the change https://github.com/biomejs/biome/blob/f2445269b6a7bff5d81ee08a6a321bc229b61b50/xtask/coverage/src/results.rs#L37-L72

---

_@WillDuke reviewed on 2026-01-12 20:03_

---

_Review comment by @WillDuke on `scripts/conformance.py`:370 on 2026-01-12 20:03_

Along with those changes, I've also added a fourth category for new false negatives which are now rendered as "True positives removed 🫤"

---

_@WillDuke reviewed on 2026-01-12 21:09_

---

_Review comment by @WillDuke on `scripts/conformance.py`:448 on 2026-01-12 21:09_

I've added an outcome column with the emojis and updated the summary sentence. Happy to iterate on it further.

---

_Review requested from @AlexWaygood by @MichaReiser on 2026-01-13 08:31_

---

_Review comment by @WillDuke on `scripts/conformance.py`:340 on 2026-01-13 09:29_

Leaving a note to myself that I need to check if expected is present here

---

_Review comment by @WillDuke on `scripts/conformance.py`:391 on 2026-01-13 09:30_

Should be {bad}{up}

---

_Review comment by @WillDuke on `scripts/conformance.py`:395 on 2026-01-13 09:30_

Should be {good}{down} 

---

_@WillDuke reviewed on 2026-01-13 09:31_

---

_Review comment by @AlexWaygood on `scripts/conformance.py`:380 on 2026-01-13 11:11_

```suggestion
def diff_format(
    diff: float,
    *,
    greater_is_better: bool = True,
    neutral: bool = False,
    is_percentage: bool = False,
):
```

---

_Review comment by @AlexWaygood on `scripts/conformance.py`:438 on 2026-01-13 11:13_

Is it good or bad if `Total` goes up? If neither, is it useful to have it at all?

I find it a bit confusing currently, because a `Total` row at the bottom usually sums all of the above rows together if it's e.g. an excel spreadsheet. But here, that's not what it does (`(-1) + (-9) + 10 = 0`):

<img width="1328" height="786" alt="image" src="https://github.com/user-attachments/assets/7ab139cb-1b3a-46a7-a401-0db1fba6be1c" />


---

_Review comment by @AlexWaygood on `scripts/conformance.py`:446 on 2026-01-13 11:14_

```suggestion
        " and the percentage of expected errors that received a diagnostic"
```

---

_Review comment by @AlexWaygood on `scripts/conformance.py`:438 on 2026-01-13 11:22_

I think I'd also find the table easier to read if the up/down arrow came first -- maybe something like this?

<img width="1272" height="936" alt="image" src="https://github.com/user-attachments/assets/597def52-ab64-4da6-bbc6-9b4441eee14b" />

---

_Review comment by @AlexWaygood on `scripts/conformance.py`:329 on 2026-01-13 11:22_

nit

```suggestion
        num_errors = sum(
            1 for g in grouped_diagnostics if source.EXPECTED in g.sources  # ty:ignore[unsupported-operator]
        )
```

---

_@AlexWaygood reviewed on 2026-01-13 11:22_

Love it

---

_@MichaReiser reviewed on 2026-01-13 11:29_

---

_Review comment by @MichaReiser on `scripts/conformance.py`:438 on 2026-01-13 11:29_

We could move `Precision` and `Recall` after the total row.


I also suggest always using a sign in the `Diff` column (+10) as it might otherwise be unclear what 0.74 means if there's no negative diff.

---
