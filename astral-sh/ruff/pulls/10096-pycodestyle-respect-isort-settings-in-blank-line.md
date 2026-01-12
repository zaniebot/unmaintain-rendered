```yaml
number: 10096
title: "[`pycodestyle`] Respect `isort` settings in blank line rules (`E3*`)"
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: blank-lines-isort
created_at: 2024-02-23T13:20:08Z
updated_at: 2024-03-05T10:20:59Z
url: https://github.com/astral-sh/ruff/pull/10096
synced_at: 2026-01-12T15:55:31Z
```

# [`pycodestyle`] Respect `isort` settings in blank line rules (`E3*`)

---

_@MichaReiser_

## Summary

This PR changes the `E3*` rules to respect the `isort` `lines-after-imports` and `lines-between-types` settings. Specifically, the following rules required changing

* `TooManyBlannkLines` : Respects both settings.
* `BlankLinesTopLevel`: Respects `lines-after-imports`. Doesn't need to respect `lines-between-types` because it only applies to classes and functions 


The downside of this approach is that `isort` and the blank line rules emit a diagnostic when there are too many blank lines. The fixes aren't identical, the blank line is less opinionated, but blank lines accepts the fix of `isort`.

<details>
	<summary>Outdated approach</summary>
Fixes https://github.com/astral-sh/ruff/issues/10077#issuecomment-1961266981

This PR changes the blank line rules to not enforce the number of blank lines after imports (top-level) if isort is enabled and leave it to isort to enforce the right number of lines (depends on the `isort.lines-after-imports` and `isort.lines-between-types` settings). 

The reason to give `isort` precedence over the blank line rules is that they are configurable. Users that always want to blank lines after imports can use `isort.lines-after-imports=2` to enforce that (specifically for imports). 

This PR does not fix the incompatibility with the formatter in pyi files that only uses 0 to 1 blank lines. I'll address this separately.

</details>

## Review
The first commit is a small refactor that simplified implementing the fix (and makes it easier to reason about what's mutable and what's not).


## Test Plan

I added a new test and verified that it fails with an error that the fix never converges. I verified the snapshot output after implementing the fix.


---

_Label `bug` added by @MichaReiser on 2024-02-23 13:20_

---

_Label `preview` added by @MichaReiser on 2024-02-23 13:20_

---

_@MichaReiser reviewed on 2024-02-23 13:22_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:668 on 2024-02-23 13:22_

The state update logic used to be inside of `check_line`. Moving the state handling out has the benefit that we can skip calling `check_line` and it also separates the state update logic from the actual checking logic which I find easier to understand.

---

_Review requested from @charliermarsh by @MichaReiser on 2024-02-23 13:22_

---

