```yaml
number: 4635
title: "[`pycodestyle`] Add `too many blank lines` (`E303`) "
type: pull_request
state: closed
author: hoel-bagard
labels: []
assignees: []
base: main
head: E303_too_many_blank_lines
created_at: 2023-05-24T15:16:32Z
updated_at: 2023-05-28T16:54:14Z
url: https://github.com/astral-sh/ruff/pull/4635
synced_at: 2026-01-12T15:55:16Z
```

# [`pycodestyle`] Add `too many blank lines` (`E303`) 

---

_@hoel-bagard_

## Summary
This PR is part of  https://github.com/charliermarsh/ruff/issues/2402, it adds the `E303` error rule with its fix.

## Test Plan

It was tested on the added fixture: `crates/ruff/resources/test/fixtures/pycodestyle/E303.py`.

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pycodestyle/rules/too_many_blank_lines.rs`:59 on 2023-05-24 15:20_

Nit: You can compare with `TextSize::new()`
```suggestion
    if line.start() > TextSize::new(0)
```

This heuristic to determine if it is the first line fails if the file starts with an UTF BOM header. I recommend you pass a boolean indicating whether this is the first line or not.

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pycodestyle/rules/too_many_blank_lines.rs`:88 on 2023-05-24 15:23_

We should use `stylist` (should be on checker) to use the user's preferred newline character.

---

_Review comment by @charliermarsh on `crates/ruff/resources/test/fixtures/pycodestyle/E303.py`:25 on 2023-05-24 15:25_

Can we instead use the `E30.py` test suite from pycodestyle itself?

---

_@charliermarsh reviewed on 2023-05-24 15:25_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pycodestyle/rules/too_many_blank_lines.rs`:68 on 2023-05-24 15:26_

Determining the line is fairly expensive using. You may also be unlucky if the previous line ends with `\r\n` because you then index right between the `\r` and `\n` and there's no good way for the locator to know that it is now between a `\r\n`. 

I'm not quite sure what the best way is to implement this rule. Ideally, it could keep a state between each line, counting the empty lines to this point (CC: @charliermarsh).



---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pycodestyle/rules/too_many_blank_lines.rs`:75 on 2023-05-24 15:28_

`Line` will have to repeat the search for the start and end of the line, which is fairly expensive. 

Is there a way that we would only need to find the end (or start) and can then construct the `TextRange` ourselves (because we know what e.g. the end of the current line is?). 

---

_@MichaReiser reviewed on 2023-05-24 15:28_

---

_Review comment by @hoel-bagard on `crates/ruff/resources/test/fixtures/pycodestyle/E303.py`:25 on 2023-05-24 15:31_

Yes, should I just add the whole test suite as-is ?

---

_@hoel-bagard reviewed on 2023-05-24 15:31_

---

_Review comment by @charliermarsh on `crates/ruff/resources/test/fixtures/pycodestyle/E303.py`:25 on 2023-05-24 15:32_

Yeah, and we'll augment it with more test cases as needed.

---

_@charliermarsh reviewed on 2023-05-24 15:32_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pycodestyle/rules/too_many_blank_lines.rs`:68 on 2023-05-24 15:33_

I believe pycodestyle tracks the number of empty lines while building up logical lines -- we should probably do the same? That's now straightforward because we emit `NonLogicalNewline` whenever we see an empty line.

---

_@charliermarsh reviewed on 2023-05-24 15:33_

---

_@MichaReiser reviewed on 2023-05-24 15:41_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pycodestyle/rules/too_many_blank_lines.rs`:68 on 2023-05-24 15:41_

