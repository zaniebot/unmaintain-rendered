```yaml
number: 11186
title: "Various small improvements to the `fuzz-parser` script"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: fuzz-build-failure
created_at: 2024-04-28T17:58:50Z
updated_at: 2024-04-28T18:17:44Z
url: https://github.com/astral-sh/ruff/pull/11186
synced_at: 2026-01-12T15:55:36Z
```

# Various small improvements to the `fuzz-parser` script

---

_@AlexWaygood_

## Summary

1. Bump dependency pins in `scripts/fuzz-parser/requirements.txt`. The latest versions of `pysource-codegen` and `pysource-minimize` have some typing improvements that mean that pyright and mypy now both give the `scripts/fuzz-parser` directory a clean bill of health.
2. If `cargo build --release` fails, print the captured stderr to the terminal so it's easy to diagnose what's gone wrong.
3. Add instructions to the module docstring (also displayed if you run `py scripts/fuzz-parser/fuzz.py --help` in a terminal window) for how to run the script with randomly selected seeds.
4. Alter `FuzzResult.print_description()` so that it prints how far through the overall fuzzing the script is. This is especially useful if you've fed randomly selected seeds to the script, as it's hard to know what the distribution of the seeds will be if they've been randomly selected.
5. Colourise `--help` output with `rich-argparse` (why not)

## Test Plan

I ran the following commands to test the script:

```
$ py scripts/fuzz-parser/fuzz.py --help
$ py scripts/fuzz-parser/fuzz.py 0-100
$ py scripts/fuzz-parser/fuzz.py $(shuf -i 0-1000000 -n 10000)
```

I also applied this diff to the parser and then ran the script again to check the output if the script found parser bugs:

<details>

```diff
--- a/crates/ruff_python_parser/src/parser/expression.rs
+++ b/crates/ruff_python_parser/src/parser/expression.rs
@@ -308,15 +308,13 @@ impl<'src> Parser<'src> {
         if let Some(unary_op) = token.as_unary_operator() {
             let expr = self.parse_unary_expression(unary_op, context);
 
-            if matches!(unary_op, UnaryOp::Not) {
-                if left_precedence > OperatorPrecedence::Not {
-                    self.add_error(
-                        ParseErrorType::OtherError(
-                            "Boolean 'not' expression cannot be used here".to_string(),
-                        ),
-                        &expr,
-                    );
-                }
+            if left_precedence > OperatorPrecedence::Not {
+                self.add_error(
+                    ParseErrorType::OtherError(
+                        "Boolean 'not' expression cannot be used here".to_string(),
+                    ),
+                    &expr,
+                );
             } else {
                 if left_precedence > OperatorPrecedence::PosNegBitNot
```

</details>

## New output

### If it passes:

<details>

```
(ruff) (fuzz-build-failure) % py scripts/fuzz-parser/fuzz.py 100-200                                                                      ~/dev/ruff
Running `cargo build --release` since no test executable was specified...
Concurrently running the fuzzer on 101 randomly generated source-code files...
Ran fuzzer successfully on seed 101                    [1/101]
Ran fuzzer successfully on seed 107                    [2/101]
Ran fuzzer successfully on seed 111                    [3/101]
Ran fuzzer successfully on seed 104                    [4/101]
Ran fuzzer successfully on seed 106                    [5/101]
Ran fuzzer successfully on seed 113                    [6/101]
Ran fuzzer successfully on seed 108                    [7/101]
Ran fuzzer successfully on seed 109                    [8/101]
Ran fuzzer successfully on seed 105                    [9/101]
Ran fuzzer successfully on seed 103                   [10/101]
Ran fuzzer successfully on seed 102                   [11/101]
Ran fuzzer successfully on seed 100                   [12/101]
Ran fuzzer successfully on seed 112                   [13/101]
Ran fuzzer successfully on seed 110                   [14/101]
Ran fuzzer successfully on seed 119                   [15/101]
Ran fuzzer successfully on seed 116                   [16/101]
Ran fuzzer successfully on seed 114                   [17/101]
Ran fuzzer successfully on seed 117                   [18/101]
Ran fuzzer successfully on seed 121                   [19/101]
Ran fuzzer successfully on seed 118                   [20/101]
Ran fuzzer successfully on seed 120                   [21/101]
Ran fuzzer successfully on seed 126                   [22/101]
Ran fuzzer successfully on seed 124                   [23/101]
Ran fuzzer successfully on seed 122                   [24/101]
Ran fuzzer successfully on seed 115                   [25/101]
Ran fuzzer successfully on seed 129                   [26/101]
Ran fuzzer successfully on seed 127                   [27/101]
Ran fuzzer successfully on seed 125                   [28/101]
Ran fuzzer successfully on seed 135                   [29/101]
Ran fuzzer successfully on seed 133                   [30/101]
Ran fuzzer successfully on seed 123                   [31/101]
Ran fuzzer successfully on seed 131                   [32/101]
Ran fuzzer successfully on seed 128                   [33/101]
```

