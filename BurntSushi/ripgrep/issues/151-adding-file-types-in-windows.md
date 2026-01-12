```yaml
number: 151
title: Adding file types in Windows
type: issue
state: closed
author: theamazingfedex
labels:
  - doc
assignees: []
created_at: 2016-10-05T23:05:44Z
updated_at: 2016-10-11T01:06:59Z
url: https://github.com/BurntSushi/ripgrep/issues/151
synced_at: 2026-01-12T18:23:11Z
```

# Adding file types in Windows

---

_@theamazingfedex_

I'm running ripgrep v0.1.17, on Windows 7 in Cmder console emulator, and every time that I try to add a custom file type, i get the error `Invalid arguments.` despite precisely following the example.

What i'm calling: `rg --type-add spark:*.spark`
I even tried doing exactly as the example showed: `rg --type-add html:*.html` but still get the same error.

Is there something I'm doing wrong somewhere?

On a separate note, great job on the tool; I've been making co-worker's jaw's drop at how fast it rips through our company repos and finds what I need. :+1: (plus it's rust, so I get to show people an awesome practical use of rust) :+1: :+1: :+1: 


---

_Comment by @BurntSushi on 2016-10-05 23:23_

@theamazingfedex Thanks for the kind words. :-)

> I even tried doing exactly as the example showed

Could you show me where you saw this example? I ask because it's wrong. I thought I fixed all of them, but I must have left one lingering somewhere.

In any case, you can't just run `rg --type-add'html:*.html'`. `rg` doesn't persist your type settings anywhere. You need to pass `--type-add` to every call to `rg`. While this seems like a pain, my intention was for folks to configure their types using aliases. (Is that even a thing on Windows? In bash I can do, `alias rg="rg --type-add 'foo:*.foo'` and then `rg -tfoo PAT` does the right thing.)


---

_Comment by @kaushalmodi on 2016-10-06 10:32_

@BurntSushi This confusion that `--type-add` stores the type association in some file has come up multiple times in these issues. I thought the same when I used `ripgrep` the first time too. 

But that `alias` example for `--type-add` is perfect. May be you can add that to the `--help` and man page?


---

_Comment by @BurntSushi on 2016-10-06 10:47_

@kaushalmodi Right, believe me, I realize this has been a pain point. :-) I will do another pass through everything that mentions `--type-add` and try to be extra clear about it. Including the alias example sounds good, although I doubt it will work verbatim for Windows users (who aren't using `cygwin`).


---

_Label `doc` added by @BurntSushi on 2016-10-06 10:47_

---

_Comment by @theamazingfedex on 2016-10-07 16:34_

I'm using Cmder, which uses clink around cmd.exe in a ConEmu window, so I do have a way of doing aliased commands. Here's some pictures:

**results of trying `--type-add html:*.html`:**
![Results of using the command](https://cloud.githubusercontent.com/assets/6488706/19197055/317b2a62-8c75-11e6-99b0-c665fffaf86f.png)

**`rg --help` (where I saw the example)**
![rg --help](https://cloud.githubusercontent.com/assets/6488706/19197117/7d97f510-8c75-11e6-9a94-762e973f24db.png)

**And if I try to do it as a one-off:**
![image](https://cloud.githubusercontent.com/assets/6488706/19197346/93cc01c2-8c76-11e6-9f8e-3e6112ecd5bf.png)

Also, I created an alias for `rg=rg --type-add 'spark:*.spark'` then tried `rg -tspark foo` and get the message: `unrecognized file type: spark`


---

_Comment by @BurntSushi on 2016-10-07 19:00_

Could you please provide a reproduction that doesn't use an alias? I can run `rg --type-add 'spark:*.spark' -tspark foo` just fine, which to me suggests that your alias isn't working.


---

_Comment by @theamazingfedex on 2016-10-07 19:01_

I just tried running the command `rg --type-add spark:*.spark -tspark foo` in git bash, and it worked, but running it in cmd does not, as displayed in the third screenshot of my previous comment.


---

_Comment by @theamazingfedex on 2016-10-07 19:03_

Well well, I just updated to v0.2.1, and ran `rg --type-add spark:*.spark -tspark foo` and it worked wonderfully. Not sure what changed, but it works now, thanks!


---

_Comment by @theamazingfedex on 2016-10-07 19:08_

Is there a blog post somewhere detailing the reasoning behind choosing to push for an alias approach vs a config-based approach for adding file types? I'm curious as to what the benefits of doing it this way are.


---

_Closed by @theamazingfedex on 2016-10-07 19:09_

---

_Comment by @BurntSushi on 2016-10-07 19:16_

No. Config files are extra complexity that I would like to avoid if
possible. I am open to adding one in the future, but I would like to hold
off for now.

I would also encourage anyone to upstream their file types if they are
types that others might use.

On Oct 7, 2016 3:09 PM, "Daniel Wood" notifications@github.com wrote:

> Closed #151 https://github.com/BurntSushi/ripgrep/issues/151.
> 
> —
> You are receiving this because you were mentioned.
> Reply to this email directly, view it on GitHub
> https://github.com/BurntSushi/ripgrep/issues/151#event-816667813, or mute
> the thread
> https://github.com/notifications/unsubscribe-auth/AAb34pA54hYz0k1BeSqV5MOP_WYtSMa_ks5qxpjZgaJpZM4KPYbL
> .


---

_Comment by @theamazingfedex on 2016-10-07 19:28_

Alright, that's fair. Forgive my ignorance, but how would I upstream a file type?


---

_Comment by @BurntSushi on 2016-10-07 19:33_

Submit a PR. There are lots of previously merged PRs that do just that, and
they would be good examples.

Sorry for the short response. On mobile. If you need more help, let me know
and I'll try to give more of an explanation when i get a chance.

On Oct 7, 2016 3:28 PM, "Daniel Wood" notifications@github.com wrote:

> Alright, that's fair. Forgive my ignorance, but how would I upstream a
> file type?
> 
> —
> You are receiving this because you were mentioned.
> Reply to this email directly, view it on GitHub
> https://github.com/BurntSushi/ripgrep/issues/151#issuecomment-252340499,
> or mute the thread
> https://github.com/notifications/unsubscribe-auth/AAb34pTi2aR0GjXXV0Y44ZYRRNA7hYxwks5qxp1lgaJpZM4KPYbL
> .


---

_Comment by @theamazingfedex on 2016-10-07 19:42_

Oh okay, I had a suspicion that was what you were implying, but I had not personally heard that terminology used for PR's, and wanted to be sure ^_^. I've got a couple types that would be widely useful; I'll make individual PR's for each one.


---

_Comment by @theamazingfedex on 2016-10-07 20:43_

#153 #154 and #155 have been opened against master


---

_Comment by @BurntSushi on 2016-10-11 01:06_

@theamazingfedex Thank you!


---
