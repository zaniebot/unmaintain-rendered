```yaml
number: 623
title: hidden files to be searched by default
type: issue
state: closed
author: akostadinov
labels: []
assignees: []
created_at: 2017-10-06T20:28:02Z
updated_at: 2023-06-10T10:24:01Z
url: https://github.com/BurntSushi/ripgrep/issues/623
synced_at: 2026-01-12T16:13:22Z
```

# hidden files to be searched by default

---

_@akostadinov_

Am I the only one that thinks hidden files should be searched by default? Especially when trying to find something related to configuration, these things are often put into hidden files in user home.

And if I'm the only one, probably #196 is what can help me.

---

_Comment by @okdana on 2017-10-06 21:21_

Excluding hidden/dot files by default is common for this class of tool — notably, `ag` and `pt` both do it. `rg` was inspired directly by `ag`, so that's probably (?) the immediate reason for the current behaviour. But i think it makes sense even if you ignore the historical baggage, since these tools are mainly designed for searching through source code, and dot files in that context are usually just noise.

If you've read that other ticket you've probably already picked this up, but just in case, this is your best work-around for now:

```
alias rg='rg --hidden'
```

---

_Comment by @BurntSushi on 2017-10-06 23:00_

@okdana has got it. This isn't changing.

---

_Closed by @BurntSushi on 2017-10-06 23:00_

---

_Comment by @kriskhaira on 2020-07-17 07:03_

The following is @okdana's solution, but with the .git folder remaining ignored:
```
alias rg="rg --hidden --glob '!.git'"
```

---

_Comment by @pkoch on 2021-08-25 22:56_

> these tools are mainly designed for searching through source code, and dot files in that context are usually just noise.

More and more, I've been finding this not to be the case anymore. I've broken pipelines over this default. Not ripgrep's fault, I should read the docs, but it's still evidence against this argument.

In any case, using `RIPGREP_CONFIG_PATH` feels like a good recourse to have.

---

_Comment by @BurntSushi on 2021-08-25 23:02_

I don't know what you mean exactly by broken pipelines, but I haven't budged on this personally. In any case, ripgrep's "smart" filtering defaults are really a part of its identity at this point.

---

_Comment by @pkoch on 2021-08-26 02:42_

CI pipelines. Those based on GitHub Actions are normally kept at `.github/workflows`.

I'm not really trying to change your mind, you seem pretty resolute. I'm just bringing evidence forward.

---

_Comment by @ssbarnea on 2021-08-26 07:12_

I agree with @pkgw here and I use that alias `rg='rg --hidden --no-follow --max-columns 255 --no-heading --column -F'` to overcome for some of the undesired defaults. In addition to that I have a `~/.ignore` file which lists things I want to be ignored with notable mentions like `.git` and `.tox` folders.

As you said, not all dot files are created equal. Almost nobody wants to search on cache files but almost for sure they want to search inside hidden config files. Even the default user config folder based on XDG standard is `~/.config`.

I do hope that @BurntSushi will reconsider his opinion on that default because we cannot really deploy our ripgrep config on each machine we use it. I think that the tricky bit about implementing this feature is that it should also include an implicit `.ignore` set of values, ones to be used when the is not found. Otherwise lots of people would be upset about getting results from inside their .git folders.

Interestingly I do not see any downvote on the proposal, only 10 upvotes. Still the number is likely too small to be considered as a real survey.

---

_Comment by @akostadinov on 2021-08-26 07:13_

<del>I'm +1 for hidden files or at least smarter hidden files filtering. There are a lot of configuration hidden files and they are not less important than other source files. The pipelines example is good. Also bundler config files come to my mind.</del>

Ops, I see that I filed the issue :)

My suggestion is that at least under Git repositories to search in hidden files, because caches and other temporary files will be in `.gitignore` anyway.

---

_Comment by @BurntSushi on 2021-08-26 08:19_

No, the default is never changing. I don't practice upvote driven development. If you can deploy ripgrep then you should be able to also deploy aliases/ignores/configs.

The defaults are not perfect all the time. That's why the tool is configurable. The defaults are just an approximation. It is also important for defaults to be simple. "Skip all hidden files" is simpler than "skip all hidden files except foo, bar and baz."

---

_Comment by @akostadinov on 2021-08-26 09:40_

@BurntSushi , how a about a new feature to skip only files ignored by the CMS system at use (git, etc.), and search in everything else?

---

_Comment by @BurntSushi on 2021-08-26 11:46_

@akostadinov That's what `-./--hidden` does. (Well, technically ripgrep will still also try to skip binary files, so you can also add `--binary`.)

---

_Comment by @akostadinov on 2021-08-27 08:55_

@BurntSushi , I mean some feature to automatically detect whether it is run in a SCM repo or not. Or a feature to read a configuration file from within a repo when such one is present.

e.g. (in a similar way how `.editorconfig` works) when running in my home dir I may want default behavior, when I run in a git repo, I may have auto-detection, or a configuration file for that specific repo.

Would some of the above suggestions be acceptable?

---

_Comment by @BurntSushi on 2021-08-27 09:32_

Also probably not happening. Others have requested it. Search the tracker. I'm on mobile.

Just setup your ignore files. You should be able to whitelist hidden files or directories.

---

_Comment by @Nakilon on 2022-08-17 04:18_

Personally I would never complain if the Github Actions wasn't invented as being put into `.github` and that now I need to grep those often to find how I solve common issues.

---

_Comment by @BurntSushi on 2022-08-17 12:30_

@Nakilon Sorry, what's the point of your comment? Just to complain? Why not put `!/.github/` in your `.ignore`? Boom. Problem solved.

---

_Comment by @Nakilon on 2023-06-09 06:31_

> put `!/.github/` in your `.ignore`?

Can't figure out how this autofiltering thing works. I have `mydir/infrared/.rgignore` with content:
```none
/*.cache.yaml
```
but
```
$ rg jobs: --sortr created mydir -g*.y*l
...mydir/infrared/token.cache.yaml
204267:Now people are getting food/gas priced out of their homes and jobs:
...
```
Why doesn't it ignore the file?



---

_Comment by @BurntSushi on 2023-06-09 11:06_

Please open a new Discussion question instead of posting in some old issue that may or may not be related.

Please also include a full reproduction that others can try.

Please also read the guide. For example, why haven't you tried using the `--debug` flag yet?

---

_Comment by @StackOverflowExcept1on on 2023-06-10 07:57_

I also ran into the same problem today when the `rg foo` command cannot find the code that is in `.github/workflows`.It makes sense to exclude the `.gitignore` files and the `.git` folder from hidden files. The hidden files should be searched by default.
For example, if you run the command `grep foo -r --include="*.yaml"`, it will find matches in hidden files. If your utility is trying to replace `grep`, why put restrictions on hidden files?

---

_Comment by @BurntSushi on 2023-06-10 10:23_

> If your utility is trying to replace `grep`

[It's not.](https://github.com/BurntSushi/ripgrep/blob/master/FAQ.md#posix4ever)

I guess I need to start locking issue threads now.

---

_Locked by @ghost on 2023-06-10 10:24_

---
