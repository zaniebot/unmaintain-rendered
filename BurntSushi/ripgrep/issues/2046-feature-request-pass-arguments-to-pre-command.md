```yaml
number: 2046
title: "[Feature Request] Pass arguments to --pre command"
type: issue
state: closed
author: toonn
labels:
  - doc
  - rollup
assignees: []
created_at: 2021-10-29T23:29:00Z
updated_at: 2023-11-25T21:58:42Z
url: https://github.com/BurntSushi/ripgrep/issues/2046
synced_at: 2026-01-12T16:13:24Z
```

# [Feature Request] Pass arguments to --pre command

---

_@toonn_

#### Describe your feature request

I'm writing a Git pre-commit hook that should prevent me from committing certain sections I have marked `DO NOT COMMIT`.

I would like to do something like the following because I only want to grep the parts of files that have been staged:

```
rg -Fl --pre 'git diff --cached -- $1'  'DO NOT COMMIT'
```

However, the `--pre` argument only supports either commands found on the `PATH` or an absolute path to a file. Right now I'm working around this by using an absolute path to a script, `~/src/repo/.git/hooks/git-diff-cached.sh`, but this is a very inelegant approach. It requires me to hardcode the repo's path in the hook. If I want to use this hook in multiple repos I have to either update that path every time or put the script in one central location.

I've attempted to use process substitution, `rg -Fl --pre <(printf 'git diff --cached -- $1')  'DO NOT COMMIT'` but named pipes are created without execute permission.

Hence, my feature request. Would it be possible to add a flag specifying arguments to be passed to the preprocessing command or having a separate `--pre-script` flag we can pass a string to rather than a file?


---

_Comment by @BurntSushi on 2021-10-30 02:10_

I'm not quite following here. Why not do something like `git diff ... | rg ...` in your hook? The pre flag should really only be relevant when you need to apply a transformation on the content in order to get it from a non-UTF-8 encoding (like a PDF) to a UTF-8 encoding.

---

_Comment by @toonn on 2021-10-30 12:25_

That's because I want to output the filename of the files containing the erroneously committed sections. If there's a match I already know the contents and `-l` would give me `<stdin>`, which is useless.

---

_Comment by @BurntSushi on 2021-10-30 13:10_

Ah I see. Yeah, this isn't really how the `--pre` flag is intended to be used. This is also a good example of the XY problem: you have a problem, came up with a solution and that solution doesn't quite fit. Which turns into a feature request to tweak that solution, but without actually discussing the full scope of the original problem you're trying to solve. So in that vein, I _think_ the problem you're trying to solve is something like this: I want to write a pre-commit hook that detects if a substring exists in the diffs of any staged files, and if such a substring exists, print the name of the file.

So I think I would solve that problem like so:

```
for f in $(git diff --cached --name-only); do
 if git diff --cached -- "$f" | rg -qF "TESTING"; then
   echo $f
 fi
done
```

As for your actual request, the reason why I won't add it is because it complicates things quite a bit. There's a categorical difference between "execute a path" and "execute a path with some arguments." At the OS level, those arguments need to be specified individually, which means ripgrep has to split those arguments somehow. Or accept the arguments in a way where they are already split.

> It requires me to hardcode the repo's path in the hook.

Also, I don't think this is true. You should be able to put a script in a repo and then call that script from a git hook. It's not clear to me why you think you need to hard code an absolute path to do this. In particular, from `man githooks`:

> Before Git invokes a hook, it changes its working directory to either $GIT_DIR in a bare repository or the root of the working tree in a non-bare repository.

So just pass a relative path to the `--pre` flag? Should work fine.

> However, the `--pre` argument only supports either commands found on the `PATH` or an absolute path to a file.

What gives you that idea?

```
$ tree
.
├── foo
└── tester

0 directories, 2 files

$ cat foo
abc
def
hij
klm
nop

$ cat tester
#!/bin/sh

exec tac "$@"

$ rg '^\w' foo
1:abc
2:def
3:hij
4:klm
5:nop

$ rg '^\w' foo --pre tac
1:nop
2:klm
3:hij
4:def
5:abc

$ rg '^\w' foo --pre ./tester
1:nop
2:klm
3:hij
4:def
5:abc
```

---

_Comment by @toonn on 2021-10-30 14:44_

This could indeed be solved with nested for loops. But I hope you can see how it would be a lot more convenient if I could pass a simple command in-line.

