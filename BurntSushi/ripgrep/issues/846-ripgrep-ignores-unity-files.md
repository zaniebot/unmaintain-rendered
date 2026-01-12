```yaml
number: 846
title: ripgrep ignores Unity files
type: issue
state: closed
author: ylluminarious
labels:
  - invalid
  - question
assignees: []
created_at: 2018-03-07T03:12:08Z
updated_at: 2018-03-07T03:47:45Z
url: https://github.com/BurntSushi/ripgrep/issues/846
synced_at: 2026-01-12T16:13:22Z
```

# ripgrep ignores Unity files

---

_@ylluminarious_

#### What version of ripgrep are you using?

```
ripgrep 0.8.1
-SIMD -AVX
```

#### What operating system are you using ripgrep on?

macOS 10.13.3

#### Describe your question, feature request, or bug.

It seems that `rg` ignores `.unity` files, which is the [Unity Editor's](https://unity3d.com/) file extension for scene files.

#### If this is a bug, what are the steps to reproduce the behavior?

I ran `rg` on a directory with `.unity` files and it yielded no results.

#### If this is a bug, what is the actual behavior?

I ran `rg --type all -i -e mystring .` in a directory which contains several `.unity` files:

```
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.
```

I did as it tells me with `rg --type all -i -e mystring . --debug`:

The full output is [here.](https://gist.github.com/ylluminarious/d5b86166a514fb439656b449fdd89909)

Some of these globs seem to belong to `rg` and others come from my project's `.gitignore` plus my global `.gitignore`.

Unfortunately, I cannot include the corpus of my search, i.e. the files that I searched with `rg` as they are confidential and very large.

#### If this is a bug, what is the expected behavior?

It should have searched my `.unity` files. I cannot tell for certain, but none of the ignore patterns in the debug output seemed to contain anything that would filter out my `.unity` files.

---

_Comment by @BurntSushi on 2018-03-07 03:29_

@ylluminarious The debug output somewhat cryptically tells you exactly the problem:

```
DEBUG/ignore::walk/ignore/src/walk.rs:1428: ignoring ./MyFile1.unity: Ignore(IgnoreMatch(Types(Glob(UnmatchedIgnore))))
```

That is, your unity file **did not match anything**. Normally, that wouldn't cause it to be excluded, but you're explicitly invoking ripgrep in its whitelist mode by using `--type all`. Literally the first line of documentation for the `--type` flag is:

> Only search files matching TYPE.

According to your debug output, you haven't defined any custom types, and since Unity is not a file format recognized by ripgrep by default, it follows that Unity files are not part of `--type all`, and therefore, are ignored *because that's what you told ripgrep to do.*

---

_Label `question` added by @BurntSushi on 2018-03-07 03:30_

---

_Comment by @ylluminarious on 2018-03-07 03:34_

@BurntSushi Thanks a lot for the answer. I'll have to keep that in mind from now on and read the man page a little more closely. I figured it was something wrong on my end.

---

_Closed by @ylluminarious on 2018-03-07 03:34_

---

_Label `invalid` added by @BurntSushi on 2018-03-07 03:47_

---