_Comment by @github-actions[bot] on 2024-02-23 13:41_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+23 -6 violations, +0 -0 fixes in 3 projects; 40 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+14 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/ff40c066fb429e113418e42698ced58ba71a6883/airflow/decorators/__init__.pyi#L102'>airflow/decorators/__init__.pyi:102:5:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/apache/airflow/blob/ff40c066fb429e113418e42698ced58ba71a6883/airflow/decorators/__init__.pyi#L105'>airflow/decorators/__init__.pyi:105:5:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/apache/airflow/blob/ff40c066fb429e113418e42698ced58ba71a6883/airflow/decorators/__init__.pyi#L158'>airflow/decorators/__init__.pyi:158:5:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/apache/airflow/blob/ff40c066fb429e113418e42698ced58ba71a6883/airflow/decorators/__init__.pyi#L191'>airflow/decorators/__init__.pyi:191:5:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/apache/airflow/blob/ff40c066fb429e113418e42698ced58ba71a6883/airflow/decorators/__init__.pyi#L203'>airflow/decorators/__init__.pyi:203:5:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/apache/airflow/blob/ff40c066fb429e113418e42698ced58ba71a6883/airflow/decorators/__init__.pyi#L257'>airflow/decorators/__init__.pyi:257:5:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/apache/airflow/blob/ff40c066fb429e113418e42698ced58ba71a6883/airflow/decorators/__init__.pyi#L294'>airflow/decorators/__init__.pyi:294:5:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/apache/airflow/blob/ff40c066fb429e113418e42698ced58ba71a6883/airflow/decorators/__init__.pyi#L315'>airflow/decorators/__init__.pyi:315:5:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/apache/airflow/blob/ff40c066fb429e113418e42698ced58ba71a6883/airflow/decorators/__init__.pyi#L465'>airflow/decorators/__init__.pyi:465:5:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/apache/airflow/blob/ff40c066fb429e113418e42698ced58ba71a6883/airflow/decorators/__init__.pyi#L633'>airflow/decorators/__init__.pyi:633:5:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/apache/airflow/blob/ff40c066fb429e113418e42698ced58ba71a6883/airflow/decorators/__init__.pyi#L668'>airflow/decorators/__init__.pyi:668:5:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/apache/airflow/blob/ff40c066fb429e113418e42698ced58ba71a6883/airflow/decorators/__init__.pyi#L688'>airflow/decorators/__init__.pyi:688:5:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/apache/airflow/blob/ff40c066fb429e113418e42698ced58ba71a6883/airflow/decorators/__init__.pyi#L720'>airflow/decorators/__init__.pyi:720:5:</a> E301 [*] Expected 1 blank line, found 0
+ <a href='https://github.com/apache/airflow/blob/ff40c066fb429e113418e42698ced58ba71a6883/airflow/decorators/__init__.pyi#L89'>airflow/decorators/__init__.pyi:89:5:</a> E301 [*] Expected 1 blank line, found 0
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/resources.py#L256'>src/bokeh/resources.py:256:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/plugins/managed_server_loop.py#L59'>tests/support/plugins/managed_server_loop.py:59:1:</a> E302 [*] Expected 2 blank lines, found 1
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/plugins/project.py#L134'>tests/support/plugins/project.py:134:1:</a> E302 [*] Expected 2 blank lines, found 1
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+6 -6 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pandas-dev/pandas/blob/89b286a699b2d023b7a1ebc468abf230d84ad547/pandas/_libs/algos.pyi#L100'>pandas/_libs/algos.pyi:100:5:</a> PLR0917 Too many positional arguments (8/5)
+ <a href='https://github.com/pandas-dev/pandas/blob/89b286a699b2d023b7a1ebc468abf230d84ad547/pandas/_libs/algos.pyi#L101'>pandas/_libs/algos.pyi:101:5:</a> PLR0917 Too many positional arguments (8/5)
- <a href='https://github.com/pandas-dev/pandas/blob/89b286a699b2d023b7a1ebc468abf230d84ad547/pandas/_libs/algos.pyi#L110'>pandas/_libs/algos.pyi:110:5:</a> PLR0917 Too many positional arguments (7/5)
+ <a href='https://github.com/pandas-dev/pandas/blob/89b286a699b2d023b7a1ebc468abf230d84ad547/pandas/_libs/algos.pyi#L111'>pandas/_libs/algos.pyi:111:5:</a> PLR0917 Too many positional arguments (7/5)
- <a href='https://github.com/pandas-dev/pandas/blob/89b286a699b2d023b7a1ebc468abf230d84ad547/pandas/_libs/tslibs/offsets.pyi#L271'>pandas/_libs/tslibs/offsets.pyi:271:9:</a> PLR0917 Too many positional arguments (6/5)
+ <a href='https://github.com/pandas-dev/pandas/blob/89b286a699b2d023b7a1ebc468abf230d84ad547/pandas/_libs/tslibs/offsets.pyi#L279'>pandas/_libs/tslibs/offsets.pyi:279:9:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/pandas-dev/pandas/blob/89b286a699b2d023b7a1ebc468abf230d84ad547/pandas/_libs/tslibs/offsets.pyi#L285'>pandas/_libs/tslibs/offsets.pyi:285:9:</a> PLR0917 Too many positional arguments (6/5)
+ <a href='https://github.com/pandas-dev/pandas/blob/89b286a699b2d023b7a1ebc468abf230d84ad547/pandas/_libs/tslibs/offsets.pyi#L294'>pandas/_libs/tslibs/offsets.pyi:294:9:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/pandas-dev/pandas/blob/89b286a699b2d023b7a1ebc468abf230d84ad547/pandas/_libs/tslibs/offsets.pyi#L297'>pandas/_libs/tslibs/offsets.pyi:297:9:</a> PLR0917 Too many positional arguments (6/5)
+ <a href='https://github.com/pandas-dev/pandas/blob/89b286a699b2d023b7a1ebc468abf230d84ad547/pandas/_libs/tslibs/offsets.pyi#L306'>pandas/_libs/tslibs/offsets.pyi:306:9:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/pandas-dev/pandas/blob/89b286a699b2d023b7a1ebc468abf230d84ad547/pandas/_libs/tslibs/offsets.pyi#L309'>pandas/_libs/tslibs/offsets.pyi:309:9:</a> PLR0917 Too many positional arguments (8/5)
+ <a href='https://github.com/pandas-dev/pandas/blob/89b286a699b2d023b7a1ebc468abf230d84ad547/pandas/_libs/tslibs/offsets.pyi#L318'>pandas/_libs/tslibs/offsets.pyi:318:9:</a> PLR0917 Too many positional arguments (8/5)
</pre>

