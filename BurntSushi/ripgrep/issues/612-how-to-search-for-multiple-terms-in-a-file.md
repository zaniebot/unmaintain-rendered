```yaml
number: 612
title: How to search for multiple terms in a file?
type: issue
state: closed
author: ylluminate
labels: []
assignees: []
created_at: 2017-09-23T18:34:53Z
updated_at: 2025-05-26T13:23:37Z
url: https://github.com/BurntSushi/ripgrep/issues/612
synced_at: 2026-01-12T16:13:22Z
```

# How to search for multiple terms in a file?

---

_@ylluminate_

I'm looking for a way to use `ripgrep` to search for multiple keywords in files.  For example, I would like to grep through all files within a folder that have both terms present anywhere in the file (in this case "head" and "qt" from the Homebrew Formula folder).  

Is there a clean way to use an and operator as such?  I though that there may be other solutions as in [other here](https://unix.stackexchange.com/a/55391/47012), however it even appears that these commands (such as `awk '/pattern1/ && /pattern2/' *`) only match via `and` per line and not the entire file...

---

_Comment by @BurntSushi on 2017-09-23 18:38_

No, ripgrep doesn't have anything to support that out of the box. It is fundamentally a line oriented searcher. You will need to write a wrapper script of some sort, probably by using the `-l` flag. I can help you write it if you like.

---

_Closed by @BurntSushi on 2017-09-23 18:38_

---

_Comment by @ylluminate on 2017-09-23 18:52_

Interesting, so I was thinking that perhaps `rg` would accept the output from `-l` as the input (`rg -l head * | rg qt`), but it's not working out as I expected since it doesn't use the output as the files args. 

Looks like I might have to use `xargs` with the `rg` `-f` arg... but that would remove a good bit of the efficiency of ripgrep it seems.

---

_Comment by @BurntSushi on 2017-09-23 19:18_

You will want to use xargs.

On Sep 23, 2017 2:52 PM, "ylluminate" <notifications@github.com> wrote:

Interesting, so I was thinking that perhaps rg would accept the output from
-l as the input (``rg -l head * | rg qt`), but it's not working out as I
expected since it doesn't use the output as the files args.

—
You are receiving this because you modified the open/close state.

Reply to this email directly, view it on GitHub
<https://github.com/BurntSushi/ripgrep/issues/612#issuecomment-331662563>,
or mute the thread
<https://github.com/notifications/unsubscribe-auth/AAb34n1ZENWSeLPFOCOQZhg7O2MUaEabks5slVOLgaJpZM4PhpDz>
.


---

_Comment by @ylluminate on 2017-09-23 19:25_

So it turns out that `-l` output also has to have the newlines converted to null first, but this seems to work: `rg -l head * | tr '\n' '\0' | xargs -0 rg qt`

---

_Comment by @BurntSushi on 2017-09-24 00:00_

@ylluminate You can do that with the `--null` flag. So `rg -l --null head * | xargs -0 rg qt` should work.

---

_Comment by @davideuler on 2025-05-26 13:23_

In case you need the multiple keyword match to implement an AND logic within a line in file. I've implemented a version to support the feature.

https://github.com/davideuler/ripgrep

Sometimes you need to find lines that contain several specific words, but the order or exact positioning of these words doesn't matter. The `--and` flag is designed for this purpose. It allows you to specify a set of space-separated keywords, and only lines containing *all* of those keywords will be matched.

**Syntax:**

```
rg --and "keyword1 keyword2 keyword3" [path...]
```
or with a primary pattern:
```
rg --and keyword1 --and keyword2 --and keyword3 [path...]
```


---
