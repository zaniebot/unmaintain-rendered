```yaml
number: 14670
title: "mdtest: allow specifying a specific test inside a file"
type: pull_request
state: merged
author: connorskees
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: mdtest-single
created_at: 2024-11-29T06:52:41Z
updated_at: 2024-11-29T15:22:25Z
url: https://github.com/astral-sh/ruff/pull/14670
synced_at: 2026-01-10T20:50:57Z
```

# mdtest: allow specifying a specific test inside a file

---

_Pull request opened by @connorskees on 2024-11-29 06:52_

Resolves https://github.com/astral-sh/ruff/issues/13868 by adding an environment variable `MDTEST_FILE_FILTER` which can be used to specify a substring for tests to be filtered by. I was working around this when implementing another PR by deleting all the other tests in a file. 

---

_Review requested from @carljm by @connorskees on 2024-11-29 06:52_

---

_Review requested from @MichaReiser by @connorskees on 2024-11-29 06:52_

---

_Review requested from @AlexWaygood by @connorskees on 2024-11-29 06:52_

---

_Review requested from @sharkdp by @connorskees on 2024-11-29 06:52_

---

_Review comment by @connorskees on `crates/red_knot_test/src/lib.rs`:72 on 2024-11-29 06:53_

maybe this printing is too prescriptive of the exact command to be run / not windows-friendly enough? happy to reword, though i think keeping at least the print for the env var name is useful and solves the discoverability problem

i also sort of wish we were able to print a further filter for the test name itself, e.g. `-- mdtest__unpacking`. i didn't find a simple+robust way to generate this name

---

_Review comment by @connorskees on `crates/red_knot_test/src/lib.rs`:38 on 2024-11-29 06:56_

just doing a simple case-sensitive substring check. this could also be changed to do a glob or regex, though then we have to think about escaping if we want to print the expected filter for a given test. shouldn't be too big of a deal, so happy to implement another solution if we want to

---

_@connorskees reviewed on 2024-11-29 06:57_

---

_@connorskees reviewed on 2024-11-29 06:57_

---

_Review comment by @connorskees on `crates/red_knot_test/src/lib.rs`:72 on 2024-11-29 06:57_

<img width="975" alt="Captura de pantalla 2024-11-29 a la(s) 1 57 31 a m" src="https://github.com/user-attachments/assets/5d09d03f-3361-4a25-b574-1889eb9e0a2f">
This is what it looks like rendered

---

_Comment by @github-actions[bot] on 2024-11-29 06:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `testing` added by @MichaReiser on 2024-11-29 07:13_

---

_Label `red-knot` added by @MichaReiser on 2024-11-29 07:13_

---

_Comment by @MichaReiser on 2024-11-29 07:14_

Thank you. 

This is just a small note for the future. Please be sure to ask before you start working on issues that are assigned to someone to avoid duplicate work. 

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/lib.rs`:66 on 2024-11-29 07:15_

```suggestion
                "\nTo rerun this specific test, set the environment variable: {MDTEST_FILE_FILTER}=\"{}\"",
```

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/lib.rs`:72 on 2024-11-29 07:18_

I think it's fine as it is now. 

