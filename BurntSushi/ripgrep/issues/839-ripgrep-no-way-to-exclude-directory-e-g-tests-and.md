```yaml
number: 839
title: "ripgrep no way to exclude directory e.g. Tests and search for all *.c files"
type: issue
state: closed
author: sumonto
labels: []
assignees: []
created_at: 2018-02-28T03:17:49Z
updated_at: 2023-01-20T03:49:02Z
url: https://github.com/BurntSushi/ripgrep/issues/839
synced_at: 2026-01-12T16:13:22Z
```

# ripgrep no way to exclude directory e.g. Tests and search for all *.c files

---

_@sumonto_

#### What version of ripgrep are you using?

ripgrep 0.7.1
-AVX -SIMD

#### What operating system are you using ripgrep on?

Mac High Sierra

#### Describe your question, feature request, or bug.

Imagine a folder structure like:
Feature
  codefiles1 (contains *.c, *.h)
  codefiles2 (contains *.c, *.h)
  tests (contains *.c, *.h)

I would like to search a string str1 in *.c, *.h files however exclude/ignore all occurrences in "tests" folder

I would assume the below syntax would do it?
rg -w str1 -tc -g '!tests\/*' 


If a feature request, please describe the behavior you want and the motivation.
Please also provide an example of how ripgrep would be used if your feature
request were added.

If a bug, please see below.

#### If this is a bug, what are the steps to reproduce the behavior?

NA

#### If this is a bug, what is the actual behavior?

NA

#### If this is a bug, what is the expected behavior?

ripgrep should have excluded files from "tests" folder


---

_Comment by @BurntSushi on 2018-02-28 03:23_

Please fill out the entire issue template. That's why it is there to guide you, because the request you've made is insufficient to act on. In particular, I have no idea what you are asking for.

Please also make sure you've read the documentation. ripgrep can of course exclude directories.

---

_Comment by @kbknapp on 2018-03-01 02:01_

@sumonto you're correct `$ rg -w str1 -tc -g '!tests/*'` would do what you want. So is that not working for you? It works on my machine with v0.8.1

```
kevin@beefcake:/tmp/rg$ rg --version
ripgrep 0.8.1
-SIMD -AVX
kevin@beefcake:/tmp/rg$ tree
.
├── code
│   ├── other.h
│   └── some.c
└── test
    └── bar.c

2 directories, 3 files
kevin@beefcake:/tmp/rg$ cat code/other.h 
foo
kevin@beefcake:/tmp/rg$ cat code/some.c 
foo
kevin@beefcake:/tmp/rg$ cat test/bar.c 
foo
kevin@beefcake:/tmp/rg$ rg -w foo -tc -g '!test/*'
code/some.c
1:foo

code/other.h
1:foo
```

---

_Comment by @BurntSushi on 2018-03-10 12:46_

It's not possible to act on this issue. Closing.

---

_Closed by @BurntSushi on 2018-03-10 12:46_

---

_Comment by @mmahmoudian on 2019-10-09 15:15_

I might know what the issue might be: ZSH

I was trying to make this work in ZSH:
```
rg --stats --ignore-case --type r --max-filesize 200K -g "!packrat" --regexp "stringi"
```
The command was suppose to only search in specific file types (in this case `r` type) and search for the text `stringi` but I want it to exclude everything in "packrat" folders in the following scenario:

```
.
├── Project1/
│   ├── packrat/
│   │   ├── lib1/R/
│   │   │   └── some_file.R
│   │   └── lib2/R/
│   │       └── some_file.R
│   ├── other_folder/
│   │   └── some_file.Rmd
│   └── some_file.R
│
└── Project2/
     ├── packrat/
     │   └── lib1/R/
     │       └── some_file.R
     └── some_file.R
```

the `!` has a meaning in ZSH, so you should escape the character (except if you use it as an alias). The correct command in my case is:

```
rg --stats --ignore-case --type r --max-filesize 200K -g "\!packrat" --regexp "stringi"
```


---

_Comment by @okdana on 2019-10-09 15:38_

You can also just use single-quotes instead of double- (which is what the OP was doing, so i don't think it was the issue there). Or you can turn off history expansion, but you lose some functionality obv

---

_Comment by @BobrImperator on 2021-03-06 15:26_

I found out that `-g` didn't work for me, but `--glob` did as per `man rg`, and final command looks like this:
`.zshrc`
`export FZF_DEFAULT_COMMAND="rg --files --follow --hidden --glob=\!.git"`

which previously looked like this
`"rg --files --follow --hidden -g \!.git"'

---

_Comment by @okybr on 2021-04-29 12:49_

@BobrImperator, use `-g '!.git'`, i.e. single-quotes ;) 

---

_Comment by @joeky888 on 2021-06-10 03:11_

Hello everyone. How do you exclude multiple directories? Currently, I am using --glob multiple times like this:

```sh
rg --hidden -g '!.git' -g '!.svn' -g '!.hg' -g '!CVS' -g '!.bzr' -g '!vendor' -g '!node_modules' -g '!dist' -g '!bin'
```

---

_Comment by @BurntSushi on 2021-06-10 10:26_

@joeky888 Why are you bumping old issues? Use Discussions to ask questions next time please.

In any case, you can use `-g '!{.git,.svn,.hg}'` (and so on) to exclude multiple directories. But your approach works fine too.

---

_Comment by @nyngwang on 2021-11-08 08:03_

@BurntSushi Thanks for the solution, it works!

---

_Comment by @dharmaturtle on 2022-01-06 16:25_

Apologies for necrobumping, but to emphasize the `directories` part of joeky888's question, this works for me for (sub)directories: `-g '!{**/node_modules/*,**/.git/*}'`

---

_Comment by @Riveascore on 2023-01-20 03:49_

@dharmaturtle Thank you!!


---
