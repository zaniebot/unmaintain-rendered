```yaml
number: 880
title: add --no-column flag to override --column (and --vimgrep)
type: issue
state: closed
author: kenorb
labels:
  - enhancement
  - help wanted
assignees: []
created_at: 2018-04-11T00:17:01Z
updated_at: 2018-04-23T23:30:03Z
url: https://github.com/BurntSushi/ripgrep/issues/880
synced_at: 2026-01-12T16:13:22Z
```

# add --no-column flag to override --column (and --vimgrep)

---

_@kenorb_

ripgrep should have a `--no-column` flag as a way to toggle the `--column` flag. Like `--no-line-number`, this should also override `--vimgrep`.

---

_Comment by @BurntSushi on 2018-04-11 00:21_

Your output shows *column numbers*, not line numbers.

Could you please explain what problem you're trying to solve? `--vimgrep` is meant specifically for parsing by external programs, particularly vim. Using it in conjunction with `--no-filename --no-line-number` seems strange to me.

---

_Label `question` added by @BurntSushi on 2018-04-11 00:22_

---

_Comment by @kenorb on 2018-04-11 00:28_

I just need the plain output without any extras. Basically what it reads "a line with more than one match will be printed more than once", so I can count the duplicates.

---

_Comment by @BurntSushi on 2018-04-11 00:31_

Your sentences seem to be in conflict with each other. "Plain output" to me is "print every line that matches." `rg pattern file -N` will do that when at a tty.

If you want each match to be on its own line, then `rg pattern file -N -o` will do that. The `-o/--only-matching` flag is the same one found in GNU grep.

---

_Comment by @kenorb on 2018-04-11 00:31_

It's basically for the workaround to do AND grepping which I haven't found, so:

    rg --vimgrep --no-filename --no-line-number "foo|bar|buzz" file.txt | sort | uniq -c | sort -nr | grep -w 3

should show me only the lines with all 3 patterns present, but because of extra column numbers, it doesn't work. So for further workaround, I'll have to filter out the output using `awk` and/or `cut` by removing these column numbers.

---

_Comment by @BurntSushi on 2018-04-11 00:32_

@kenorb You're overthinking it. Look at the output of `rg 'foo|bar|buzz' file.txt | cat`. ripgrep toggles slightly different options depending on whether you're piping or printing to a tty.

---

_Comment by @BurntSushi on 2018-04-11 00:33_

See also: `rg 'foo|bar|buzz' file.txt -o | cat`.

---

_Comment by @kenorb on 2018-04-11 00:34_

I need basically `foo&bar&buzz`-like pattern, so I need line with all patterns present at the same time, which would be `foo bar buzz` in this case. So having 3 the same lines and counting the duplicates could help me to find those lines.

---

_Comment by @BurntSushi on 2018-04-11 00:36_

@kenorb Then I have no idea what you're doing. `rg foo | rg bar | rg buzz` prints only the lines that contain `foo`, `bar` and `buzz`. You should be solving this problem the same way you'd solve it with grep.

---

_Comment by @kenorb on 2018-04-11 00:36_

I know, but I was looking for some method to run `rg` only once for simplicity, but it didn't end well.

---

_Comment by @BurntSushi on 2018-04-11 00:37_

Then why in the world did you file a bug report about `--vimgrep`? I am very confused.

---

_Comment by @BurntSushi on 2018-04-11 00:37_

If that's what your after, then I guess you probably want #875. (It's not clear whether it will ever happen.)

---

_Comment by @kenorb on 2018-04-11 00:38_

I would expect that `--no-line-number` would suppress column numbers as well. Otherwise, I'd expect another param like `--no-column-number` which is not there.

---

_Comment by @BurntSushi on 2018-04-11 00:41_

`--no-line-number` does suppress line numbers, even when paired with `--vimgrep`. So your expectation is satisfied already.

A request for a `--no-column` flag (to match the existing `--column` flag) is reasonable, but merely for consistency sake.

As far as I can tell, your use of `--vimgrep` here makes no sense at all. So if you don't want column numbers, then don't use it.

---

_Label `enhancement` added by @BurntSushi on 2018-04-11 00:42_

---

_Label `help wanted` added by @BurntSushi on 2018-04-11 00:42_

---

_Label `question` removed by @BurntSushi on 2018-04-11 00:42_

---

_Renamed from "-N doesn't suppress line numbers completely when used with --vimgrep" to "add --no-column flag to override --column (and --vimgrep)" by @BurntSushi on 2018-04-11 00:42_

---

_Comment by @kenorb on 2018-04-11 00:43_

So I would expect this output:

```
$ rg --vimgrep --no-filename --no-line-number "foo|bar|buzz" file.txt  | cut -d: -f2-
foo
foo bar
foo bar
foo bar buzz
foo bar buzz
foo bar buzz
```

but without `cut` which won't work anyway when there are multiple-digits. So if `--no-line-number` can't support suppressing column numbers, then I'd expect `--no-column-number`.

---

_Comment by @BurntSushi on 2018-04-11 00:43_

I've updated this issue to reflect the actual request.

---

_Comment by @BurntSushi on 2018-04-11 00:46_

@kenorb OK, I'm finding this conversation to be frustrating. It's going in circles. I don't know where, how or why you're confused. If this is just a request for something as simple as `--no-column`, then fine. Let's leave it at that. But your examples with `--vimgrep` and your questions about "and" grepping combined with all this talk about `cut`, `awk` and showing patterns that actually do "or" grepping has made this issue way harder to understand than it needs to be.

---

_Comment by @okdana on 2018-04-11 00:50_

Seems like the reason @kenorb is interested in `--vimgrep` is just that it causes `rg` to print one copy of a line for each time it matches the pattern. I don't think (?) that functionality is exposed through any other option. So his request could be fulfilled either by adding an option to disable column numbers, which could be combined with `--vimgrep`, or by adding a new option that just enables that duplicate-printing behaviour. I think that's it, anyway?

---

_Comment by @BurntSushi on 2018-04-11 00:52_

@okdana Thanks for lending some clarity to this issue! I forgot that `--vimgrep` emitted the entire line, which is of course different than what `-o` does.

---

_Closed by @BurntSushi on 2018-04-23 23:30_

---