We do, but the physical lines rule operates on the String and I think the logical lines check intentionally skips empty lines... Where would you place this rule? Would this be something new? A rule that directly works on the tokens.

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pycodestyle/rules/too_many_blank_lines.rs`:68 on 2023-05-24 15:42_

You'd place it at the next logical line -- you'd check the number of preceding empty lines, I think?

---

_@charliermarsh reviewed on 2023-05-24 15:42_

---

_Comment by @github-actions[bot] on 2023-05-24 17:12_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.9±0.03ms     2.7 MB/sec    1.01     15.0±0.03ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.6 MB/sec    1.00      3.7±0.00ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.00    375.2±2.92µs     7.9 MB/sec    1.00    376.1±0.93µs     7.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.01ms     4.1 MB/sec    1.01      6.3±0.01ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.01      7.5±0.02ms     5.4 MB/sec    1.00      7.5±0.01ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1597.4±3.56µs    10.4 MB/sec    1.00   1591.0±1.98µs    10.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    173.8±0.23µs    17.0 MB/sec    1.02    176.8±6.95µs    16.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.01ms     7.5 MB/sec    1.00      3.4±0.01ms     7.5 MB/sec
parser/large/dataset.py                    1.00      5.7±0.01ms     7.2 MB/sec    1.02      5.8±0.01ms     7.0 MB/sec
parser/numpy/ctypeslib.py                  1.00   1126.2±0.69µs    14.8 MB/sec    1.01   1134.6±0.61µs    14.7 MB/sec
parser/numpy/globals.py                    1.00    115.3±0.31µs    25.6 MB/sec    1.02    117.4±0.38µs    25.1 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.01ms    10.4 MB/sec    1.01      2.5±0.01ms    10.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     22.3±0.71ms  1867.3 KB/sec    1.02     22.7±0.83ms  1832.9 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.6±0.22ms     3.0 MB/sec    1.04      5.8±0.23ms     2.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   668.5±31.94µs     4.4 MB/sec    1.04   693.1±40.81µs     4.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.6±0.50ms     2.6 MB/sec    1.01      9.7±0.34ms     2.6 MB/sec
linter/default-rules/large/dataset.py      1.01     11.3±0.46ms     3.6 MB/sec    1.00     11.2±0.33ms     3.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.4±0.09ms     7.0 MB/sec    1.00      2.4±0.11ms     7.0 MB/sec
linter/default-rules/numpy/globals.py      1.02   279.5±14.61µs    10.6 MB/sec    1.00   274.3±14.48µs    10.8 MB/sec
linter/default-rules/pydantic/types.py     1.01      5.1±0.25ms     5.0 MB/sec    1.00      5.0±0.20ms     5.1 MB/sec
parser/large/dataset.py                    1.00      9.0±0.35ms     4.5 MB/sec    1.17     10.5±0.38ms     3.9 MB/sec
parser/numpy/ctypeslib.py                  1.00  1717.3±72.78µs     9.7 MB/sec    1.14  1952.4±63.56µs     8.5 MB/sec
parser/numpy/globals.py                    1.00   174.5±11.14µs    16.9 MB/sec    1.12    194.6±8.65µs    15.2 MB/sec
parser/pydantic/types.py                   1.00      3.8±0.17ms     6.7 MB/sec    1.15      4.4±0.13ms     5.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@hoel-bagard reviewed on 2023-05-25 00:08_

---

_Review comment by @hoel-bagard on `crates/ruff/src/rules/pycodestyle/rules/too_many_blank_lines.rs`:68 on 2023-05-25 00:08_

Looking at the pycodestyle rule again, my implementation is wrong anyway. The following code should generate an error:

```python
class MyClass(object):
    def func1():
        pass


    def func2():
        pass
```
-> `E303 too many blank lines (2)`


I'll try again using logical lines.

---

_@hoel-bagard reviewed on 2023-05-25 11:10_

---

_Review comment by @hoel-bagard on `crates/ruff/src/rules/pycodestyle/rules/too_many_blank_lines.rs`:68 on 2023-05-25 11:10_

@MichaReiser @charliermarsh I've had another go using logical lines. It's a very, very rough draft (that might have broken a few other rules), but could you tell me if you think it's worth continuing on that path ?

The modifications I made to the `logical_lines.rs` file can be seen [here](https://github.com/charliermarsh/ruff/compare/main...hoel-bagard:ruff:add_E301?expand=1#diff-c44781a9e0e9d6258049aeaaf314a3e415ee3ffd19e7a32cb6a00945f7cb219d), and the checking function is [here](https://github.com/hoel-bagard/ruff/blob/add_E301/crates/ruff/src/rules/pycodestyle/rules/logical_lines/blank_lines.rs#L300). I've also had to remove the explicit skip of empty lines [here](https://github.com/charliermarsh/ruff/compare/main...hoel-bagard:ruff:add_E301?expand=1#diff-dc30d96352cd482de3acfae061d6c936c886fb1b98a00d84e9ae4dbc057e11f8R153).

I still haven't solved the issue with `\r\n`, but maybe I could count the number of characters to remove along with the number of blank lines ?

---

_Review comment by @calumy on `crates/ruff/src/rules/pycodestyle/rules/too_many_blank_lines.rs`:25 on 2023-05-25 12:38_

Because this issue is fixed by black, the rule name should be added to the [`KNOWN_FORMATTING_VIOLATIONS` list](https://github.com/charliermarsh/ruff/blob/main/scripts/check_docs_formatted.py#L25-L54) to stop CI flagging that it should be fixed.

---

_@calumy reviewed on 2023-05-25 12:38_

---

_@hoel-bagard reviewed on 2023-05-25 13:35_

---

_Review comment by @hoel-bagard on `crates/ruff/src/rules/pycodestyle/rules/too_many_blank_lines.rs`:25 on 2023-05-25 13:35_

Is there a list somewhere with black's rules names ?

---

_Review comment by @calumy on `crates/ruff/src/rules/pycodestyle/rules/too_many_blank_lines.rs`:25 on 2023-05-25 15:09_

In this case, you would add `too_many_blank_lines` to the list as `KNOWN_FORMATTING_VIOLATIONS` relates to Ruff's rules rather than black.

---

_@calumy reviewed on 2023-05-25 15:09_

---

_@hoel-bagard reviewed on 2023-05-25 15:15_

---

_Review comment by @hoel-bagard on `crates/ruff/src/rules/pycodestyle/rules/too_many_blank_lines.rs`:25 on 2023-05-25 15:15_

I see, thanks!

---

_Comment by @hoel-bagard on 2023-05-26 03:26_

I'm closing this PR since I'm redoing it using logical lines instead of physical lines.

---

_Closed by @hoel-bagard on 2023-05-26 03:26_

---
