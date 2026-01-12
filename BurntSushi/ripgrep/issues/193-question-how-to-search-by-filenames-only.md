```yaml
number: 193
title: "[Question] how to search by filenames only?"
type: issue
state: closed
author: hh9527
labels: []
assignees: []
created_at: 2016-10-24T09:44:51Z
updated_at: 2023-12-20T03:19:46Z
url: https://github.com/BurntSushi/ripgrep/issues/193
synced_at: 2026-01-12T16:13:21Z
```

# [Question] how to search by filenames only?

---

_@hh9527_

`--files` will list all files which respect ignore files. Is there a flag to list filenames which match the given filename pattern?


---

_Comment by @BurntSushi on 2016-10-24 11:00_

From `rg --help`:

```
    -g, --glob GLOB ...        Include or exclude files for searching that
                               match the given glob. This always overrides any
                               other ignore logic. Multiple glob flags may be
                               used. Globbing rules match .gitignore globs.
                               Precede a glob with a '!' to exclude it.
```


---

_Closed by @BurntSushi on 2016-10-24 11:00_

---

_Comment by @YPCrumble on 2016-12-21 15:39_

@BurntSushi this is related to the issue I just raised, #284. The I believe the question here is "How can I search for a pattern in the filename, and return the filenames that include that pattern in the path/filename?"

It appears that the -g flag limits my search _for text within the file_ to the filenames matching the -g pattern. Does that explain the nuance? 

Example: 

If there is a file relative to my directory, `./stories/story.txt` that contains the text "Once upon a time", I want to be able to run `rg -g story` and return `./stories/story.txt`. It appears that the -g flag would return nothing in this query, but it would return "Once upon a time" if I try `rg Once -g story.*`.

---

_Comment by @hraban on 2019-07-19 11:55_

I have the following alias to shim this in my ~/.bashrc:

```sh
alias rgf='rg --files | rg'
```

This allows you to do:

```sh
$ rgf 2018
lib/lib.es2018.full.d.ts
lib/lib.es2018.promise.d.ts
lib/lib.es2018.intl.d.ts
lib/lib.es2018.regexp.d.ts
lib/lib.es2018.asynciterable.d.ts
lib/lib.es2018.d.ts
```


---

_Comment by @johnyradio on 2019-08-26 01:58_

> ```shell
> --files
> ```

how would you do that without the alias?

---

_Comment by @hraban on 2019-08-26 13:16_

@johnyradio just type the command directly:

    rg --files | rg 2018

A Bash alias just substitutes the command for whatever the contents of the alias is, then continues evaluating as usual. That's all an alias is:

    alias rgf="rg --files | rg"
    rgf 2018

=

    rg --files | rg 2018



---

_Comment by @johnyradio on 2019-08-26 16:48_

Many thanks! Super helpful.


---

_Comment by @dzhang-b on 2020-04-03 01:24_

`rg --files | rg <pattern> <directory>`  or `rgf <pattern> <directory>`won't work. It will search all contents in the directory, not the filename in that directory. 

It is best if we can add an actual flag to do this.

---

_Comment by @hraban on 2020-06-10 17:44_

> `rg --files | rg <pattern> <directory>` or `rgf <pattern> <directory>`won't work. It will search all contents in the directory, not the filename in that directory.
> 
> It is best if we can add an actual flag to do this.

    $ rg --files [directory] | rg <pattern>

if you want to do this as a "rgf" style alias, use a function instead:

```sh
function rgf {
    rg --files $2 | rg $1
}
```

---

_Comment by @Piping on 2020-06-12 15:35_

@hraban Thanks for the tip, I updated the logic a bit for future newcomer.
```sh
rf() {
  if [ -z "$2" ]
  then
      rg --files | rg "$1"
  else
      rg --files "$2" | rg "$1"
  fi
}
```

---

_Comment by @lamyergeier on 2020-06-29 00:29_