</details>

<details>
<summary>Screenshot</summary>

![image](https://github.com/astral-sh/ruff/assets/66076021/2bb17322-8acf-4466-83b8-889b100e0b1b)

</details>

### With randomly selected seeds:

<details>

```
(ruff) (fuzz-build-failure) % py scripts/fuzz-parser/fuzz.py $(shuf -i 0-1000000 -n 30)                                                   ~/dev/ruff
Running `cargo build --release` since no test executable was specified...
Concurrently running the fuzzer on 30 randomly generated source-code files...
Ran fuzzer successfully on seed 197558                  [1/30]
Ran fuzzer successfully on seed 202549                  [2/30]
Ran fuzzer successfully on seed 25324                   [3/30]
Ran fuzzer successfully on seed 690                     [4/30]
Ran fuzzer successfully on seed 227842                  [5/30]
Ran fuzzer successfully on seed 303051                  [6/30]
Ran fuzzer successfully on seed 158142                  [7/30]
Ran fuzzer successfully on seed 257554                  [8/30]
Ran fuzzer successfully on seed 52360                   [9/30]
Ran fuzzer successfully on seed 233166                 [10/30]
Ran fuzzer successfully on seed 46711                  [11/30]
Ran fuzzer successfully on seed 300496                 [12/30]
Ran fuzzer successfully on seed 236817                 [13/30]
Ran fuzzer successfully on seed 202823                 [14/30]
Ran fuzzer successfully on seed 316977                 [15/30]
Ran fuzzer successfully on seed 336683                 [16/30]
Ran fuzzer successfully on seed 739140                 [17/30]
Ran fuzzer successfully on seed 615627                 [18/30]
Ran fuzzer successfully on seed 716382                 [19/30]
Ran fuzzer successfully on seed 341779                 [20/30]
Ran fuzzer successfully on seed 624059                 [21/30]
Ran fuzzer successfully on seed 584114                 [22/30]
Ran fuzzer successfully on seed 530446                 [23/30]
Ran fuzzer successfully on seed 304631                 [24/30]
Ran fuzzer successfully on seed 589529                 [25/30]
Ran fuzzer successfully on seed 427851                 [26/30]
Ran fuzzer successfully on seed 885136                 [27/30]
Ran fuzzer successfully on seed 757847                 [28/30]
Ran fuzzer successfully on seed 823008                 [29/30]
Ran fuzzer successfully on seed 852077                 [30/30]
No bugs found!
```

</details>

<details>
<summary>Screenshot</summary>

![image](https://github.com/astral-sh/ruff/assets/66076021/a403f7d3-4925-4cfb-a33c-e89b210b6993)

</details>

### If it fails:

<details>

```
(ruff) (fuzz-build-failure) % py scripts/fuzz-parser/fuzz.py 0-500                                                                        ~/dev/ruff
Running `cargo build --release` since no test executable was specified...
Concurrently running the fuzzer on 501 randomly generated source-code files...
Ran fuzzer successfully on seed 12                     [1/501]
Ran fuzzer successfully on seed 0                      [2/501]
Ran fuzzer successfully on seed 8                      [3/501]
Ran fuzzer successfully on seed 1                      [4/501]
Ran fuzzer successfully on seed 7                      [5/501]
Ran fuzzer successfully on seed 4                      [6/501]
Ran fuzzer on seed 9                                   [7/501]
The following code triggers a bug:

name_3 - ~name_0

Ran fuzzer on seed 2                                   [8/501]
The following code triggers a bug:

-~name_1

Ran fuzzer on seed 10                                  [9/501]
The following code triggers a bug:

+name_3 not in ~name_4

Ran fuzzer on seed 6                                  [10/501]
The following code triggers a bug:

+~name_3

Ran fuzzer successfully on seed 13                    [11/501]
```

</details>

<details>
<summary>Screenshot</summary>

![image](https://github.com/astral-sh/ruff/assets/66076021/91fc5635-cf04-4527-aded-a7fbe47b44fe)

</details>

### `--help` output

<details>
<summary>Screenshot</summary>

![image](https://github.com/astral-sh/ruff/assets/66076021/11a28499-ba0b-4ca3-9509-801d6a9bdda9)

</details>

---

_Label `internal` added by @AlexWaygood on 2024-04-28 17:58_

---

_Merged by @AlexWaygood on 2024-04-28 18:17_

---

_Closed by @AlexWaygood on 2024-04-28 18:17_

---

_Branch deleted on 2024-04-28 18:17_

---
