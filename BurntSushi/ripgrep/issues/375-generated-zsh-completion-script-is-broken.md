```yaml
number: 375
title: Generated zsh completion script is broken
type: issue
state: closed
author: kastiglione
labels:
  - question
assignees: []
created_at: 2017-02-21T21:35:28Z
updated_at: 2017-04-25T16:27:05Z
url: https://github.com/BurntSushi/ripgrep/issues/375
synced_at: 2026-01-12T16:13:21Z
```

# Generated zsh completion script is broken

---

_@kastiglione_

It's cool that part of the build system generates completions for the various shells. However, the zsh completion script is broken. After typing `rg <TAB>` it completes either `PATTERN` or `PATH`, literally. Attempting `rg somepattern <TAB>` will complete flags rather than a path. This is with version 0.4.0 from homebrew. Perhaps a newer version of clap fixes this.

---

_Comment by @BurntSushi on 2017-02-21 22:06_

Yeah, this isn't really a feature I can maintain or fix. I don't have the expertise. :-(

This should be filed against the clap repo and/or the feature should be removed.

---

_Label `question` added by @BurntSushi on 2017-02-21 22:06_

---

_Comment by @kastiglione on 2017-02-21 23:27_

I'll report there and follow up here.

---

_Comment by @kbknapp on 2017-02-22 04:03_

I said this in the clap issue, but just for awareness I'll say it here too; I believe this has to do with Zsh completions in general and not specific to what clap is generating. Generating the literal word `PATTERN` or `PATH` for positional/free arguments is simply because there's no way know what exactly to complete.. clap doesn't know what a `PATTERN` is supposed to be, so the placeholder of the argument name is used. Bash *does* fall back to path completion (*not* based on the argument name, but if there's nothing else to complete), but AFAIK there isn't a way to do this in Zsh. The only part of clap that *could* make this better is kbknapp/clap-rs#568 allowing you to write arbitrary Rust code to use while completing, but it would only make this better if someone wrote the additional Rust code to use when attempting to complete a `PATTERN` or `PATH`, i.e. it won't magically fix it.

As far as "broken" I'd say that's a slight stretch because I use it daily (Zsh is my daily shell and I use `rg` frequently). 
```
$ rg -<tab>
$ rg --<tab>
$ rg [pattern] --f<tab>
$ rg -f<tab>
```
All do exactly what I'd expect, and IMO does it better than even Bash because there are short/long flags listed together, they have some help context, and even allow one to scroll through the options.

As for completing `$ rg somepattern <tab>`, yes, this should be either the literal `PATH` (see comments above) or list options/flags if no other positional/free args exist. However, unlike in Bash I'm not aware of a way to say, "This arg comes first, this one second, etc." in Zsh. If someone knows a way, I'd love to hear it! (Seriously. I'm not being facetious)

Of course I'm totally willing, and would be even be happy to be proven wrong and shown where the error is, or how the generated Zsh completion code can be made better. Saying I'm an expert in Zsh completion scripts would be stretch of the imagination of epic proportions! :wink:

---

_Comment by @BurntSushi on 2017-02-22 12:05_

@kbknapp Thanks for looking into it! I'll comment on the clap issue so we don't have an echo effect. :-)

---

_Comment by @BurntSushi on 2017-04-09 13:12_

I'm going to close this because I don't know what the next steps are on ripgrep's side of things.

---

_Closed by @BurntSushi on 2017-04-09 13:12_

---

_Comment by @Brottweiler on 2017-04-10 19:43_

Would it be possible to remove the Zsh completions? I rather have no completion, so that I can complete filenames/folders using regular Zsh completion, than having broken completion.

---

_Comment by @blueyed on 2017-04-10 19:45_

Makes sense to me, too.
And then it could be re-added to zsh-completions again (https://github.com/zsh-users/zsh-completions/commit/b7c11f4020246a533cb470d50b94598e190164e9).

---

_Comment by @BurntSushi on 2017-04-10 20:26_

> I rather have no completion, so that I can complete filenames/folders using regular Zsh completion, than having broken completion.

I don't quite understand. Why not just ignore the zsh completion file that's in the ripgrep distribution? Moreover, there is at least one other person in this thread that claims that current completion script *isn't* broken, so it seems a little rude to just up and remove it.

---

_Comment by @blueyed on 2017-04-10 22:09_

@BurntSushi 
Good point about just ignoring it. IIRC I am using the old version from zsh-completions myself (after asking for yours to get installed for the Arch package).

For whom does it work properly though?!

---

_Comment by @BurntSushi on 2017-04-10 23:37_

@blueyed @kbknapp said:

> As far as "broken" I'd say that's a slight stretch because I use it daily (Zsh is my daily shell and I use rg frequently).

---

_Comment by @thom-nic on 2017-04-25 15:35_

Can anyone point to a "correct" _rg site-function that can be loaded while we wait for this to get fixed in the linked project?  I've found a [SO post](http://stackoverflow.com/a/17984833/213983) that explains how to override the builtin site-functions, now I just need a fixed version to use instead!

---

_Comment by @blueyed on 2017-04-25 16:27_

@thom-nic 
https://github.com/zsh-users/zsh-completions/commit/b7c11f4020246a533cb470d50b94598e190164e9

---
