```yaml
number: 3005
title: "chore: Use `LF` on all platforms "
type: pull_request
state: merged
author: MichaReiser
labels: []
assignees: []
merged: true
base: main
head: git-attributes
created_at: 2023-02-18T10:08:25Z
updated_at: 2023-02-21T07:57:13Z
url: https://github.com/astral-sh/ruff/pull/3005
synced_at: 2026-01-12T15:55:12Z
```

# chore: Use `LF` on all platforms 

---

_@MichaReiser_

I worked on #2993 and ran into issues that the formatter tests are failing on Windows because `writeln!` emits `\n` as line terminator on all platforms, but `git` on Windows converted the line endings in the snapshots to `\r\n`.

I then tried to replicate the issue on my Windows machine and was surprised that all linter snapshot tests are failing on my machine. I figured out after some time that it is due to my global git config keeping the input line endings rather than converting to `\r\n`. 

Luckily, I've been made aware of #2033 which introduced an "override" for the `assert_yaml_snapshot` macro that normalizes new lines, by splitting the formatted string using the platform-specific newline character. This is a clever approach and gives nice diffs for multiline fixes but makes assumptions about the setup contributors use and requires special care whenever we use line endings inside of tests. 

I recommend that we remove the special new line handling and use `.gitattributes` to enforce the use of `LF` on all platforms [guide](https://docs.github.com/en/get-started/getting-started-with-git/configuring-git-to-handle-line-endings). This gives us platform agnostic tests without having to worry about line endings in our tests or different git configurations.

## Note

It may be necessary for Windows contributors to run the following command to update the line endings of their files

```bash
git rm --cached -r .
git reset --hard
```



---

_Renamed from "other: Use `LF` on all platforms " to "test: Use `LF` on all platforms " by @MichaReiser on 2023-02-18 10:32_

---

_Renamed from "test: Use `LF` on all platforms " to "chore: Use `LF` on all platforms " by @MichaReiser on 2023-02-18 10:32_

---

_@MichaReiser reviewed on 2023-02-18 11:06_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pycodestyle/mod.rs`:71 on 2023-02-18 11:06_

This is the only test where the output depends on where the test is run. That's why I'm handling this test specifically.

---

_Marked ready for review by @MichaReiser on 2023-02-18 11:06_

---

_Comment by @charliermarsh on 2023-02-18 11:07_

\cc @sbrugman who has the context from #2033 

---

_Comment by @sbrugman on 2023-02-18 11:30_

This is indeed an improvement over the current situation for developers, where we don't account for git settings that may have been overwritten. 

The most important thing is that we do keep testing crlf functionality (which is detected from the file regardless of platform). Will this still test that new changes work with both type of line endings?
(I think our tests failed correctly)

---

_@sbrugman reviewed on 2023-02-18 11:50_

---

_Review comment by @sbrugman on `crates/ruff/src/rules/pycodestyle/mod.rs`:71 on 2023-02-18 11:50_

In https://github.com/charliermarsh/ruff/pull/2033 we chose not to use a special line ending token. In this case however, since it's only the approach taken there would not much differ. (cc @andersk who came up with the idea to split newlines).

---

_Comment by @MichaReiser on 2023-02-18 14:22_

> The most important thing is that we do keep testing crlf functionality (which is detected from the file regardless of platform). Will this still test that new changes work with both type of line endings?

Hmm, that's a good point. No, tests will run with the same line ending as they have in the committed `resources` folder. I think that's an improvement because it guarantees that tests have a predictable outcome across platforms and contributors can reproduce and fix test failures regardless of the platform they're using. However, it means that we reduced the CR LF test coverage. We can do a few things if we want to keep that coverage high (and potentially combine these approaches).

1. Write unit tests that assert the specific behavior that should be different between files using LF and CR LF 
1. Create a spec test that explicitly uses CR LF file endings. Add a `.gitattributes` file to that directory that enforces CR LF file endings or override the file in the root `.gitattributes` file
1. Change our linter (and potentially formatter) test to run twice, once with the content as it is on disk and a second time where the `LF`s are replaced with `CR LF`. 

I recommend going with 1 and 2 (there's already a test that asserts CR LF behavior)  but I don't know how much a source of bugs the line endings have been in the past that would warrant running all tests twice 

---

_Comment by @charliermarsh on 2023-02-19 15:16_

Yeah this makes sense to me. I think we should add a handful of additional, dedicated tests for CRLF line endings, and mark those explicitly as you've done here. But I'd be fine to merge this and add those in a future PR.

> I recommend going with 1 and 2 (there's already a test that asserts CR LF behavior) but I don't know how much a source of bugs the line endings have been in the past that would warrant running all tests twice

I would say it hasn't been a _major_ source of bugs, especially since we introduced the `stylist` pattern which abstracts away some of the newline handling, _and_ we established the pattern of consistently passing newline information to LibCST's `CodegenState`. Now that those patterns exist and are prevalent across the codebase, they tend to be properly picked up in all new code changes.

(FWIW, there's at least one _known_ issue around newline handling which is that the `textwrap` module always uses LF newlines, so we have to do extra work whenever we use the `indent` or `dedent` methods. This bit us once, creating an infinite fix loop for import sorting. So it might be worth ensuring we have a CRLF for at least one `isort` test.)


---

_Merged by @charliermarsh on 2023-02-20 20:13_

---

_Closed by @charliermarsh on 2023-02-20 20:13_

---

_Comment by @MichaReiser on 2023-02-21 07:57_

> (FWIW, there's at least one known issue around newline handling which is that the textwrap module always uses LF newlines, so we have to do extra work whenever we use the indent or dedent methods. This bit us once, creating an infinite fix loop for import sorting. So it might be worth ensuring we have a CRLF for at least one isort test.)

That makes sense. I further recommend patching/forking `textwrap`, adding functionality to support different line endings, and testing the handling of different line endings. This will prevent contributors from running into the same issue and gives us test coverage close to the implementation. 

---
