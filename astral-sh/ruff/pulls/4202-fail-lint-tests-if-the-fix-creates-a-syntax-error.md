```yaml
number: 4202
title: Fail lint tests if the fix creates a syntax error
type: pull_request
state: merged
author: MichaReiser
labels: []
assignees: []
merged: true
base: main
head: test-for-syntax-errors
created_at: 2023-05-03T07:47:56Z
updated_at: 2023-05-05T05:59:35Z
url: https://github.com/astral-sh/ruff/pull/4202
synced_at: 2026-01-12T04:28:19Z
```

# Fail lint tests if the fix creates a syntax error

---

_Pull request opened by @MichaReiser on 2023-05-03 07:47_

This PR adds a check to `test_path` that ensures that the source after the fixes are applied does not contain any syntax errors and aborts by printing the last applied fixes, the syntax error, and the fixed document's source if there are any syntax errors. 

This should help narrowing down syntax errors when developing fixes. 

Example output

```

Fixed source has a syntax error where the source document does not. This is a bug in one of the generated fixes:
FLY001.py:10:5: E999 SyntaxError: f-string: unterminated string
   |
10 | x = f"foobar"
11 | y = "foobar"
12 | f"{\"{'Finally'}, \" + f'{greeting}'}{subject}"
   |     ^ E999
   |


Last generated fixes:
FLY001.py:4:8: FLY001 [*] Consider `f"Finally, {greeting}"` instead of concatenation
  |
4 | subject = "World"
5 | msg = greeting + " " + subject
6 | msg2 = "Finally, " + greeting + " " + subject
  |        ^^^^^^^^^^^^^^^^^^^^^^ FLY001
7 | msg3 = f"Finally, {greeting} " + subject
8 | msg4 = "Finally, " + f"{greeting} {subject}"
  |
  = help: Replace with `f"Finally, {greeting}"`

ℹ Suggested fix
1 1 | greeting = "Hello"
2 2 | subject = "World"
3 3 | msg = greeting + " " + subject
4   |-msg2 = "Finally, " + greeting + " " + subject
  4 |+msg2 = f"Finally, {greeting}" + " " + subject
5 5 | msg3 = f"Finally, {greeting} " + subject
6 6 | msg4 = "Finally, " + f"{greeting} {subject}"
7 7 | # msg5 = "{'Finally'}, " + f"{greeting}" + f"{subject}"  # TODO: autofix syntax error

FLY001.py:5:8: FLY001 [*] Consider `f"Finally, {greeting} {subject}"` instead of concatenation
  |
5 | msg = greeting + " " + subject
6 | msg2 = "Finally, " + greeting + " " + subject
7 | msg3 = f"Finally, {greeting} " + subject
  |        ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ FLY001
8 | msg4 = "Finally, " + f"{greeting} {subject}"
9 | # msg5 = "{'Finally'}, " + f"{greeting}" + f"{subject}"  # TODO: autofix syntax error
  |
  = help: Replace with `f"Finally, {greeting} {subject}"`

ℹ Suggested fix
2 2 | subject = "World"
3 3 | msg = greeting + " " + subject
4 4 | msg2 = "Finally, " + greeting + " " + subject
5   |-msg3 = f"Finally, {greeting} " + subject
  5 |+msg3 = f"Finally, {greeting} {subject}"
6 6 | msg4 = "Finally, " + f"{greeting} {subject}"
7 7 | # msg5 = "{'Finally'}, " + f"{greeting}" + f"{subject}"  # TODO: autofix syntax error
8 8 | x = f"foo" + f"bar"

FLY001.py:6:8: FLY001 [*] Consider `f"Finally, {greeting} {subject}"` instead of concatenation
   |
 6 | msg2 = "Finally, " + greeting + " " + subject
 7 | msg3 = f"Finally, {greeting} " + subject
 8 | msg4 = "Finally, " + f"{greeting} {subject}"
   |        ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ FLY001
 9 | # msg5 = "{'Finally'}, " + f"{greeting}" + f"{subject}"  # TODO: autofix syntax error
10 | x = f"foo" + f"bar"
   |
   = help: Replace with `f"Finally, {greeting} {subject}"`

ℹ Suggested fix
3 3 | msg = greeting + " " + subject
4 4 | msg2 = "Finally, " + greeting + " " + subject
5 5 | msg3 = f"Finally, {greeting} " + subject
6   |-msg4 = "Finally, " + f"{greeting} {subject}"
  6 |+msg4 = f"Finally, {greeting} {subject}"
7 7 | # msg5 = "{'Finally'}, " + f"{greeting}" + f"{subject}"  # TODO: autofix syntax error
8 8 | x = f"foo" + f"bar"
9 9 | y = "foo" + "bar"

FLY001.py:8:5: FLY001 [*] Consider `f"foobar"` instead of concatenation
   |
 8 | msg4 = "Finally, " + f"{greeting} {subject}"
 9 | # msg5 = "{'Finally'}, " + f"{greeting}" + f"{subject}"  # TODO: autofix syntax error
10 | x = f"foo" + f"bar"
   |     ^^^^^^^^^^^^^^^ FLY001
11 | y = "foo" + "bar"
12 | "{'Finally'}, " + f"{greeting}" + f"{subject}"
   |
   = help: Replace with `f"foobar"`

ℹ Suggested fix
5 5 | msg3 = f"Finally, {greeting} " + subject
6 6 | msg4 = "Finally, " + f"{greeting} {subject}"
7 7 | # msg5 = "{'Finally'}, " + f"{greeting}" + f"{subject}"  # TODO: autofix syntax error
8   |-x = f"foo" + f"bar"
  8 |+x = f"foobar"
9 9 | y = "foo" + "bar"
10 10 | "{'Finally'}, " + f"{greeting}" + f"{subject}"

FLY001.py:9:5: FLY001 [*] Consider `"foobar"` instead of concatenation
   |
 9 | # msg5 = "{'Finally'}, " + f"{greeting}" + f"{subject}"  # TODO: autofix syntax error
10 | x = f"foo" + f"bar"
11 | y = "foo" + "bar"
   |     ^^^^^^^^^^^^^ FLY001
12 | "{'Finally'}, " + f"{greeting}" + f"{subject}"
   |
   = help: Replace with `"foobar"`

ℹ Suggested fix
6  6  | msg4 = "Finally, " + f"{greeting} {subject}"
7  7  | # msg5 = "{'Finally'}, " + f"{greeting}" + f"{subject}"  # TODO: autofix syntax error
8  8  | x = f"foo" + f"bar"
9     |-y = "foo" + "bar"
   9  |+y = "foobar"
10 10 | "{'Finally'}, " + f"{greeting}" + f"{subject}"

FLY001.py:10:1: FLY001 [*] Consider `f"{\"{'Finally'}, \" + f'{greeting}'}{subject}"` instead of concatenation
   |
10 | x = f"foo" + f"bar"
11 | y = "foo" + "bar"
12 | "{'Finally'}, " + f"{greeting}" + f"{subject}"
   | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ FLY001
   |
   = help: Replace with `f"{\"{'Finally'}, \" + f'{greeting}'}{subject}"`

ℹ Suggested fix
7  7  | # msg5 = "{'Finally'}, " + f"{greeting}" + f"{subject}"  # TODO: autofix syntax error
8  8  | x = f"foo" + f"bar"
9  9  | y = "foo" + "bar"
10    |-"{'Finally'}, " + f"{greeting}" + f"{subject}"
   10 |+f"{\"{'Finally'}, \" + f'{greeting}'}{subject}"

FLY001.py:10:1: FLY001 [*] Consider `f"{{'Finally'}}, {greeting}"` instead of concatenation
   |
10 | x = f"foo" + f"bar"
11 | y = "foo" + "bar"
12 | "{'Finally'}, " + f"{greeting}" + f"{subject}"
   | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ FLY001
   |
   = help: Replace with `f"{{'Finally'}}, {greeting}"`

ℹ Suggested fix
7  7  | # msg5 = "{'Finally'}, " + f"{greeting}" + f"{subject}"  # TODO: autofix syntax error
8  8  | x = f"foo" + f"bar"
9  9  | y = "foo" + "bar"
10    |-"{'Finally'}, " + f"{greeting}" + f"{subject}"
   10 |+f"{{'Finally'}}, {greeting}" + f"{subject}"


Source with applied fixes:
greeting = "Hello"
subject = "World"
msg = greeting + " " + subject
msg2 = f"Finally, {greeting}" + " " + subject
msg3 = f"Finally, {greeting} {subject}"
msg4 = f"Finally, {greeting} {subject}"
# msg5 = "{'Finally'}, " + f"{greeting}" + f"{subject}"  # TODO: autofix syntax error
x = f"foobar"
y = "foobar"
f"{\"{'Finally'}, \" + f'{greeting}'}{subject}"
```

