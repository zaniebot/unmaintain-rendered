```yaml
number: 91
title: "Searching filenames (`-g` option in `ag`)"
type: issue
state: closed
author: Valloric
labels: []
assignees: []
created_at: 2016-09-25T22:29:39Z
updated_at: 2024-05-23T11:52:03Z
url: https://github.com/BurntSushi/ripgrep/issues/91
synced_at: 2026-01-12T16:13:21Z
```

# Searching filenames (`-g` option in `ag`)

---

_@Valloric_

Ag supports using `-g` to recursively search filenames matching the provided pattern. Ripgrep doesn't have this feature (to the best of my knowledge; I've read the `--help` output three times looking for it); it's the only ag feature holding me back from fully switching to ripgrep. :)

Since ripgrep already has something unrelated mapped to `-g`, the option name should be something else. The incompatibility with ag will be unfortunate, but survivable.  


---

_Comment by @BurntSushi on 2016-09-25 22:30_

I'm pretty sure this has been reported at least several times now. :-) `rg` has `--files` for listing files and `-g` for glob matching.


---

_Closed by @BurntSushi on 2016-09-25 22:34_

---

_Comment by @Valloric on 2016-09-25 22:36_

> I'm pretty sure this has been reported at least several times now. :-) 

Sorry about that! It might be indicative of a documentation "bug"; it seems people are having trouble locating this feature.


---

_Comment by @Valloric on 2016-09-25 22:40_

Ah, I get it now. It's a combination of two flags: `--files` lists all the files that would have been searched and `-g` can be used to only search files matching a pattern.

This is very non-obvious. It requires users to fully understand _two_ separate, unrelated options and to realize that when combined, they can be used to solve their problem.

Since ripgrep will have lots of users coming from ag who will expect a simple single-flag solution, it might be a good idea to have a single flag (much like ag) for this use-case, if for any reason, than to save you the headache of closing all these issue reports. :)


---

_Comment by @BurntSushi on 2016-09-25 22:51_

I'd like to solve this with better documentation. It's clear this is an important feature to many, but it's decidedly subservient to ripgrep's primary focus, so I'd like to avoid adding extra flags for it.


---

_Comment by @gabrielmagno on 2016-09-29 16:16_

That would be a great feature. But it is important to notice the differences.

Let's say I have this directory tree:

```
.
└── path
    └── to
        ├── file
        │   ├── pattern (file)
        │   └── pattern.txt (file)
        ├── pattern
        │   └── fileA.txt (file)
        └── patterns
            └── fileB.txt (file)
```
- `ag` returns all the files that have "pattern" in **any part** of its **path**

```
$ ag -g "pattern"
path/to/pattern/fileA.txt
path/to/patterns/fileB.txt
path/to/file/pattern
path/to/file/pattern.txt
```
- `rg` returns all the files that have "pattern" as its **name**

```
$ rg --files -g "pattern"
path/to/file/pattern
```

So, ideally, if such a feature is implemented in ripgrep to reproduce what The Silver Search does, it should check the path (not only the name) and also match any part of the string (not an exact match, not a glob).


---

_Comment by @BurntSushi on 2016-09-29 16:46_

Remember that `-g` gives you the ability to use a _glob_, so if you want to match any component of the path, then `**/pattern/**` will work. (Well, to match `pattern.txt` and `path/to/patterns`, you will actually need `**/pattern*/**`.)


---

_Comment by @nateozem on 2016-12-27 04:56_

In case somebody wouldn't know the syntax for doing a search with the condition of excluding files with a certain name using the command line, here is an example to do so:

    $ rg "from_str" -g "\!tags" -g "repos/*"
                        ^^^

