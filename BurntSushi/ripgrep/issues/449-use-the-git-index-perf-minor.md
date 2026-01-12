```yaml
number: 449
title: "use the git index [perf, minor?]"
type: issue
state: closed
author: mallochine
labels:
  - question
assignees: []
created_at: 2017-04-13T23:58:24Z
updated_at: 2017-05-08T22:07:00Z
url: https://github.com/BurntSushi/ripgrep/issues/449
synced_at: 2026-01-12T16:13:22Z
```

# use the git index [perf, minor?]

---

_@mallochine_

First I'd like to say how fricking impressed I was with ripgrep, especially from the very thorough technical overview in that blog post. 'ripgrep' inspired me to spend a lot more time looking at rust -- maybe there's something there that I didn't see before.

Anyway. On to the bug report. As mentioned in the blog post, "git grep has a special advantage: it does not need to traverse the directory hierarchy. It can discover the set of files to search straight from its git index". Soo....the optimization is to just eliminate git grep's special advantage? Hm.

As far as I can tell, this is within the scope of ripgrep's mission. After all, ripgrep is already reading from .gitignore, one of git's special files, as a _default_ behavior...

To bring some context into this bug, I noticed that for queries with no regex, git grep was still noticeably faster. For example: "git grep asdf -- '*.json'.

Anyway, cheers. ripgrep is really cool.

---

_Comment by @BurntSushi on 2017-04-14 00:08_

> First I'd like to say how fricking impressed I was with ripgrep, especially from the very thorough technical overview in that blog post. 'ripgrep' inspired me to spend a lot more time looking at rust -- maybe there's something there that I didn't see before.

<3

> Anyway. On to the bug report. As mentioned in the blog post, "git grep has a special advantage: it does not need to traverse the directory hierarchy. It can discover the set of files to search straight from its git index". Soo....the optimization is to just eliminate git grep's special advantage? Hm.

> As far as I can tell, this is within the scope of ripgrep's mission. After all, ripgrep is already reading from .gitignore, one of git's special files, as a default behavior...

That is potentially interesting, but there are problems. For example, ripgrep supports search specific `.ignore` files in addition to `.gitignore`. A `.ignore` file can whitelist things that are ignored in `.gitignore`:

```
[andrew@Cheetah ripgrep-449] echo test > foo
[andrew@Cheetah ripgrep-449] rg test
foo
1:test
[andrew@Cheetah ripgrep-449] echo foo > .gitignore
[andrew@Cheetah ripgrep-449] rg test
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.
[andrew@Cheetah ripgrep-449] echo '!foo' > .ignore
[andrew@Cheetah ripgrep-449] rg test
foo
1:test
```

Which effectively means that even if ripgrep could read git's index, then it would still need to traverse the directory contents to look for files that have been whitelisted by other `.ignore` files.

Of course, this doesn't mean the optimization is useless. I'm sure one can imagine cases where it might help. e.g., If you're searching MySQL's repo, then its `.gitignore` is huge and takes a good chunk of time to compile and match. So if we could rule that out by using git's index---even if we still have to traverse the directory and check for other `.ignore` files---there could still be a win there.

With that said, that sounds like a lot of implementation complexity and I'm not quite sure it's worth it.

> To bring some context into this bug, I noticed that for queries with no regex, git grep was still noticeably faster. For example: "git grep asdf -- '*.json'.

Weird. I have the opposite conclusion. This was in a checkout of Chromium (which is huge):

```
[andrew@Cheetah chromium.src] time git grep Openbox | wc -l
5

real    0m1.611s
user    0m2.313s
sys     0m2.723s
[andrew@Cheetah chromium.src] time rg Openbox | wc -l
5

real    0m0.366s
user    0m2.020s
sys     0m2.053s
```

Can you give more details about your benchmark? What did you run it on?

---

_Label `question` added by @BurntSushi on 2017-04-14 00:08_

---

_Comment by @mallochine on 2017-04-14 00:34_

Oh, no, ripgrep is faster than vanilla git grep almost surely. I felt that part of the reason is that you get to specify file types to look for. ripgrep -tjson was _way_ faster than just git grep

To level the playing field, I wondered whether git grep had an option to search for file types. Then I found "git grep asdf -- '*json'". That did the trick. git grep tiny bit faster than ripgrep. And I thought this was due to git's special advantage.

Let me just show the terminal output.

```
[main]$ time git grep '^message' -- '*.proto' | wc -l
4114
git grep '^message' -- '*.proto'  0.10s user 0.01s system 117% cpu 0.096 total
```
```
[main]$ time $HOME/bin/rg '^message' -g '*.proto' | wc -l
4114
$HOME/bin/rg '^message' -g '*.proto'  0.29s user 0.27s system 369% cpu 0.150 total
```

To give some context, the use-case here was, say, if I wanted to search for all the protobuf messages in the repository. I ran these grep commands on my company's repo (Nutanix), which is huge. There's a couple million lines of code in there.

---

_Comment by @mallochine on 2017-04-14 00:40_

oh...just noticed rg using a lot more system than git grep. If I level the playing field...(git grep uses 1 thread)

```
[main]$ time $HOME/bin/rg -j1 'message' -g '*.proto' | wc -l
5794
$HOME/bin/rg -j1 'message' -g '*.proto'  0.27s user 0.25s system 78% cpu 0.669 total
```
```
[main]$ time git grep --threads 1 'message' -- '*.proto' | wc -l
5794
git grep --threads 1 'message' -- '*.proto'  0.09s user 0.02s system 113% cpu 0.097 total
```
Yep, git grep got a threads feature recently. Must have been pretty recent...didn't even had it in my old version 2.12

---

_Comment by @BurntSushi on 2017-04-14 01:31_

Yeah, that actually makes sense. If your repo is huge and has some gitignore files in it, then ripgrep is going to spend a good chunk of time handling those.

Note that when I wrote the blog post, ripgrep used parallelism only for search. But now it parallelizes directory traversal itself, which permits gitignore matching to be parallelized as well. This let ripgrep recoup most of the losses it had against `git grep` (and then some, occasionally :-)).

If you're interested in Rust, the parallel iterator might be interesting to you. It doesn't use any unsafe code or any mutexes and Rust's type system guarantees no data races. It also builds every `gitignore` matcher exactly once and reuses them across all threads: https://github.com/BurntSushi/ripgrep/blob/71585f6d476265338e9ee4e924b4f43b02f2d233/ignore/src/walk.rs#L839-L1272

---

_Comment by @BurntSushi on 2017-05-08 22:07_

I'm going to close this. The idea is clever, and while it has the potential to make ripgrep faster in some repos, it seems like it would require a somewhat sizable addition to implementation complexity. I'd welcome folks to experiment with things here, and if a simple way to use git's index crops up (and also interacts with the rest of ripgrep's logic in a simple way, which is the harder part I think), then I'd be open to that.

---

_Closed by @BurntSushi on 2017-05-08 22:07_

---
