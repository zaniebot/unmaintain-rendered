```yaml
number: 2993
title: "test(ruff_python_formatter): Run all Black tests"
type: pull_request
state: merged
author: MichaReiser
labels: []
assignees: []
merged: true
base: main
head: micha/formatter-tests
created_at: 2023-02-17T18:16:57Z
updated_at: 2023-02-22T14:25:06Z
url: https://github.com/astral-sh/ruff/pull/2993
synced_at: 2026-01-12T15:55:12Z
```

# test(ruff_python_formatter): Run all Black tests

---

_@MichaReiser_

This PR changes the testing infrastructure to run all black tests and:

* Pass if Ruff and Black generate the same formatting
* Fail and write a markdown snapshot that shows the input code, the differences between Black and Ruff, Ruffs output, and Blacks output

This is achieved by introducing a new `fixture` macro (open to better name suggestions) that "duplicates" the attributed test for every file that matches the specified glob pattern. Creating a new test for each file over having a test that iterates over all files has the advantage that you can run a single test, and that test failures indicate which case is failing. 

The `fixture` macro also makes it straightforward to e.g. setup our own spec tests that test very specific formatting by creating a new folder and use insta to assert the formatted output.

---

_Review comment by @MichaReiser on `Cargo.toml`:3 on 2023-02-17 18:17_

TODO: Move to its own PR

---

_@MichaReiser reviewed on 2023-02-17 18:17_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/snapshots/ruff_python_formatter__tests__black_test__attribute_access_on_number_literals_py.snap`:9 on 2023-02-17 18:20_

You can override the filetype for .py.snap in your JetBrains IDE to get markdown rendering

![image](https://user-images.githubusercontent.com/1203881/219748195-d1f71cc4-fc07-41c3-bcb5-0c92af14d470.png)


---

_@MichaReiser reviewed on 2023-02-17 18:20_

---

_@MichaReiser reviewed on 2023-02-17 18:21_

---

_Review comment by @MichaReiser on `crates/ruff_testing_macros/src/lib.rs`:171 on 2023-02-17 18:21_

This is where all the fun of the new macro is happening

---

_Comment by @MichaReiser on 2023-02-18 09:21_

Hmm. The windows tests are all failing now because the markdown file is written using `writeln` that always inserts `\n` but the checked out snapshots use `\cr\n` because we use no `.gitattributes` file. 

I tried to add a `.gitattributes` file that ensures that all platforms use the same line endings when checking out the repository. 

```
* text=auto eol=auto

crates/ruff/resources/test/fixtures/isort/line_ending_crlf.py text eol=crlf
crates/ruff/resources/test/fixtures/pycodestyle/W605_1.py text eol=crlf

ruff.schema.json linguist-generated=true text=auto eol=lf
```

But now many of the linter tests are failing which seems a bit frightening. Does it mean that ruff doesn't correctly handle the case where a project uses the non-default line ending of the operating system?

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Differences ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot file: crates\ruff\src\rules\flake8_comprehensions\snapshots\ruff__rules__flake8_comprehensions__tests__C400_C400.py.snap
Snapshot: C400_C400.py
Source: C:\Users\Micha\git\ruff:39
───────────────────────────────────────────────────────────────────────────────
Expression: diagnostics
───────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬──────────────────────────────────────────────────────────────────
   24    24 │     row: 4
   25    25 │     column: 1
   26    26 │   fix:
   27    27 │     content:
   28       │-      - "["
   29       │-      - "    x for x in range(3)"
   30       │-      - "]"
         28 │+      - "[\n    x for x in range(3)\n]"
   31    29 │     location:
   32    30 │       row: 2
   33    31 │       column: 4
   34    32 │     end_location:
────────────┴──────────────────────────────────────────────────────────────────
```

Any ideas where I should start looking? Is this in the parser or does the `stylist` infer the wrong line endings (why are new lines embedded in the string literal)?

---

_Comment by @andersk on 2023-02-18 09:29_

