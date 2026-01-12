```yaml
number: 2713
title: Get help for a single flag
type: pull_request
state: closed
author: ltrzesniewski
labels: []
assignees: []
base: master
head: help-single-flag
created_at: 2024-01-13T20:12:15Z
updated_at: 2025-07-29T18:15:56Z
url: https://github.com/BurntSushi/ripgrep/pull/2713
synced_at: 2026-01-12T18:23:14Z
```

# Get help for a single flag

---

_@ltrzesniewski_

This is a little QoL feature I wanted to have for some time.

ripgrep's `--help` output is long, *really* long. It's inconvenient to look for a flag docs in that huge blob of text. This PR adds a feature which lets you output the verbose help of a single flag.

We may discuss about the details, but I had to make a few choices, and I hope the behavior I ended up with is fine. I'll be happy to improve it though if needed.

Specifically, now you can use `-h` or `--help` along with a *single* other argument, such as `E`, `-E`, `encoding`, `--encoding`, `no-encoding`, `--no-encoding`, you get the idea. Any two combinations in these two sets will get you the same result: `E -h` is the same as `--help --encoding`. Note that `-h` still gets you the *verbose* help of the flag, as it didn't seem useful to me to output the short help of a single flag.

The idea is that I can start with:

```
$ rg -h
```

Read the short help, then press the <kbd>up</kbd> key and just add a `p` (for instance) to the previous line:

```
$ rg -h p
```

Which outputs:

```
ripgrep 14.1.0 (rev 648a65f197)
Andrew Gallant <jamslam@gmail.com>

-p, --pretty
    This is a convenience alias for --color=always --heading --line-number.
    This flag is useful when you still want pretty output even if you're
    piping ripgrep to another program or file. For example: rg -p foo |
    less -R.
```

Also note that currently, if you add a non-existing flag name to `-h`/`--help`, it will behave as before and print the whole help. Would you rather have it print an error message instead?

---

_Comment by @BurntSushi on 2024-01-14 13:11_

I could have sworn someone proposed this before (and not too long ago), but the main problem this general idea is that it changes how `-h/--help` works in ways that I think are sub-optimal. In particular, I very specifically make ripgrep behave such that if there is a `-h` or a `--help` _anywhere_, then it stops everything it's doing and dumps the help output. That help output should be very easy to provoke, and showing only a subset of output because `--help` was placed before a flag would be pretty confusing IMO. And in particular, I'm not aware of others tools doing the same thing. This means that I don't believe its conventional. (And ripgrep is kind of already pushing the boundary of conventions here by showing different output for `-h` and `--help`.)

> ripgrep's `--help` output is long, _really_ long. It's inconvenient to look for a flag docs in that huge blob of text.

I acknowledge that this is a problem. But I haven't seen any exploration of alternatives. For example, I open `rg --help` in a pager, e.g., `rg --help | less`. And whether in that mode or in `man rg`, I use the pager's search functionality to find the flag I'm looking for. From your description of the workflow enabled by this PR, it doesn't seem all that different? And the benefit of my workflow is that it composes better with existing tools that folks using ripgrep are likely to know how to use (a pager).

---

_Comment by @ltrzesniewski on 2024-01-14 13:33_

This still stops everything else when you add `-h` or `--help` anywhere. This changes the behavior only when there are exactly 2 arguments, but yes, after implementing this I realized the convention breaking aspect could be a problem, and I understand this is probably a bad idea in the end.

What would you say about accepting an optional value for `-h`/`--help`? This would make `rg -h p` or `rg -h=p` print the help for `pretty`, but `rg -h -p` would continue to print the full help. This way, it wouldn't change the meaning of other flags when `-h`/`--help` is specified, so it would remain conventional behavior.

I know I can use a pager, but in my opinion, that's not very convenient. The workflow from this PR gets you to the text you want faster.

---

_Comment by @BurntSushi on 2024-01-14 13:44_

That doesn't really work either. Consider `rg foo` and then you rewrite it as `rg -h foo`. Since `foo` is invalid, I feel like the ideal behavior would be to report `foo` as invalid. But that changes the existing behavior (which I legitimately use today) where it just shows you the full help output. So... for an invalid flag, you could just show the full help output (with maybe a warning at the top? but it seems easy to miss), but that seems like a little bit of a bummer. And then of course, if you are searching for `rg ignore-case` and then do `rg -h ignore-case`, there really is no choice other than to show the help output only for the `--ignore-case` flag.

