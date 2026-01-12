```yaml
number: 45
title: add flag to specify one or more additional ignore files
type: issue
state: closed
author: kaushalmodi
labels:
  - enhancement
assignees: []
created_at: 2016-09-24T03:59:34Z
updated_at: 2021-07-29T13:21:18Z
url: https://github.com/BurntSushi/ripgrep/issues/45
synced_at: 2026-01-12T16:13:21Z
```

# add flag to specify one or more additional ignore files

---

_@kaushalmodi_

Hello,

This might be a feature borrowed from `ag`.

It is very convenient to have a global `~/.rgignore` that applies **everywhere**. It would contain stuff like:

```
*~
*.lib
*.cdb
*.dm
*.tag
*.oa
*.png
*.db
*.state
*.SVM
*.dat
*.sdc
*.il
*.tr
*.Cat
*.cfg
*.info
*.stateScripts
*#*#
TAGS
GTAGS
GRTAGS
GPATH
```

Then, even if I am working in `/proj/SOMEPRJ/` and if I have `/proj/SOMEPRJ/.rgignore`, then the `SOMEPRJ` specific `.rgignore` + `~/.rgignore`, both will be respected.

That way I do not copy the common stuff in the `.rgignore` of all the projects. _Note that `~` (or `/home/$USER`) and `/proj/SOMEPRJ/` do not share the same parent dir._


---

_Comment by @BurntSushi on 2016-09-24 04:13_

Hmm. Would you be opposed to this going in `$XDG_CONFIG_HOME/ripgrep/ignore`? That way, we won't need to specify any weird interaction between the current semantics, which are "obeys parent directory `.ignore` files," and the new semantics, which if we did what you suggested would be, "obeys parent directory unless $HOME/.ignore exists, in which case, it's used everywhere." `$XDG_CONFIG_HOME` feels a bit more right.


---

_Label `enhancement` added by @BurntSushi on 2016-09-24 04:13_

---

_Comment by @kaushalmodi on 2016-09-24 04:16_

I am on RHEL6.6 and `echo $XDG_CONFIG_HOME` returns 

> XDG_CONFIG_HOME: Undefined variable.

Also this is work machine.. so I don't have admin rights. Do we have an xdg alternative in that case?


---

_Comment by @BurntSushi on 2016-09-24 04:44_

Sorry, by `$XDG_CONFIG_HOME`, I meant "follow the XDG basedir spec." Specifically, if `XDG_CONFIG_HOME` isn't set, then it defaults to `$HOME/.config`.


---

_Comment by @kaushalmodi on 2016-09-24 04:55_

Hmm, I symlinked `$HOME/.config/ripgrep/ignore` to `~/.agignore`. I also tried few other symlink names. But none helped.. The files ignored in `~.agignore` are still being searched.

```
km²~/.config/:ripgrep> lta                                                        
total 8.0K
drwxr-xr-x 28 kmodi users 4.0K Sep 24 00:48 ../
lrwxrwxrwx  1 kmodi users   21 Sep 24 00:52 .rpignore -> /home/kmodi/.agignore
lrwxrwxrwx  1 kmodi users   21 Sep 24 00:52 .ignore -> /home/kmodi/.agignore
lrwxrwxrwx  1 kmodi users   21 Sep 24 00:52 ignore -> /home/kmodi/.agignore
drwxr-xr-x  2 kmodi users 4.0K Sep 24 00:52 ./
km²~/.config/:ripgrep> pwd                                                             
/home/kmodi/.config/ripgrep
km²~/.config/:ripgrep> 
```


---

_Comment by @BurntSushi on 2016-09-24 04:57_

Well, `$HOME/.config/ripgrep/ignore` functionality doesn't exist. I was proposing it and asking if it would satisfy your use case. :-)


---

_Comment by @kaushalmodi on 2016-09-24 04:58_

Ah OK, that would work as long as I can maintain just one common global ignore file :)

Thanks!


---

_Renamed from "Support ~/.rgignore in addition to .rgignore files in current+parent dirs" to "support XDG_CONFIG_HOME/ripgrep/ignore" by @BurntSushi on 2016-09-24 23:44_

---

_Comment by @kaushalmodi on 2016-09-26 12:21_

Here's another idea to implement the same. I like it better because then I use an alias to have the complete `rg` configuration control.

How about having a switch named `--ignore-file`. Here's a spec suggestion for that:

> --ignore-file FILE: Read the `.ignore` file FILE before reading the `.ignore`, `.gitignore`, etc. in the current or parent directories. Multiple `--ignore-file` flags may be used.


---

_Comment by @BurntSushi on 2016-09-26 12:30_

