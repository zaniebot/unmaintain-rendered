```yaml
number: 4430
title: Autoformat confusable units
type: pull_request
state: merged
author: covracer
labels: []
assignees: []
merged: true
base: main
head: confusable-units
created_at: 2023-05-14T13:18:52Z
updated_at: 2024-04-26T13:16:19Z
url: https://github.com/astral-sh/ruff/pull/4430
synced_at: 2026-01-10T22:37:01Z
```

# Autoformat confusable units

---

_Pull request opened by @covracer on 2023-05-14 13:18_

I've seen errors crop up from using the different micro and mu characters. Follow matching recommendations on which character to prefer for micro, ohm, and angstrom. References:
* Section 22.2 Letterlike Symbols, subsection Unit Symbols, page 877 of [The Unicode Standard, Version 15.0
](https://www.unicode.org/versions/Unicode15.0.0/UnicodeStandard-15.0.pdf)
* Section 2.5 Duplicated Characters of [Unicode Technical Report 25](https://www.unicode.org/reports/tr25/)
* [SI brochure](https://www.bipm.org/documents/20126/41483022/SI-Brochure-9-EN.pdf)
* https://github.com/unicode-org/icu/blob/main/icu4c/source/data/unidata/confusables.txt

---

_Comment by @github-actions[bot] on 2023-05-14 13:25_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+2, -0, 0 error(s))

<details><summary>zulip (+2, -0)</summary>
<p>

```diff
+ zerver/tests/test_invite.py:1197:32: RUF001 [*] String contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
+ zerver/tests/test_signup.py:1033:29: RUF001 [*] String contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
```

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| RUF001 | 2 | 2 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     13.9±0.02ms     2.9 MB/sec    1.00     13.9±0.02ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.4±0.01ms     4.9 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    428.0±0.47µs     6.9 MB/sec    1.00    424.9±0.45µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.01ms     4.4 MB/sec    1.00      5.9±0.02ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.01ms     6.0 MB/sec    1.00      6.8±0.01ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1508.9±2.89µs    11.0 MB/sec    1.00   1494.0±3.40µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.02    173.2±1.81µs    17.0 MB/sec    1.00    169.7±1.31µs    17.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.00ms     8.2 MB/sec    1.00      3.1±0.01ms     8.2 MB/sec
parser/large/dataset.py                    1.01      5.2±0.01ms     7.8 MB/sec    1.00      5.2±0.00ms     7.8 MB/sec
parser/numpy/ctypeslib.py                  1.00   1020.1±0.54µs    16.3 MB/sec    1.00   1019.2±0.66µs    16.3 MB/sec
parser/numpy/globals.py                    1.00    105.6±0.27µs    27.9 MB/sec    1.00    105.5±0.16µs    28.0 MB/sec
parser/pydantic/types.py                   1.01      2.2±0.00ms    11.4 MB/sec    1.00      2.2±0.00ms    11.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.6±0.20ms     2.5 MB/sec    1.00     16.6±0.31ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.05ms     4.0 MB/sec    1.00      4.2±0.04ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.01   506.7±14.96µs     5.8 MB/sec    1.00    501.2±6.66µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.08ms     3.6 MB/sec    1.00      7.0±0.08ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.14ms     4.9 MB/sec    1.00      8.3±0.07ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1771.6±24.87µs     9.4 MB/sec    1.00  1778.4±26.63µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    203.4±3.13µs    14.5 MB/sec    1.02    207.3±7.52µs    14.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.05ms     6.9 MB/sec    1.01      3.8±0.07ms     6.8 MB/sec
parser/large/dataset.py                    1.00      6.3±0.05ms     6.4 MB/sec    1.02      6.4±0.07ms     6.3 MB/sec
parser/numpy/ctypeslib.py                  1.00  1192.4±16.10µs    14.0 MB/sec    1.02  1221.5±20.10µs    13.6 MB/sec
parser/numpy/globals.py                    1.00    123.3±2.36µs    23.9 MB/sec    1.01    124.7±2.84µs    23.7 MB/sec
parser/pydantic/types.py                   1.00      2.7±0.04ms     9.4 MB/sec    1.01      2.7±0.03ms     9.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `scripts/update_ambiguous_characters.py`:35 on 2023-05-15 07:02_

Nit: I wonder if we should instead change the type to `<char, char>`. Having the `char`s in place could make the table a bit more readable and avoids the `unwrap` calls. 

---

_@MichaReiser reviewed on 2023-05-15 07:11_

Thanks for your contribution. 

I'm not very familiar with unicode confusables. So bare with me ;) 

Our current table is based on the VS code table and the standard seems to differentiate between mixed script and other confusables. I believe we currently only test for mixed-script confusables (similar to [unicode-security](https://github.com/unicode-rs/unicode-security/blob/45178d16579a39c61e016333c8c20dc52fb9db26/src/tables.rs#L4314)). Do you know if these added scripts are also mixed script confusables or general confusables?

---

_Comment by @covracer on 2023-05-16 12:00_

Thanks for taking a look!

The VS Code list only contains "... characters (key) that are confusable with a basic ascii [0x20 to 0x7E] character (value)". https://github.com/hediet/vscode-unicode-data/blob/a2ee3e17acaad2957f910cf6c46f6ea2c6eae753/index.ts#L159

I wonder why they did that. Was it an easy starting point and they haven't been motivated to build it out further? Would a full table would be too slow in node.js? 🤷

I too am new to Unicode security, and am still trying to figure out how to apply domain name spoofing concepts to code linting and autoformatting. TR 39 suggested reading TR 36 first, where I found this:

> While compatibility normalization and mixed-script detection can handle the majority of spoofing cases, they do not handle single-script confusables. https://www.unicode.org/reports/tr36/#Single_Script_Spoofing

So it seems like normalization comes first in the process, before mixed script or single script handling. The substitutions I'm adding are normalizations.

A longer discussion of normalization from TR 36:
> Fortunately the design of IDN prevents a huge number of spoofing attacks. All conformant users of [[IDNA2003](https://www.unicode.org/reports/tr36/#IDNA2003)] are required to process domain names to convert what are called [compatibility-equivalent](http://www.unicode.org/glossary/#compatibility_equivalent) characters into a unique form using a process called compatibility normalization (NFKC)—for more information on this, see [[UAX15](https://www.unicode.org/reports/tr36/#UAX15)]. This processing eliminates most possibilities for visual spoofing by mapping away a large number of visually confusable characters and sequences.

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/ruff/rules/confusables.rs`:2121 on 2023-06-02 03:10_

Based on Section 2.5 in https://www.unicode.org/reports/tr25/, should this also include Kelvin sign?

---

_@charliermarsh reviewed on 2023-06-02 03:10_

---

_@charliermarsh reviewed on 2023-06-02 03:10_

---

_Review comment by @charliermarsh on `scripts/update_ambiguous_characters.py`:35 on 2023-06-02 03:10_

@MichaReiser - That seems reasonable to me, but will it increase the size of this map?

---

_Review comment by @covracer on `crates/ruff/src/rules/ruff/rules/confusables.rs`:2121 on 2023-06-03 11:50_

The list from VS Code covers Kelvin with `(8490, 75)`.

---

_@covracer reviewed on 2023-06-03 11:50_

---

_Review requested from @charliermarsh by @covracer on 2023-06-03 12:22_

---

_Comment by @zanieb on 2023-10-19 16:16_

I believe we were concerned about increasing the size of the map here but with use of `match` it shouldn't matter anymore? @covracer would you be interested in updating with the latest changes on `main` and we can try to get this merged?

---

_Assigned to @charliermarsh by @charliermarsh on 2023-11-03 04:42_

---

_Merged by @charliermarsh on 2023-11-03 04:58_

---

_Closed by @charliermarsh on 2023-11-03 04:58_

---

_Comment by @charliermarsh on 2023-11-03 04:59_

Thanks, and sorry for the extensive delay here.

---

_Comment by @github-actions[bot] on 2023-11-03 05:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+9 -0 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/644813cd2e149f53ebfefc2e68ef60d421608ab4/examples/interaction/tools/subcoordinates_zoom.py#L22'>examples/interaction/tools/subcoordinates_zoom.py:22:23:</a> RUF001 String contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/0cdb37c445fc0a0a2144f981fd1acbf4f07d88ee/pandas/_libs/tslibs/timedeltas.pyi#L58'>pandas/_libs/tslibs/timedeltas.pyi:58:6:</a> RUF001 String contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
+ <a href='https://github.com/pandas-dev/pandas/blob/0cdb37c445fc0a0a2144f981fd1acbf4f07d88ee/pandas/core/indexes/base.py#L5352'>pandas/core/indexes/base.py:5352:26:</a> RUF003 Comment contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
+ <a href='https://github.com/pandas-dev/pandas/blob/0cdb37c445fc0a0a2144f981fd1acbf4f07d88ee/pandas/core/indexes/base.py#L5353'>pandas/core/indexes/base.py:5353:46:</a> RUF003 Comment contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
+ <a href='https://github.com/pandas-dev/pandas/blob/0cdb37c445fc0a0a2144f981fd1acbf4f07d88ee/pandas/core/indexes/base.py#L5354'>pandas/core/indexes/base.py:5354:69:</a> RUF003 Comment contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
+ <a href='https://github.com/pandas-dev/pandas/blob/0cdb37c445fc0a0a2144f981fd1acbf4f07d88ee/pandas/tests/io/test_clipboard.py#L67'>pandas/tests/io/test_clipboard.py:67:34:</a> RUF001 String contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
+ <a href='https://github.com/pandas-dev/pandas/blob/0cdb37c445fc0a0a2144f981fd1acbf4f07d88ee/pandas/tests/scalar/timedelta/test_constructors.py#L306'>pandas/tests/scalar/timedelta/test_constructors.py:306:25:</a> RUF001 String contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/0b3f7a5a6bc0736ec8eb209fe951f487969e86a6/zerver/tests/test_invite.py#L1281'>zerver/tests/test_invite.py:1281:32:</a> RUF001 String contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
+ <a href='https://github.com/zulip/zulip/blob/0b3f7a5a6bc0736ec8eb209fe951f487969e86a6/zerver/tests/test_signup.py#L1021'>zerver/tests/test_signup.py:1021:29:</a> RUF001 String contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF001 | 6 | 6 | 0 | 0 | 0 |
| RUF003 | 3 | 3 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+9 -0 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/644813cd2e149f53ebfefc2e68ef60d421608ab4/examples/interaction/tools/subcoordinates_zoom.py#L22'>examples/interaction/tools/subcoordinates_zoom.py:22:23:</a> RUF001 String contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/0cdb37c445fc0a0a2144f981fd1acbf4f07d88ee/pandas/_libs/tslibs/timedeltas.pyi#L58'>pandas/_libs/tslibs/timedeltas.pyi:58:6:</a> RUF001 String contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
+ <a href='https://github.com/pandas-dev/pandas/blob/0cdb37c445fc0a0a2144f981fd1acbf4f07d88ee/pandas/core/indexes/base.py#L5352'>pandas/core/indexes/base.py:5352:26:</a> RUF003 Comment contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
+ <a href='https://github.com/pandas-dev/pandas/blob/0cdb37c445fc0a0a2144f981fd1acbf4f07d88ee/pandas/core/indexes/base.py#L5353'>pandas/core/indexes/base.py:5353:46:</a> RUF003 Comment contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
+ <a href='https://github.com/pandas-dev/pandas/blob/0cdb37c445fc0a0a2144f981fd1acbf4f07d88ee/pandas/core/indexes/base.py#L5354'>pandas/core/indexes/base.py:5354:69:</a> RUF003 Comment contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
+ <a href='https://github.com/pandas-dev/pandas/blob/0cdb37c445fc0a0a2144f981fd1acbf4f07d88ee/pandas/tests/io/test_clipboard.py#L67'>pandas/tests/io/test_clipboard.py:67:34:</a> RUF001 String contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
+ <a href='https://github.com/pandas-dev/pandas/blob/0cdb37c445fc0a0a2144f981fd1acbf4f07d88ee/pandas/tests/scalar/timedelta/test_constructors.py#L306'>pandas/tests/scalar/timedelta/test_constructors.py:306:25:</a> RUF001 String contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/0b3f7a5a6bc0736ec8eb209fe951f487969e86a6/zerver/tests/test_invite.py#L1281'>zerver/tests/test_invite.py:1281:32:</a> RUF001 String contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
+ <a href='https://github.com/zulip/zulip/blob/0b3f7a5a6bc0736ec8eb209fe951f487969e86a6/zerver/tests/test_signup.py#L1021'>zerver/tests/test_signup.py:1021:29:</a> RUF001 String contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF001 | 6 | 6 | 0 | 0 | 0 |
| RUF003 | 3 | 3 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @charliermarsh on 2023-11-03 12:58_

Note to self: revisit these ecosystem changes today.

---

_Comment by @charliermarsh on 2023-11-03 13:50_

\cc @zanieb - Do you have an opinion on whether this should be a preview change...?

---

_Comment by @zanieb on 2023-11-03 15:12_

@charliermarsh looks like it creates new violations and it's not a bug fix so it should probably be preview?

---

_Comment by @charliermarsh on 2023-11-03 16:09_

Done in https://github.com/astral-sh/ruff/pull/8473.

---

_Branch deleted on 2023-11-30 15:21_

---

_Comment by @covracer on 2023-11-30 16:12_

Thanks! I've looked over the hits and made a best guess at how to proceed. Over time I'll try to test and submit Pull Requests.

> [examples/interaction/tools/subcoordinates_zoom.py:22:23:](https://github.com/bokeh/bokeh/blob/644813cd2e149f53ebfefc2e68ef60d421608ab4/examples/interaction/tools/subcoordinates_zoom.py#L22) RUF001 String ...

I would suggest applying the fix for this abbreviation of microvolts in [tooltip text](https://docs.bokeh.org/en/latest/docs/examples/interaction/tools/subcoordinates_zoom.html).

> [pandas/_libs/tslibs/timedeltas.pyi:58:6:](https://github.com/pandas-dev/pandas/blob/0cdb37c445fc0a0a2144f981fd1acbf4f07d88ee/pandas/_libs/tslibs/timedeltas.pyi#L58) RUF001 String ...
> [pandas/tests/scalar/timedelta/test_constructors.py:306:25:](https://github.com/pandas-dev/pandas/blob/0cdb37c445fc0a0a2144f981fd1acbf4f07d88ee/pandas/tests/scalar/timedelta/test_constructors.py#L306) RUF001 String ...

timedeltas.pyi and timedeltas.pyx list many aliases for microseconds (among other units). test_constructors.py tests using the aliases. I would suggest `# noqa: RUF001`.

> [pandas/core/indexes/base.py:5352:26:](https://github.com/pandas-dev/pandas/blob/0cdb37c445fc0a0a2144f981fd1acbf4f07d88ee/pandas/core/indexes/base.py#L5352) RUF003 Comment ...
> [pandas/core/indexes/base.py:5353:46:](https://github.com/pandas-dev/pandas/blob/0cdb37c445fc0a0a2144f981fd1acbf4f07d88ee/pandas/core/indexes/base.py#L5353) RUF003 Comment ...
> [pandas/core/indexes/base.py:5354:69:](https://github.com/pandas-dev/pandas/blob/0cdb37c445fc0a0a2144f981fd1acbf4f07d88ee/pandas/core/indexes/base.py#L5354) RUF003 Comment ...

This multiline comment documents performance analysis timings. I would suggest applying the fix.

> [pandas/tests/io/test_clipboard.py:67:34:](https://github.com/pandas-dev/pandas/blob/0cdb37c445fc0a0a2144f981fd1acbf4f07d88ee/pandas/tests/io/test_clipboard.py#L67) RUF001 String ...

This is part of `dict[str, list[str]]` test data for `utf8`. ASCII and not-confusable Latin-1 Supplement characters are included alongside characters from higher Unicode blocks. I would suggest applying the fix.

> [zerver/tests/test_invite.py:1281:32:](https://github.com/zulip/zulip/blob/0b3f7a5a6bc0736ec8eb209fe951f487969e86a6/zerver/tests/test_invite.py#L1281) RUF001 String ...
> [zerver/tests/test_signup.py:1021:29:](https://github.com/zulip/zulip/blob/0b3f7a5a6bc0736ec8eb209fe951f487969e86a6/zerver/tests/test_signup.py#L1021) RUF001 String ...

These tests use the same copy+pasted string to test "non-ASCII characters", 3 from the Basic Latin block and 6 from the Latin-1 Supplement block (5 of which aren't flagged as confusable). Unless the project would like to begin testing characters from higher blocks, I would suggest dropping the character or adding `# noqa: RUF001`.

---

_Comment by @covracer on 2024-04-26 11:52_

Bokeh updated in https://github.com/bokeh/bokeh/pull/13668

---

_Comment by @charliermarsh on 2024-04-26 13:16_

Thanks @covracer!

---