> I know I can use a pager, but in my opinion, that's not very convenient. The workflow from this PR gets you to the text you want faster.

I think the exact opposite personally...

I'm not sure I'm going to budge on changing the behavior of `-h/--help`. One thing I am maybe more open to is another flag that does what you want here. But that might negatively impact your workflow enough as to not be worth it.

---

_Comment by @ltrzesniewski on 2024-01-14 13:57_

Why would you rewrite `rg foo` to `rg -h foo` instead of `rg foo -h` by just appending the `-h` to the existing command? And do you think many other people would add a `-h` flag in the middle?

I suppose a warning at the bottom could be added (since you wouldn't see it at the top), but I'd say that getting the full help text already tells you that the value is not valid, even if there's no warning.

I'm not sure that adding another flag to the already long list would be a good idea, especially since `-H` is already assigned. The goal here is to reduce typing after all.


---

_Comment by @BurntSushi on 2024-01-14 14:14_

> Why would you rewrite `rg foo` to `rg -h foo` instead of `rg foo -h` by just appending the `-h` to the existing command? And do you think many other people would add a `-h` flag in the middle?

Because I might be trying to add a flag between `rg` and `foo` and realize that I've forgotten the name or the semantics or whatever. So I just still a `-h` in there. The point is that I can put it _anywhere_. This really was an intentional design on my part.

---

_Comment by @ltrzesniewski on 2024-01-14 14:24_

Ok, I understand. Let me know if you change your mind or if you get a better idea, I'll be happy to implement it.

---

_Closed by @ltrzesniewski on 2024-01-14 14:24_

---

_Comment by @gcflymoto on 2024-01-14 15:06_

How about `rg --describe -p --encoding` ?? I.e., whatever options are given describe all of them. This is a poor man's version of asking ChatGPT to explain what a particular ripgrep command line performs. 

---

_Comment by @BurntSushi on 2024-01-14 15:40_

That's roughly what I meant above with a new flag. But it isn't really compatible with the workflow described I think. The idea is that you go from `-h` to `--help flag`. Although I guess even then you need to type out another flag name anyway.

---

_Comment by @ltrzesniewski on 2024-01-14 16:37_

Yes, this PR only printed the help for a single flag because it handled the special case of 2 command-line arguments, but printing the help of several flags in a single command wouldn't be a problem to implement.


---

_Comment by @ltrzesniewski on 2025-07-29 17:35_

@BurntSushi since you're preparing a new release, I thought again about this PR, and had an idea:

What would you think about *having* to use the `=` separator on the `-h` flag in order to get help about a single flag? e.g. `rg -h=p` would output help about the `-p` flag only, while `rg -h p` would continue to work like today.

I realize this would still be unconventional, but it wouldn't introduce a new flag, and it wouldn't break the existing behavior. I also now believe the initial proposal wasn't very good, but this new idea seems much more reliable, as you wouldn't type `-h=` unintentionally. 

---

_Comment by @BurntSushi on 2025-07-29 17:43_

As a purely practical matter, `lexopt` (the crate ripgrep uses for CLI arg parsing) doesn't seem to let you differentiate between `-hp` and `-h=p`. i.e., `lexopt` abstracts over whether the `=` is present or not. With that said, I don't think that's a deal breaker. Having `-hp` work differently from today is probably a change I could live with.

With that said, I'm still not overly keen on adding this as a feature to support at all. I really just think folks should pipe the output into a pager or use `man rg`, and then use the pager's search feature to get to where you want. Alternatively, if you _really_ want this workflow, I think the `--help` and `-h` output is structured enough that you could write a pretty simple wrapper script to parse what you want and print that.

---

_Comment by @ltrzesniewski on 2025-07-29 18:15_

Just so you know, I was suggesting this feature from the viewpoint of a Windows user, where there's no good built-in pager for instance, so this would've been more useful than on Linux.

But I understand if you'd rather not have this, especially if there would have been issues with `lexopt`. Thanks for having considered it. 

---
