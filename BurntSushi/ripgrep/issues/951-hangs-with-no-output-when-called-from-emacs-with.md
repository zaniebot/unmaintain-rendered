```yaml
number: 951
title: Hangs with no output when called from Emacs with make-process and pipe connection type
type: issue
state: closed
author: alphapapa
labels:
  - question
  - doc
assignees: []
created_at: 2018-06-16T05:06:54Z
updated_at: 2021-10-05T12:38:28Z
url: https://github.com/BurntSushi/ripgrep/issues/951
synced_at: 2026-01-12T16:13:22Z
```

# Hangs with no output when called from Emacs with make-process and pipe connection type

---

_@alphapapa_

#### What version of ripgrep are you using?

ripgrep 0.8.1 (rev c8e9f25b85)
-SIMD -AVX

#### How did you install ripgrep?

Debian package downloaded from link in readme.

#### What operating system are you using ripgrep on?

Ubuntu 14.04
Emacs 26.1

#### Describe your question, feature request, or bug.

`rg` does not seem to work properly with regard to pipes.

`grep` works:
```el
(let ((default-directory "~/src/emacs/emacs"))
  (setq argh (make-process :name "grep"
                           :command (list "rgrep" "TODO")
                           :connection-type 'pipe
                           :buffer (get-buffer-create "*grep*"))))
(with-current-buffer (process-buffer argh)
  (length (buffer-string))) ;; => 114100
(process-status argh) ;; => exit
```
`rg` does not:
```el
(let ((default-directory "~/src/emacs/emacs"))
  (setq argh (make-process :name "rg"
                           :command (list "rg" "TODO")
                           :connection-type 'pipe
                           :buffer (get-buffer-create "*rg*"))))
(with-current-buffer (process-buffer argh)
  (length (buffer-string))) ;; => 0
(process-status argh) ;; => run
```
The process is still running:
```sh
$ pgrep -a rg
31513 /usr/bin/rg TODO
```
However, if I use the `pty` type, it works:
```el
(let ((default-directory "~/src/emacs/emacs"))
  (setq argh (make-process :name "rg"
                           :command (list "rg"
                                          "TODO")
                           :connection-type 'pty
                           :buffer (get-buffer-create "*rg*"))))
(with-current-buffer (process-buffer argh)
  (length (buffer-string))) ;; => 96824
(process-status argh) ;; => exit
```
`ag` also seems to work correctly, like `grep`.

Thanks for your help.

---

_Comment by @BurntSushi on 2018-06-16 10:31_

Sorry, but this bug report isn't something I can understand. Please reproduce the problem outside emacs. Another problem with this bug report is that I have no idea what it is you are trying to do. Another problem is that you say that grep works, but you actually seem to be invoking something else, namely, `rgrep`. I don't know what that is.

The typical root cause of this sort of behavior is that ripgrep is blocking on stdin because it thinks you want to search stdin. Namely, when no paths are given to ripgrep, then it must guess at what you want. If it looks like there is something on stdin, then it will search stdin. Otherwise, it searches the current working directory.

Often, the simplest work around is too just be explicit. e.g., `rg TODO -` for searching stdin or `rg TODO ./` for searching the current working directory.

ag should not be used as a model of correctness. It has so many bugs that it sometimes only does the "right" thing accidentally. With that said, it would be interesting to compare the stdin checking code in both tools, but I'm in mobile.

---

_Label `question` added by @BurntSushi on 2018-06-17 00:19_

---

_Comment by @alphapapa on 2018-06-17 01:07_

Hi,

> Often, the simplest work around is too just be explicit. e.g., rg TODO - for searching stdin or rg TODO ./ for searching the current working directory.

Thanks, you're right that the problem was that the directory to search was unspecified.  This works:

```el
(let ((default-directory "~/src/emacs/emacs"))
  (setq argh (make-process :name "rg"
                           :command (list "rg" "TODO" ".")  ; <- "." specified the current directory
                           :connection-type 'pipe  ; <- using a pipe instead of a pty
                           :buffer (get-buffer-create "*rg*"))))
(with-current-buffer (process-buffer argh)
  (length (buffer-string)))  ;; => 210825
(process-status argh)  ;; => exit
```

