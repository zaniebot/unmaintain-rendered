```yaml
number: 1394
title: "ignore: WalkBuilder.add_ignore() patterns are not rooted in the path being walked"
type: issue
state: open
author: dragonmaus
labels:
  - bug
  - question
  - gitignore
assignees: []
created_at: 2019-09-30T20:35:57Z
updated_at: 2022-05-12T05:26:33Z
url: https://github.com/BurntSushi/ripgrep/issues/1394
synced_at: 2026-01-12T16:13:23Z
```

# ignore: WalkBuilder.add_ignore() patterns are not rooted in the path being walked

---

_@dragonmaus_

`Alternative title: No way to specify "root path" for WalkBuilder.add_ignore()`

<not-ripgrep-snip>

#### Describe your question, feature request, or bug.

When building an instance of `Walk` using `ignore::WalkBuilder`, the patterns in files added by `.add_ignore()` are matched against the current working directory of the process, instead of the root of the tree being `Walk`ed.

#### If this is a bug, what are the steps to reproduce the behavior?

After downloading the files in [this gist](https://gist.github.com/dragonmaus/65aec98cdc163693c162ac2884bf0cc5), perform the following steps:

```sh
mkdir src
mv main.rs src
cargo build --release
target/release/test -I ignore .              # this is "Test 1"
cd ..
test/target/release/test -I test/ignore test # this is "Test 2"
```

#### If this is a bug, what is the actual behavior?

Test 1 produces the expected output:
```
.
./Cargo.lock
./Cargo.toml
./ignore
./src
./src/main.rs
```

Test 2, however, also includes the contents of the target directory (full output [here](https://gist.github.com/dragonmaus/c7fcc6b5e8f631cfd19060e9dad82c95)).

#### If this is a bug, what is the expected behavior?

Test 2 should produce the following output:
```
test
test/Cargo.lock
test/Cargo.toml
test/ignore
test/src
test/src/main.rs
```

Changing the pattern in the file `ignore` to `/test/target/` produces the desired results, but is not what I would expect (or want) to have to do.

---

_Comment by @tommilligan on 2019-10-03 12:03_

I'd like to take this issue - I've reproduced it and should have a fix this week.

Minimal test case:

```diff
diff --git a/ignore/src/walk.rs b/ignore/src/walk.rs
index e00b29a..f765b7f 100644
--- a/ignore/src/walk.rs
+++ b/ignore/src/walk.rs
@@ -1919,6 +1919,22 @@ mod tests {
         );
     }
 
+    #[test]
+    fn explicit_ignore_absolute_path() {
+        let td = tmpdir();
+        let igpath = td.path().join(".not-an-ignore");
+        mkdirp(td.path().join("a"));
+        wfile(&igpath, "/a");
+        wfile(td.path().join("foo"), "");
+        wfile(td.path().join("a/foo"), "");
+        wfile(td.path().join("bar"), "");
+        wfile(td.path().join("a/bar"), "");
+
+        let mut builder = WalkBuilder::new(td.path());
+        assert!(builder.add_ignore(&igpath).is_none());
+        assert_paths(td.path(), &builder, &["foo", "bar"]);
+    }
+
     #[test]
     fn gitignore_parent() {
         let td = tmpdir();
```

---

_Comment by @BurntSushi on 2020-02-15 16:08_

@dragonmaus Thank you very much for the detailed bug report! Unfortunately, I'm quite unsure how to fix this. Please see my comment in #1399: https://github.com/BurntSushi/ripgrep/pull/1399#issuecomment-586613865

---

_Label `bug` added by @BurntSushi on 2020-02-15 16:08_

---

_Label `question` added by @BurntSushi on 2020-02-15 16:08_

---

_Comment by @dragonmaus on 2020-02-15 16:33_

@BurntSushi Thank you for looking into this, and thank you @tommilligan for doing the legwork.

I will have to re-evaluate how I am [using](/dragonmaus/tarball.rs/blob/master/src/main.rs#L174) `WalkBuilder`; perhaps there is some cognitive disconnect on my end, or maybe a novel solution will present itself.

---

_Comment by @BurntSushi on 2020-02-15 19:32_

I don't think you are doing anything wrong. I think your use case is valid. I just don't know how quite to do it with the current API. We _could_ use @tommilligan's solution with a new method, e.g., `add_ignore_relative`, and just panic if `add` is called to add additional roots. That would make your use case work. I wouldn't be _too_ opposed to doing this since IMO the `ignore` API is an unmitigated disaster at this point anyway. So adding another quirk isn't that big of a deal. Once I summon the courage to redo the crate, I can rethink everything from first principles. The `ignore` crate has gone through a lot of requirements evolution and has badly needed an overhaul for quite some time now. But it will a long time before I get to doing it. (I'd say on the order of a year.)

So if you want a work-around until then, then I think I'd be okay with the aforementioned tweaks to @tommilligan's PR. (Apologies for closing it, but I want to completely clear the PR queue before doing the next release.)

---

_Label `gitignore` added by @BurntSushi on 2020-05-08 11:28_

---
