```yaml
number: 4226
title: Fix replace_whitespace() tabulation to space
type: pull_request
state: merged
author: JonathanPlasse
labels: []
assignees: []
merged: true
base: main
head: fix-replace-whitespace
created_at: 2023-05-04T16:41:27Z
updated_at: 2023-05-08T14:53:28Z
url: https://github.com/astral-sh/ruff/pull/4226
synced_at: 2026-01-12T03:56:39Z
```

# Fix replace_whitespace() tabulation to space

---

_Pull request opened by @JonathanPlasse on 2023-05-04 16:41_

In some case too much spaces were added.

---

_Review comment by @JonathanPlasse on `crates/ruff/src/rules/pycodestyle/snapshots/ruff__rules__pycodestyle__tests__E101_E101.py.snap`:19 on 2023-05-04 16:45_

This number of spaces now matches the original file with the tabulation size set to 4.

---

_@JonathanPlasse reviewed on 2023-05-04 16:45_

---

_Comment by @github-actions[bot] on 2023-05-04 17:13_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     16.9±0.09ms     2.4 MB/sec    1.00     16.8±0.08ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.0±0.03ms     4.1 MB/sec    1.00      4.0±0.02ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.01    495.4±3.38µs     6.0 MB/sec    1.00    491.4±2.24µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.05ms     3.6 MB/sec    1.00      7.0±0.06ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.01      8.3±0.04ms     4.9 MB/sec    1.00      8.2±0.04ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1770.7±8.46µs     9.4 MB/sec    1.00   1752.4±4.19µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    193.7±1.15µs    15.2 MB/sec    1.01    194.8±0.47µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.7±0.01ms     6.9 MB/sec    1.00      3.7±0.03ms     7.0 MB/sec
parser/large/dataset.py                    1.02      6.7±0.01ms     6.1 MB/sec    1.00      6.5±0.01ms     6.3 MB/sec
parser/numpy/ctypeslib.py                  1.02   1289.6±2.20µs    12.9 MB/sec    1.00   1267.3±3.13µs    13.1 MB/sec
parser/numpy/globals.py                    1.02    130.8±1.69µs    22.6 MB/sec    1.00    128.7±1.23µs    22.9 MB/sec
parser/pydantic/types.py                   1.02      2.8±0.01ms     9.0 MB/sec    1.00      2.8±0.00ms     9.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.0±0.12ms     2.4 MB/sec    1.01     17.2±0.12ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.05ms     3.9 MB/sec    1.01      4.3±0.04ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    433.3±8.55µs     6.8 MB/sec    1.02    443.4±6.00µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.07ms     3.5 MB/sec    1.01      7.3±0.08ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.5±0.05ms     4.8 MB/sec    1.03      8.8±0.07ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1790.6±16.01µs     9.3 MB/sec    1.03  1844.9±12.26µs     9.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    189.1±3.25µs    15.6 MB/sec    1.03    195.4±3.74µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.03ms     6.7 MB/sec    1.03      3.9±0.03ms     6.5 MB/sec
parser/large/dataset.py                    1.00      6.8±0.03ms     6.0 MB/sec    1.03      7.0±0.03ms     5.8 MB/sec
parser/numpy/ctypeslib.py                  1.00  1298.5±12.32µs    12.8 MB/sec    1.02  1329.2±11.28µs    12.5 MB/sec
parser/numpy/globals.py                    1.00    133.7±1.40µs    22.1 MB/sec    1.02    136.8±1.70µs    21.6 MB/sec
parser/pydantic/types.py                   1.00      2.9±0.02ms     8.9 MB/sec    1.03      3.0±0.02ms     8.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/message/text.rs`:280 on 2023-05-05 06:11_

Lol really, did I forget to increment the column? That's embarrassing. 


My understanding is that tabs don't operate on character width but on character counts, meaning, we should increment the column by one for every character (except newlines, because they reset the column to 0)

```suggestion
		match c {
			'\t' => {
                let tab_width = TAB_SIZE - column % TAB_SIZE;

                if index < usize::from(annotation_range.start()) {
                    range += TextSize::new(tab_width - 1);
                } else if index < usize::from(annotation_range.end()) {
                    range = range.add_end(TextSize::new(tab_width - 1));
                }

                result.push_str(&source[last_end..index]);

                for _ in 0..tab_width {
                    result.push(' ');
                }

                last_end = index + 1;
            }
            '\n' | '\r' => {
                column = 0;
                continue;
            },
            _ => {}
		}
		
		column += 1;
```

---

_@MichaReiser approved on 2023-05-05 06:12_

---

_Merged by @MichaReiser on 2023-05-08 12:03_

---

_Closed by @MichaReiser on 2023-05-08 12:03_

---

_Branch deleted on 2023-05-08 14:53_

---