> Sorry, but this bug report isn't something I can understand. Please reproduce the problem outside emacs. 

This is difficult (for me, at least) because the problem seems to be at a deeper level than, e.g. calling `rg` from a shell, and I'm not an expert in how Unix processes work.  

> The typical root cause of this sort of behavior is that ripgrep is blocking on stdin because it thinks you want to search stdin. Namely, when no paths are given to ripgrep, then it must guess at what you want. If it looks like there is something on stdin, then it will search stdin. Otherwise, it searches the current working directory.

Yes, it seems that, basically, when `rg` thinks it's outputting to a pipe, and there is no specified directory to search, it doesn't do any searching (calling with `--debug` does give debug output, but no searching happens, and the program blocks).

However, `grep` doesn't work this way: when it's called with output to a pipe but no data on STDIN, it searches for files, just as it does when called with output to a tty.

Maybe this is the intended behavior, that when `rg` is called with STDOUT going to a pipe, it expects to search STDIN rather than files.  However, I'm not sure if this is documented, e.g.:
```sh
rg --help
...
    rg [OPTIONS] PATTERN [PATH ...]
    rg [OPTIONS] [-e PATTERN ...] [-f FILE ...] [PATH ...]
    rg [OPTIONS] --files [PATH ...]
    rg [OPTIONS] --type-list
```
The only mention of STDIN is:
```
        --no-filename                       
            Never print the file path with the matched lines. This is the default when
            ripgrep is explicitly instructed to search one file or stdin.
```
So I guess that either this needs to be documented clearly, or the behavior needs to be improved to search for files unless data is received on STDIN.  For parity with `grep`, I suppose the latter is better.

> ag should not be used as a model of correctness. It has so many bugs that it sometimes only does the "right" thing accidentally. With that said, it would be interesting to compare the stdin checking code in both tools, but I'm in mobile.

Indeed, I looked at `ag`'s code and repo, and it definitely has some issues regarding option handling.

> Another problem is that you say that grep works, but you actually seem to be invoking something else, namely, rgrep. I don't know what that is.

`rgrep` is a standard alias for `grep -r`.  On Debian, it's part of the `grep` package:
```sh
$ cat /usr/bin/rgrep
#!/bin/sh                                                                                                                                                                                                                                           
                                                                                                                                                                                                                                                    
exec grep -r "$@"
```

Thanks for your help.

---

_Comment by @BurntSushi on 2018-06-17 01:31_

The fact that `rgrep` is aliased to `grep -r` explains the mystery. ripgrep has no `-r` flag because recursive search is the default, but ripgrep should also support searching stdin when available without the user needing to specify `-`. Thus, ripgrep must guess at what you want. `grep -r` does not do any such thing, because `-r` always makes it do recursive search. It ignores what's on stdin. Example:

```
$ grep -Hr a /tmp/elsewhere
/tmp/elsewhere:a
$ grep -Hr a ./in-current-dir
./in-current-dir:a
$ grep -Hr a
in-current-dir:a
$ cat /tmp/elsewhere | grep -Hr a
in-current-dir:a
```

Thus, there is no such thing as "parity" in this case. **This is one of the fundamental differences between ripgrep and grep.**

Note that I still don't know what it is you're trying to achieve. Why are you connecting a pipe to your process when you don't want to send anything on stdin?

> This is difficult (for me, at least) because the problem seems to be at a deeper level than, e.g. calling rg from a shell, and I'm not an expert in how Unix processes work.

When no file paths are given to ripgrep, it uses a heuristic to decide what to do. Namely, ripgrep will block to search on stdin when no paths are given **and** stdin is not a tty **and** stdin is either a file or a fifo. In all other cases, it searches the current working directory. When you connect a pipe to it, my guess is that ripgrep probably sees that as a fifo and blocks.

> Indeed, I looked at ag's code and repo, and it definitely has some issues regarding option handling.

The only difference that I can see between ag and ripgrep on this particular issue is that ripgrep checks if stdin is a tty or not. If it's a tty, then it always does recursive search. The corollary to that is that the only way for ripgrep to search stdin is if its stdin is _not_ a tty. ag has no such check. All it does is check if stdin is a file or a fifo (which ripgrep also does):

