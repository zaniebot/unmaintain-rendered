```yaml
number: 1350
title: "[FEATURE REQUEST] Export matches' metadata to text file"
type: issue
state: closed
author: NightMachinery
labels:
  - question
assignees: []
created_at: 2019-08-16T07:21:40Z
updated_at: 2019-08-17T15:02:38Z
url: https://github.com/BurntSushi/ripgrep/issues/1350
synced_at: 2026-01-12T16:13:23Z
```

# [FEATURE REQUEST] Export matches' metadata to text file

---

_@NightMachinery_

I love https://github.com/aykamko/tag, but it's hacky, buggy and unmaintained. If rg adds a `--export-matches somefile.txt` option, so that all matches are numbered like this:
![image](https://user-images.githubusercontent.com/36224762/62473849-d95a1080-b7b6-11e9-8084-9d1e97930a0e.png)
And their metadata is stored in `somefile.txt`:
```
Match i: FILE_PATH LINE_NUMBER COLUMN_NUMBER
```

This way we can write a shell function to use `somefile.txt` to easily open our editor of choice on the desired match.
I know there are editor-specific plugins that use `--vimgrep` to accomplish this, but this is a different use case. With this, I can search any collection of files (The editors usually like to just search project directories) from the shell, using options like context and whatnot, and open any application for the results.

---

_Comment by @BurntSushi on 2019-08-16 11:39_

Could you please explain why the `--json` flag is insufficient for your use case?

---

_Label `question` added by @BurntSushi on 2019-08-16 11:39_

---

_Comment by @NightMachinery on 2019-08-17 14:28_

Is there a way to ask `rg` to store the json output in a file, and output
otherwise normally? Since my use case is an interactive one, where `rg`
works as it always does, but there is an added functionality of jumping to
matches.
The other problem is that I need to see the matches numbered in the
interactive output, so I can invoke the function that reads the json file
with the index of the match I want to jump to.
I know I can rewrite a whole new frontend using `--json` to do all these,
but that's obviously way overkill and arduous.
(Have you taken a look at the project I linked?)

On Fri, Aug 16, 2019 at 4:10 PM Andrew Gallant <notifications@github.com>
wrote:

> Could you please explain why the --json flag is insufficient for your use
> case?
>
> —
> You are receiving this because you authored the thread.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/1350?email_source=notifications&email_token=AIUL56ROKJF57VODUKDFQHTQE2GZHA5CNFSM4IMEZU52YY3PNVWWK3TUL52HS4DFVREXG43VMVBW63LNMVXHJKTDN5WW2ZLOORPWSZGOD4OMVFA#issuecomment-521980564>,
> or mute the thread
> <https://github.com/notifications/unsubscribe-auth/AIUL56URZBML26S5N5VXD73QE2GZHANCNFSM4IMEZU5Q>
> .
>


---

_Comment by @BurntSushi on 2019-08-17 15:02_

It sounds like you can build your tool as a wrapper around the JSON output of ripgrep.

---

_Closed by @BurntSushi on 2019-08-17 15:02_

---
