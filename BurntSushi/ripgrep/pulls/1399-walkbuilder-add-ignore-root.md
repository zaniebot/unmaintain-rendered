```yaml
number: 1399
title: Walkbuilder add ignore root
type: pull_request
state: closed
author: tommilligan
labels: []
assignees: []
base: master
head: walkbuilder-add-ignore-root
created_at: 2019-10-07T16:34:13Z
updated_at: 2020-02-15T16:08:51Z
url: https://github.com/BurntSushi/ripgrep/pull/1399
synced_at: 2026-01-12T18:23:13Z
```

# Walkbuilder add ignore root

---

_@tommilligan_

See #1394 for full description.

> When building an instance of Walk using ignore::WalkBuilder, the patterns in files added by .add_ignore() are matched against the current working directory of the process, instead of the root of the tree being Walked.

This PR changes the behaviour or `WalkBuilder.add_ignore()`. **This is a breaking change.** Absolute file paths are now resolved based on the directory the `WalkBuilder` was initialised with.

Example directory structure:
```log
// /home
//   /subdir
//     - ignorefile
//     - README.md
```
Contents of `/home/subdir/ignorefile`:
```log
/README.md
```

The below code will now always ignore `/home/subdir/README.md` rather than `./README.md` of wherever the executing process is.

```rust
// psuedococde
let mut builder = WalkBuilder::new("/home/subdir");
builder.add_ignore("/home/subdir/ignorefile);
```

---

_Comment by @tommilligan on 2019-10-08 16:30_

Test failure appears to be unrelated.

---

_Comment by @BurntSushi on 2020-02-15 16:07_

@tommilligan Thanks for investigating this! However, I don't think I can merge this PR in its current form, and I'm not quite sure what the fix _should_ be. As you point out, this is a breaking change. Unfortunately, this is not a _desirable_ breaking change. Namely, the current behavior is actually intended as far as I can tell. If you look at ripgrep's docs for the `--ignore-file` flag, then you'll see that all ignore files provided in this way (which use `WalkBuilder::add_ignore`, and the `--ignore-file` flag is why this method was added) are _supposed_ to be matched relative to the current working directory.

So you might say, "OK, that's fine, then we can add a new `add_ignore`-like method that matches relative to the root." While I would be okay with that, the problem here is that a `WalkBuilder` _doesn't actually have a single root_. You can add multiple additional paths via the `add` method. The ignore files processed by `add_ignore` would then be incorrectly matched on subsequent paths.

---

_Comment by @BurntSushi on 2020-02-15 16:08_

(I am going to close this PR for now for procedural reasons, but I will leave #1394 open for now I think.)

---

_Closed by @BurntSushi on 2020-02-15 16:08_

---