> > `rg --files | rg <pattern> <directory>` or `rgf <pattern> <directory>`won't work. It will search all contents in the directory, not the filename in that directory.
> > It is best if we can add an actual flag to do this.
> 
> ```
> $ rg --files [directory] | rg <pattern>
> ```
> 
> if you want to do this as a "rgf" style alias, use a function instead:
> 
> ```shell
> function rgf {
>     rg --files $2 | rg $1
> }
> ```

What you suggested searches in the entire path and not just in the filename (what the OP had asked)!

---

_Comment by @lamyergeier on 2020-06-29 01:01_

To search Python in filenames:

```
Search=Python
rg --files "${PWD}" | rg --regexp "${Search}[^/]*$" | sort | nl
```

Note: Use above in Linux and Mac (for windows replace [^/] with [^\]):

---

_Comment by @ssbarnea on 2020-09-16 11:47_

I still hope to see native support for it. There is a huge UX benefit on having it as a default feature and the number of user upthumbs explains it.

---

_Comment by @BurntSushi on 2020-09-16 11:57_

Use `fd` instead. 

---

_Comment by @AtomicNess123 on 2021-02-07 08:25_

> ```
> rg --files | rg 2018
> ```

Thanks, very useful. What does the | mean? Also, this finds filenames that do not match the query because the files contents contain the query. I am only interested in the filename.

---

_Comment by @BurntSushi on 2021-02-07 16:45_

@AtomicNess123 `rg --files | rg 2018` will only print file names that contain `2018`.

The `|` is a shell pipeline. It redirects the output of `rg --files` into the input of `rg 2018`.

---

_Comment by @AtomicNess123 on 2021-02-07 20:54_

Mmm... I have to digest that. Could you give a more illustrative example? No rush, though...

---

_Comment by @BurntSushi on 2021-02-07 21:04_

```
$ touch foo2018 foo bar2018 bar
$ rg --files
foo
foo2018
bar2018
bar
$ rg --files | rg 2018
foo2018
bar2018
```

At no point are the contents of any files searched in the above example.

---

_Comment by @AtomicNess123 on 2021-02-07 21:44_

Thanks. What is the difference then between `rg --files | rg 2018` and `rg --files -g '*2018*' ` ?
Thanks for your time!

---

_Comment by @BurntSushi on 2021-02-07 22:02_

The former will print all paths in the current tree that contain 2018. The latter will only print paths containing 2018 where all of its parent directories also contain 2018. The `-g` flag influences which directories ripgrep descends into.

---

_Comment by @AtomicNess123 on 2021-02-08 08:10_

Thanks, very insightful!

---

_Comment by @AtomicNess123 on 2021-02-08 10:58_

Actually, it won't work like that in my case:

```bash
rg --files -g '*helm*'                                                                                                       
xxx-helm-xxx.el
helm/xxx-helm-xxx.el
helm/sub2/xxx-helm-xxx.el
helm/sub2/xxxhelmxxx.el
helm/xxxhelmxxx.el
a/xxx-helm-xxx.el
a/sub2/xxx-helm-xxx.el
a/sub2/xxxhelmxxx.el
a/xxxhelmxxx.el
xxxhelmxxx.el

rg --files | rg helm               
xxx-helm-xxx.el
helm/xxx-helm-xxx.el
helm/sub2/xxx-helm-xxx.el
helm/sub2/xxxhelmxxx.el
helm/xxxhelmxxx.el
a/xxx-helm-xxx.el
a/sub2/xxx-helm-xxx.el
a/sub2/xxxhelmxxx.el
a/xxxhelmxxx.el
xxxhelmxxx.el
```

As you can see, the -g flag makes no difference.

---

_Comment by @chaselambda on 2022-01-07 21:00_

> Use `fd` instead.

Probably referencing https://github.com/sharkdp/fd. TIL.

---

_Comment by @JosephTLyons on 2023-12-20 03:00_

Any way to search for all file nams with a given pattern without having to specify a search pattern?

```bash
〉rg -g *.json                                                                                                             
ripgrep requires at least one pattern to execute a search
```

---

_Comment by @BurntSushi on 2023-12-20 03:19_

Yes. Use the `--files` flag.

---
