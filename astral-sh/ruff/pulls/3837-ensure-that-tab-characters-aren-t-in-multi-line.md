```yaml
number: 3837
title: "Ensure that tab characters aren't in multi-line strings before throwing a violation"
type: pull_request
state: merged
author: evanrittenhouse
labels: []
assignees: []
merged: true
base: main
head: 3674_multiline_tabs
created_at: 2023-04-01T01:55:48Z
updated_at: 2023-06-15T21:14:03Z
url: https://github.com/astral-sh/ruff/pull/3837
synced_at: 2026-01-12T03:43:29Z
```

# Ensure that tab characters aren't in multi-line strings before throwing a violation

---

_Pull request opened by @evanrittenhouse on 2023-04-01 01:55_

Fixes #3674 and #3438

---

_Comment by @github-actions[bot] on 2023-04-01 02:26_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.04     21.8±1.10ms  1909.2 KB/sec    1.00     21.0±0.53ms  1982.5 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      5.5±0.20ms     3.0 MB/sec    1.00      5.3±0.22ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   593.5±17.81µs     5.0 MB/sec    1.03   609.0±46.49µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.0±0.39ms     2.8 MB/sec    1.00      9.0±0.36ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.00     10.2±0.44ms     4.0 MB/sec    1.00     10.2±0.34ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.11ms     7.9 MB/sec    1.08      2.3±0.22ms     7.4 MB/sec
linter/default-rules/numpy/globals.py      1.00   244.0±11.22µs    12.1 MB/sec    1.08   264.6±19.40µs    11.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.6±0.17ms     5.5 MB/sec    1.05      4.8±0.21ms     5.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     20.0±0.70ms     2.0 MB/sec    1.00     19.6±0.62ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.1±0.19ms     3.3 MB/sec    1.01      5.1±0.22ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.01   598.3±32.18µs     4.9 MB/sec    1.00   590.3±20.67µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      8.6±0.36ms     3.0 MB/sec    1.00      8.4±0.39ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00      9.8±0.34ms     4.2 MB/sec    1.01      9.9±0.34ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.07ms     7.9 MB/sec    1.08      2.3±0.10ms     7.3 MB/sec
linter/default-rules/numpy/globals.py      1.00   236.6±46.68µs    12.5 MB/sec    1.06   250.4±18.24µs    11.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.4±0.11ms     5.9 MB/sec    1.07      4.6±0.18ms     5.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Marked ready for review by @evanrittenhouse on 2023-04-01 15:26_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/physical_lines.rs`:165 on 2023-04-01 17:47_

Hmm, I think it's going to be difficult to make this sufficiently performant and robust with the current approach.

One way to think about what's happening here is that we already go through this process of identifying strings when we _tokenize_ the source code, so we're repeating a lot of work here.

Another observation is that we're paying a high cost in the common case to avoid an edge case (most users won't have this rule enabled, most code won't contain tabs, and many tabs won't be in strings).

A few ideas that we could try to leverage here:

1. We could add a `Vec<Range>` to `Indexer` to track all known string locations. Then here, if we detect tab indentation, we could check whether the location is within one of those string ranges. Would need to benchmark to know whether this is sufficiently performant. We already iterate over all tokens there, so grabbing the string ranges is cheap, but we do then have to store them all. (We'd also only need to check "Is this location in a string?" in the event that we detect a tab, so we wouldn't have to pay that price continuously as we do here.)
2. Maybe @MichaReiser has an idea of how this could be achieved within the logical lines framework?


---

_@charliermarsh reviewed on 2023-04-01 17:47_

---

_@evanrittenhouse reviewed on 2023-04-01 18:58_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/checkers/physical_lines.rs`:165 on 2023-04-01 18:58_

That's a good point @charliermarsh, I didn't think about the tokenization process. Where do we do that? I like your Vec<Range> idea. That would be much cleaner.

---

_@charliermarsh reviewed on 2023-04-01 19:02_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/physical_lines.rs`:165 on 2023-04-01 19:02_

If you take a look at `crates/ruff_python_ast/src/source_code/indexer.rs`, that struct gets a chance to extract meta-information from the token stream, so you could include `Tok::String` in that loop and build up a list of locations. If you look at the caller of _this_ function, it passes in `indexer.commented_lines()` already. So you could change it to take `indexer` directly.

---

_@evanrittenhouse reviewed on 2023-04-01 19:35_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/checkers/physical_lines.rs`:165 on 2023-04-01 19:35_

Sounds good. Let me try doing that - hopefully the benchmarks improve with that approach. Doing it this way felt off anyway, but that's on me; I didn't even think to check for tokenization. I need to brush up on the whole parsing/lexing/tokenizing process

---

_@evanrittenhouse reviewed on 2023-04-01 21:42_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/checkers/physical_lines.rs`:165 on 2023-04-01 21:42_

Marking as draft until that's done.

---

_Converted to draft by @evanrittenhouse on 2023-04-01 21:42_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/source_code/indexer.rs`:147 on 2023-04-04 06:38_

Nit: You can use `assert_eq(indexer.string_lines(), &vec![Range::new(...)])` to do the comparision. This has the advantage that the test prints a nice diff if `string_lines` is empty or contains additional elements. 

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/source_code/indexer.rs`:187 on 2023-04-04 06:39_

Could you add a test for a single line string?

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/source_code/indexer.rs`:49 on 2023-04-04 06:40_