The reason I didn't think relative paths would work is I tried it (used a path relative to the hook though, seems like the reason it didn't work, my bad) *and* the documentation:

> This option expects the COMMAND program to either be an absolute path or to be available in your PATH.

I think the feature request does fit the existing option very well but I understand there can be implementation issues that would make a new option necessary. Being able to write a single-line preprocessor is incredibly useful, it could even subsume the current behavior because you could run `exec ./my-script` or similar. I saw there were a lot of issues about compression levels, this would be easily addressed by being able to pass arguments.

Doesn't ripgrep already need to deal with argument splitting or is that taken care of by the shell? If it is, couldn't the feature just rely on invoking a shell, something like `bash -c 'user command'`?

---

_Comment by @BurntSushi on 2021-10-30 15:11_

> This could indeed be solved with nested for loops. But I hope you can see how it would be a lot more convenient if I could pass a simple command in-line.

Of course. But the win here is _very_ small and the costs required to achieve that win are pretty high. It's not like the shell I wrote for you is terribly complicated here. And this isn't even something you need to run by hand a lot. You're looking to shove this into a pre-commit hook script and then forget about it. It doesn't make sense to optimize for convenience in those sorts of scenarios IMO.

>  saw there were a lot of issues about compression levels, this would be easily addressed by being able to pass arguments.

It's also easily solved with a wrapper script.

> Doesn't ripgrep already need to deal with argument splitting or is that taken care of by the shell?

The shell.

> If it is, couldn't the feature just rely on invoking a shell, something like `bash -c 'user command'`?

That's a non-starter because it relies on executing a shell. Doesn't work if bash isn't installed.

I think the bottom line here is that the existing facilities are flexible enough to make pretty much anything work, with a simple implementation and obvious behavior, even if it isn't always the most convenient.

It looks like the docs should be updated, but otherwise this is `wontfix`. Sorry.

---

_Label `doc` added by @BurntSushi on 2021-10-30 15:11_

---

_Comment by @toonn on 2021-10-30 16:30_

Would a patch that implements a new flag or expands on the behavior of `--pre` have a chance of being accepted?

---

_Comment by @BurntSushi on 2021-10-30 16:48_

In order to answer that, I have to know what new behavior it implements... If you're talking about this feature request, then no, it wouldn't, for reasons I already described.

---

_Comment by @florianpircher on 2022-06-22 22:46_

I would love an arguments parameter for `--pre` as well. For me, a command that I work with validates the file if run as `COMMAND FILE` and pretty-prints its otherwise binary contents with `COMMAND -p FILE` (and there are other output forms that are sometimes desired like `COMMAND -convert json FILE` or `COMMAND -convert xml FILE`). Sure, I could create a custom script for each output format and specify that script as the command, but allowing for arguments to the command would simplify the process:

```
rg --pre COMMAND --pre-args '-p' FILE
```

---

_Comment by @BurntSushi on 2022-06-22 23:01_

@florianpircher Yes, the intended path here would indeed be to create a custom script for each thing you need.

---

_Comment by @florianpircher on 2022-06-23 07:46_

OK, understandable. I guess this issue can be closed then.

---

_Closed by @BurntSushi on 2023-11-24 20:11_

---

_Label `doc` removed by @BurntSushi on 2023-11-24 20:11_

---

_Label `wontfix` added by @BurntSushi on 2023-11-24 20:11_

---

_Comment by @toonn on 2023-11-25 14:01_

@BurntSushi, you said [earlier up the thread](https://github.com/BurntSushi/ripgrep/issues/2046#issuecomment-955297724) that the docs should be updated to properly reflect that `--pre` works with paths relative to the current working directory. Could this issue be reopened until that is addressed? It doesn't seem to be changed in the man page for 13.0.0 that I have locally.

---

_Reopened by @BurntSushi on 2023-11-25 14:12_

---

_Label `wontfix` removed by @BurntSushi on 2023-11-25 14:19_

---

_Label `doc` added by @BurntSushi on 2023-11-25 14:19_

---

_Label `rollup` added by @BurntSushi on 2023-11-25 14:19_

---

_Closed by @BurntSushi on 2023-11-25 20:04_

---

_Comment by @toonn on 2023-11-25 21:58_

Thank you! :heart: 

I still wish the feature would be considered but clearer docs always help.

---
