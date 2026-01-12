```yaml
number: 208
title: Custom highlighting
type: issue
state: closed
author: stej
labels:
  - question
assignees: []
created_at: 2016-11-01T10:06:35Z
updated_at: 2021-01-22T17:16:53Z
url: https://github.com/BurntSushi/ripgrep/issues/208
synced_at: 2026-01-12T16:13:21Z
```

# Custom highlighting

---

_@stej_

I use ripgrep for searching log files which works great. I like regexes a lot, so searching that supports them and is fast is always very helpful.

My question is - did you consider adding some more highlighting features so that I can highlight some parts of the text but not to include them in search pattern?
Example:
I search log files for batches with id 255
```
rg -F 255
7715:2016-10-01 05:22:00.683 DEBUG 41 PublishEvent CreateBatch{"BatchId":"b768eaf88faa4b....","Id":255 ...}
7824:2016-10-01 05:22:07.853 DEBUG 257 PublishEvent ProcessBatch{"UserId":98345,"Id":255 ...}
7888:2016-10-01 05:22:12.731 DEBUG 108 PublishEvent DeleteBatch{"Reason":"external","Id":255 ...}
```
It's ok to highlight the **255** value, but I'd like to get the event names (CreateBatch, ProcessBatch, ..) highlighted. This would help for quick eye scanning through the results.

---

_Comment by @BurntSushi on 2016-11-01 11:04_

I'm not particularly inclined to add another feature for this. It seems too complex to support more regexes that don't actually impact search results.

Is there any particular reason why you can't use pipes? For example, this works:

```
$ rg -F 255 issues-208 | rg '(Create|Process|Delete)Batch'
1:7715:2016-10-01 05:22:00.683 DEBUG 41 PublishEvent CreateBatch{"BatchId":"b768eaf88faa4b....","Id":255 ...}
2:7824:2016-10-01 05:22:07.853 DEBUG 257 PublishEvent ProcessBatch{"UserId":98345,"Id":255 ...}
3:7888:2016-10-01 05:22:12.731 DEBUG 108 PublishEvent DeleteBatch{"Reason":"external","Id":255 ...}
```


---

_Label `question` added by @BurntSushi on 2016-11-01 11:05_

---

_Comment by @stej on 2016-11-01 20:42_

Honestly I didn't know about the pipeing functionality. I use ripgrep on windows where the command line tools have been quite useless. But this might help for sure in the future.

Back to the question - your proposed solution might work for some cases. But when I consider that the possible event types are hudreds, then it's not usable. 
If I use regex `"PublishEvent\s\w+"`, then both words will be highlighed.
I was considering also lookbehind (regex like `"(?<=PublishEvent\s)\w+"`), but according to https://doc.rust-lang.org/regex/regex/index.html#syntax it's not supported.

This is not critical feature, but might be interesting for some people. Anyway, I understand that implementing this feature is not very idiomatic.


---

_Comment by @BurntSushi on 2016-11-01 20:50_

Yeah, I'm going to close this. I can kind of understand why you'd want this, but it seems like too much trouble.

I will say that one possibly simpler approach would be to support an option to indicate a specific capture group to highlight instead of the full regex. So for example, you could do `rg 'PublishEvent\s(\w+)' --highlight 1` and it would only color the text that matched `(\w+)`.


---

_Closed by @BurntSushi on 2016-11-01 20:50_

---

_Comment by @texastoland on 2020-04-23 08:08_

I was just searching if there's a way to integrate with [`bat`](https://github.com/sharkdp/bat) for the same  reason.

---

_Comment by @dufferzafar on 2021-01-22 17:09_

@BurntSushi Re-iterating what @texastoland has suggested above. Has an integration with `bat` been suggested before? 

Since rg & bat are both rust and available as a library this is doable from a strictly technical stand point. The default behaviour of ripgrep is to cluster matches per file. Having the source also highlighted would be great. 

[`delta`](https://github.com/dandavison/delta) is an great example of using `bat`'s functionality.

I currently use `fzf` + `bat` + `ripgrep` like so:

```bash
rgf()
{
    RG_PREFIX="rg --column --line-number --no-heading --color=always --smart-case "
    FZF_DEFAULT_COMMAND="$RG_PREFIX ''" \
    fzf --ansi --phony \
        --bind "change:reload:$RG_PREFIX {q} || true" \
        --preview-window=top:30% --preview "preview.sh -v {} | rg --pretty --colors 'match:bg:red' --colors 'match:fg:white' --no-line-number --ignore-case --context 3 {q}"
}
```

Where `preview.sh` is: https://github.com/mitsuhiko/dotfiles/blob/master/helpers/preview.sh

---

_Comment by @BurntSushi on 2021-01-22 17:16_

> Has an integration with `bat` been suggested before?

No, and I don't see it happening. At least not within ripgrep.

---