```c
    rv = fstat(fileno(stdin), &statbuf);
    if (rv == 0) {
        if (S_ISFIFO(statbuf.st_mode) || S_ISREG(statbuf.st_mode)) {
            opts.search_stream = 1;
        }
    }
```

That ag doesn't block is actually interesting, because in order for ripgrep to search stdin, it must detect stdin as either a file or a fifo. That should cause ag to also attempt to search stdin. So probably something else is going on, but diving into emacs is beyond the scope of what I'm willing to debug.

> So I guess that either this needs to be documented clearly

It might be worth a mention in the docs, but ripgrep's default behavior is pretty much what you'd expect it to do. If you don't give it a path, then it searches CWD, unless you're piping something into it, in which case, it searches that data instead.

---

_Label `doc` added by @BurntSushi on 2018-06-17 01:31_

---

_Comment by @alphapapa on 2018-06-17 01:57_

Thanks, I understand better now.  I think it was actually an accident that I originally ran `rgrep` in that example rather than `grep`--I think I was trying to turn `rg` into `grep` and I overlooked the `r` at the beginning of the word.  Silly mistakes...  :/

> Note that I still don't know what it is you're trying to achieve. Why are you connecting a pipe to your process when you don't want to send anything on stdin?

Because Emacs' process code works much better when it uses a pipe rather than emulating a tty.  Chris Wellons explains the details in his blog: http://nullprogram.com/blog/2018/01/17/  It's very interesting how he used DTrace to figure out what was going on between `curl` and Emacs.

> It might be worth a mention in the docs, but ripgrep's default behavior is pretty much what you'd expect it to do. If you don't give it a path, then it searches CWD, unless you're piping something into it, in which case, it searches that data instead.

In this case, the problem was indeed that I misunderstood `rg`'s behavior and expectation of receiving data on STDIN.  It would be very helpful if this were explained in the docs.  As a first step, may I suggest changing the command examples to:
```
    rg [OPTIONS] PATTERN [PATH ...]
    rg [OPTIONS] [-e PATTERN ...] [-f FILE ...] [PATH ...]
    rg [OPTIONS] --files [PATH ...]
    rg [OPTIONS] --type-list
    command | rg [OPTIONS] PATTERN
```
That at least shows that connecting a pipe to `rg` implies searching STDIN and is not intended for searching paths.

Thanks.

---

_Comment by @BurntSushi on 2018-06-17 02:02_

@alphapapa I like that doc suggestion, yes!

---

_Closed by @BurntSushi on 2018-07-22 13:39_

---

_Comment by @jtrakk on 2021-01-12 21:37_

I am not sure but https://github.com/cosmicexplorer/helm-rg/issues/29 might be related since it involves emacs hanging when calling rg. TBH I didn't really understand the resolution here but posting the link in case some future reader finds it helpful.

---

_Comment by @jwbargsten on 2021-10-05 06:06_

So, after the jump to ripgrep 13, also neovim's `jobstart()` started to hang when calling ripgrep. I found a solution and I leave this comment for other people to "connect the dots". As the doc suggests, supply a path as last argument to ripgrep:

```
rg [OPTIONS] PATTERN [PATH ...]
# or if you want to start in the current dir:
rg [OPTIONS] PATTERN ./
```

This is also very well documented in the manpage/documentation of ripgrep:

> ripgrep will automatically detect if stdin exists and search stdin for a regex pattern, e.g. ls | rg foo. In some environments, stdin may exist when it shouldn't. To turn off stdin detection explicitly specify the directory to search, e.g. rg foo ./.

In most cases it should be therefore sufficient to add a `./` as last paramenter

This has also been suggested at [vi.stackexchange.com](https://vi.stackexchange.com/questions/25025/neovim-jobstart-not-receiving-stdout-sometimes-if-async-i-think)

---

_Comment by @BurntSushi on 2021-10-05 12:38_

Aye. See also: https://github.com/BurntSushi/ripgrep/issues/1892

---