It might be possible to use the [`module_path`](https://doc.rust-lang.org/std/macro.module_path.html) macro in the test function that you then pass through to the run call

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/lib.rs`:18 on 2024-11-29 07:20_

I would rename the environment variable to `MDTEST_TEST_FILTER` because the filter isn't filtering files, but the test cases defined in a test suite (md file)

---

_@MichaReiser approved on 2024-11-29 07:20_

This looks good. Could we add some documentation to the crate README that explains the environment variable and its syntax

---

_@MichaReiser approved on 2024-11-29 07:49_

---

_@AlexWaygood approved on 2024-11-29 11:52_

Thanks for the PR @connorskees! I really appreciate it.

This is probably the easiest version of this feature to implement, so I'm fine with landing it for now and then iterating. But there are a bunch of things I'm not really a _massive_ fan of here. Again, we don't necessarily need to make any of these changes here in this PR, I'm just noting them for posterity:

## Usability on Windows

We don't have any people working at Astral right now who develop primarily on Windows, but I'm sure we have contributors (or potential contributors) who use Windows primarily. I used to use Windows primarily, and setting environment variables is kind of a pain on Windows. I'm not sure there even is a way to set them ephemerally as part of a single process. Ideally I think this would be a CLI flag rather than an environment variable. (But, as I say, I'm sure that's harder to implement, so I'm fine with going for an environment variable for now.)

## Other tests are still shown by `cargo test` as having run even though all subtests are skipped

E.g. here's what I see if I create a deliberate failure in multiple test files, but then only select one subtest (using ``MDTEST_TEST_FILTER='A single bare `except`' cargo test -p red_knot_python_semantic --test mdtest``):

<details>
<summary>Screenshot</summary>

![image](https://github.com/user-attachments/assets/d92c25aa-f90d-4fb8-a096-ba8042696bfd)

</details>

Ideally all the mdtests except for `test mdtest__exception_control_flow` would show as `... SKIPPED`. Otherwise it gives the impression that you're running more tests than you actually are.

## Very long command needed

To select [this test](https://github.com/astral-sh/ruff/blob/main/crates/red_knot_python_semantic/resources/mdtest/exception/control_flow.md#a-single-bare-except) I need to type this command at the terminal:

```
MDTEST_TEST_FILTER='A single bare `except`' cargo test -p red_knot_python_semantic --test mdtest
```

That's really quite a long command, and note that I need to include the backticks around the word "except", which isn't obvious if you're reading the name of the test from [the rendered Markdown on GitHub](https://github.com/astral-sh/ruff/blob/main/crates/red_knot_python_semantic/resources/mdtest/exception/control_flow.md#a-single-bare-except). It might be better if, as well as being able to select tests by name, `red_knot_test` assigned each subtest a unique "test ID", and we also allowed selecting tests by their test IDs. The framework could tell you the IDs of the tests that failed in its error report, allowing you to easily rerun just those tests using e.g.

```
MDTEST_TEST_ID=42 cargo test -p red_knot_python_semantic --test mdtest
```

---

_Comment by @MichaReiser on 2024-11-29 11:58_

> I'm not sure there even is a way to set them ephemerally as part of a single process. Ideally I think this would be a CLI flag rather than an environment variable. (But, as I say, I'm sure that's harder to implement, so I'm fine with going for an environment variable for now.)

There is, it's just a bit more awkward: `set RUST_BACKTRACE=1 && cargo bench --bench accumulator`

> That's really quite a long command, an

That's true. However, the test runner printing the command for you somewhat mitigates this. I don't like numbers, but maybe we can come up with another identifier.

> Other tests are still shown by cargo test as having run even though all subtests are skipped

This probably requires our own test runner. We could try extracting the full test name using `module_path`, see https://github.com/astral-sh/ruff/pull/14670/files/249b8134a37aae59e51cb0f93e7d65c7ba191879#diff-3ac9b4142b5271acdd34e8027226477c72f0ecf71e1dfc2b3a7295b13a7d8e0f

@connorskees do you want to give this a try in a follow up PR?

---

_Merged by @MichaReiser on 2024-11-29 11:59_

---

_Closed by @MichaReiser on 2024-11-29 11:59_

---

_Comment by @connorskees on 2024-11-29 15:12_

hm `module_path!` seems not to include the name of the function. i _was_ able to get the name of the function by copying some code directly from `dir_test`. it's not too much code if we're fine with a less-elegant solution:

```rs
fn test_name(test_func_name: String, fixture_path: &Path) -> String {
    assert!(fixture_path.is_file());

    let dir_path = format!("{}/resources/mdtest", std::env!("CARGO_MANIFEST_DIR"));
    let rel_path = fixture_path.strip_prefix(dir_path).unwrap();
    assert!(rel_path.is_relative());

    let mut test_name = test_func_name;
    test_name.push_str("__");

    let components: Vec<_> = rel_path.iter().collect();

    for component in &components[0..components.len() - 1] {
        let component = component
            .to_string_lossy()
            .replace(|c: char| c.is_ascii_punctuation(), "_");
        test_name.push_str(&component);
        test_name.push('_');
    }

    test_name.push_str(
        &rel_path
            .file_stem()
            .unwrap()
            .to_string_lossy()
            .replace(|c: char| c.is_ascii_punctuation(), "_"),
    );

    test_name
}

// ...

let test_name = test_name("mdtest".to_owned(), fixture_path);
```

---

_Comment by @MichaReiser on 2024-11-29 15:22_

Ah right. You could try using `type_name`: 

```rust
pub fn main() {
    struct Type;

    println!("{}", std::any::type_name::<Type>());
}
```

and then hide it behind a macro. 

But I'm also okay constructing the name from the test file

---
