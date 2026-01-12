```yaml
number: 1442
title: "glob matching doesn't seem to work consistenly for plain directory match"
type: issue
state: closed
author: w08r
labels:
  - doc
  - rollup
assignees: []
created_at: 2019-12-06T14:53:11Z
updated_at: 2020-03-15T17:19:17Z
url: https://github.com/BurntSushi/ripgrep/issues/1442
synced_at: 2026-01-12T16:13:23Z
```

# glob matching doesn't seem to work consistenly for plain directory match

---

_@w08r_

#### What version of ripgrep are you using?

```
▶ rg --version                                                                                                                                                                    
ripgrep 11.0.1
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

Compiled from source

#### What operating system are you using ripgrep on?

MacOS (Catalina)

#### Describe your question, feature request, or bug.

I think that passing a plain directory as argument to `-g` should match files in that directory. Moreover, prefixing a match string with `!` should negate the results. This seems to the behaviour most of the time.

#### If this is a bug, what are the steps to reproduce the behavior?

```
## from temp, empty directory
mkdir a b
echo hello > a/test.txt
cp a/test.txt b
rg -g a hello ## return nothing
rg -g '!b' hello ## returns match from file in 'a'
```

#### If this is a bug, what is the actual behavior?

Above script yields following (showing output of negated search only)
```
▶ sh /tmp/run.sh                                                                                                                                                                  
a/test.txt
1:hello
```

#### If this is a bug, what is the expected behavior?

Should have yielded results for either both or neither (preferably both) the invocations in the reproduction steps.


---

_Comment by @w08r on 2019-12-06 14:54_

I am very new to rust, but if this is an issue you would like mending, I would be happy to have a stab at a PR.

---

_Comment by @BurntSushi on 2019-12-06 15:01_

This is correct behavior. The `-g/--glob` flag applies to _every_ file path searched. So the `-g a` behavior will cause ripgrep to descend into your `a` directory but _not_ your `b` directory. However, `a` doesn't match any file inside your `a` directory. You can confirm this behavior by adding a `a/a` file:

```
$ echo hello > a/a
$ rg -g a hello
a/a
1:hello
```

If you do want to search everything in `a`, then you need to use a glob pattern that _both_ matches the directory and the the files inside of it:

```
$ rg -g 'a/**' hello
a/a
1:hello

a/test.txt
1:hello
```

Your negation pattern works as you expect because it avoids descending into the `b` directory completely.

With all of this said, while the behavior is correct, the docs for the `--glob` flag could really be improved, so we can use this ticket to track that.

---

_Label `doc` added by @BurntSushi on 2019-12-06 15:01_

---

_Comment by @w08r on 2019-12-06 15:27_

Thanks for the quick response, sir, and for the awesome tool!

---

_Label `rollup` added by @BurntSushi on 2020-03-15 14:30_

---

_Closed by @BurntSushi on 2020-03-15 17:19_

---
