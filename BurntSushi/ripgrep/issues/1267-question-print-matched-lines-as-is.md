```yaml
number: 1267
title: "Question: Print matched lines as-is"
type: issue
state: closed
author: sheharyarn
labels: []
assignees: []
created_at: 2019-04-23T22:58:22Z
updated_at: 2024-02-04T13:40:05Z
url: https://github.com/BurntSushi/ripgrep/issues/1267
synced_at: 2026-01-12T16:13:23Z
```

# Question: Print matched lines as-is

---

_@sheharyarn_


#### Describe your question, feature request, or bug.

Is there an option to tell `ripgrep` to print the matched lines as-is? I've gone over the man but  didn't find anything that worked. I tried all options with `--color` but I still lose the original text color.

An example use-case is with [`hub`](https://hub.github.com) where I want to keep the original text color/formatting being output by the program:

<img width="704" alt="image" src="https://user-images.githubusercontent.com/2355108/56620731-c55f1a00-65de-11e9-897a-4231d73f8eb4.png">


#### What version of ripgrep are you using?

```
ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

```
brew install ripgrep
```

#### What operating system are you using ripgrep on?

```
macOS Mojave 10.14.4
```


---

_Comment by @BurntSushi on 2019-04-23 23:19_

This isn't a problem with ripgrep. ripgrep doesn't strip coloring from the output of hub. The more likely thing is that hub detects that it isn't printing to a tty, and therefore suppresses color output on its own. You thus need to figure out how to tell hub to keep its color enabled forcefully.

---

_Closed by @BurntSushi on 2019-04-23 23:19_

---

_Comment by @sheharyarn on 2019-04-23 23:34_

My bad, you're right. This fixed it for me:

```
hub issue -a me --color=always | rg -i term
```

Thanks!

---

_Comment by @funatsufumiya on 2024-01-21 07:31_

Thanks to this question, I could achieve coloring control 'as-is' with `fd`.

results as FYI:

- `$ fd rs --color=always | rg 'examples'`
<img width="478" alt="スクリーンショット 2024-01-21 16 28 40" src="https://github.com/BurntSushi/ripgrep/assets/3406260/cabe99e7-fa5d-4b4d-baf5-7e29fad758ec">

- `$ fd rs --color=always | rg 'examples' --color=never`
<img width="581" alt="スクリーンショット 2024-01-21 16 28 56" src="https://github.com/BurntSushi/ripgrep/assets/3406260/6d15729f-2eb4-47aa-b50a-c5b96109ac6b">



---

_Comment by @lmn451 on 2024-02-04 12:34_

Hey all,

Looks like it same issue, but in my case is far as I understand git don`t care where to printed.

<img width="518" alt="image" src="https://github.com/BurntSushi/ripgrep/assets/14910239/35e92494-47a8-4004-90e5-73bf80f5c7ba">

I expect removed/added lines to preserve color to the end of line

@BurntSushi , can you  please advise on this?  



---

_Comment by @BurntSushi on 2024-02-04 13:21_

You expected color to be preserved from the output of `git diff`, and you showed a screenshot of... color being preserved. There are things colored other than `test`. So I don't see the problem.

---

_Comment by @BurntSushi on 2024-02-04 13:22_

Maybe you're referring to the things that appear after `test`? If so, there's nothing to be done about that. You can't just arbitrarily compose ANSI color streams and expect the output to be sensible. Sorry.

---

_Comment by @lmn451 on 2024-02-04 13:30_


Oops, looks like I might not have explained my question very well earlier – sorry about that! 😅 What I'm trying to do is highlight certain words in my terminal without messing up the other colors that are already there from a `git diff`. It's a bit surprising that we don't seem to have a straightforward way to do this yet. Do you happen to know if there's a tool or a trick that could help me out? I'm not too fussed about the technical details; I just want something that works smoothly. Thanks a bunch for your help!

---

_Comment by @BurntSushi on 2024-02-04 13:39_

Nope. Sorry. I'm not the right person to ask. I'm not a terminal expert. I know how to get it to emit colors and that's pretty much it. You could write an ANSI interpreter to manage the stream of ANSI color codes where you could intercept resets and replace them with instructions to "reset back to what the colors were before" instead of just "reset back to no colors." But I don't see how to do that in a generic way because you don't know which colors were emitted by each application. So like, even hypothetically, I don't see a way to do it. But like I said, I'm not the right person to ask, so I wouldn't an interpret a "can't be done" from me with 100% certainty.

---