When you say "Read the ignore file before reading .ignore .gitignore in current or parent directories," do you mean that `--ignore-file` takes precedence over them? (In contrast, I'd expect `.ignore`/`.gitignore` to take precedence over a global config file.)


---

_Comment by @kaushalmodi on 2016-09-26 12:42_

> do you mean that --ignore-file takes precedence over them? 

No, here was my thinking..

Let's use we did `rg --ignore-file ~/.ignore foo` and suppose we had:

```
$ cat ~/.ignore
*.log
```

Suppose we had a `/proj` and 

```
$ cat /proj/.ignore
!imp.log
```

Then if `~/.ignore` (FILE arg to `--ignore-file`) is read first, `/proj/.ignore` will be able to white-list some part out of the ignore set by the previously read file _if we are running `rg --ignore-file ~/.ignore foo` from the `/proj` directory._

So, _read first_ implies _open to be overridden_.


---

_Comment by @BurntSushi on 2016-09-26 13:02_

@kaushalmodi Hmm, yes, I see, I think those semantics makes sense.


---

_Comment by @davidosomething on 2016-09-26 18:52_

FWIW the_silver_searcher uses `--path-to-ignore SOMEPATH` and the file name used is (as of 0.33.0) `.ignore`


---

_Comment by @kaushalmodi on 2016-09-27 16:01_

Allowing multiple `--ignore-file` becomes even more useful as I see an application where I can have a central `.ignore` file for the whole project (version controlled), and then users can add their specific `.ignore` files on top of that if they wish.


---

_Comment by @BurntSushi on 2016-09-28 20:50_

@kaushalmodi I definitely like the idea of `--ignore-file` (allowing multiple uses) better than a global ignore file. If we just add that, does that satisfy your use case?


---

_Comment by @kaushalmodi on 2016-09-28 20:58_

> If we just add that, does that satisfy your use case?

Absolutely! Then I need to simply add that to my alias and then it becomes as good as a global ignore file. What's even better is that I can have more than one global ignores.. one that is central to the whole project, others which could be user-specific.


---

_Comment by @BurntSushi on 2016-09-28 21:00_

Perfect. Much easier to implement. Hopefully tonight. :-)


---

_Renamed from "support XDG_CONFIG_HOME/ripgrep/ignore" to "add flag to specify one or more additional ignore files" by @BurntSushi on 2016-09-28 21:00_

---

_Comment by @kaushalmodi on 2016-09-28 21:12_

Thanks! Just to confirm that they will have the lowest priority when compared with the ones in the current/parent directories [as in my example above](https://github.com/BurntSushi/ripgrep/issues/45#issuecomment-249558700), correct?


---

_Comment by @BurntSushi on 2016-09-28 21:35_

Yes. The terminology I'd like to use is "precedence." That is, ignore files specified via --ignore-file have a lower precedence than any other ignore file. That means a whitelisted pattern in an ignore file in a directory will override an ignore pattern in an --ignore-file.


---

_Comment by @BurntSushi on 2016-09-29 01:11_

This is slightly trickier than I thought and probably exceeds my budget for middle-of-the-week work unfortunately. The issue is that every ignore file needs to interpret paths relative to _itself_ (these are the semantics of `.gitignore`, and frankly, nothing else really makes much sense), and current ignore code makes this a little tricky. I'm happy to hack this in because I have plans to overhaul the ignore code anyway, but it requires a bit more attention than I'm capable of right now.


---

_Comment by @kaushalmodi on 2016-10-11 01:28_

I hope that the overhaul in the globbing code now makes it possible to implement this. 


---

_Comment by @BurntSushi on 2016-10-11 01:34_

No. The ignore code and the glob code are related but distinct. Sorry.

Refactoring the ignore code is pretty high on my list because I hope to get perf gains out of it.

I just got married. I'll get to this when I can.


---

_Comment by @kaushalmodi on 2016-10-11 01:36_

> I just got married. I'll get to this when I can.

Understood. Congratulations! :)


---

_Comment by @BurntSushi on 2016-10-30 01:21_

Fixed in #202.


---

_Closed by @BurntSushi on 2016-10-30 01:21_

---

_Comment by @OJFord on 2021-07-28 18:49_

@BurntSushi I think it's unfortunate that there's no way (?) to get the behaviour you proposed but ultimately decided against:

```
# $XDG_CONFIG_HOME/ripgrep/profile
export RIPGREP_CONFIG_PATH="$XDG_CONFIG_HOME/ripgrep/ripgreprc"
```

```
# ripgreprc
--ignore-file="$XDG_CONFIG_HOME/ripgrep/ignore"
```

```
# ignore
*.lock
```

> "$XDG_CONFIG_HOME/ripgrep/ignore": No such file or directory (os error 2)


Would you be open to making something possible here? If so, which issue do I open - 'parse env vars in ripgreprc', or 'revisit global ignore file'? 🙂 

NB: another (probably simpler) approach could be to allow relative (to the `RIPGREP_CONFIG_PATH`) paths:
> ./ignore: No such file or directory (os error 2)



(edit: when I say 'no way', I suppose I mean 'with static config files' - obviously I could have my `.profile` write out the location to `ripgreprc`, or `alias` it. It's just the former seems.. not great, and the latter would really need to be a wrapper script on PATH, since I'd also want this in vim for example, so that ends up quite 'hacky' too.)

---

_Comment by @BurntSushi on 2021-07-29 13:21_

@OJFord See: https://github.com/BurntSushi/ripgrep/issues/1792

I don't know why you see wrapper scripts as hacky. I have a whole bunch of them in my `~/bin`.

---