</p>
</details>
<details><summary>Changes by rule (3 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E301 | 14 | 14 | 0 | 0 | 0 |
| PLR0917 | 12 | 6 | 6 | 0 | 0 |
| E302 | 3 | 3 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Converted to draft by @MichaReiser on 2024-02-23 13:51_

---

_Comment by @MichaReiser on 2024-02-23 13:52_

Hmm, it's slightly more complicated than that... 

```
if TYPE_CHECKING:
    from typing_extensions import TypeAlias

def model_link(fullname: str) -> str:
    # (double) escaped space at the end is to appease Sphinx
    # https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html#gotchas
    return f":class:`~{fullname}`\\ "
```

It doesn't handle the above correctly (should raise an error) because it assumes that it is after an import. 

---

_Marked ready for review by @MichaReiser on 2024-02-23 14:07_

---

_Comment by @charliermarsh on 2024-02-26 21:13_

I don't know that I have better suggestions, but I'll admit that I'm nervous to make the rule contingent on whether another rule is _enabled_, since it means `ruff check --select E` and `ruff check --select E --select I` will show discontinuous diagnostics, which can make for a really confusing debugging experience.


---

_Comment by @MichaReiser on 2024-02-26 21:15_

Hmm that's a good point. The alternative is to never touch blank lines after imports and direct users towards isort? Although that's a bit overkill if all you want is 2 blank lines 

---

_Comment by @MichaReiser on 2024-02-27 12:50_

So the options I see are

* Remove the blank line rules 
* Never enforce blank lines after or between imports (forcing users to use isort)
* Disable blank lines when isort is activated (what we do for other incompatible rules)
* Use `isort.lines_after_imports` or `isort.lines_between_types` after imports or between imports in the blank line rule (A bit weird that it respects options from another rule)


---

_Comment by @hoel-bagard on 2024-02-27 13:13_

> Use `isort.lines_after_imports` or `isort.lines_between_types` after imports or between imports in the blank line rule (A bit weird that it respects options from another rule)

I was thinking about that one. As you said it's a bit weird to use options from another rule, but that allows using both rule sets.
Also, this weirdness would likely not be visible to a user (it seems unlikely to me that someone would specify the isort options but not select isort), so it would be a matter of handling the added complexity in the blank rules in a way that is maintainable.

> Never enforce blank lines after or between imports (forcing users to use isort)

This option is seems more straightforward to implement, document and maintain.

---

_Renamed from "Disable Blank line rules for top level statements after imports" to "[`pycodestyle`] Respect `isort` settings in blank line rules (`E3*`)" by @MichaReiser on 2024-03-01 15:40_

---

_Comment by @MichaReiser on 2024-03-01 16:24_

The ecosystem changes look okay to me. It's not the main change but i must have fixed those rules by setting `self.follows` to `Other` when the indent decreased.

---

_Review request for @charliermarsh removed by @MichaReiser on 2024-03-01 16:24_

---

_Review requested from @charliermarsh by @MichaReiser on 2024-03-01 16:24_

---

_Comment by @MichaReiser on 2024-03-01 16:26_

@hoel-bagard would you mind taking a look at the changes? I think you know the rule the best.

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:155 on 2024-03-03 10:19_

```suggestion
/// * [`lint.isort.lines-between-types`]: For `import` statements directly following a `from..import` statement or vice versa.
```

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:825 on 2024-03-03 12:28_

Just to clarify, this means that blank lines is more lenient than isort if `lines_between_types == 0`, but uses the value from isort otherwise. Which means that `E303` can be "stricter" (only accept a single blank line) 
when `lines_between_types == 1` than when  `lines_between_types == 0` ?

I'm guessing that this is to avoid blank lines triggering on something formatted by isort if `lines_between_types > max_lines_level`. Wouldn't it be simpler to use whichever value is highest ?

```suggestion
            std::cmp::max(
                max_lines_level,
                u32::try_from(self.lines_between_types).unwrap_or(u32::MAX),
            )
```

---

_@hoel-bagard reviewed on 2024-03-03 12:39_

The changes look good to me, I just had two questions about parts that weren't clear to me.

Also, not important but some ifs were over indented so I made [a PR to fix that](#10208).

---

_Review comment by @hoel-bagard on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:761 on 2024-03-03 12:40_

I'm not sure I understand this justification. I would have expected the default/automatic value from isort instead of `BLANK_LINES_TOP_LEVEL`.

---

_@hoel-bagard reviewed on 2024-03-03 12:40_

---

_@charliermarsh reviewed on 2024-03-04 15:33_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:155 on 2024-03-04 15:33_

I think elsewhere we use `from ... import ...` to refer to these.

---

_@charliermarsh approved on 2024-03-04 15:34_

---

_Comment by @sciyoshi on 2024-03-04 19:32_

Just to confirm, this change won't fix the issue in #10077, correct? I'm testing things out with this branch but still see the infinite loop when running on .pyi files.

---

_Comment by @MichaReiser on 2024-03-05 09:26_

> Just to confirm, this change won't fix the issue in #10077, correct? I'm testing things out with this branch but still see the infinite loop when running on .pyi files.

Nice find. It seems specific to a class coming after imports when using `lines-after-imports=1` Let me revisit the tests to see why this case isn't covered.

---

_Comment by @MichaReiser on 2024-03-05 09:45_

> Just to confirm, this change won't fix the issue in #10077, correct? I'm testing things out with this branch but still see the infinite loop when running on .pyi files.

Okay, the issue is specific to pyi files and is fixed by https://github.com/astral-sh/ruff/pull/10098

---

_@MichaReiser reviewed on 2024-03-05 09:56_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:825 on 2024-03-05 09:56_

Yeah, I think that's a very reasonable simplification to make. The rule doesn't need to match `isort` exactly, it just need to accept isorted code

---

_@MichaReiser reviewed on 2024-03-05 09:58_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs`:761 on 2024-03-05 09:58_

I expanded the comment explaining the `isort` logic. `isort` defaults to 2 if before a class or function definition but that wasn't clear from my original comment.

---

_Comment by @MichaReiser on 2024-03-05 09:58_

@hoel-bagard thank you for your review and fixing the indentation.

---

_Merged by @MichaReiser on 2024-03-05 10:09_

---

_Closed by @MichaReiser on 2024-03-05 10:09_

---

_Branch deleted on 2024-03-05 10:09_

---
