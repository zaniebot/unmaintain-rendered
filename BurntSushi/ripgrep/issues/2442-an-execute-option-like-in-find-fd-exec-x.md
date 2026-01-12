```yaml
number: 2442
title: "an 'execute' option like in find/fd (--exec/-x)"
type: issue
state: closed
author: marcotrosi
labels:
  - wontfix
assignees: []
created_at: 2023-03-03T15:43:30Z
updated_at: 2023-03-03T16:27:59Z
url: https://github.com/BurntSushi/ripgrep/issues/2442
synced_at: 2026-01-12T16:13:24Z
```

# an 'execute' option like in find/fd (--exec/-x)

---

_@marcotrosi_

#### Describe your feature request

I would like to have an 'execute' option like in find/fd (--exec/-x) to run a command for every file that contains a matching or non-matching pattern.
I know this can be solved in the shell but it's annoying.

Just to be clear, the pattern shall very normally match the content, and not the filename. For that we have find/fd.
But the execution of a command shall be on the files that contain that pattern.
e.g.
`rg 'some.*pattern' --exec mv {} subfolder/`


---

_Comment by @BurntSushi on 2023-03-03 15:48_

I think this is beyond the scope of what I want to be in ripgrep. I myself avoid using things like `--exec` in tools that have them today, because it's basically another little DSL you have to understand that is embedded inside your shell. So I actually find it far _more_ annoying than just using the shell or something like `xargs`.

---

_Closed by @BurntSushi on 2023-03-03 15:48_

---

_Label `wontfix` added by @BurntSushi on 2023-03-03 15:48_

---

_Comment by @mazunki on 2023-03-03 16:10_

While I personally avoid `-x`/`-exec` myself, there are a few times where it's quite useful. Sometimes we don't have the luxury of using pipes, for one reason or another; and sometimes the pipe can affect performance quite a bit. Also, doing proper piping requires some boilerplate.

Compare
`sudo rg 'pattern' -x cp -t some/destination/ {}`
versus
`sudo rg -l -0 'pattern' | sudo xargs -0 cp -t some/destination`.

Half the point of using `ripgrep` and `fd`, for me at least, is that it takes away a lot of the boilerplate I would do with `grep` and `find` otherwise. Why is this in particular any different?

---

_Comment by @BurntSushi on 2023-03-03 16:25_

> Why is this in particular any different?

There are an unbounded number of ways that ripgrep can reduce or eliminate boiler plate. I can't accept and maintain all of them. Therefore, I must draw a line somewhere between "we do some things that reduce boiler plate" and "we avoid doing other things that reduce boiler plate." Where that line is drawn is not informed by some rigorous formal process that consistently follows a sequence of objective criteria. Loosely speaking, it is informed by, in no particular order:

1. My perception of how broad the utility of the feature is.
2. What work-arounds look like.
3. Implementation complexity.
4. How likely it is to beget more feature requests.

This just doesn't clear the bar for me. Sorry.

---

_Comment by @marcotrosi on 2023-03-03 16:27_

fair. It's your project and I'm happy you responded to the proposal. I can work with xargs too. Thanks, and thanks for ripgrep. Have a great day :-) 

---
