```yaml
number: 1078
title: "-M should show a preview of the line  if the line is too long"
type: issue
state: closed
author: dylan-chong
labels:
  - enhancement
  - question
assignees: []
created_at: 2018-10-05T04:08:36Z
updated_at: 2024-10-22T22:31:32Z
url: https://github.com/BurntSushi/ripgrep/issues/1078
synced_at: 2026-01-12T16:13:22Z
```

# -M should show a preview of the line  if the line is too long

---

_@dylan-chong_

#### What version of ripgrep are you using?

ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)


#### How did you install ripgrep?

Homebrew


#### What operating system are you using ripgrep on?

Mac high sierra 10.13.6

#### Describe your question, feature request, or bug.

When I specify a line length with -M, and a line is too long, I cannot see
anything about what the line is. The silver searcher does what I believe to be
a more useful behaviour, which is show a preview of the line for the first lot
of characters. This is much more useful because I can tell whether this line is
generated JavaScript or not, especially if I specify the line length of -M to
be something higher like 100.

Please change the behaviour to be more like the silver searcher

```
# Current behaviour
> \rg -M 15 Hello
ios/ToDoLater.xcodeproj/project.pbxproj
696:[Omitted long line with 1 matches]

# Ideal behaviour
> \ag -W 15 Hello
ios/ToDoLater.xcodeproj/project.pbxproj
696:                    productName  [...]

# What the line looks like without the column limit
> \rg Hello
ios/ToDoLater.xcodeproj/project.pbxproj
696:                    productName = "Hello World";
```



---

_Comment by @BurntSushi on 2018-10-05 10:36_

I don't think I'm willing to change the existing behavior here.

The problem I have with ag's behavior is that it makes it much less obvious that a line has been truncated. I guess this is more of a display/UX problem that could be solved as opposed to a problem with the idea itself.

If we do this, it will probably have to be behind a flag, e.g., `--max-columns-preview`.

---

_Label `enhancement` added by @BurntSushi on 2018-10-05 10:36_

---

_Label `question` added by @BurntSushi on 2018-10-05 10:36_

---

_Comment by @okdana on 2018-10-05 11:14_

For whatever it might be worth, i found ripgrep's `-M` surprising and unhelpful when i first switched over as well. Not just because my brain's been tainted by `ag` use (though that may be part of it), but because the proposed (`ag`-like) behaviour seems consistent with the way the other `--max-*` options work; `--max-count` just stops after that many matches, and `--max-depth` just stops after that many directory levels, so i expected `--max-columns` to just stop after that many columns.

I know how much you care about BC, and i don't often run into files with stupidly long lines anyway, so i never mentioned it — but i ended up just removing `-M` from my alias because, whenever i did encounter them, having it there was more irritating than not.

I don't currently have any strong opinions on the UX/BC stuff other than that, but there's another perspective for you, in any case

---

_Comment by @BurntSushi on 2018-10-05 11:19_

@okdana Thanks! If `--max-columns-preview` became a thing, do you think you would add `-M {n} --max-columns-preview` to your alias?

---

_Comment by @okdana on 2018-10-05 11:31_

For sure, yeah

---

_Comment by @BenSandeen on 2018-11-02 21:18_

I don't know if you want this here are or as its own separate issue, but it would be nice if ripgrep had flags analogous to `-A` and `-B` for truncating characters before and after a match in extremely long lines (e.g., minified javascript files).

I find myself often enough having to scroll for tens of seconds, searching for the match.  I know there's an easy enough workaround by piping the output into `less` if you're using a bash shell (thank goodness for the Windows Subsystem for Linux!), but I'd have no idea how to do that in a Windows command prompt or powershell.

I think it would be nice to have ripgrep truncate long lines by default.  If, say, 500 characters before and after the match were retained, that would still make it clear that there's a lot of stuff in the line without overwhelming the output.  This 500 character before and after default would, I hope, not artificially truncate long lines in normal files.

If this idea has support, I'd be happy to take a stab at implementing it (although it'd be nice to be pointed in the right direction).

---

_Comment by @BurntSushi on 2018-11-02 22:56_

@BenSandeen That sounds like a different feature than this. I'm not sure it's a good fit though.

I don't think any sort of trimming/truncation will ever happen by default though.

---

_Comment by @BurntSushi on 2019-04-12 17:24_

@okdana You mentioned that you liked how `ag` handled this, but I can't get `ag` to trim long lines at all. (I'm looking at it so that I can compare its behavior.) I don't see any relevant options other than `--print-long-lines`, but it implies that its long line limit is enabled by default (which the docs claim to be 2K). But I can't get it to work, even with a very long line (>8000 bytes).

... okay, looking at ag's source, I see why:

https://github.com/ggreer/the_silver_searcher/blob/965f71dcbc9d98da5347f1256f650f8b7dcf4625/src/options.h#L67

Ah okay, looking at `print.c`, I see there is a `width` option:

https://github.com/ggreer/the_silver_searcher/blob/965f71dcbc9d98da5347f1256f650f8b7dcf4625/src/print.c#L135-L137

Working backwards from that lead me to its `-W/--width` flag, which is not in its man page (where I was looking), but is in its `--help` output.

All right, so its behavior appears to be that it cuts off the line at the given number of bytes, and then prints a ` [...]` suffix.

Should ripgrep do the same thing? Or should it use something more conspicuous like it does today, e.g., `[Omitted remainder of long line]` or perhaps even better, `[Omitted remainder of long line with N additional matches]`. Although that's a bit wordy.

---

_Comment by @okdana on 2019-04-12 18:06_

Oh, yeah. I had to go back and check too, i honestly haven't touched `ag` in so long now.

I personally like the `[...]` thing, it's very brief and straight-forward. An indication of how many unprinted matches there are sounds like a good idea too, though. What about something in between, like `[... N more matches]`?

If you're concerned about conspicuousness you might also consider giving it its own colour/style thing (possibly configured by default?), e.g. `--colors=omit:fg:yellow`.

Just random thoughts though; i don't think i feel *super* strongly about it. My use case for this is preventing absurdly long lines (like minified JavaScript) from flooding my screen, so a few-dozen characters doesn't make too big a difference to me

---

_Comment by @BurntSushi on 2019-04-12 18:12_

I like `[... N more matches]`. :-) I'll start without a new color category, and we can add that in later if there's demand for it.

Actually doing this, I see why I did it the way I did it. It is... quite thorny to get this right with respect to coloring matches and multiline results.

---

_Closed by @BurntSushi on 2019-04-14 23:29_

---

_Comment by @AtomicNess123 on 2021-05-05 09:38_

How to get long lines printed? I am getting the "[Omitted long line with 1 matches]" result. Thanks!

---

_Comment by @BurntSushi on 2021-05-05 11:03_

You're getting that because that's what you asked for. It's not enabled by default.

---

_Comment by @AtomicNess123 on 2021-05-05 11:05_

Thanks.

---

_Comment by @logiota on 2024-10-22 22:31_

when I run 
rg  --max-columns-preview 12 'memory'

I get 

rg: memory: IO error for operation on memory: No such file or directory (os error 2)


---
