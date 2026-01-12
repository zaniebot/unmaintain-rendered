```yaml
number: 1024
title: "file:line:col doesn't play well with `--no-heading --context`"
type: issue
state: closed
author: timotheecour
labels:
  - question
assignees: []
created_at: 2018-08-23T08:07:18Z
updated_at: 2019-08-08T22:35:51Z
url: https://github.com/BurntSushi/ripgrep/issues/1024
synced_at: 2026-01-12T16:13:22Z
```

# file:line:col doesn't play well with `--no-heading --context`

---

_@timotheecour_

#### What version of ripgrep are you using?

ripgrep 0.9.0 (rev f1e025873f)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

brew install --HEAD ripgrep

#### What operating system are you using ripgrep on?

OSX

#### Describe your question, feature request, or bug.
```
rg --no-heading -C2 'translatable to any command'
GUIDE.md-6-and that readers have passing familiarity with using command line tools. This
GUIDE.md-7-also assumes a Unix-like system, although most commands are probably easily
GUIDE.md:8:translatable to any command line shell environment.
GUIDE.md-9-
GUIDE.md-10-
```

* the context lines show with `-` instead of `:` which doesn't allow to open these using common tools like sublime text (or even with built in commands say on iterm2 which allow you to click on a file to open it), eg:
`subl GUIDE.md:8:` works
`subl GUIDE.md-7-` doesn't work ; user needs to edit the cmd line

I'd like instead (at least via option --context-header-regular):
```
rg --no-heading -C2 'translatable to any command'
GUIDE.md:6:-and that readers have passing familiarity with using command line tools. This
GUIDE.md:7:-also assumes a Unix-like system, although most commands are probably easily
GUIDE.md:8:*translatable to any command line shell environment.
GUIDE.md:9:-
GUIDE.md:10:-
```
which still shows distinction between main line and context (via * vs -)




---

_Comment by @BurntSushi on 2018-08-23 09:48_

Huh? Sorry but I don't understand this request. What does : vs - have to do
with opening files? Sounds like a bug in your text editor.

On Thu, Aug 23, 2018, 04:07 Timothee Cour <notifications@github.com> wrote:

> What version of ripgrep are you using?
>
> ripgrep 0.9.0 (rev f1e0258
> <https://github.com/BurntSushi/ripgrep/commit/f1e025873f2eed114141e69a39dcf92f11b09971>
> )
> -SIMD -AVX (compiled)
> +SIMD +AVX (runtime)
> How did you install ripgrep?
>
> brew install --HEAD ripgrep
> What operating system are you using ripgrep on?
>
> OSX
> Describe your question, feature request, or bug.
>
> rg --no-heading -C2 'translatable to any command'
> GUIDE.md-6-and that readers have passing familiarity with using command line tools. This
> GUIDE.md-7-also assumes a Unix-like system, although most commands are probably easily
> GUIDE.md:8:translatable to any command line shell environment.
> GUIDE.md-9-
> GUIDE.md-10-
>
>
>    - the context lines show with - instead of : which doesn't allow to
>    open these using common tools like sublime text (or even with built in
>    commands say on iterm2 which allow you to click on a file to open it), eg:
>    subl GUIDE.md:8: works
>    subl GUIDE.md-7- doesn't work ; user needs to edit the cmd line
>
> I'd like instead (at least via option --context-header-regular):
>
> rg --no-heading -C2 'translatable to any command'
> GUIDE.md:6:-and that readers have passing familiarity with using command line tools. This
> GUIDE.md:7:-also assumes a Unix-like system, although most commands are probably easily
> GUIDE.md:8:*translatable to any command line shell environment.
> GUIDE.md:9:-
> GUIDE.md:10:-
>
> which still shows distinction between main line and context (via * vs -)
>
> —
> You are receiving this because you are subscribed to this thread.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/1024>, or mute the thread
> <https://github.com/notifications/unsubscribe-auth/AAb34qPT6a79aXzszZO7zm1zVR_XEgLNks5uTmK3gaJpZM4WJCnN>
> .
>


---

_Comment by @timotheecour on 2018-08-23 09:55_

sublime text (as many other tools) understands `subl file`, `subl file:line` and `subl file:line:column` , and has effect of opening file at given line and column.
it doesn't understand when a `-` is given in place of a `:` though (as `-` is treated as a filename character, not a delimiter)

this leads to 2 issues:

* when we copy a line of the `rg` output and paste it after `subl `, it requires changing the `-` to `:` otherwise it won't work
* when using semantic history in iterm2 on OSX (which allows clicking on file names output in the terminal), it also won't work:
![image](https://user-images.githubusercontent.com/2194784/44518907-65475b00-a680-11e8-8f25-1b062840b6f1.png)


---

_Comment by @timotheecour on 2018-08-23 10:05_

I see you're replying from email instead of github so you won't see my updated reply ; reposting here:

sublime text (as many other tools) understands `subl file`, `subl file:line` and `subl file:line:column` , and has effect of opening file at given line and column.
it doesn't understand when a `-` is given in place of a `:` though (as `-` is treated as a filename character, not a delimiter)

this leads to 2 issues:

* when we copy a line of the `rg` output and paste it after `subl `, it requires changing the `-` to `:` otherwise it won't work
* when using semantic history in iterm2 on OSX (which allows clicking on file names output in the terminal), it also won't work:
here's the image:

![image](https://user-images.githubusercontent.com/2194784/44518907-65475b00-a680-11e8-8f25-1b062840b6f1.png)



---

_Comment by @BurntSushi on 2018-08-23 10:51_

I'm going to pass on this. `file-line-{match}` is the standard grep format for showing contextual lines. I don't think another flag is worth adding to change this behavior. If you want other programs to understand the `file-line` format, then teach them. If you can't teach them, then write a wrapper script that understands the `file-line` format and translates it to something they understand.

---

_Closed by @BurntSushi on 2018-08-23 10:51_

---

_Label `question` added by @BurntSushi on 2018-08-23 10:52_

---

_Comment by @timotheecour on 2019-08-08 22:00_

@BurntSushi could we please re-open? `file-line-match` is inherently ambiguous because `-` is valid in filenames, so there's no reliable way to teach tools without introducing some other issues / corner cases, eg:

```txt
compiler/README-10-bar.txt
```
is this a file `compiler/README-10-bar.txt` or `compiler/README` at line `10` followed by context `bar.txt`
there's no way to tell.

Allowing customizing the 1st separator is simple and would solve this; eg:
```
# default:
compiler/README-10-bar.txt
# with rg --contextSep=':'
compiler/README:10-bar.txt
```


---

_Comment by @okdana on 2019-08-08 22:35_

`:` is just as valid in file names (at least on UNIX), so that doesn't seem like it would be much better

Anyway, if you use `-0` the file name will be separated from the line number (or match) by a `NUL`; the output should be pretty reliably machine-parseable then

---
