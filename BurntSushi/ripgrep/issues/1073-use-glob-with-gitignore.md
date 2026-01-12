```yaml
number: 1073
title: Use glob with .gitignore
type: issue
state: closed
author: adzenith
labels:
  - invalid
assignees: []
created_at: 2018-10-01T14:43:09Z
updated_at: 2018-10-08T18:05:24Z
url: https://github.com/BurntSushi/ripgrep/issues/1073
synced_at: 2026-01-12T16:13:22Z
```

# Use glob with .gitignore

---

_@adzenith_

#### What version of ripgrep are you using?

```
ripgrep 0.10.0 (rev 8a7db1a918)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

GitHub binary release via Apt.

#### What operating system are you using ripgrep on?

`Linux 4.15.0-33-generic #36~16.04.1-Ubuntu`

#### Describe your question, feature request, or bug.

I am trying to use `-g` to add a glob to my `.gitignore`, rather than replacing the `.gitignore` entirely. Just `-g` seems to replace the `.gitignore`. I've tried to add the `.gitignore` back with `--ignore` and `--ignore-file .gitignore` and neither does the trick. I've also tried adding a `-g '*'` both before and after these flags, in the hope that I could first add the glob I want, then exclude stuff in the `.gitignore`, and finally include everything else - but that seems to just include everything, as if the last glob overrides the `.gitignore` again. Is there a way to do what I'm trying to do?

Example call, trying to search in the project but also in the build directory:
`rg 'search_string' -g '.build' --ignore-file .gitignore`

---

_Comment by @BurntSushi on 2018-10-01 14:47_

Please provide a reproducible example. As the issue template requested, please provide the input, actual output and expected output.

---

_Comment by @adzenith on 2018-10-01 19:36_

The template says "If this is a bug, what are the steps to reproduce the behavior?" - I didn't think of it as a bug, so I didn't fill in that section. It's either a feature request or a request for information on the feature, if that feature already exists. With that said, here's a script you can run to see what I mean if it's helpful:

```
mkdir example && cd example
git init
echo 'foo/*' >> .gitignore
echo 'foo2/*' >> .gitignore
mkdir foo
mkdir foo2
echo result > foo/bar
echo result > foo2/bar
echo result > bar
rg result
# --  expected == actual  --
# bar
# 1:result
rg -g 'foo2/*' result
# --  expected == actual  --
# foo2/bar
# 1:result
rg -g 'foo2/*' result --ignore  # or some other flag?
# --  expected/desired  --
# bar
# 1:result
# 
# foo2/bar
# 1:result
#
#
# --  actual  --
# foo2/bar
# 1:result
```

---

_Comment by @BurntSushi on 2018-10-01 20:06_

The `--glob` flag is implemented using whitelist semantics. What that means is that if you provide at least one whitelist item, then ripgrep will require that everything it searches match *at least one item in the whitelist*. So when you use `-g 'foo2/*'`, you're effectively telling ripgrep to only search files in `foo2` and nothing else. This is intended behavior and I don't see that changing.

If you just want to add additional files to your search, then just tell ripgrep to search them using standard shell globs. For example, `rg result ./ ./foo2/*`.

I think you'll want to pay attention to the docs on precedence. For example, the glob flag takes precedence over any type of .gitignore file. There is no way to mix and match them.

---

_Label `invalid` added by @BurntSushi on 2018-10-08 18:04_

---

_Comment by @BurntSushi on 2018-10-08 18:05_

Closing due to inactivity from the OP. As far as I can tell, things are working as intended. In particular, the intended way to search directories without respecting ignore rules is to just tell ripgrep to search the directory.

---

_Closed by @BurntSushi on 2018-10-08 18:05_

---
