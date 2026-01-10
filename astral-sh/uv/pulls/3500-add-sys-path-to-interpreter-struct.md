```yaml
number: 3500
title: "add `sys_path` to `Interpreter` struct"
type: pull_request
state: merged
author: ChannyClaus
labels:
  - internal
assignees: []
merged: true
base: main
head: syspath
created_at: 2024-05-10T03:15:06Z
updated_at: 2024-05-10T07:05:46Z
url: https://github.com/astral-sh/uv/pull/3500
synced_at: 2026-01-10T14:37:54Z
```

# add `sys_path` to `Interpreter` struct

---

_Pull request opened by @ChannyClaus on 2024-05-10 03:15_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
likely necessary to resolve https://github.com/astral-sh/uv/issues/2500

made this a separate PR in an attempt to make the changes as small as possible; let me know if it's preferred to keep them as a single PR.
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
- edited the test in `interpreter.rs`
- tested manually via `println!` 

```
$ cargo run --quiet pip show test
["/Users/chankang/Library/Caches/uv/.tmpKzNEPN", "/Users/chankang/.pyenv/versions/3.12.2/lib/python312.zip", "/Users/chankang/.pyenv/versions/3.12.2/lib/python3.12", "/Users/chankang/.pyenv/versions/3.12.2/lib/python3.12/lib-dynload", "/Users/chankang/repos/uv/.venv/lib/python3.12/site-packages"]
warning: Package(s) not found for: test
chankang@chans-Air ~/repos/uv -  (syspath)
$ git diff
diff --git a/crates/uv-interpreter/src/environment.rs b/crates/uv-interpreter/src/environment.rs
index 33b785ce..8ebf0864 100644
--- a/crates/uv-interpreter/src/environment.rs
+++ b/crates/uv-interpreter/src/environment.rs
@@ -106,6 +106,7 @@ impl PythonEnvironment {
     /// Some distributions also create symbolic links from `purelib` to `platlib`; in such cases, we
     /// still deduplicate the entries, returning a single path.
     pub fn site_packages(&self) -> impl Iterator<Item = &Path> {
+        println!("{:?}", self.interpreter.sys_path());
         if let Some(target) = self.interpreter.target() {
             Either::Left(std::iter::once(target.root()))
         } else {
chankang@chans-Air ~/repos/uv -  (syspath)
$ python -c "import sys; print(sys.path)"
['', '/Users/chankang/.pyenv/versions/3.12.2/lib/python312.zip', '/Users/chankang/.pyenv/versions/3.12.2/lib/python3.12', '/Users/chankang/.pyenv/versions/3.12.2/lib/python3.12/lib-dynload', '/Users/chankang/.pyenv/versions/3.12.2/lib/python3.12/site-packages']
chankang@chans-Air ~/repos/uv -  (syspath)
```

<!-- How was it tested? -->


---

_Converted to draft by @ChannyClaus on 2024-05-10 03:24_

---

_Marked ready for review by @ChannyClaus on 2024-05-10 03:44_

---

_@konstin approved on 2024-05-10 06:41_

Thanks, small PR size is great

---

_Merged by @konstin on 2024-05-10 06:41_

---

_Closed by @konstin on 2024-05-10 06:41_

---

_Label `internal` added by @konstin on 2024-05-10 07:05_

---
