```yaml
number: 800
title: Custom ignore files can not be used exclusively
type: issue
state: closed
author: sharkdp
labels:
  - bug
assignees: []
created_at: 2018-02-13T21:57:05Z
updated_at: 2018-02-14T11:53:29Z
url: https://github.com/BurntSushi/ripgrep/issues/800
synced_at: 2026-01-12T16:13:22Z
```

# Custom ignore files can not be used exclusively

---

_@sharkdp_

#### What version of the ignore crate are you using?

ignore-0.4.0

#### Describe your question, feature request, or bug.

It would be great if the custom ignore files (that are available since ignore-0.4.0) could be used exclusively without having to enable `.ignore` or `.gitignore` files as well.

#### If this is a bug, what are the steps to reproduce the behavior?

Run the following test case:
``` rust
#[test]
fn custom_ignore() {
    let td = TempDir::new("walk-test-").unwrap();
    let custom_ignore = ".customignore";
    mkdirp(td.path().join("a"));
    wfile(td.path().join(custom_ignore), "foo");
    wfile(td.path().join("foo"), "");
    wfile(td.path().join("a/foo"), "");
    wfile(td.path().join("bar"), "");
    wfile(td.path().join("a/bar"), "");

    let mut builder = WalkBuilder::new(td.path());

    // THE FOLLOWING FOUR LINES HAVE BEEN
    // ADDED W.R.T. AN EXISTING TEST CASE
    builder.ignore(false);
    builder.git_ignore(false);
    builder.git_global(false);
    builder.git_exclude(false);

    builder.add_custom_ignore_filename(&custom_ignore);
    assert_paths(td.path(), &builder, &["bar", "a", "a/bar"]);
}
```

#### If this is a bug, what is the actual behavior?

The test case fails because the `foo` files are not ignored:
```
---- walk::tests::custom_ignore stdout ----
	thread 'walk::tests::custom_ignore' panicked at 'assertion failed: `(left == right)`
  left: `["a", "a/bar", "a/foo", "bar", "foo"]`,
 right: `["a", "a/bar", "bar"]`', src/walk.rs:1554:8
```

#### If this is a bug, what is the expected behavior?

The test case succeeds.

#### Analysis

The cause for this behavior is the following function in `ignore/src/dir.rs`:
``` rust
    fn has_any_ignore_options(&self) -> bool {
        self.ignore || self.git_global || self.git_ignore || self.git_exclude
    }
```
.. which returns `false` if all of these are disabled. This, in turn, causes `Ignore::matched` to skip ignore files alltogether:
``` rust
        if self.0.opts.has_any_ignore_options() {
            let mat = self.matched_ignore(path, is_dir);
            if mat.is_ignore() {
                return mat;
            } else if mat.is_whitelist() {
                whitelisted = mat;
            }
        }
```

If the above behavior is actually intended, a simple bugfix would be to add the following:
``` rust
        if self.0.opts.has_any_ignore_options() || !self.0.custom_ignore_filenames.is_empty() {
            // ...
        }
```

---

_Comment by @BurntSushi on 2018-02-13 22:00_

Thanks for the clear bug report! This definitely sounds like a bug, and `has_any_ignore_options` should probably be removed in favor of a predicate that covers all of the cases.

---

_Label `bug` added by @BurntSushi on 2018-02-13 22:00_

---

_Comment by @sharkdp on 2018-02-13 22:23_

By the way, ripgrep itself is not "affected" (I think) because the custom ignore file (`.rgignore`) is always enabled together with `.ignore`. Therefore, `IgnoreOptions::ignore` will be enabled if `.rgignore` files should be used and `has_any_ignore_options()` returns `true`.

---

_Comment by @BurntSushi on 2018-02-13 23:21_

@sharkdp Yup I believe you're right. Thanks for noticing this and filing a bug. :)

---

_Closed by @BurntSushi on 2018-02-14 11:53_

---