---

_Comment by @MichaReiser on 2023-05-03 07:48_

Current dependencies on/for this PR:
* main
  * **PR #4202** <a href="https://app.graphite.dev/github/pr/charliermarsh/ruff/4202" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This comment was auto-generated by [Graphite](https://app.graphite.dev/github/pr/charliermarsh/ruff/4202?utm_source=stack-comment).

---

_Comment by @github-actions[bot] on 2023-05-03 08:05_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.8±0.82ms     2.7 MB/sec    1.09     16.1±0.67ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.26ms     4.6 MB/sec    1.08      3.9±0.13ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.04   497.8±11.05µs     5.9 MB/sec    1.00   479.0±16.67µs     6.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.6±0.32ms     3.8 MB/sec    1.03      6.8±0.20ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.02      8.3±0.42ms     4.9 MB/sec    1.00      8.1±0.23ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1788.5±12.40µs     9.3 MB/sec    1.00  1762.8±29.05µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    199.3±1.52µs    14.8 MB/sec    1.00    198.5±3.63µs    14.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.05ms     7.0 MB/sec    1.01      3.7±0.13ms     6.9 MB/sec
parser/large/dataset.py                    1.01      6.4±0.25ms     6.4 MB/sec    1.00      6.3±0.30ms     6.4 MB/sec
parser/numpy/ctypeslib.py                  1.07  1149.9±61.28µs    14.5 MB/sec    1.00  1071.8±71.87µs    15.5 MB/sec
parser/numpy/globals.py                    1.00    113.0±3.98µs    26.1 MB/sec    1.12    126.3±6.55µs    23.4 MB/sec
parser/pydantic/types.py                   1.00      2.6±0.09ms    10.0 MB/sec    1.02      2.6±0.13ms     9.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     20.7±0.88ms  2012.5 KB/sec    1.01     20.8±0.54ms  1998.1 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.05      5.3±0.23ms     3.2 MB/sec    1.00      5.0±0.20ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   603.7±27.26µs     4.9 MB/sec    1.01   612.2±31.26µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      8.6±0.31ms     3.0 MB/sec    1.00      8.5±0.32ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00     10.3±0.40ms     3.9 MB/sec    1.01     10.4±0.35ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.11ms     7.5 MB/sec    1.00      2.2±0.11ms     7.5 MB/sec
linter/default-rules/numpy/globals.py      1.00   240.4±13.05µs    12.3 MB/sec    1.03   248.4±17.90µs    11.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.5±0.24ms     5.7 MB/sec    1.03      4.6±0.18ms     5.6 MB/sec
parser/large/dataset.py                    1.03      8.6±0.27ms     4.8 MB/sec    1.00      8.3±0.48ms     4.9 MB/sec
parser/numpy/ctypeslib.py                  1.05  1642.8±69.05µs    10.1 MB/sec    1.00  1562.8±68.77µs    10.7 MB/sec
parser/numpy/globals.py                    1.01   166.0±10.05µs    17.8 MB/sec    1.00    164.1±8.45µs    18.0 MB/sec
parser/pydantic/types.py                   1.03      3.6±0.13ms     7.0 MB/sec    1.00      3.5±0.12ms     7.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@charliermarsh approved on 2023-05-04 15:45_

---

_Merged by @MichaReiser on 2023-05-05 05:59_

---

_Closed by @MichaReiser on 2023-05-05 05:59_

---

_Branch deleted on 2023-05-05 05:59_

---
