```yaml
number: 340
title: Do not ignore hidden files that are tracked by git.
type: issue
state: closed
author: anntzer
labels:
  - doc
assignees: []
created_at: 2017-01-23T05:58:10Z
updated_at: 2019-08-07T16:17:25Z
url: https://github.com/BurntSushi/ripgrep/issues/340
synced_at: 2026-01-12T16:13:21Z
```

# Do not ignore hidden files that are tracked by git.

---

_@anntzer_

It looks like neither of `-u`, `-uu`, and `-uuu` allow "search exactly the files that git is tracking, including the hidden ones" (i.e., `git grep`).  If this is correct, could you consider adding this possibility?
Thanks in advance.

---

_Comment by @BurntSushi on 2017-01-23 10:33_

Could you please provide a test case and describe expected output versus actual output?

---

_Comment by @anntzer on 2017-01-23 17:32_

```
#!/bin/sh
git init /tmp/tmprepo
cd /tmp/tmprepo
echo ignoredfile >.gitignore
echo foo >normalfile
echo foo >.hiddenfile
echo foo >ignoredfile

echo 'rg -l:'
rg -l foo

echo 'rg -l -u:'
rg -l -u foo

echo 'rg -l -uu:'
rg -l -uu foo

echo 'rg -l -uuu:'
rg -l -uuu foo
```
outputs
```
rg -l:
normalfile
rg -l -u:
ignoredfile
normalfile
rg -l -uu:
ignoredfile
.hiddenfile
normalfile
rg -l -uuu:
ignoredfile
.hiddenfile
normalfile
```
so, as far as I can see, there is no way to have normalfile and .hiddenfile but not ignoredfile.

---

_Comment by @BurntSushi on 2017-01-23 18:08_

I don't see your **expected** output anywhere. What do you expect to see?

---

_Comment by @BurntSushi on 2017-01-23 18:08_

> so, as far as I can see, there is no way to have normalfile and .hiddenfile but not ignoredfile.

I don't understand. Why doesn't `rg foo --hidden` work for you? It prints `.hiddenfile` and `normalfile` for me.

---

_Comment by @anntzer on 2017-01-23 18:14_

My expected output was some flag that would return `.hiddenfile` and `normalfile`, but as you mention, `--hidden` is indeed the correct flag -- my bad.
I would suggest adding a mention of this option next to `-u`, especially as `-u` is listed in the "COMMON OPTIONS" section of the man page but not `--hidden`.
Feel free to close this.

---

_Label `doc` added by @BurntSushi on 2017-01-23 18:15_

---

_Comment by @BurntSushi on 2017-01-23 18:15_

Docs can definitely be clearer. I'll mark this as a doc bug. Thank you. :-)

---

_Comment by @anntzer on 2017-01-24 07:24_

Ah, but I noticed that `--hidden` includes `.git` in the search:
```
#!/bin/sh
git init /tmp/tmprepo
cd /tmp/tmprepo
rg --hidden Unnamed
```
outputs
```
Initialized empty Git repository in /tmp/tmprepo/.git/
.git/description
1:Unnamed repository; edit this file 'description' to name the repository.

.git/hooks/update.sample
55:"Unnamed repository"* | "")
```
but the contents of `.git` should probably be ignored unless some number of `-u` is passed?  (Not sure what makes the most sense here.)

---

_Comment by @BurntSushi on 2017-01-24 10:41_

It's working as intended. --hidden removes the hidden filter and therefore
shows the .git directory. If you want to ignore it, then add it to an
.ignore file.

On Jan 24, 2017 2:24 AM, "Antony Lee" <notifications@github.com> wrote:

> Ah, but I noticed that --hidden includes .git in the search:
>
> #!/bin/sh
> git init /tmp/tmprepo
> cd /tmp/tmprepo
> rg --hidden Unnamed
>
> outputs
>
> Initialized empty Git repository in /tmp/tmprepo/.git/
> .git/description
> 1:Unnamed repository; edit this file 'description' to name the repository.
>
> .git/hooks/update.sample
> 55:"Unnamed repository"* | "")
>
> but the contents of .git should probably be ignored unless some number of
> -u is passed? (Not sure what makes the most sense here.)
>
> —
> You are receiving this because you commented.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/340#issuecomment-274728726>,
> or mute the thread
> <https://github.com/notifications/unsubscribe-auth/AAb34mvqwuqRjT-lyKKYEc8BdvfJ2LMuks5rVac4gaJpZM4Lqr_f>
> .
>


---

_Comment by @anntzer on 2017-01-24 11:11_

Obviously `rg --hidden` can have whatever semantics *you* decide, but it seems a bit strange that (when in a git repo) it would mean "search all nonbinary files that *either* are tracked by git (including hidden ones), *or* are in .git/".  It would seem more reasonable to have

no flag: search all nonbinary, nonhidden files that are tracked by git
--hidden: search all nonbinary files that are tracked by git
-u: search all nonbinary, nonhidden files, without caring about git (so including .git/)
-u --hidden: search all nonbinary files, without caring about git (so including .git/)

-uu seems equivalent to -u --hidden, by the way.  I don't know how much you care about backcompat, but if you want the flags to be orthogonal to each other perhaps you can remove -uuu and let the new -uu correspond to the old -uuu (as the old -uu is covered by -u --hidden).