Nit:
```suggestion
			match tok {
				Tok::Comment(_) => {
	                commented_lines.push(start.row());
	            }
                Tok::String {
                    value: _,
                    kind: _,
                    triple_quoted: true
                } => {
                	string_lines.push(Range::new(*start, *end));
	            }
```

To express that the branches are mutually exclusive 

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pycodestyle/rules/tab_indentation.rs`:48 on 2023-04-04 06:43_

You can use rust's built in [`binary-search`](https://doc.rust-lang.org/std/vec/struct.Vec.html#method.binary_search) method. 

---

_@MichaReiser approved on 2023-04-04 06:43_

---

_@evanrittenhouse reviewed on 2023-04-04 16:51_

---

_Review comment by @evanrittenhouse on `crates/ruff_python_ast/src/source_code/indexer.rs`:187 on 2023-04-04 16:51_

Yep, for sure. It also seems like there's a corner case I need to fix. With the current implementation, these two lines ([88](https://github.com/charliermarsh/ruff/blob/main/crates/ruff/resources/test/fixtures/pycodestyle/W19.py#L88), [137](https://github.com/charliermarsh/ruff/blob/main/crates/ruff/resources/test/fixtures/pycodestyle/W19.py#L137)) don't get counted as violations. I'll look into it after work, I'm wondering if it's maybe something where the tab character is on the line after a triple-quoted string.

---

_@evanrittenhouse reviewed on 2023-04-04 18:22_

---

_Review comment by @evanrittenhouse on `crates/ruff_python_ast/src/source_code/indexer.rs`:187 on 2023-04-04 18:22_

Actually, @MichaReiser I believe the single-line strings are covered on lines 140 and 176 of the file diff. Is that what you meant, or did you mean something like 
```
r""""multiline string on one line""""#;
```

---

_Marked ready for review by @evanrittenhouse on 2023-04-05 02:56_

---

_Review requested from @charliermarsh by @evanrittenhouse on 2023-04-05 02:56_

---

_Review requested from @MichaReiser by @evanrittenhouse on 2023-04-05 02:56_

---

_@MichaReiser reviewed on 2023-04-05 11:58_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/source_code/indexer.rs`:187 on 2023-04-05 11:58_

Uhm sorry. I meant adding a test for a single quote string (`'test'` or `"test"`) to make sure the indexer doesn't pick them up incorrectly. 

---

_@evanrittenhouse reviewed on 2023-04-05 11:59_

---

_Review comment by @evanrittenhouse on `crates/ruff_python_ast/src/source_code/indexer.rs`:187 on 2023-04-05 11:59_

No problem! That's covered in the first test on line 139

---

_Comment by @charliermarsh on 2023-04-06 22:56_

(Doing some testing, then will merge tonight!)

---

_@charliermarsh reviewed on 2023-04-06 23:39_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pycodestyle/rules/tab_indentation.rs`:32 on 2023-04-06 23:39_

@evanrittenhouse - Does this look reasonable, to use the built-in `binary_search_by` here?

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pycodestyle/rules/tab_indentation.rs`:53 on 2023-04-06 23:40_

I think it's sufficient to just check these, and not check the "Is it on the start _and_ end line" condition, since if it's on the start _and_ end line, it'll be caught by the first condition... But hoping I'm not thinking about it incorrectly.

---

_@charliermarsh reviewed on 2023-04-06 23:40_

---

_@charliermarsh reviewed on 2023-04-06 23:40_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pycodestyle/rules/tab_indentation.rs`:43 on 2023-04-06 23:40_

(I think we need to count characters here, since `.column()` is based on characters.)

---

_@evanrittenhouse reviewed on 2023-04-06 23:45_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/pycodestyle/rules/tab_indentation.rs`:32 on 2023-04-06 23:45_

Yep! Wasn't aware it existed, seems much cleaner

---

_@evanrittenhouse reviewed on 2023-04-06 23:47_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/pycodestyle/rules/tab_indentation.rs`:43 on 2023-04-06 23:47_

How do you mean?

---

_@evanrittenhouse reviewed on 2023-04-06 23:49_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/pycodestyle/rules/tab_indentation.rs`:53 on 2023-04-06 23:49_

Nah, I think that's right. IIRC I included a test case which contained a triple-quoted string on one line, so if that passes I think we should be good.

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pycodestyle/rules/tab_indentation.rs`:43 on 2023-04-07 02:25_

`indent.find('\t')` returns the index of the starting _byte_ for the tab character, but some characters can require multiple bytes (think unicode) -- so `str.len()` in Rust doesn't always return what you might expect if coming from another programming language.

---

_@charliermarsh reviewed on 2023-04-07 02:25_

---

_Merged by @charliermarsh on 2023-04-07 02:25_

---

_Closed by @charliermarsh on 2023-04-07 02:25_

---

_@evanrittenhouse reviewed on 2023-04-07 03:11_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/pycodestyle/rules/tab_indentation.rs`:43 on 2023-04-07 03:11_

Oh interesting, I didn't know that. I'll have to read up on it, thanks!

---

_Branch deleted on 2023-06-15 21:14_

---
