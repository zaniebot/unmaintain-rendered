```yaml
number: 684
title: "--ignore-file doesn't work"
type: issue
state: closed
author: mimoo
labels:
  - question
assignees: []
created_at: 2017-11-17T15:23:05Z
updated_at: 2018-02-18T22:17:57Z
url: https://github.com/BurntSushi/ripgrep/issues/684
synced_at: 2026-01-12T16:13:22Z
```

# --ignore-file doesn't work

---

_@mimoo_

Hello,

The --ignore-file option doesn't work.



---

_Comment by @BurntSushi on 2017-11-17 15:29_

This bug report is not actionable. Please include a minimally reproducible example that demonstrates the problem you're experiencing. Please include all inputs, the expected output and the actual output. Please also include the version of ripgrep you're using and the OS you're running. Please make an effort to describe the problem in such a way that others can reproduce it exactly.

Next time you file a bug report (on any issue tracker for any project), please answer these questions up front instead of requiring other people to spend time asking for these things.

---

_Label `question` added by @BurntSushi on 2017-11-17 15:29_

---

_Comment by @mimoo on 2017-11-17 16:04_

Sorry, here's a bit more info, I'm using macOS:

```
$ rg --version                                                
ripgrep 0.6.0
-AVX -SIMD
```

I'm testing this on golang's standard library that contains `_test.go` files that I'd like to ignore

```
$ /usr/local/Cellar/go/1.7.1/libexec/src/crypto/tls/ rg --ignore-file tls_test.go panic
...
tls_test.go
...
```

I tried `--ignore-file "tls_test.go"` or `--ignore-file "*_test.go"` nothing works.

Switching to `ag` it works.

---

_Comment by @BurntSushi on 2017-11-17 16:10_

Please read the documentation of the flag you're using:

```
        --ignore-file <FILE>...             
            Specify additional ignore files for filtering file paths. Ignore files should be in the
            gitignore format and are matched relative to the current working directory. These ignore
            files have lower precedence than all other ignore files. When specifying multiple ignore
            files, earlier files have lower precedence than later files.
```

If you want to ignore a specific file or files that end in a certain suffix, then use `rg -g '!*_test.go' panic`. There are more examples [in the README](https://github.com/BurntSushi/ripgrep#whirlwind-tour).

---

_Closed by @BurntSushi on 2017-11-17 16:10_

---

_Comment by @mimoo on 2017-11-17 16:58_

I read the documentation. What is wrong with the way I use `--ignore-file`? I'm not sure I understand your answer. I've looked for information elsewhere here but I can't find any.

---

_Comment by @BurntSushi on 2017-11-17 17:01_

The `--ignore-file` flag, as documented, specifies a path to a file that specifies a list of ignore rules, which is of the same format as `.ignore` and `.gitignore`. In the command you've provided, you're telling ripgrep to read ignore rules from `tls_test.go`, which is a Go source file, and not a file containing ignore rules.

If I run the command you provided, ripgrep is *telling* you that the given ignore file is invalid by spewing a bunch of errors.

---

_Comment by @mimoo on 2017-11-17 18:05_

Oh ok I see! I guess I did not understand the phrasing like this.


---

_Comment by @clehene on 2018-02-18 22:01_

Just spent about 30 minutes trying to figure the same thing out. The documentation may be accurate, but still not making it easy to understand it and very easy to confuse.

This:
>  specifies a path to a file that specifies a list of ignore rules, which is of the same format as .ignore and .gitignore

should be in the documentation instead.

---

_Comment by @BurntSushi on 2018-02-18 22:07_

@clehene Could you say what is confusing about the current docs specifically? As far as I can tell, they basically say the same thing as your revision. They even include a tip to use `-g` if you want to include/exclude files directly on the command line.

---

_Comment by @clehene on 2018-02-18 22:14_

@BurntSushi not sure if this is better https://github.com/BurntSushi/ripgrep/pull/817 but the goal is to make the intention clear immediately.

I did `rg --help` then searched for `ignore`, got to `ignore-file` and understood it takes ignore patterns. Tried, failed read a few more times. Tried with gitignore patterns, failed and after a few googles got to this issue :). I think it's the fact that I was sure this is the right flag that made me overlook the rest. Perhaps the flag name is too easy to confuse..


---

_Comment by @BurntSushi on 2018-02-18 22:17_

@clehene Interesting! I definitely agree that the flag name is not ideal. You are not the only person I've seen get hung up on this. Let's iterate on the exact re-wording in your PR. :)

---