As shown, you may need to escape `!` with `\`,  if wish to exclude files from search.

---

_Comment by @BurntSushi on 2016-12-27 07:59_

Or just use single quotes.

On Dec 26, 2016 11:56 PM, "nateozem" <notifications@github.com> wrote:

> In case somebody wouldn't know the syntax for doing a search with the
> condition of excluding files with a certain name using the command line,
> here is an example to do so:
>
> $ rg "from_str" -g "\!tags" -g "repos/*"
>                     ^^^
>
> As shown, you may need to escape ! with \, if wish to exclude files from
> search.
>
> —
> You are receiving this because you modified the open/close state.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/91#issuecomment-269269896>,
> or mute the thread
> <https://github.com/notifications/unsubscribe-auth/AAb34sr7aMsWRt2-EZ0DUTcu_0dCcxmwks5rMJqIgaJpZM4KGBgW>
> .
>


---

_Comment by @h5rdly on 2016-12-30 16:52_

Are there search parameters for rg that would list any files that match `pattern`, but any folder that matches `pattern` will only appear by itself, aka not relisted with everything inside it? 

Say, for 'python' I'd like to get - 
c:/python27/
c:/python27/python.exe
c:/python27/pythonw.exe
c:/pdfs/pythonconf.pdf
.[more such stuff]
.

Is that possible directly, without parsing the output?



---

_Comment by @BurntSushi on 2016-12-30 16:55_

No. ripgrep will never print directories, only file paths. ripgrep isn't a `find` replacement.

Note that there's enough of `libripgrep` done that you could write your own CLI tool in Rust fairly easily to do this. I would be happy to mentor that project.

---

_Comment by @blueyed on 2017-01-21 17:49_

Note that `--files` also includes binary files (is this a bug?)
The help says:
> Print each file that would be searched without actually performing the search.

So to replicate `ag -g .` I am using `rg --files-with-matches .` now.
A difference here is that `rg` does not include empty files like `ag` does, even when using `''` or `'.?'` for the pattern.

---

_Comment by @BurntSushi on 2017-01-21 18:00_

> Note that --files also includes binary files (is this a bug?)

No. "binary" files isn't a fact one can know without actually searching the file contents.

> So to replicate ag -g . I am using rg --files-with-matches .

Those aren't the same. `ag -g .` is like `rg --files`. `rg --files-with-matches` is like `ag --files-with-matches`.

> A difference here is that rg does not include empty files like ag does, even when using '' or '.?' for the pattern.

Whether a file is empty or not has no impact on the output of `--files`. In an empty directory:

```
$ touch hi
$ rg --files
hi
```

---

_Comment by @blueyed on 2017-01-21 19:24_

@BurntSushi 
Thanks for your reply.
I got confused because I had a global `.agignore` file that excluded `*.png`, and thought that `ag -g .` would exclude binary files therefore already.

> Whether a file is empty or not has no impact on the output of `--files`

I was referring to `--files-with-matches` here, which I think is better to exclude binary files (I am using `rg` here to list files for `ctags`).
I think it's good to not include empty files here, but still wonder if you could have a pattern that is always true, also for empty files (e.g. `^` or `.?`)? 

---

_Comment by @BurntSushi on 2017-01-21 22:01_

> I was referring to --files-with-matches here, which I think is better to exclude binary files (I am using rg here to list files for ctags).

It does:

```
$ echo -e 'z\x00' > test
$ rg --files-with-matches z
$ rg --files-with-matches -a z
test
```

Please note that "binary" detection cannot possibly be perfect. It uses a simple heuristic: if there's a `NUL` byte, then the file is considered binary. This is what GNU grep does. (ripgrep does have a bug here, see #52.)

> I think it's good to not include empty files here, but still wonder if you could have a pattern that is always true, also for empty files (e.g. ^ or .?)?

ripgrep is a line oriented searcher. If a file doesn't contain any lines, then there's nothing to match. What is your use case?

In the future, could you please open new issues for new bug reports/feature requests?

---

_Comment by @blueyed on 2017-01-21 22:19_

>> I was referring to --files-with-matches here, which I think is better to exclude binary files (I am using rg here to list files for ctags).

> It does:

And that is good.

> ripgrep is a line oriented searcher. If a file doesn't contain any lines, then there's nothing to match.

I see, but somehow thought that `^` should be always true.

But `grep -l '^' *` behaves the same, it makes sense, and it is a good way to skip empty files (using `.`, which would work even when `^` would stand for "anything").

> In the future, could you please open new issues for new bug reports/feature requests?

I get you, but it is still about mimicking `ag -g` (and `ag -g .` in particular), isn't it?

>  What is your use case?
>> (I am using `rg` here to list files for `ctags`).

I was using `ag -g . | ctags --links=no -L-` to generate `tags` for my dotfiles (a globally included tags file).

I've replaced it now with `rg --files-with-matches --no-ignore-vcs . | ctags --links=no -L-`.

---

_Comment by @BurntSushi on 2017-01-21 22:28_

> I get you, but it is still about mimicking ag -g (and ag -g . in particular), isn't it?

Definitely not. I think there is a lot of value in having a similar interface as other tools, but I've never added something to ripgrep just for the sake of mimicking another tool. :-)

In the future, please open new issues to discuss new features or bugs. This particular issue is closed and done with. If there's a problem, we should start a new discussion.

> I've replaced it now with rg --files-with-matches --no-ignore-vcs . | ctags --links=no -L-.

Why does this require printing empty files? Surely you won't be able to jump to any symbol defined in an empty file. ;-)

---

_Comment by @blueyed on 2017-01-21 22:40_

@BurntSushi 
I do not want the empty files - it's a nice side effect of the mimicking / workaround though.

>> about mimicking ag -g (and ag -g . in particular), isn't it?

> Definitely not. I think there is a lot of value in having a similar interface as other tools, but I've never added something to ripgrep just for the sake of mimicking another tool. :-)

Sure, but then this issue is still the place to come to when you want to simulate it - that's what I've meant.

All is fine from my side.. :)
Thanks!

---

_Comment by @BurntSushi on 2017-01-21 22:49_

@blueyed I see, OK. I had a really hard time understanding what you were trying to say! Glad all is well.

---

_Comment by @blueyed on 2017-01-21 23:31_

@BurntSushi 
Thanks for a faster and bug-free (with regard to the .gitignore handling) replacement for `ag`!

---

_Comment by @Valloric on 2017-03-02 21:36_

Seems like I misunderstood what `-g` did originally; it's for a _glob_ match, not a regex match.

Given that, I can't say that `rg --files -g 'pattern'` is adequate. What I'd like to see as a user is a _regex_ match on the _filenames_.

---

_Comment by @mcandre on 2017-08-16 19:59_

Is there no combination of flags for ripgrep allowing users to constrain both the filename and the content simultaneously? I'd like to search for Makefile's with "\$\$" in them, for example. Currently working around this with `find . -name Makefile -exec grep "\\$\\$" {} \;`

---

_Comment by @BurntSushi on 2017-08-16 20:13_

@mcandre What have you tried? If `rg -F '$$'` searches too much, then restrict it using exactly the techniques outlined above: `rg -F '$$' -g Makefile`.

---

_Comment by @mcandre on 2017-08-16 20:15_

@BurntSushi Thanks for the tip, works like a charm!

I wonder why the ripgrep CLI parsing doesn't treat

`rg -g Makefile '$$'`

as

`rg -g Makefile -F '$$'`

Oh well, at least the latter works on my machine^TM.

---

_Comment by @okdana on 2017-08-16 20:20_

Because `$` is a regex meta-character used to match the end of the line; `-F` escapes all meta-characters so that they're treated literally, which is what you wanted in this case

Might be worth reviewing something like http://www.regular-expressions.info/quickstart.html

---

_Comment by @mcandre on 2017-08-23 16:31_

@okdana Lol I totally forgot about that! Yeah, `-F '$$'` works on my machine, and I suppose `'\$\$'` would work as well.

---

_Comment by @rosshadden on 2017-09-19 14:59_

@BurntSushi I apologize if you find these kinds of issues annoying, but clearly based on your comments in some of them you understand that a lot of us want this behavior.  Your focus up to this point has been on showing that ripgrep already has the behavior, but I think you should heavily consider focusing on making it as easy as it is in `ag`, `pt`, and `ack`.

Using `-g` in all of those tools is not just a gimmick, it's incredibly useful.  In fact I use it just about as much as I do the main functionality.  A lot of projects like [fzf](https://github.com/junegunn/fzf), [unite.vim](https://github.com/Shougo/unite.vim/blob/master/doc/unite.txt), and [denite.vim](https://github.com/Shougo/denite.nvim/blob/master/doc/denite.txt) even officially recommend using this feature for getting lists of files, as `-g ''` is a really convenient and fast git-agnostic way to do `git ls-files`.  In fact doing a search for " -g" on fzf's readme shows five occurrences of using `ag -g` in different ways.

To be clear I'm not proposing you make it `-g`.  But please consider adding a flag that does the equivalent.  It will shut all of us up and make us feel much better about jumping into using and loving ripgrep.  Thanks!

---

_Comment by @BurntSushi on 2017-09-19 15:10_

@rosshadden What are you actually asking for? The `-g` flag's behavior is already set and it isn't changing. Are you asking for a new flag that combines `-g` and `--files`? If so, just define an alias?

    alias rgg="rg --files"

---

_Comment by @rosshadden on 2017-09-19 15:49_

Yes I was asking for a flag that combines them.  I do have an alias for this (called `rgg` even, haha), but feel like it should be an official flag shortcut.  It just felt like an elephant in the room sort of thing so I brought it up.  A lot of us end up here or in the other duplicate issues for this exact reason, and while yes having it documented helps, I still think you should consider making it easier and more comfortable for people.

Anyway that's all I'll say about it, and I'll leave you alone now.  Thanks again for the project. I never thought I'd move off `ack` until I found `ag`.  Never thought I'd move off that until I found `pt`, and I never thought I'd move off that until I found `rg`.  Heh.

---

_Comment by @tupton on 2017-12-06 18:05_

> Well, to match `pattern.txt` and `path/to/patterns`, you will actually need `**/pattern*/**`.

I had to add an extra `-g "pattern.*"` to match file names that end in `pattern.txt` or `pattern.js` or whatever – that is, "pattern" + an extension. `**/pattern*/**` matches file names like "patternfoo.txt" but not "pattern.txt".

I too would like to see an option to match a regex on filenames (including path.)

---

_Comment by @sadid on 2018-05-25 06:34_

Based on my experience `ag -g 'pattern'` isn't equivalent to `rg --files -g 'pattern'` when I call ag to find files with specific pattern in the filename/path. As I tinker, `find . -type f | rg 'pattern'` is somehow equivalent. 

---

_Comment by @okdana on 2018-05-25 06:42_

They should be basically equivalent, but you might see different output depending on the exact pattern (`rg`'s glob syntax is more robust — and more similar to git's — than `ag`'s) as well as any ignore patterns you might have. You can try adding `-u` or `--no-ignore-vcs`, for example, if `ag` shows files that `rg` doesn't

---

_Comment by @sadid on 2018-05-25 08:28_

@okdana, Here is my test case:
a directory structure (without any git) called test:

```
test
  \_a
      \_aa
          \_ target.md
      \_test.md
   \_b
       \_target.md
   \_c
```
  
then these two commands are not equal `ag -g 'target'` and `rg --files -g 'target'` with or without `-u` or `--no-ignore-vcs`.  The `ag` finds the `target.md` and put it in the output but `rg` doesn't. 

*At the moment I still use `rg` in other use cases but for this use cases I can use `fd`, a new `find` replacement.*

---

_Comment by @okdana on 2018-05-25 08:41_

Ah, that's because `ag` actually uses PCRE to match file paths — and it matches them against the *entire* path, too. For some reason i'd thought it was using its git-ish glob matching for that.

The `rg` equivalent to `ag -g target`, then, is more or less `rg --files -g '*target*'`.

---

_Comment by @mahiki on 2018-08-27 22:09_

I also was attempting to move from `ag` to `rg`.

The first thing I tried was a PCRE regex match of file paths, which I now have learned is insufficient. Yes `find` is the great way to do that, but `ag` and `rg` respect ignore files, binaries, etc.

I **truly** enjoy finding files by name using regex patterns and not globs, in `ag` its simple:

    ag -g 'pattern1|foo\d{4}' ~/Documents

Is there no was such a feature could be implemented?

---

_Comment by @BurntSushi on 2018-08-27 22:41_

@mahiki It already exists. `rg --files ~/Documents | rg 'pattern1|foo\d{4}'`. Otherwise, no, the glob flags will remain glob flags. If you want a tool that specializes in listing files and can match them with regexes, then you might consider [`fd`](https://github.com/sharkdp/fd). Also, your regex is not specific to PCRE.

---

_Comment by @borekb on 2019-08-20 18:09_

Thanks for the mention of `fd`! I cloned a repository somewhere on my disk and wanted to abuse ripgrep to find it; `fd` is the right tool for that as I need to list only directories.

---

_Comment by @WhyNotHugo on 2020-05-10 14:30_

I'm trying to achieve what's been discussed on this thread, but it's not working for me, I'm not sure if I've misunderstood something, or if something's broken. Say I want to find files named `vendor`:

```
$ rg --files . -g vendor
$ rg --files vendor
vendor: No such file or directory (os error 2)
$ find . -iname vendor
./idf/core/static/www/js/vendor
./idf/core/static/www/css/vendor
./enviatufoto/static/www/js/vendor
```

What am I doing wrong? With `ag`, this is basically `ag -g vendor`, but I can't figure out how it works with `rg`.

---

_Comment by @BurntSushi on 2020-05-10 18:41_

@WhyNotHugo The glob is applied to every path ripgrep traverses. Is `vendor` a directory? If so, `vendor` on its own won't match anything inside of `vendor`. Use `rg --files -g '**/vendor/**'` instead.

---

_Comment by @lamyergeier on 2020-06-29 00:42_

@BurntSushi  `--files` `-g` doesn't respect the contents of `.gitignore`

```
rg --files --type md -g "*Python*" "${PWD}"
```

---

_Comment by @BurntSushi on 2020-06-29 00:45_

Please file a new and *complete* big report. Your comment isn't actionable.

---

_Comment by @lamyergeier on 2020-06-29 01:02_

I solved this as follows:

To search Python in filenames:

```
Search=Python
rg --files "${PWD}" | rg --regexp "${Search}[^/]*$" | sort | nl
```

---

_Comment by @jjjchens235 on 2020-10-06 22:07_

piggybacking off of @anishmittal2020 
This function returns any file name AND directory matches, **though directories themselves cannot be returned**, as the --file flag returns a list of files only, directory paths are excluded.
```
function f() {
	#find any files or directories that match arg
	rg --files "${PWD}" | rg --regexp ".*/.*$1.*" #| sort | nl
}
```

Put in .bashrc, and then call it on the command line.
If searching for filename or directory names that contain the word 'vendor':
`f vendor`

Taking @WhyNotHugo example:
```
./idf/core/static/www/js/vendor/bar.py
./idf/vendor.py
./idf/core/static/www/js/vendor/
```

The first two files will be returned, the first because the path contains a dir called vendor, and the second because it contains a filename with vendor. The last file is a dir, it will not be returned.


**Edit:** I hate to admit it, but I should have just used fd from the get-go. It allows for regex arguments, and user can specify if they want to search for files, directories, or both.

```
function f() {
	#list of flags: https://github.com/sharkdp/fd#command-line-options
	#optional flags should be passed in first, then PATTERN and PATH
	fd -H -a -p  "$@"
}
```

---

_Comment by @AtomicNess123 on 2021-02-07 09:07_

I have read the whole thread. I have not successfully managed to just find all filenames containing "helm".
When I do:

`rga ~/.emacs.d --files -g "*helm*"   
`

I just finds filenames with detached "helm" (i.e., "xxx-helm-xxx.el"), but not "xxxhelmxxx.el".
How to specify this? Thanks!

---

_Comment by @BurntSushi on 2021-02-07 16:44_

@AtomicNess123 Works just fine for me:

```
$ touch xxx-helm-xxx.el xxxhelmxxx.el
$ l
total 0
-rw-rw-r-- 1 andrew users 0 Feb  7 11:40 xxx-helm-xxx.el
-rw-rw-r-- 1 andrew users 0 Feb  7 11:40 xxxhelmxxx.el
$ rg --files -g '*helm*'
xxx-helm-xxx.el
xxxhelmxxx.el
```

---

_Comment by @AtomicNess123 on 2021-02-07 20:52_

Thanks. Actually, it does work in your example. 
But not when I do:

`§ rg --files -g '*helm*' ~/.emacs.d  `

In this case, when I specify the folder to search within, it only finds the "xxx-helm-xxx.el" instances, and not the "xxxhelmxxx.el" ones.

---

_Comment by @BurntSushi on 2021-02-07 21:03_

Also works just fine:

```
$ mkdir .emacs.d
$ touch .emacs.d/xxx-helm-xxx.el .emacs.d/xxxhelmxxx.el
$ rg --files -g '*helm*' .emacs.d/
.emacs.d/xxx-helm-xxx.el
.emacs.d/xxxhelmxxx.el
```

I can't think of a reason why it isn't working for you. Sorry. To be honest, it doesn't make sense to me why `*helm*` would match `xxxhelmxxx` but not `xxx-helm-xxx`. I suspect there is something else going awry. Please consider making a smaller reproduction.

---

_Comment by @AtomicNess123 on 2021-02-07 21:28_

I won't work like this in my system.
```
rg --version  
ripgrep 12.1.1
```
What is your version?


---

_Comment by @BurntSushi on 2021-02-07 21:31_

Same, but there aren't any recent changes that would impact this AFAIK. Please consider trying my exact set of commands in a fresh directory. Try running with the `--debug` flag. Paste its output and all the exact commands you're running.

---

_Comment by @AtomicNess123 on 2021-02-07 21:31_

Other question: how many levels (subdirectories) will the command search?

---

_Comment by @BurntSushi on 2021-02-07 21:31_

All...

---

_Comment by @AtomicNess123 on 2021-02-07 21:37_

Then, it makes no sense. It works in your example. But I want to find any filename with "helm" in the name. If I search from root or any other folder, it only finds the files at the .emacs.d/modules and .emacs.d/other, but not in .emacs.d/elpa. It is very strange. But when I enter the elpa directory and run the command from there, it finds all the files.

---

_Comment by @AtomicNess123 on 2021-02-07 21:41_

Ok... does .gitignore affect results? It seems elpa is in .gitignore!

---

_Comment by @BurntSushi on 2021-02-07 21:58_

Of course. The first two sentences of the readme:

> ripgrep is a line-oriented search tool that recursively searches your current directory for a regex pattern. By default, ripgrep will respect your .gitignore and automatically skip hidden files/directories and binary files.

This is why I suggested the `--debug` flag. It will tell you which things ripgrep skips and why.

---

_Comment by @AtomicNess123 on 2021-02-08 08:09_

Yes, you were right. I'm just newbie in command line functions. I wonder if it's possible to override .gitignore setting? 

---

_Comment by @BurntSushi on 2021-02-08 11:49_

Sure. See the guide. If you have a specific question then open a new discussion post.

---

_Comment by @sergeevabc on 2021-05-02 12:02_

Sorry to dig up this issue once again, but as a Windows user I believe workarounds provided earlier are not sterling, because `alias rgg` that combines `--files -g part-of-filename` is a Linux thing and `fd` utility to use instead of `rg` to search for filenames is [still buggy][1] to be relied upon. Yet this task arises daily (at least on my end), so I would vote with both hands for a short switch (to remember it easily and not to type long now imaginary --search-for-filenames). For example, `-ff`.

[1]: https://github.com/sharkdp/fd/issues/752

---

_Comment by @BurntSushi on 2021-05-02 12:23_

Other software being buggy is not something I'm receptive towards as a motivation for adding more features to ripgrep.

---

_Comment by @AtomicNess123 on 2021-07-27 09:17_

> piggybacking off of @anishmittal2020
> This function returns any file name AND directory matches, **though directories themselves cannot be returned**, as the --file flag returns a list of files only, directory paths are excluded.
> 
> ```
> function f() {
> 	#find any files or directories that match arg
> 	rg --files "${PWD}" | rg --regexp ".*/.*$1.*" #| sort | nl
> }

