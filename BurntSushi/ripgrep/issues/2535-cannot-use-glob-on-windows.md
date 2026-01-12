```yaml
number: 2535
title: Cannot use glob on Windows
type: issue
state: closed
author: ashrasmun
labels: []
assignees: []
created_at: 2023-06-14T08:31:54Z
updated_at: 2023-06-14T13:39:45Z
url: https://github.com/BurntSushi/ripgrep/issues/2535
synced_at: 2026-01-12T16:13:24Z
```

# Cannot use glob on Windows

---

_@ashrasmun_

#### What version of ripgrep are you using?

```
w:\sandbox\test>rg --version
ripgrep 13.0.0 (rev af6b6c543b)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

https://github.com/BurntSushi/ripgrep/releases

#### What operating system are you using ripgrep on?

Windows 10

#### Describe your bug.

I would like to search for text in a subset of files, not all of them. I tried using `-g/--glob`, but it doesn't seem to work.

#### What are the steps to reproduce the behavior?

```
w:\sandbox\test>type nul>test.txt

w:\sandbox\test>nvim test.txt # Add 'lib' to the file

w:\sandbox\test>rg lib
test.txt
lib

w:\sandbox\test>rg lib -g '*'
No files were searched, which means ripgrep probably applied a filter you didn't expect.
Running with --debug will show why files are being skipped.
```

#### What is the actual behavior?

https://gist.github.com/ashrasmun/245ec0e48dd2b8377b0aa4616b7e9b2d

#### What is the expected behavior?

I would like to search for string 'lib' in a narrow set of files, not all of them.


---

_Comment by @BurntSushi on 2023-06-14 13:02_

Thanks for the report. Can you say why you didn't run with `--debug` and show the output, as the error message suggests?

My best guess is that you have a Windows problem. IIRC, single quotes don't work the way your command suggests you think they do: https://superuser.com/questions/324278/how-to-make-windows-command-prompt-treat-single-quote-as-though-it-is-a-double-q

It might be a good idea to read a tutorial on how to use the Windows command line. I'm not a Windows user myself so I'm not sure what the best one might be.

---

_Comment by @ltrzesniewski on 2023-06-14 13:34_

They actually provided the `--debug` output in the gist, and it says:

> `glob converted to regex: Glob { glob: "**/'*'", re: "(?-u)^(?:/?|.*/)'[^/]*'$"` [...]

So yeah, the single quotes are interpreted literally and they end up in the glob pattern.

---

_Comment by @BurntSushi on 2023-06-14 13:38_

Ah nice! I completely missed the gist link. Thanks for pointing that out. :)

---

_Locked by @ghost on 2023-06-14 13:39_

---
