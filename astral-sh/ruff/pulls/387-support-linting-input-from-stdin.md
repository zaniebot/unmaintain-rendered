```yaml
number: 387
title: Support linting input from stdin
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: support-stdin
created_at: 2022-10-10T13:46:00Z
updated_at: 2022-10-11T17:28:25Z
url: https://github.com/astral-sh/ruff/pull/387
synced_at: 2026-01-12T05:48:45Z
```

# Support linting input from stdin

---

_Pull request opened by @harupy on 2022-10-10 13:46_

Resolve #249

Test:

https://user-images.githubusercontent.com/17039389/194880579-70cb418c-142a-4bfe-b331-1e1a51e23e68.mov


---

_Comment by @charliermarsh on 2022-10-10 14:04_

Wooo!

---

_Comment by @charliermarsh on 2022-10-10 21:24_

@harupy - Is this ready for review?

---

_Marked ready for review by @harupy on 2022-10-10 23:18_

---

_Comment by @harupy on 2022-10-10 23:18_

@charliermarsh Yes!

---

_Comment by @harupy on 2022-10-11 12:31_

@charliermarsh Do you think we need tests?

```rust
#[cfg(test)]
mod tests {
    use std::str;

    use anyhow::Result;
    use assert_cmd::Command;

    #[test]
    fn test_stdin_success() -> Result<()> {
        let mut cmd = Command::cargo_bin("ruff")?;
        cmd.args(&["-"]).write_stdin("").assert().success();
        Ok(())
    }

    #[test]
    fn test_stdin_error() -> Result<()> {
        let mut cmd = Command::cargo_bin("ruff")?;
        let output = cmd
            .args(&["-"])
            .write_stdin("import os\n")
            .assert()
            .failure();
        assert!(str::from_utf8(&output.get_output().stdout)?.contains("-:1:1: F401"));
        Ok(())
    }

    #[test]
    fn test_stdin_error_filename() -> Result<()> {
        let mut cmd = Command::cargo_bin("ruff")?;
        let output = cmd
            .args(&["-", "--stdin-filename", "F401.py"])
            .write_stdin("import os\n")
            .assert()
            .failure();
        assert!(str::from_utf8(&output.get_output().stdout)?.contains("F401.py:1:1: F401"));
        Ok(())
    }

    #[test]
    fn test_stdin_autofix() -> Result<()> {
        let mut cmd = Command::cargo_bin("ruff")?;
        let output = cmd
            .args(&["-", "--fix"])
            .write_stdin("import os\n")
            .assert()
            .success();
        assert!(str::from_utf8(&output.get_output().stdout)?.contains("Found 0 error(s) (1 fixed)"));
        Ok(())
    }
}
```

---

_Comment by @charliermarsh on 2022-10-11 13:04_

@harupy - Yeah let's include those. Do they rely on `ruff` already having been built and installed locally as a binary? Or do they work "from scratch"?

If the former, let's put them under a separate feature, like:

```toml
[features]
integration_test = []
```

And then:

```rust
#[cfg(all(test, feature = "integration_test"))]
...
```

What do you think?

---

_Comment by @harupy on 2022-10-11 13:37_

@charliermarsh I'm looking at https://doc.rust-lang.org/cargo/reference/cargo-targets.html#integration-tests which says:

> Binary targets are automatically built if there is an integration test. This allows an integration test to execute the binary to exercise and test its behavior. The CARGO_BIN_EXE_<name> [environment variable](https://doc.rust-lang.org/cargo/reference/environment-variables.html#environment-variables-cargo-sets-for-crates) is set when the integration test is built so that it can use the [env macro](https://doc.rust-lang.org/std/macro.env.html) to locate the executable.

`assert_cmd` uses `CARGO_BIN_EXE_<name>`:

https://github.com/assert-rs/assert_cmd/blob/295731db56a3887dbc1cfe6601436b42e4646a2e/src/cargo.rs#L204

I think they work "from scratch".

---

_Comment by @charliermarsh on 2022-10-11 13:41_

This looks great! Going to make one small API tweak to `fixer.rs` then merge.

---

_Comment by @charliermarsh on 2022-10-11 13:54_

@harupy - I ended up disabling autofix, since it's a little confusing to say that errors are "fixed" when we're not actually writing _back_ in any way. Let me know if you disagree though!


---

_Comment by @harupy on 2022-10-11 13:55_

@charliermarsh Makes sense!

---

_Merged by @charliermarsh on 2022-10-11 13:56_

---

_Closed by @charliermarsh on 2022-10-11 13:56_

---

_Comment by @charliermarsh on 2022-10-11 13:56_

Thank you for this, great change.

---

_Comment by @fsouza on 2022-10-11 15:02_

@charliermarsh one way that `--autofix` could work is by writing the fixed code to stdout. What do you think?

This would enable editor integration (like usually the editor sends the source to stdin and read it back from stdout)

---

_Comment by @charliermarsh on 2022-10-11 15:04_

Yeah that's doable! Is there any example of another tool that does this? Wondering about things like: Would we only write the fixed code to stdout when `--fix` is passed in? Would we write back the fixed code if `--fix` is passed in but no changes are needed?

---

_Comment by @fsouza on 2022-10-11 15:27_

@charliermarsh yeah, in general that's what tools do. Some examples of tools that do that include gofmt, autoflake and black:

```shell
# gofmt with code that needs fixing
％ echo "package               main\n\n\n" | gofmt
package main
```

```shell
# gofmt with code that doesn't need fixing
％ echo "package main" | gofmt
package main
```

```shell
# autoflake with code that needs fixing
％ echo "import os\nprint('hi')" | autoflake --remove-all-unused-imports -
print('hi')
```

```shell
# autoflake with code that doesn't need fixing
％ echo "import os\nprint(os.linesep)" | autoflake --remove-all-unused-imports -
import os
print(os.linesep)
```

```shell
# black with code that needs fixing
％ echo "import os\nprint(         os.linesep)" | black --quiet -
import os

print(os.linesep)
```

```shell
# black with code that doesn't need fixing
％ echo "import os\n\nprint(os.linesep)" | black --quiet -
import os

print(os.linesep)
```

---

_Comment by @charliermarsh on 2022-10-11 15:28_

Thank you for this, really helpful!


---

_Branch deleted on 2022-10-11 15:47_

---

_Comment by @harupy on 2022-10-11 15:53_

Tweaked the source code to write the fixed code to stdout and tested it with VSCode:

https://user-images.githubusercontent.com/17039389/195140097-824c403a-20a4-459d-8674-b36c7860cf18.mov

Worked like a charm!

---

_Comment by @charliermarsh on 2022-10-11 16:00_

Extremely cool. Maybe we write to `stdout` if `--fix` is passed in, but otherwise write back the errors...?

---

_Comment by @charliermarsh on 2022-10-11 16:01_

Or maybe we move that behind an entirely separate subcommand.

---

_Comment by @fsouza on 2022-10-11 16:02_

I like the idea of reusing `--fix`

---

_Comment by @harupy on 2022-10-11 16:03_

+1 to reusing `--fix`

---

_Comment by @fsouza on 2022-10-11 17:28_

@harupy I love your demo. Do you plan on sending a PR for that? If not I can send a PR.

---
