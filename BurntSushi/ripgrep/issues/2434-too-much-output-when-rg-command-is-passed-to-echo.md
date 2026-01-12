```yaml
number: 2434
title: "too much output when `rg` command is passed to `echo`"
type: issue
state: closed
author: lyrixx
labels: []
assignees: []
created_at: 2023-02-28T16:09:08Z
updated_at: 2023-02-28T16:57:15Z
url: https://github.com/BurntSushi/ripgrep/issues/2434
synced_at: 2026-01-12T16:13:24Z
```

# too much output when `rg` command is passed to `echo`

---

_@lyrixx_

#### What version of ripgrep are you using?

```
rg --version
ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

with cargo

#### What operating system are you using ripgrep on?

```
Distributor ID: Ubuntu
Description:    Ubuntu 22.04.2 LTS
Release:        22.04
Codename:       jammy
```

#### Describe your bug.

RG return the list of the file, instead of what it matches in the document

#### What are the steps to reproduce the behavior?

Use the following code to reproduce:

```
mkdir ripgrep
cd ripgrep

touch a b c d e
cat > CHANGELOG.md <<'EOF'
CHANGELOG
=========

6.3
---

 * foo bar baz

6.2
---

 * something else

EOF
```

I have a more complex regex, but I have simplified it to show the bug


Not in a sub-shell, no trailing space ✅ 
```
rg '6.3\n---\n\n' -U --multiline-dotall  CHANGELOG.md 
4:6.3
5:---
6:
```

Not in a sub-shell, with trailing space ✅ 
```
>/tmp/foobar/ripgrep rg '6.3\n---\n\n ' -U --multiline-dotall  CHANGELOG.md 
4:6.3
5:---
6:
7: * foo bar baz
```

In a subshell, no trailing space ✅ 
```
echo `rg '6.3\n---\n\n' -U --multiline-dotall  CHANGELOG.md` 
6.3 ---
```

In a subshell, with trailing space ❌ 

```
                                          #⬇ There is a trailing space here
>/tmp/foobar/ripgrep echo `rg '6.3\n---\n\n ' -U --multiline-dotall  CHANGELOG.md` 
6.3 --- a b c CHANGELOG.md d e foo bar baz
```

---

As you can see, there is something super strange. As soon as I put a trailing space, rg print in the middle of the output the files in the current directly!

---

_Comment by @lyrixx on 2023-02-28 16:12_

Ohhhh. Just understood the bug ! Sorry for the noise !

---

_Closed by @lyrixx on 2023-02-28 16:12_

---

_Comment by @BurntSushi on 2023-02-28 16:34_

If you fix your own problem, it is good practice to include the answer so that if others have the same problem, it will be more helpful. In this case, I actually get different output than you for the last example:

```
$ echo `rg '6.3\n---\n\n ' -U --multiline-dotall  CHANGELOG.md`
6.3 --- * foo bar baz
```

Which looks correct to me. That trailing space matches something on the next line, and thus ripgrep prints the whole thing.

I don't think this has anything with running ripgrep as a sub-process. The output is just a little weird because you're passing it to `echo`.

---

_Renamed from "Issue when run in a subprocess" to "too much output when `rg` command is passed to `echo`" by @BurntSushi on 2023-02-28 16:35_

---

_Comment by @lyrixx on 2023-02-28 16:57_

Sorry for not putting my answer here :( 

the problem was indeed about echoing something that containes `*` and so it globs...

I fixed the issue by surrounding it with `"`:


```
>/tmp/foobar/ripgrep echo `rg '6.3\n---\n\n ' -U --multiline-dotall  CHANGELOG.md` 
6.3 --- a b c CHANGELOG.md d e foo bar baz
>/tmp/foobar/ripgrep echo "`rg '6.3\n---\n\n ' -U --multiline-dotall  CHANGELOG.md`"
6.3
---

 * foo bar baz
```

---
