```yaml
number: 3036
title: "ignore::Walk Fails to Ignore Files Specified in .gitignore of a tempdir"
type: issue
state: closed
author: pluveto
labels: []
assignees: []
created_at: 2025-04-20T14:55:06Z
updated_at: 2025-04-20T15:02:15Z
url: https://github.com/BurntSushi/ripgrep/issues/3036
synced_at: 2026-01-12T16:13:25Z
```

# ignore::Walk Fails to Ignore Files Specified in .gitignore of a tempdir

---

_@pluveto_

### Please tick this box to confirm you have reviewed the above.

- [x] I have a different issue.

### What version of ripgrep are you using?

ignore = "0.4"

### How did you install ripgrep?

N/A

### What operating system are you using ripgrep on?

N/A

### Describe your bug.

When using ignore::Walk::new() to traverse a directory structure, files and directory contents specified in a local .gitignore file are not being ignored as expected. The walk seems to proceed as if the .gitignore rules (*.log and target/ in this case) are not being applied, leading to the inclusion of normally ignored files in the results.



### What are the steps to reproduce the behavior?

The following minimal Rust test case demonstrates the issue. It sets up a temporary directory with a .gitignore file, a file that should not be ignored (a.rs), a file that should be ignored (b.log), and a directory with a file that should be ignored (target/c.o).

```rs
use std::fs;
use std::path::PathBuf;
use tempfile::tempdir; // Needs tempfile crate: `cargo add tempfile --dev`
use ignore; // Needs ignore crate: `cargo add ignore`

#[test]
fn test_ignore_not_working() {
    // 1. Setup: Create a temporary directory
    let dir = tempdir().unwrap();
    let dir_path = dir.path();
    println!("Test directory: {}", dir_path.display());

    // 2. Create .gitignore with rules to ignore *.log and target/
    // Note: Ensure newline separation for rules.
    let gitignore_content = "*.log\ntarget/\n";
    fs::write(dir_path.join(".gitignore"), gitignore_content).expect("Failed to write .gitignore");
    println!(".gitignore content written.");

    // 3. Create files:
    //    - a.rs (should NOT be ignored)
    //    - b.log (should be ignored by *.log rule)
    //    - target/c.o (should be ignored by target/ rule)
    fs::write(dir_path.join("a.rs"), "// A").expect("Failed to write a.rs");
    fs::write(dir_path.join("b.log"), "Log B").expect("Failed to write b.log");
    fs::create_dir(dir_path.join("target")).expect("Failed to create target dir");
    fs::write(dir_path.join("target/c.o"), "Object C").expect("Failed to write target/c.o");
    println!("Test files created.");

    // 4. Perform the walk using default settings
    let matches: Vec<PathBuf> = ignore::Walk::new(dir_path)
        .map(|result| {
            let entry = result.expect("Walk resulted in an error");
            println!("Walk found: {}", entry.path().display()); // Debug print
            entry.path().to_owned()
        })
        .collect();

    println!("Collected matches: {:#?}", matches);

    // 5. Assertions
    // This assertion passes
    assert!(matches.contains(&dir_path.join("a.rs")), "a.rs should be present");

    // This assertion FAILS: `b.log` IS present in `matches`, but should NOT be.
    assert!(!matches.contains(&dir_path.join("b.log")), "b.log should be ignored but was found");

    // This assertion FAILS: `target/c.o` IS present in `matches`, but should NOT be.
    // Note: ignore::Walk yields directories too by default, but the check here is for the file within.
    // If target/ itself was yielded, that might be okay depending on settings, but target/c.o should not be.
    assert!(!matches.contains(&dir_path.join("target/c.o")), "target/c.o should be ignored but was found");

    // This assertion passes
    assert!(!matches.contains(&dir_path.join(".gitignore")), ".gitignore itself should be ignored");
}
```

### What is the actual behavior?

The matches vector incorrectly includes the paths that should have been ignored: a.rs, b.log, and target/c.o.

Example actual matches vector might look like (relative paths): [".", "./.gitignore", "./a.rs", "./b.log", "./target", "./target/c.o"] or similar depending on whether hidden files/dirs are included.

Crucially, the assertions assert!(!matches.contains(&dir_path.join("b.log"))) and assert!(!matches.contains(&dir_path.join("target/c.o"))) fail because these paths are present in the matches vector.

### What is the expected behavior?

The matches vector collected from ignore::Walk::new(dir_path) should only contain the path to a.rs. The files b.log and target/c.o should be excluded because they match the rules (*.log, target/) specified in the .gitignore file located in the walked directory root. The .gitignore file itself should also be excluded by default due to being hidden.

The expected final matches vector (relative paths for simplicity): ["./a.rs"] (or potentially ["."], ["./a.rs"] if the root dir itself is included, depending on exact walk configuration/version).

---

_Comment by @BurntSushi on 2025-04-20 14:59_

A `.gitignore` file is only respected when a git repository is present, unless [`WalkBuilder::require_git`](https://docs.rs/ignore/latest/ignore/struct.WalkBuilder.html#method.require_git) is disabled.

---

_Comment by @pluveto on 2025-04-20 15:02_

Thanks, make sense

---

_Closed by @pluveto on 2025-04-20 15:02_

---