#2033 has some relevant context, especially the `add_redaction` call in our `assert_yaml_snapshot` wrapper.

---

_Comment by @MichaReiser on 2023-02-18 10:02_

> #2033 has some relevant context, especially the `add_redaction` call in our `assert_yaml_snapshot` wrapper.

Ohh thanks! It would have taken me forever to find this

---

_Comment by @MichaReiser on 2023-02-18 11:07_

Depends on #3005 to fix the windows build or we must change this PR to explicitly handle the line ending character on different platforms.

---

_@MichaReiser reviewed on 2023-02-21 08:10_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/lib.rs`:87 on 2023-02-21 08:10_

There are now more tests that panic than when I initially created this PR. I think it would be good for us to fix the panics first to get better visibility of our changes (and avoid having any new tests that cause panics)

---

_Marked ready for review by @MichaReiser on 2023-02-21 08:27_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/lib.rs`:135 on 2023-02-21 16:06_

This piece is just for convenience, right? Since it's a deterministic function of the Ruff and Black output included below?

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/lib.rs`:190 on 2023-02-21 16:06_

This is a nice pattern.

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/snapshots/ruff_python_formatter__tests__black_test__beginning_backslash_py.snap`:46 on 2023-02-21 16:07_

Do we have control over these trailing newlines in the snapshot?

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/snapshots/ruff_python_formatter__tests__black_test__beginning_backslash_py.snap`:43 on 2023-02-21 16:08_

I think there may've been some kind of error in the `.expect` creation script that made the original snapshots which led to us retaining trailing newlines in the Black output. (Black trims these.) Doesn't have to be fixed in this PR, but we'll need to fix them.

---

_Review comment by @charliermarsh on `crates/ruff_testing_macros/Cargo.toml`:2 on 2023-02-21 16:09_

What are the tradeoffs RE adding a new crate vs. including these in `ruff_macros`?

---

_@charliermarsh reviewed on 2023-02-21 16:12_

This is fantastic and completely solves the problems I've been running into with testing and iteration.

---

_@MichaReiser reviewed on 2023-02-22 07:08_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/lib.rs`:135 on 2023-02-22 07:08_

That's correct. I found it useful when working on rome formatter to have all information in a single file, so that you don't need to search for the expected output file. 

---

_@MichaReiser reviewed on 2023-02-22 07:09_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/lib.rs`:190 on 2023-02-22 07:09_

Thanks. It may be familiar from working on the formatter ;) 

---

_@MichaReiser reviewed on 2023-02-22 07:16_

---

_Review comment by @MichaReiser on `crates/ruff_testing_macros/Cargo.toml`:2 on 2023-02-22 07:16_

In my view it mainly comes down if you prefer having fewer crates or more fine granular crates. 

My preference is to have more fine granular crates because:

* Smaller crates mean there are fewer changes that require a re-compile of its code
* You can be explicit about dependencies
  * the testing macros don't need to depend on itertools
  * The test macros can be a `dev` dependency, ensuring that its functionality isn't used in production code
* I find smaller crates help navigate the code base and specific names tell you if this crate is the right crate for your code addition

The main downside is that we end up with more crates, resulting in a longer `crates` list. This doesn't seem to be a problem today. 

---

_@MichaReiser reviewed on 2023-02-22 07:41_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/snapshots/ruff_python_formatter__tests__black_test__beginning_backslash_py.snap`:46 on 2023-02-22 07:41_

I would have thought so but that doesn't seem to be the case. Using `trim_end()`  on the generated content doesn't change anything and the two trailing new lines are present in other snapshot tests. 


---

_@MichaReiser reviewed on 2023-02-22 07:49_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/snapshots/ruff_python_formatter__tests__black_test__beginning_backslash_py.snap`:43 on 2023-02-22 07:49_

Done. I added an editorconfig file that sets `final_new_line = false` for the expect files. 

---

_Merged by @charliermarsh on 2023-02-22 14:25_

---

_Closed by @charliermarsh on 2023-02-22 14:25_

---
