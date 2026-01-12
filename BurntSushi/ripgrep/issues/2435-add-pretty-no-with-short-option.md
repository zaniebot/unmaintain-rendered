```yaml
number: 2435
title: add --pretty=no (with short option)
type: issue
state: closed
author: nkiesel
labels:
  - enhancement
assignees: []
created_at: 2023-02-28T21:35:53Z
updated_at: 2023-03-02T02:02:21Z
url: https://github.com/BurntSushi/ripgrep/issues/2435
synced_at: 2026-01-12T16:13:24Z
```

# add --pretty=no (with short option)

---

_@nkiesel_

#### Describe your feature request

In nearly all cases where I pipe ripgrep output, I send it to less or bat, and thus want to use --pretty. I thus would prefer to make that the default (using ripgrep configuration), and only overwrite this in cases where I send it to some other consumer.  Thus, I would like to have a "-P' or so option which would negate the pretty default, so that "rg xxx | less" would work like "rg -p xxx | less", and "rg -P xxx | less" would work like the current behavior.


---

_Comment by @BurntSushi on 2023-02-28 21:38_

I'd be open to a pull request that added a `--no-pretty` flag to override `--pretty`. It should not have a short flag. (And certainly cannot be `-P`, as `-P` is short for `--pcre2`.)

---

_Label `enhancement` added by @BurntSushi on 2023-02-28 21:38_

---

_Comment by @nkiesel on 2023-03-01 19:34_

Yes, `-P` is clearly wrong. But why not a short flag? I would have to type this in for e.g `rg --no-pretty foo | sed ...` If I make `--pretty` my default.

---

_Comment by @nkiesel on 2023-03-01 19:38_

Other idea: `git grep` automatically uses a pager for longer output to terminal. With that, `rg -p foo | less -RF` could be shortened to `rg foo`. Was that ever considered?

---

_Comment by @BurntSushi on 2023-03-01 19:51_

Because ripgrep has almost used every lowercase and uppercase letter for short flags already. I was a bit too relaxed with handing them out in the beginning, but now I've flipped to the other extreme: to add a new short flag requires compelling reasoning. Your use case merits filling in a gap, but it does not rise to the level of "compelling enough for a short flag."

And yes, there are issues on the tracker about adding a pager. Generally rejected due to cross platform concerns and the fact that you can do it on your own with an alias/function/wrapper script.

---

_Comment by @nkiesel on 2023-03-02 02:02_

I settled for 
```zsh
rg () {
	if [[ -t 1 ]]
	then
		command rg -p "$@" | less -RF
	else
		command rg "$@"
	fi
}
```

---

_Closed by @nkiesel on 2023-03-02 02:02_

---
