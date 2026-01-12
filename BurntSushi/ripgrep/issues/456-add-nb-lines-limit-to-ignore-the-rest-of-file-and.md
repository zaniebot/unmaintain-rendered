```yaml
number: 456
title: Add --nb-lines-limit to ignore the rest of file and speedup the seach
type: issue
state: closed
author: zimski
labels: []
assignees: []
created_at: 2017-04-21T15:40:41Z
updated_at: 2025-07-17T16:30:41Z
url: https://github.com/BurntSushi/ripgrep/issues/456
synced_at: 2026-01-12T16:13:22Z
```

# Add --nb-lines-limit to ignore the rest of file and speedup the seach

---

_@zimski_

I would tell to `ripgrep` to search only in let's say the first 5 lines on each file, because it's the only relevant part for the use-case.

I want this feature to speedup the search on a huge number of large files with an known structure and the relevant part is only in the first 5 lines for example.

it's like a combine of `find | head -n5 | grep`

```
0. #------------- header with the data I am looking for
.... lines
5.#-------------
the rest of the file should be ignored
...
```

If you think that this usecase can be usefull for you, I can contribute.

---

_Comment by @BurntSushi on 2017-04-21 15:45_

That's interesting. You should actually be able to run `find | head -n5 | rg` and have it work just like it would with `grep`.

The key problem with taking over file discovery is that it reduces composition with other tools. However, ripgrep can still be used just like you would use `grep` in this case.

---

_Comment by @richarson on 2017-04-23 19:03_

@zimski, I would have said: "It's like GNU grep's -m/--max-count option" :)

I don't know if other versions of grep have a similar option.


---

_Comment by @BurntSushi on 2017-04-23 19:34_

@richarson I don't think it's like that all though? And if it was, that would be good news because ripgrep has the `-m/--max-count` flag. The OP seems to be asking for "only search the first 5 lines" rather than "show me the first 5 matches."

---

_Comment by @zimski on 2017-04-23 20:00_

Yep @BurntSushi  you are right, @richarson maybe my issue description was not clear enough.

To make the usecase more easy to understand, I will explain how I want to use this feature.

I have a lot of servers caching on disk a lot of contents using Nginx ( like a CDN )

To speedup the cache flushing and make it more smart (flush only some urls .. some domains) we insert some `KEY` on the header of the cached file.

the file stored looks like this
```
KEY:  domain/urls ... uac
HTTP headers

file content
```

To flush we run a `ripgrep` on the root cache folder and catch the targeted files to remove them. 

However, the cached file can be large, and `ripgrep` will search on the whole file if it doesn't find nothing in the header part ( obviously it will find nothing and all this time/cpu was wasted :\ )

So if I can to tell to `ripgrep` to consider only the first line .. I will gain a lot.

I hope this will be make this feature proposal more clearer and considerable.

I have done some experiment using the only `find head and grep` and I gain a lot, but I am sure that it can be more efficient (memory/cpu) if it was implemented inside `ripgrep` 

---

_Comment by @BurntSushi on 2017-04-23 20:26_

> I have done some experiment using the only find head and grep and I gain a lot, but I am sure that it can be more efficient (memory/cpu) if it was implemented inside ripgrep

Doubtful. Maybe a little. You could test this by creating an identical directory structure, but with only the first five lines of each file you're trying to search. Then compare `rg` with `find | head | rg`.

This seems really niche to me. I think I'd rather you try to find a way to use `find` instead.

---

_Comment by @richarson on 2017-04-24 22:20_

@BurntSushi you're right of course, I misinterpreted what @zimski was asking for.

And good to know ripgrep has that option too :)


---

_Comment by @BurntSushi on 2017-05-08 21:36_

I'm going to close this under the principle that ripgrep can't add a new flag for every use case under the sun. Yes, we'd all love to have the pretty output, but falling back to standard pipelining sometimes should be OK.

---

_Closed by @BurntSushi on 2017-05-08 21:36_

---

_Comment by @Rodrigodd on 2025-07-17 16:30_

I stumbled upon this issue while trying to search for this flag, so I’m documenting it here in case it helps anyone else.

---

Using `find | head -n5 | rg` doesn’t work. The command `find | head -n5` simply lists the names of 5 files. I tried to get it to work using something like `find . -exec`, but then you lose the filename information in the output.

However, it is possible to use the `--pre` option instead:

```sh
rg pattern --pre=head
```

This limits the search to the first 10 lines of each file. Note that the `--pre` option doesn't allow you to pass arguments to the command, so you cannot easily search instead only the first 5 lines or the first 100 bytes. For anything more complicated you could follow the help for the option:

       --pre=COMMAND
           For each input PATH, this flag causes ripgrep to search the standard output of COMMAND PATH instead of the contents of PATH.   This  option
           expects  the  COMMAND program to either be a path or to be available in your PATH. Either an empty string COMMAND or the --no-pre flag will
           disable this behavior.

           WARNING     When this flag is set, ripgrep will unconditionally spawn a process for every file that is searched. Therefore, this can  incur
                       an unnecessarily large performance penalty if you don't otherwise need the flexibility offered by this flag. One possible miti‐
                       gation to this is to use the --pre-glob flag to limit which files a preprocessor is run with.

           A preprocessor is not run when ripgrep is searching stdin.

           When searching over sets of files that may require one of several preprocessors, COMMAND should be a wrapper program which first classifies
           PATH based on magic numbers/content or based on the PATH name and then dispatches to an appropriate preprocessor. Each COMMAND also has its
           standard input connected to PATH for convenience.

           For example, a shell script for COMMAND might look like:

               case "$1" in
               *.pdf)
                   exec pdftotext "$1" -
                   ;;
               *)
                   case $(file "$1") in
                   *Zstandard*)
                       exec pzstd -cdq
                       ;;
                   *)
                       exec cat
                       ;;
                   esac
                   ;;
               esac

           The  above  script  uses  pdftotext to convert a PDF file to plain text. For all other files, the script uses the file utility to sniff the
           type of the file based on its contents. If it is a compressed file in the Zstandard format, then pzstd is used to decompress  the  contents
           to stdout.

           This overrides the -z/--search-zip flag.


---
