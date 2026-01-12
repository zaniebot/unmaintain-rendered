```yaml
number: 2498
title: Restricting directories leads gitignore to be interpreted too strongly
type: issue
state: closed
author: est31
labels:
  - bug
assignees: []
created_at: 2023-04-23T09:57:39Z
updated_at: 2023-04-23T19:57:11Z
url: https://github.com/BurntSushi/ripgrep/issues/2498
synced_at: 2026-01-12T16:13:24Z
```

# Restricting directories leads gitignore to be interpreted too strongly

---

_@est31_

#### What version of ripgrep are you using?

```
ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### What operating system are you using ripgrep on?

`NixOS 22.11 Raccoon`

#### Describe your bug.

Ripgrep ignores some files through .gitignore that git doesn't ignore.

#### What are the steps to reproduce the behavior?

```
$ git clone https://github.com/rust-lang/rust
$ cd rust
$ rg "Returns an rvalue suitable for use"
compiler/rustc_mir_build/src/build/expr/as_rvalue.rs
22:    /// Returns an rvalue suitable for use until the end of the current
$ rg "Returns an rvalue suitable for use" compiler/
<nothing>
$ rg "Returns an rvalue suitable for use" compiler/rustc_mir_build/src/build/
compiler/rustc_mir_build/src/build/expr/as_rvalue.rs
22:    /// Returns an rvalue suitable for use until the end of the current
```

As you can see, ripgrep doesn't traverse into the `build/` directory, but only if its directory is restricted. I'd put this on .gitignore containing `build/` and ripgrep interpreting that too strongly.

git has no problems with the gitignore file, when you do:

```
$ touch compiler/rustc_mir_build/src/build/hello.rs
$ git add compiler/
```

git adds that new file.

git behaviour is a bit weird here because `build/` is meant to apply to all directories, including non-top-level ones. I'm not sure what the logic in git is but for ripgrep it was a bit unexpected for me. Btw, the file does show up in vscode's search even if you restrict yourself to `compiler/`.

---

_Comment by @BurntSushi on 2023-04-23 11:46_

So I can't quite tell which bug is being reported here. Could you say what the _expected_ output of each `rg` command is?

Basically:

1. ripgrep has problems applying gitignore rules when searching explicit sub-directories. I believe there are several outstanding issues about this already. See for example: #1757, #1050, #829 and #278. I've basically resisted fixing it until I've had a chance to rethink `ignore`.
2. You mention `git add some-file-that-is-ignored`. And then `git` handles it fine from there. That's an inconsistency between ripgrep and git that probably won't ever be resolved. The solution is to make `.gitignore` files precisely correct. That is, `.gitignore` should line up with files that are actually in revision control.

---

_Comment by @est31 on 2023-04-23 17:53_

> 2\. You mention `git add some-file-that-is-ignored`. And then `git` handles it fine from there.

FTR, the same happens if you do `git add .`, git picks up the file. For ignored files, git wants you to pass the -f option. And git status reports the file as changed, while no such behaviour occurs when you do the same with properly ignored files.

The behaviour I expect is that ripgrep is consistent with git, and consistent with itself. If you don't restrict the directories, it works fine, see output above.

I guess one workaround would be to turn the `build/` into `/build/` in rustc's gitignore, but it also feels like a ripgrep bug to me.

---

_Comment by @BurntSushi on 2023-04-23 17:55_

Can you clarify the expected output for each of your `rg` commands please?

---

_Comment by @est31 on 2023-04-23 17:56_

Actual output:

```
$ rg "Returns an rvalue suitable for use"
compiler/rustc_mir_build/src/build/expr/as_rvalue.rs
22:    /// Returns an rvalue suitable for use until the end of the current
$ rg "Returns an rvalue suitable for use" compiler/
<nothing>
$ rg "Returns an rvalue suitable for use" compiler/rustc_mir_build/src/build/
compiler/rustc_mir_build/src/build/expr/as_rvalue.rs
22:    /// Returns an rvalue suitable for use until the end of the current
```

Expected output:

```
$ rg "Returns an rvalue suitable for use"
compiler/rustc_mir_build/src/build/expr/as_rvalue.rs
22:    /// Returns an rvalue suitable for use until the end of the current
$ rg "Returns an rvalue suitable for use" compiler/
22:    /// Returns an rvalue suitable for use until the end of the current
$ rg "Returns an rvalue suitable for use" compiler/rustc_mir_build/src/build/
compiler/rustc_mir_build/src/build/expr/as_rvalue.rs
22:    /// Returns an rvalue suitable for use until the end of the current
```

---

_Comment by @BurntSushi on 2023-04-23 17:58_

All righty, thanks. I _suspect_ this is a duplicate, but I don't have the context paged into cache to know for sure. It does look like a ripgrep bug.

(I sadly don't expect this to get fixed any time soon because I don't expect this to get fixed until `ignore` has been rethought.)

---

_Label `bug` added by @BurntSushi on 2023-04-23 17:58_

---

_Comment by @est31 on 2023-04-23 18:01_

Hmm yeah it seems a lot like a dupe of #278. Closing.

---

_Closed by @est31 on 2023-04-23 18:01_

---

_Comment by @est31 on 2023-04-23 19:57_

oh I've checked rustc's gitignore, it seems to indeed be a case of #1050 -- the build dir is negatively exempted from the "build should be ignored" rule in gitignore. That's why it work in git...

---