Thanks for this. What is the meaning of  "${PWD}"?

---

_Comment by @chris-w-jarvis on 2021-08-12 19:59_

Whats wrong with doing `rg --files | rg foo` ?

This seems to work fine for me :)

---

_Comment by @foucist on 2022-03-23 20:26_

Inspired by the earlier alias, I'm trying something like this.   Function is named ag because old habits die hard 😅

    function ag() { rg --files | rg ".*/.*$1.*" | sort | rg $1 }

---

_Comment by @mcandre on 2022-03-24 23:31_

Appreciate the snippet! Good to know.

One reason to bake this functionality into rg itself, is that sort may have
different syntax on Windows.

And any sorting is most efficiently done with appropriate data structures
as early as possible (e.g., "online" algorithm), rather than after the fact.

On Wed, Mar 23, 2022, 3:26 PM James Robey ***@***.***> wrote:

> Inspired by the earlier alias, I'm trying something like this. Function is
> named ag because old habits die hard 😅
>
> function ag() { rg --files | rg ".*/.*$1.*" | sort | rg $1 }
>
>
> —
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/91#issuecomment-1076787036>,
> or unsubscribe
> <https://github.com/notifications/unsubscribe-auth/AAABJRGFSUSKJF77H426E3LVBN5AJANCNFSM4CQYDALA>
> .
> You are receiving this because you were mentioned.Message ID:
> ***@***.***>
>