---

_Comment by @BurntSushi on 2017-01-24 12:06_

> it would mean "search all nonbinary files that either are tracked by git (including hidden ones), or are in .git/"

But that's not what it means. `--hidden` simply means "search hidden files." The `--hidden` flag is orthogonal to whether a file is tracked by `git` or not.

> -uu seems equivalent to -u --hidden, by the way.

Yes. The `-u` flags are aliases. `-u` is `--no-ignore`, `-uu` is `--no-ignore --hidden` and `-uuu` is `--no-ignore --hidden -a`.

> no flag: search all nonbinary, nonhidden files that are tracked by git

This seems to be the core of your suggestion. Unfortunately, ripgrep doesn't operate based on what's tracked in git, but rather, what's in your gitignore files. Therefore, it is inherently imperfect.

The intended solution here is to either whitelist specific hidden files that you want searched, or to pass `--hidden` and blacklist files you don't want searched. That's the point of `.ignore` files: it provides a way to override default behavior if the default behavior is undesirable.

---

_Comment by @anntzer on 2017-01-24 16:00_

Ah, I see, rg operates in a world where there is no knowledge of git, but there just happens to be already written .ignore files that are called ".gitignore".  I guess that's fine (especially as I can .ignore .git/ from my $HOME as you suggest), just a bit... unexpected, at least for me.

---

_Comment by @bdarfler on 2017-02-18 18:13_

I came across this exact issue while trying to come up with a solid set of flags to use with `ctrlp` in vim. I ended up settling on `rg --hidden --glob '!.git'` which does what I want and, since it lives in my `.vimrc`, I don't mind the verbosity. I do think it's surprising to find `rg` searching through the `.git` directory but I can also understand the mental model of `rg` not wanting to be "git aware" or any other VCS for that matter.

---

_Comment by @BurntSushi on 2017-02-18 18:56_

@bdarfler `rg` *is* git aware though. It knows how to read `.gitignore` files and implements the rules described in `man gitignore`, including knowing to read your `$HOME/.gitconfig`. The problem here is that you're passing the `--hidden` flag, which says, "search hidden files and directories." That includes `.git`.

---

_Comment by @bdarfler on 2017-02-18 19:05_

Thanks, I wasn't being as clear as I could have been. I just wanted to point out that this seems to be a common use case and it might be nice to have a flag for it. However, there is an easy enough workaround. Thanks for the awesome tool!

---

_Comment by @BurntSushi on 2017-02-18 19:14_

@bdarfler Fair enough. There are two other possibilities. One is to create an alias, e.g., `alias rg="rg --hidden -g '!.git'"`. Another is to encode the rules into an `.ignore` file. e.g.,

```
$ cat $HOME/.config/ripgrep/ignore
!.*
.git
```

and then use `rg --ignore-file $HOME/.config/ripgrep/ignore` (and perhaps create an alias).

---

_Comment by @bdarfler on 2017-02-18 19:17_

Cool! My primary use case is for `ctrlp` fuzzy finding in vim so I just tuck the `rg --hidden -g '!.git'` away in my `.vimrc` and that is sufficient for me.

---

_Closed by @BurntSushi on 2017-03-13 00:25_

---

_Comment by @tombh on 2017-04-18 02:00_

Sorry to bring this back up, but just to add that I'm in the `ctrlp` situation as well and also using @bdarfler's fix (thanks). Maybe this reinforces the idea that expecting `--hidden` to also ignore `.git/` is a very specific use case.

---

_Comment by @BurntSushi on 2017-04-19 12:10_

@tombh Right. Especially if this is a config knob inside of some vim plugin somewhere. Having to use `--hidden -g'!.git'` seems fine in that case.

---

_Comment by @mgeisler on 2017-09-18 15:37_

Let me just add that I had exactly the same expectation as @anntzer: I expected `rg` to keep ignoring files ignored by Git (and thus not recurse into `.git/`) when `--hidden` is used.

That `rg --hidden` includes files inside `.git/` is technically correct since `.git/` isn't mentioned in any of my `.gitignore` files. The reason is of course that Git knows to ignore the directory by default :smiley: 

I solved it by creating a `~/.ignore` file with the following:
```
.git/
.hg/
.svn/
```
That makes `--hidden` work the way I had expected it to work in the first place.

---

_Comment by @mshkrebtan on 2019-08-07 16:17_

So, I came up with the following configuration in my `.vimrc`:

```
if executable('rg')
  let g:ctrlp_use_caching = 0
  let g:ctrlp_user_command = 'rg --files --no-ignore --ignore-file ~/.config/ripgrep/ignore --hidden %s'
  let g:ackprg = 'rg --vimgrep --no-ignore --ignore-file ~/.config/ripgrep/ignore --hidden'
endif
```

This allows me to open and search through hidden files, even if they are in `.gitignore`.

But then I set an ignore file which has `**/.git/*`, `**/.terraform/*`, `[._]*.sw[a-p]` and so on, so that I do not see a `.filename.swp` in my CtrlP list and ack.vim does not perform search in `.terraform` directories.

Works like a charm! Thank you, @BurntSushi!

---
