```yaml
number: 4167
title: "Introduce `tab-size` to correcly calculate the line length with tabulations"
type: pull_request
state: merged
author: JonathanPlasse
labels: []
assignees: []
merged: true
base: main
head: fix-e501-with-tabulation
created_at: 2023-05-01T06:14:52Z
updated_at: 2023-05-24T08:25:35Z
url: https://github.com/astral-sh/ruff/pull/4167
synced_at: 2026-01-12T03:50:02Z
```

# Introduce `tab-size` to correcly calculate the line length with tabulations

---

_Pull request opened by @JonathanPlasse on 2023-05-01 06:14_

- Close #4016 

I added the setting `tab-size` to set the tabulation width in checks like `E501`.
I use as default `4` as it is the most common case.

With `tab-size = 1` or `tab-size = 2`:
![tab_size_1_2](https://user-images.githubusercontent.com/13716151/235414978-6c6a04c1-12ba-4d13-bcab-4621625c6336.png)
With `tab-size = 4`:
![tab_size_4](https://user-images.githubusercontent.com/13716151/235415039-75bc9e87-1ab6-4c9f-98b1-98ca860f232e.png)
With `tab-size = 8`:
![tab_size_8](https://user-images.githubusercontent.com/13716151/235415078-b89f91f7-2b84-40c8-a82e-e2268b655389.png)

> **Warning**
> The violation locations are offset in the snapshots.

---

_Comment by @github-actions[bot] on 2023-05-01 06:26_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.1±0.12ms     2.9 MB/sec    1.02     14.3±0.15ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.02ms     5.0 MB/sec    1.02      3.4±0.03ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    414.8±0.68µs     7.1 MB/sec    1.03    426.4±3.52µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.04ms     4.4 MB/sec    1.02      5.9±0.04ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.6±0.03ms     6.2 MB/sec    1.02      6.7±0.03ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1425.5±7.89µs    11.7 MB/sec    1.03   1469.6±1.45µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    157.7±0.24µs    18.7 MB/sec    1.07    168.1±1.05µs    17.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.6 MB/sec    1.03      3.1±0.01ms     8.3 MB/sec
parser/large/dataset.py                    1.01      5.2±0.01ms     7.8 MB/sec    1.00      5.2±0.01ms     7.9 MB/sec
parser/numpy/ctypeslib.py                  1.01   1024.1±0.50µs    16.3 MB/sec    1.00   1017.0±2.44µs    16.4 MB/sec
parser/numpy/globals.py                    1.01    104.8±0.19µs    28.2 MB/sec    1.00    104.1±0.25µs    28.3 MB/sec
parser/pydantic/types.py                   1.01      2.2±0.01ms    11.3 MB/sec    1.00      2.2±0.01ms    11.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     20.7±0.58ms  2016.4 KB/sec    1.02     21.0±0.75ms  1985.5 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.2±0.20ms     3.2 MB/sec    1.02      5.2±0.21ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   604.1±28.12µs     4.9 MB/sec    1.04   626.8±34.68µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.6±0.33ms     3.0 MB/sec    1.01      8.6±0.34ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.00     10.1±0.45ms     4.0 MB/sec    1.00     10.1±0.31ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.07ms     8.0 MB/sec    1.03      2.1±0.09ms     7.8 MB/sec
linter/default-rules/numpy/globals.py      1.00   233.7±14.36µs    12.6 MB/sec    1.09   253.9±17.78µs    11.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.5±0.17ms     5.7 MB/sec    1.03      4.6±0.17ms     5.5 MB/sec
parser/large/dataset.py                    1.00      8.1±0.20ms     5.0 MB/sec    1.00      8.1±0.24ms     5.0 MB/sec
parser/numpy/ctypeslib.py                  1.01  1549.9±70.00µs    10.7 MB/sec    1.00  1529.8±67.91µs    10.9 MB/sec
parser/numpy/globals.py                    1.01    157.0±9.09µs    18.8 MB/sec    1.00    155.1±7.79µs    19.0 MB/sec
parser/pydantic/types.py                   1.01      3.5±0.13ms     7.4 MB/sec    1.00      3.4±0.16ms     7.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pycodestyle/helpers.rs`:28 on 2023-05-01 10:49_

Nit: Should we introduce a `TabSize`newtype wrapper (using `u32` or even a `u16` or `u8` should probably be sufficient) to pass a more meaningful type?

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pycodestyle/helpers.rs`:40 on 2023-05-01 10:50_

Nit
```suggestion
        width += if matches!(c, '\t') {
            tab_size - (width % tab_size)
        } else {
        	c.width().unwrap_or(0)
      	};
```

---

_@MichaReiser requested changes on 2023-05-01 10:53_

Thank you. 

There are a few places where we do not suggest a code fix if the line length after fixing exceeds the limit. We need to change these checks to handle tabs correctly too. 

https://github.com/search?q=repo%3Acharliermarsh%2Fruff%20.width()&type=code

---

_Converted to draft by @JonathanPlasse on 2023-05-04 17:41_

---

_Marked ready for review by @JonathanPlasse on 2023-05-18 16:19_

---

_@JonathanPlasse reviewed on 2023-05-18 17:10_

---

_Review comment by @JonathanPlasse on `crates/ruff/src/rules/pycodestyle/helpers.rs`:111 on 2023-05-18 17:10_

I think it should use the character count instead of the width to determine the width of a tabulation.

---

_Comment by @JonathanPlasse on 2023-05-18 17:12_

Should I add a fixture for all affected rules with tabulations and UTF-8 characters?

---

_@JonathanPlasse reviewed on 2023-05-18 17:15_

---

_Review comment by @JonathanPlasse on `crates/ruff/src/rules/pycodestyle/helpers.rs`:111 on 2023-05-18 17:15_

Like for the tabulation handling in snapshots.
https://github.com/charliermarsh/ruff/blob/a3aa841fc94cdc1973a97446108ae7468e2e9f71/crates/ruff/src/message/text.rs#L236-L286

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_simplify/rules/ast_if.rs`:293 on 2023-05-19 07:29_

Nit: What do you think of renaming the method to `line_width`?

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_simplify/rules/ast_if.rs`:516 on 2023-05-19 07:33_

Hmm this is interesting. I wouldn't have thought that I need to pass the column when writing this code myself. I wonder if we could design the API in a way to guide users to do the right "thing" by default.

Maybe

```
LineWidth::from_start_of_line(text, tab_width) -> LineWidth
LineWidth::add_str(self, text) -> LineWidth
```

Using a newtype wrapper makes it a bit harder to get this wrong because it e.g. doesn't support adding to line widths with `+` 

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pycodestyle/helpers.rs`:111 on 2023-05-19 07:35_

Agree. The tab_size must be column based.

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pycodestyle/snapshots/ruff__rules__pycodestyle__tests__tab_size_8.snap`:67 on 2023-05-19 07:36_

Can we throw in some unicode, just to makes sure we handle it correctly

---

_Review comment by @MichaReiser on `crates/ruff/src/settings/defaults.rs`:31 on 2023-05-19 07:37_

Nit: It could be worth introducing a `TabSize` newtype wrapper so that we can change the internal representation in a single place if we ever want to. 

---

_@MichaReiser reviewed on 2023-05-19 07:38_

> Should I add a fixture for all affected rules with tabulations and UTF-8 characters?

That would be awesome to ensure we don't accidentally break it in future refactors.

---

_Converted to draft by @JonathanPlasse on 2023-05-20 07:19_

---

_@JonathanPlasse reviewed on 2023-05-20 15:15_

---

_Review comment by @JonathanPlasse on `crates/ruff/src/settings/defaults.rs`:31 on 2023-05-20 15:15_

Done!

---

_Review comment by @JonathanPlasse on `crates/ruff/src/rules/pycodestyle/snapshots/ruff__rules__pycodestyle__tests__tab_size_8.snap`:67 on 2023-05-20 15:19_

Done!
The ranges in the snapshots are ok but not in VSCode.
![Screenshot from 2023-05-20 17-11-43](https://github.com/charliermarsh/ruff/assets/13716151/f5180443-f469-41e1-a3d1-1fda87467b08)

---

_@JonathanPlasse reviewed on 2023-05-20 15:19_

---

_Marked ready for review by @JonathanPlasse on 2023-05-20 15:19_

---

_Comment by @JonathanPlasse on 2023-05-20 17:41_

All concerned fixtures now have tests with UTF-8 and check with line length. Some also contain tabulations.
I would like to merge this and maybe work on the VSCode range offset problem with UTF-8 characters with E501 later.

---

_Review requested from @MichaReiser by @JonathanPlasse on 2023-05-20 18:11_

---

_@JonathanPlasse reviewed on 2023-05-20 18:20_

---

_Review comment by @JonathanPlasse on `crates/ruff/src/rules/pycodestyle/snapshots/ruff__rules__pycodestyle__tests__tab_size_8.snap`:67 on 2023-05-20 18:20_

VSCode only counts one character for 💣

---

_Renamed from "Fix E501 with tabulation" to "Calculate line-length taking into account tab-size" by @JonathanPlasse on 2023-05-21 09:25_

---

_Renamed from "Calculate line-length taking into account tab-size" to "Introduce `tab-size` to correcly calculate the line length with tabulations" by @JonathanPlasse on 2023-05-21 09:26_

---

_Review comment by @JonathanPlasse on `crates/ruff/src/settings/line_width.rs`:31 on 2023-05-21 16:51_

Use the [Typestate Pattern](http://cliffle.com/blog/rust-typestate/) to only have `new`, `add_str`, `add_char`, `add_width` on `Width` and not `LineLength`.

---

_@JonathanPlasse reviewed on 2023-05-21 16:51_

---

_Review comment by @MichaReiser on `crates/ruff/src/message/text.rs`:251 on 2023-05-22 06:57_

Nit: Can we add a comment explaining why the truncation is "fine" here? 

```
// SAFETY: The difference is a value in the range [1..TAB_SIZE] which is guaranteed to be less than `u32`.
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pycodestyle/helpers.rs`:69 on 2023-05-22 07:02_

You could implement `PartialOrd`, and `Ord` to avoid the unwrapping together with implementing `impl Add for Width` and impl `Sub for Width` so that you can write: `if width - last_chunk <= limit`

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pycodestyle/snapshots/ruff__rules__pycodestyle__tests__tab_size_1.snap`:15 on 2023-05-22 07:04_

Is the caret at the right position? It seems to point past the end?

---

_Review comment by @MichaReiser on `crates/ruff/src/settings/line_width.rs`:14 on 2023-05-22 07:05_

You can derive `Default` automatically

```suggestion
#[derive(Clone, Copy, Debug, CacheKey, Default)]
#[cfg_attr(feature = "schemars", derive(schemars::JsonSchema))]
pub struct Length;
```

---

_Review comment by @MichaReiser on `crates/ruff/src/settings/line_width.rs`:38 on 2023-05-22 07:07_

What's the reason that we initialize the width to `88`? Or what's the meaning of the `width` field? 

---

_Review comment by @MichaReiser on `crates/ruff/src/settings/line_width.rs`:31 on 2023-05-22 07:12_

Do I understand it correctly that the main benefit of applying this pattern is that we can share the `width()` implementation between `Width` and `LineLength`. 

If that's the case, I would prefer to duplicate the method in both structs instead. It makes the code significantly easier to reason about and the duplication is minimal (results in the same generated code). 

It would also allow us to split the logic into multiple files. That would be beneficial because `Width` (as far as I understand) isn't used in any setting context. We should move it out of the `settings` module because of that.

---

_Review comment by @MichaReiser on `crates/ruff/src/settings/line_width.rs`:11 on 2023-05-22 07:14_

Can we document all public types, including their public members? 

---

_@MichaReiser reviewed on 2023-05-22 07:15_

Nice work! I have a few comments around the implementations of the new width structs, and are uncertain if the caret is correctly placed in one test. But it is looking great overall!

---

_@JonathanPlasse reviewed on 2023-05-22 09:00_

---

_Review comment by @JonathanPlasse on `crates/ruff/src/rules/pycodestyle/helpers.rs`:69 on 2023-05-22 09:00_

It is intentional, here this is safe as last_chunk does not contain whitespace so the width can be subtracted, but it is not correct in general, so I did not add an API for it and use `width()` to signal that.

---

_@MichaReiser reviewed on 2023-05-22 09:02_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pycodestyle/helpers.rs`:69 on 2023-05-22 09:02_

I see. I like that. What do you think of renaming the method to `get` similar to `NonZeroU32::get` or implement `From<Width> for u32` and use `u32::from(width) - u32::from(last_chunk) <= u32::from(limit)`? Mainly because I would expect `width.width()` to return a `Width`...

---

_@JonathanPlasse reviewed on 2023-05-22 09:02_

---

_Review comment by @JonathanPlasse on `crates/ruff/src/settings/line_width.rs`:38 on 2023-05-22 09:02_

`LineWidth<Length>` or `LineLength` is the type representing line length settings.
`88` comes from `LINE_LENGTH`.

---

_@JonathanPlasse reviewed on 2023-05-22 19:29_

---

_Review comment by @JonathanPlasse on `crates/ruff/src/rules/pycodestyle/snapshots/ruff__rules__pycodestyle__tests__tab_size_1.snap`:15 on 2023-05-22 19:29_

Github seems to render 💣 with only 1.5 width instead of 2 in VSCode and vim.
```text
💣💣
123
```

---

_Review requested from @MichaReiser by @JonathanPlasse on 2023-05-22 19:33_

---

_@MichaReiser approved on 2023-05-23 06:34_

Nice work. I like it a lot how you centralized the logic for all the line width checks. 

Could you add some documentation to `LineLength` and `LineWdith` and move `LineWidth` out of the `settings` module (it's not used in a settings context if I understand it correctly)? 

---

_Comment by @MichaReiser on 2023-05-24 06:37_

Thank you.

---

_Merged by @MichaReiser on 2023-05-24 06:37_

---

_Closed by @MichaReiser on 2023-05-24 06:37_

---

_Comment by @JonathanPlasse on 2023-05-24 08:24_

You are welcome, thank you for your review.

---

_Branch deleted on 2023-05-24 08:25_

---