---

_Comment by @PyKoch on 2024-05-23 10:58_

> In case somebody wouldn't know the syntax for doing a search with the condition of excluding files with a certain name using the command line, here is an example to do so:
> 
> ```
> $ rg "from_str" -g "\!tags" -g "repos/*"
>                     ^^^
> ```
> 
> As shown, you may need to escape `!` with `\`, if wish to exclude files from search.

Please excuse the newbie question here: 
I am trying to search all pdf files and want to exclude those that contain 'ms' in the file name. 
This is what I entered: 
`rg --type pdf -i "velocity" --sort-files -g '\!ms' -g '*paper_3*' >> test_p3_no_ms.txt`
Sadly, there are still files with 'ms' in the title included. Any help would be greatly appreciated. 

---

_Comment by @BurntSushi on 2024-05-23 11:52_

@PyKoch Please don't ask questions in old unrelated issues. This issue is a feature request for some other feature.

And when you do ask questions, please include a reproduction. Here's how you do it: you include _all_ relevant details so that others can see what you're seeing. This means providing inputs, showing the outputs _you_ see and the outputs you _want_ to see. Basically, it comes down to "show, don't tell." Here, I'll show you.

First create an empty directory with some files:

```
$ mkdir rg-search-issue
$ cd rg-search-issue
$ touch msfoo fooms ms quux
```

And now run `rg --files` to show that it correctly finds all of the files I created above:

```
$ rg --files
quux
ms
fooms
msfoo
```

And now my attempt at filtering:

```
$ rg --files -g '\!ms'
$
```

If you did that, then folks helping you wouldn't have to guess at your problem.

The first thing to realize here is that ripgrep isn't printing _any_ files. This seems different from what you report where there are files being printed. Since you didn't share any details about your environment, _and_ you provided a command more complex than necessary to demonstrate your specific issue with filtering out things that contain `ms`, I'm not sure what's going on.

In any case, there are two issues here. Assuming you're on Unix, the first issue is that you're escaping `!` in a single quoted value. A comment above said that `!` needed to be escaped, but that was in a double quoted string. The second issue is that the glob `ms` doesn't match `AAmsAA`. The glob `ms` only matches `ms`. You need a wildcard to match around it. You seemingly did this for `*paper_3*`... So just do it for `ms`:

```
$ rg --files -g '!ms'
quux
fooms
msfoo
$ rg --files -g '!*ms*'
quux
```

In the future, please don't ask questions in old issues. There's an entire [discussion forum](https://github.com/BurntSushi/ripgrep/discussions) open for that purpose.

---
