```yaml
number: 2607
title: Prevent empty search pattern to match every line in every file
type: issue
state: closed
author: hnorkowski
labels:
  - wontfix
assignees: []
created_at: 2023-09-11T09:34:38Z
updated_at: 2023-09-11T15:54:20Z
url: https://github.com/BurntSushi/ripgrep/issues/2607
synced_at: 2026-01-12T16:13:24Z
```

# Prevent empty search pattern to match every line in every file

---

_@hnorkowski_

#### Option to disable searching when given an empty pattern

When using ripgrep with an empty pattern (e.g. `rg ""`) it matches every line in every file. This was unexpected behavior for me but `grep` behaves the same (e.g. `grep --recursive "" .`). However there are use-cases where this behavior is problematic: 

I am trying to use ripgrep with skim to search a code base with a preview. There is an [example](https://github.com/lotabout/skim#preview-window) on their github page that uses `ag` aka. [the silver searcher](https://github.com/ggreer/the_silver_searcher). I tried to use `rg` instead of `ag` like this: `sk --ansi -i -c 'rg --color always --line-number "{}"' --preview "preview.sh {}"`. This works, however in contrast to `ag` `rg` starts listing every line of every file in the code base because before the user of `sk` inputs anything `""` is provided to `rg` as pattern. This can generate a LOT of overhead depending on the code base. Therefore it would be cool if `rg ""` would no produce results or offer an option to disable it.


---

_Comment by @BurntSushi on 2023-09-11 11:38_

> When using ripgrep with an empty pattern (e.g. `rg ""`) it matches every line in every file. This was unexpected behavior for me but `grep` behaves the same (e.g. `grep --recursive "" .`).

Yes, this is correct behavior. The empty regex matches the empty string and every string (including the empty string) contains the empty string. Just like every set is a superset of the empty set.

See also: https://github.com/rust-lang/regex/discussions/896

> however in contrast to ag rg starts listing every line of every file in the code base because before the user of sk inputs anything "" is provided to rg as pattern.

IMO, the way `ag` behaves is a bug:

```
$ echo bar | ag ''
ERR: Error: No query. What do you want to search for?
$ echo bar | rg ''
bar
$ echo bar | grep ''
bar
$ echo bar | ack ''
bar
```

It's not like PCRE doesn't support searching for empty patterns:

```
$ echo bar | ag '(?:)'
bar
```

IMO, this request is basically an instance of [this xkcd](https://xkcd.com/1172/).

I also think this is just generally a poor interaction pattern with tools like skim (and also fzf). I agree that starting a search with an exhaustive search for the empty pattern is bad UX, but that doesn't mean we should change the meaning of the empty pattern. It means we shouldn't start searches with the empty pattern. Instead, either start without searching at all (probably a harder interaction pattern to implement) or start it with a regex that can never match. In the latter case, you can use `rg 'a^b'` for the current release. In the next release, `rg '[a&&b]'` will work.

Once you have that, it's not hard to write a wrapper to do a translation for you:

```
$ echo bar | rg ''
bar
$ cat /tmp/rg-no-empty-pattern
#!/bin/bash

args=()
for arg; do
  if [ "$arg" = "" ]; then
    args=("${args[@]}" "a^b")
  else
    args=("${args[@]}" "$arg")
  fi
done
exec rg "${args[@]}"
$ echo bar | /tmp/rg-no-empty-pattern ''
$
```


---

_Closed by @BurntSushi on 2023-09-11 11:38_

---

_Label `wontfix` added by @BurntSushi on 2023-09-11 11:38_

---

_Comment by @hnorkowski on 2023-09-11 15:47_

Yes, currently I am using a wrapper script but its not a very portable solution

> IMO, this request is basically an instance of [this xkcd](https://xkcd.com/1172/)

Yes it is. That's why I suggested to make it an option: Just a parameter called `--skip-empty-pattern` or something.

Would you be willing to accept a PR that would add such an option?

---

_Comment by @BurntSushi on 2023-09-11 15:54_

No, sorry. ripgrep already has way too many flags, and if I accepted every request such as yours, it would grow to an unimaginable number. You'll need to use a wrapper script like the one I gave you or change the interaction pattern.

---
