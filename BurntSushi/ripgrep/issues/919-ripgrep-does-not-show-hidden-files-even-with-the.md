```yaml
number: 919
title: RipGrep does not show hidden files even with the --hidden flag
type: issue
state: closed
author: dkarter
labels: []
assignees: []
created_at: 2018-05-13T05:46:23Z
updated_at: 2018-05-13T16:40:08Z
url: https://github.com/BurntSushi/ripgrep/issues/919
synced_at: 2026-01-12T16:13:22Z
```

# RipGrep does not show hidden files even with the --hidden flag

---

_@dkarter_

#### What version of ripgrep are you using?

ripgrep 0.8.1
-SIMD -AVX

#### How did you install ripgrep?

`brew install ripgrep`

#### What operating system are you using ripgrep on?

macOS 10.13.4

#### Describe your question, feature request, or bug.

To replicate:

1. Create a directory (`mkdir example`)
2. Create a hidden file `touch .you-dont-see-me`
3. Create a non-hidden file `touch you-see-me`
4. Run ripgrep to list all files including hidden files `rg --files --hidden`

Expected:
All files are shown in the output
```
you-see-me
.you-dont-see-me
```

Actual:

```
❯ rg --files --hidden
you-see-me
```

With the `--debug` flag: https://gist.github.com/dkarter/4eb7acfd90c4326f05de8fc15b3a3ee5

For comparison try `ag -l --hidden -g ""` which produces the expected output.

---

_Comment by @dkarter on 2018-05-13 05:50_

Never mind figured it out. I had some weird .gitignore in an upper level directory - deleting it fixed the "issue".

---

_Closed by @dkarter on 2018-05-13 05:50_

---

_Comment by @BurntSushi on 2018-05-13 11:44_

Glad you figured it out! Note that your debug output tells you what each file was ignored:

```
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./.you-dont-see-me: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/dkarter/dev/.gitignore"), original: ".*", actual: "**/.*", is_whitelist: false, is_only_dir: false })))
you-see-me
```

---

_Comment by @dkarter on 2018-05-13 16:40_

Yea that was my clue :)

---
