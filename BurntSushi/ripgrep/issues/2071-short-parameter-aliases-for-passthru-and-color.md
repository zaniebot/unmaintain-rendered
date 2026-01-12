```yaml
number: 2071
title: Short parameter aliases for --passthru and --color never
type: issue
state: closed
author: God-damnit-all
labels: []
assignees: []
created_at: 2021-11-15T16:30:30Z
updated_at: 2021-11-15T18:54:00Z
url: https://github.com/BurntSushi/ripgrep/issues/2071
synced_at: 2026-01-12T16:13:24Z
```

# Short parameter aliases for --passthru and --color never

---

_@God-damnit-all_

--passthru and `--color never` are my two most commonly used parameters (though rarely together). It would be nice if they had parameter aliases.

For --passthru, I suggest making it `-p` and taking the alias away from --pretty in the process, since I'm fairly sure both --passthru and --pcre2 (`-P`) see much higher usage.

For `--color never`, maybe `-#`? Since # is often associated with comments, removing color from the output makes sense to me.

Though, if taking `-p` away from --pretty isn't an option, I'd much rather have `-#` given to --passthru.

---

_Comment by @BurntSushi on 2021-11-15 16:37_

I think in order to move forward here, we really need to look at use cases. Can you say more about why you want short aliases for these other than frequency? For example, I very rarely use `--color never` or `--passthru`, so just talking about frequency doesn't quite cut it for me. What leads you to using those flags so often? By elaborating here, you might teach me a new workflow. Or we might find an alternative workflow that avoids stressing the length of the flags.

The situation with short flags is regrettable. Running out of real estate there is not something I had anticipated early on, and then it just kind of snuck up on me. That means that previously allocated short flags (with `--pretty` being potentially one example) would be unlikely to get the same treatment in the present. But I would need a _really_ compelling reason to completely change the behavior of an existing flag. That's pretty close to a non-starter as I suspect there is no reason compelling enough in this case.

---

_Comment by @God-damnit-all on 2021-11-15 17:06_

> I think in order to move forward here, we really need to look at use cases. Can you say more about why you want short aliases for these other than frequency? For example, I very rarely use `--color never` or `--passthru`, so just talking about frequency doesn't quite cut it for me. What leads you to using those flags so often? By elaborating here, you might teach me a new workflow. Or we might find an alternative workflow that avoids stressing the length of the flags.
> 
> The situation with short flags is regrettable. Running out of real estate there is not something I had anticipated early on, and then it just kind of snuck up on me. That means that previously allocated short flags (with `--pretty` being potentially one example) would be unlikely to get the same treatment in the present. But I would need a _really_ compelling reason to completely change the behavior of an existing flag. That's pretty close to a non-starter as I suspect there is no reason compelling enough in this case.

For `--color never`, that's more something I use when remoting in, since the color doesn't always display properly.

For passthru, say I want to get quick results for help. `rg --help | rg 'capture'` for instance. That tells me there's functionality for capture groups in ripgrep, but I don't really get the full section I wanted. So I need to do a passthru on that and go to where I see things highlighted.

In any case, I would like to see `-#` given to `--passthru` at least, since it's very handy for any situation you want something highlighted but you need the full output.

---

_Comment by @BurntSushi on 2021-11-15 17:17_

> For passthru, say I want to get quick results for help. `rg --help | rg 'capture'` for instance. That tells me there's functionality for capture groups in ripgrep, but I don't really get the full section I wanted. So I need to do a passthru on that and go to where I see things highlighted.
> 
> In any case, I would like to see `-#` given to `--passthru` at least, since it's very handy for any situation you want something highlighted but you need the full output.

For this, I usually use `-C50` or something, and adjust as needed. `--passthru` is an interesting option, but if the corpus is large and matches are few, then it's not really practical.

> For `--color never`, that's more something I use when remoting in, since the color doesn't always display properly.

In this case, I would suggest `alias rg="rg --color never"`. But more generally, you should be able to make coloring work. I ssh into remote machines all the time, and I have coloring working everywhere AFAIK.

---

_Comment by @God-damnit-all on 2021-11-15 17:37_

> `--passthru` is an interesting option, but if the corpus is large and matches are few, then it's not really practical.

In that case, I could do `rg --help | rg --color always --passthru 'project' | less -R` and read the output that way.

---

_Comment by @BurntSushi on 2021-11-15 18:19_

> In that case, in say, PowerShell, I can do `rg --help | rg --color always --passthru 'project' | Out-Host -Paging` (though PowerShell's pager isn't that great).

Not quite. The key bit you're missing here is _if the corpus is large_. Say you're searching a file with millions of lines and the first and only match is on line 10,000,000. You aren't going to want to scroll to that in a pager, yet, that is exactly what you'll have to do if you use the `--passthru` flag. But if you use `-C50`, then it will only spit out the 50 lines before and after each match. `-C50` is shorter and scales better than `--passthru`.

---

_Comment by @God-damnit-all on 2021-11-15 18:22_

> Not quite. The key bit you're missing here is _if the corpus is large_. Say you're searching a file with millions of lines and the first and only match is on line 10,000,000. You aren't going to want to scroll to that in a pager, yet, that is exactly what you'll have to do if you use the `--passthru` flag. But if you use `-C50`, then it will only spit out the 50 lines before and after each match. `-C50` is shorter and scales better than `--passthru`.

I'll have to use that more, but most of the time I'd still prefer having the full output whenever the corpus isn't too large. That does give me an idea though, What if `-C?` or `-C#` could be an alias for --passthru?

---

_Comment by @BurntSushi on 2021-11-15 18:50_

That's in theory possible, but it seems a bit... obtuse to me personally.

To be clear, this isn't necessarily about me trying to convince you per se. This is about meeting a high bar to not only use up precious real estate, but to potentially assign a short alias that doesn't make a lot of sense _just_ because it's available.

---

_Comment by @God-damnit-all on 2021-11-15 18:54_

Well, alright.

---

_Closed by @God-damnit-all on 2021-11-15 18:54_

---
