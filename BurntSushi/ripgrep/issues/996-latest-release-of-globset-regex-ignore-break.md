```yaml
number: 996
title: Latest release of globset/regex/ignore break ripgrep
type: issue
state: closed
author: igor-raits
labels: []
assignees: []
created_at: 2018-07-29T06:21:50Z
updated_at: 2018-07-29T14:41:04Z
url: https://github.com/BurntSushi/ripgrep/issues/996
synced_at: 2026-01-12T16:13:22Z
```

# Latest release of globset/regex/ignore break ripgrep

---

_@igor-raits_

While updating crates in Fedora I noticed that ripgrep regression tests are now failing.

```
---- ignore_git stdout ----
	thread 'ignore_git' panicked at '
===== "/builddir/build/BUILD/ripgrep-0.8.1/target/release/deps/../rg" "Sherlock" "." =====
command succeeded but expected failure!
cwd: /builddir/build/BUILD/ripgrep-0.8.1/target/release/deps/ripgrep-tests/ignore_git/70
status: exit code: 0
stdout: sherlock:For the Doctor Watsons of this world, as opposed to the Sherlock
sherlock:be, to a very large extent, the result of luck. Sherlock Holmes
stderr: 
=====
', tests/workdir.rs:264:13
note: Run with `RUST_BACKTRACE=1` for a backtrace.
---- no_parent_ignore_git stdout ----
	thread 'no_parent_ignore_git' panicked at 'assertion failed: `(left == right)`
  left: `"sherlock:For the Doctor Watsons of this world, as opposed to the Sherlock\nsherlock:be, to a very large extent, the result of luck. Sherlock Holmes\nwatson:For the Doctor Watsons of this world, as opposed to the Sherlock\nwatson:be, to a very large extent, the result of luck. Sherlock Holmes\n"`,
 right: `"sherlock:For the Doctor Watsons of this world, as opposed to the Sherlock\nsherlock:be, to a very large extent, the result of luck. Sherlock Holmes\n"`', tests/tests.rs:695:5
---- regression_127 stdout ----
	thread 'regression_127' panicked at 'assertion failed: `(left == right)`
  left: `"foo/sherlock:For the Doctor Watsons of this world, as opposed to the Sherlock\nfoo/sherlock:be, to a very large extent, the result of luck. Sherlock Holmes\nfoo/watson:For the Doctor Watsons of this world, as opposed to the Sherlock\nfoo/watson:be, to a very large extent, the result of luck. Sherlock Holmes\n"`,
 right: `"foo/watson:For the Doctor Watsons of this world, as opposed to the Sherlock\nfoo/watson:be, to a very large extent, the result of luck. Sherlock Holmes\n"`', tests/tests.rs:934:5
---- regression_131 stdout ----
	thread 'regression_131' panicked at '
===== "/builddir/build/BUILD/ripgrep-0.8.1/target/release/deps/../rg" "test" "." =====
command succeeded but expected failure!
cwd: /builddir/build/BUILD/ripgrep-0.8.1/target/release/deps/ripgrep-tests/regression_131/96
status: exit code: 0
stdout: TopÑapa:test
stderr: 
=====
', tests/workdir.rs:264:13
---- regression_16 stdout ----
	thread 'regression_16' panicked at '
===== "/builddir/build/BUILD/ripgrep-0.8.1/target/release/deps/../rg" "xyz" "." =====
command succeeded but expected failure!
cwd: /builddir/build/BUILD/ripgrep-0.8.1/target/release/deps/ripgrep-tests/regression_16/100
status: exit code: 0
stdout: ghi/toplevel.txt:xyz
def/ghi/subdir.txt:xyz
stderr: 
=====
', tests/workdir.rs:264:13
---- regression_65 stdout ----
	thread 'regression_65' panicked at '
===== "/builddir/build/BUILD/ripgrep-0.8.1/target/release/deps/../rg" "xyz" "." =====
command succeeded but expected failure!
cwd: /builddir/build/BUILD/ripgrep-0.8.1/target/release/deps/ripgrep-tests/regression_65/131
status: exit code: 0
stdout: a/bar:xyz
a/foo:xyz
stderr: 
=====
', tests/workdir.rs:264:13
---- regression_807 stdout ----
	thread 'regression_807' panicked at 'assertion failed: `(left == right)`
  left: `".a/b/file:test\n.a/c/file:test\n"`,
 right: `".a/c/file:test\n"`', tests/tests.rs:1246:5
---- regression_67 stdout ----
	thread 'regression_67' panicked at 'assertion failed: `(left == right)`
  left: `"foo/bar:test\ndir/bar:test\n"`,
 right: `"dir/bar:test\n"`', tests/tests.rs:852:5
---- regression_87 stdout ----
	thread 'regression_87' panicked at '
===== "/builddir/build/BUILD/ripgrep-0.8.1/target/release/deps/../rg" "test" "." =====
command succeeded but expected failure!
cwd: /builddir/build/BUILD/ripgrep-0.8.1/target/release/deps/ripgrep-tests/regression_87/135
status: exit code: 0
stdout: foo:test
stderr: ./.gitignore: line 2: error parsing glob '**no-vcs**': invalid use of **; must be one path component
=====
', tests/workdir.rs:264:13
---- regression_90 stdout ----
	thread 'regression_90' panicked at '
==========
command failed but expected success!
command: "/builddir/build/BUILD/ripgrep-0.8.1/target/release/deps/../rg" "test" "."
cwd: /builddir/build/BUILD/ripgrep-0.8.1/target/release/deps/ripgrep-tests/regression_90/137
status: exit code: 1
stdout: 
stderr: No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.
==========
', tests/workdir.rs:243:13
failures:
    ignore_git
    no_parent_ignore_git
    regression_127
    regression_131
    regression_16
    regression_65
    regression_67
    regression_807
    regression_87
    regression_90
test result: FAILED. 145 passed; 10 failed; 0 ignored; 0 measured; 0 filtered out
error: test failed, to rerun pass '--test integration'
error: Bad exit status from /var/tmp/rpm-tmp.enf08C (%check)
```

https://kojipkgs.fedoraproject.org/work/tasks/1980/28681980/build.log

---

_Comment by @igor-raits on 2018-07-29 06:22_

Just more specifically, there were following changes in buildroot for ripgrep: https://apps.fedoraproject.org/koschei/build/5071357

---

_Comment by @BurntSushi on 2018-07-29 13:13_

Oof. This is #448 I think, biting me hard. One of the bug fixes in the `ignore` crate is that it no longer respects `.gitignore` when outside a git repository. Some of the tests (`ignore_git` is an easy one to inspect) don't actually setup a `.git` repository and just create a `.gitignore`. But, since I run ripgrep's test suite from within a git repository (and, presumably, in CI as well), the tests naturally succeed.

I'm going to see if I can try to make inroads on #448. Solving it perfectly might be difficult, but it occurs to me that one approach might be to create each test's working directory in `/tmp`, which should prevent some obvious forms of state from infecting the test suite.

@ignatenkobrain Does that sound like a reasonable path?

---

_Comment by @igor-raits on 2018-07-29 13:17_

For now I can just do git init from place I run tests.

---

_Comment by @BurntSushi on 2018-07-29 14:25_

@ignatenkobrain My hope is that #998 will help fix this. I don't think this will be available to you until the next release of ripgrep (unless you want to patch in the integration test updates, yuck).

---

_Closed by @BurntSushi on 2018-07-29 14:41_

---
