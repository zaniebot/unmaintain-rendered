```yaml
number: 759
title: Excluded directories in .gitignore are ignored
type: issue
state: closed
author: codido
labels:
  - question
assignees: []
created_at: 2018-01-25T11:23:11Z
updated_at: 2018-01-31T02:06:39Z
url: https://github.com/BurntSushi/ripgrep/issues/759
synced_at: 2026-01-12T16:13:22Z
```

# Excluded directories in .gitignore are ignored

---

_@codido_

If a directory matches a pattern in .gitignore but then explicitly excluded (with '!'), its contents is still ignored by ripgrep.

---

_Label `question` added by @BurntSushi on 2018-01-25 11:37_

---

_Comment by @BurntSushi on 2018-01-25 11:37_

@codido This bug report contains insufficient detail to be actionable. Please provide a small reproducible example. Please show all inputs, actual output and expected output. Please also include the version of ripgrep you are using.

---

_Comment by @codido on 2018-01-25 11:53_

The following demonstrates the issue:
```
mkdir -p test/foo
echo foobar > test/foo/bar
echo 'f*' > test/.gitignore
echo '!foo' >> test/.gitignore
rg foo
```

Expected output:
```
test/foo/bar
1:foobar
```

Actual output:
```
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.
```

---

_Comment by @BurntSushi on 2018-01-25 12:01_

This is basically a different variation of this recently reported issue: https://github.com/BurntSushi/ripgrep/issues/755

The problem is that `f*` will trip on paths like `./test/foo/bar` (globs without an explicit `/` in them will permit `*` to match `/`, as per `man gitignore` semantics). In particular, your `!foo` whitelist rule works in the sense that `./test/foo` is whitelisted (you can see confirmation of this by running ripgrep with `--debug`, as indicated in the error message you got). But whitelisting `./test/foo` does not mean that `./test/foo/bar` is whitelisted.

You can fix this is one of two ways. In addition to the `!foo` pattern, add `!**/foo/**` (or `!/foo/**` as appropriate). This will cause your `test/foo/bar` file to get whitelisted. The other way to fix this is to make your initial ignore rule more specific. That is, use `**/f*` instead of `f*`.

---

_Comment by @codido on 2018-01-25 12:12_

@BurntSushi Thanks for elaborating on that (and of course for ripgrep :)).

While I can fix this in my own trees (if I ever do this), I actually ran into this in an external project. It should be noted that git is OK with these (that is, it doesn't ignore foo/bar), as well as ag, so shouldn't ripgrep behave the same?

---

_Comment by @BurntSushi on 2018-01-25 12:21_

ag should never ever be used as the gold standard for gitignore rules. If it gets this case right, it's by accident. It gets tons of other cases wrong because it doesn't try that hard to implement gitignore semantics correctly. (Last time I checked, it doesn't even support glob precedence, and it just recently got minimal support for whitelisting.)

In order for ripgrep to "behave the same," we need a specification of what `git` is actually doing. gitignore handling is *incredibly* complex, and just inventing new rules on the fly is unlikely to work well. In ripgrep's case, we try to match the specification written in `man gitignore` precisely. If someone can manage to tweak that specification in a rigorous way that fixes a case like yours, then I might be open to exploring it further. There's no obvious tweak that comes to mind for this case, other than something like, "a higher precedent whitelist rule on a parent directory overrides a lower precedent rule on a file in that directory." But that's trivially wrong. e.g., if you had `*.log` as a gitignore rule but `foo` as a whitelist rule, then you'd still expect `foo/wat.log` to be ignored. So there is something deeper going on here that needs to be figured out before we can fix this.

Note that ripgrep can never be 100% in sync with git. For example, if you have a file that is ignored but is nevertheless committed, then git will still track it but ripgrep will still ignore it.

In the abstract, I agree that in an ideal world, the files known to git are exactly the same as the files searched by ripgrep.

---

_Comment by @codido on 2018-01-25 12:40_

@BurntSushi Sure thing, thanks again.

---

_Comment by @BurntSushi on 2018-01-31 02:06_

Based on my previous comment, I'm going to close this. I'm open to alternative ideas here, but they likely need to be well researched.

---

_Closed by @BurntSushi on 2018-01-31 02:06_

---
