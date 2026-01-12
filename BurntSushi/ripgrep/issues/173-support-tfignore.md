```yaml
number: 173
title: Support .tfignore
type: issue
state: closed
author: alejandro5042
labels:
  - help wanted
  - question
assignees: []
created_at: 2016-10-13T00:55:47Z
updated_at: 2016-11-19T14:35:13Z
url: https://github.com/BurntSushi/ripgrep/issues/173
synced_at: 2026-01-12T18:23:11Z
```

# Support .tfignore

---

_@alejandro5042_

`.tfignore` files are exactly like `.gitignore` files but for local workspaces in Team Foundation Version Control (TFVC). TFVC is a centralized VCS from Microsoft supported by their TFS products. `ripgrep` should support it since it's as easy as applying the same rules as `.gitignore`.

For more info: https://www.visualstudio.com/en-us/docs/tfvc/add-files-server#customize-which-files-are-ignored-by-version-control


---

_Comment by @BurntSushi on 2016-10-13 01:15_

Is there a specification other than that link? If not and if that link is a complete description, then the two file formats are most definitely not the same. For one, the `*` wildcard isn't escaped in gitignore. For another, gitignore supports more types of globs (recursive wildcards for example) and has more explicitly defined semantics on whether wildcards can match path separators or not.

How widely used is this version control system? Keep in mind that every ignore file supported requires an additional stat call for every directory searched.


---

_Comment by @alejandro5042 on 2016-10-13 01:56_

You're right. I was too casual in creating this issue and in my language. Without having a better spec, it's hard to know what the differences are and I cannot find a better one online.

Having used gitignore and tfignore files for years, I've always just copied the gitignore to tfignore and it has seemed to work. (Granted, I am not a frequent TFS local workspace user.) The one difference I noted was that tfignore treats all slashes the same, for compatibility with gitignore. I believe what you are seeing with `\*.txt` is treated as `/*.txt`. So yes, they are not "exactly the same" as I mentioned before.

TFS is widely used in the Windows ecosystem. For years, it was the only source control system that shipped with Visual Studio. Lately, Microsoft and their ecosystem has been slowly transitioning to Git, but many repos will continue to be in TFS for years. Even though we're trying to transition, it will be some time and we have hundreds of devs who would love to use this tool. I suppose I can copy the .tfignore file to .gitignore for us and check that in.

There are [65 non-trivial tfignore files](https://github.com/search?utf8=%E2%9C%93&q=filename%3A.tfignore+size%3A%3E5&type=Code&ref=searchresults) checked into GitHub--I'm assuming TFS repos that transitioned to Git. Perhaps gleaming over those files can give us more info. (Non-trivial as there are thousands of .tfignore files that just ignore the git directory.)

I am fine with this issue sitting until further input. I just wanted to bring up that TFS is a major source control system that has a similar ignore file that isn't supported by your [awesome] tool.


---

_Comment by @alejandro5042 on 2016-10-13 01:58_

Good point on the stat call. I recommend awaiting further input to see if anyone else is affected; the workaround is to simply copy the file, or transition to Git... 😄 


---

_Comment by @BurntSushi on 2016-10-13 02:26_

All right. Thanks for additional information. A good example of a VCS that I would like to support next is Mercurial.

I'm on mobile, but I'll take a closer look at those tfignore files you linked. I'll also hunt around for a proper specification. (Without that, it will be hard to add correct support.)


---

_Comment by @hefengrren on 2016-10-13 07:29_

One more thing, just to remind: what if there are multiple ignore files?
Maybe an option defaults to GIT, but could specify one (TFS or HG).

In my case, I have `.gitignore` and `.tfignore`.
I'd use `.gitignore`: moved from TFS to GIT and `.tfignore` is not updated anymore.


---

_Comment by @BurntSushi on 2016-10-13 11:10_

If there are multiple ignore files in the same directory, we could just define a precedence and use both.


---

_Label `question` added by @BurntSushi on 2016-10-13 11:11_

---

_Comment by @BurntSushi on 2016-10-29 00:25_

FYI, [this `.tfignore` file](https://github.com/vinayakDhar/TicketingService/blob/ac9b61dee147e52079a12e0cd44771a0737985c0/.tfignore) seems to include a comment with a reasonably well written specification. It is definitely _not_ identical to the `gitignore` format. That means this needs its own matcher implementation (which isn't that bad, so long as everything can be mapped to standard Unix globs, which looks feasible).


---

_Comment by @BurntSushi on 2016-10-29 00:25_

I'd say I'm personally unlikely to implement something like this (because I feel like it's not widely used), but I'd be happy to accept a PR and maintain it.


---

_Label `help wanted` added by @BurntSushi on 2016-10-29 00:25_

---

_Comment by @BurntSushi on 2016-11-19 14:34_

I'm going to close this because I'd like to keep the issue tracker culled to things that are likely to get worked on in a reasonable time frame. I don't think this will get worked on unless someone wants it badly enough to do it. If that happens, we can re-open this issue and assign it to them.


---

_Closed by @BurntSushi on 2016-11-19 14:34_

---
