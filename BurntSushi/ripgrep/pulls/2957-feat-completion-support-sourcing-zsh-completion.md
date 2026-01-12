```yaml
number: 2957
title: "feat(completion): support sourcing zsh completion dynamically"
type: pull_request
state: merged
author: vegerot
labels: []
assignees: []
merged: true
base: master
head: vegerot/source_completion
created_at: 2024-12-30T23:36:56Z
updated_at: 2025-01-03T04:29:44Z
url: https://github.com/BurntSushi/ripgrep/pull/2957
synced_at: 2026-01-12T18:23:14Z
```

# feat(completion): support sourcing zsh completion dynamically

---

_@vegerot_

Summary:
Previously, you needed to save the completion script to a file and then
source it.  Now, you can dynamically source completions in zsh by
running

```zsh
$ source <(rg --generate complete-zsh)
```

Test plan:
1. Run `source <(rg --generate complete-zsh)`
2. Run `rg --generate=complete-zs<TAB>`

Before this commit, you would get an error after step 1.
After this commit, it should work as expected.

Closes #2956


---

_Review comment by @BurntSushi on `FAQ.md`:129 on 2024-12-31 00:06_

Why are we suggesting this method? I'm okay with supporting it as long as it doesn't have any costs to doing so, but I don't see a good reason to specifically highlight it in the docs.

---

_@BurntSushi reviewed on 2024-12-31 00:06_

r? @okdana 

---

_@vegerot reviewed on 2024-12-31 00:22_

---

_Review comment by @vegerot on `FAQ.md`:129 on 2024-12-31 00:22_

Personal preference.  I personally like doing it this way because it makes installation easier.  Since the generation is fast, I don't need to think about installing `_rg` and can just put in my zshrc

```zsh
  if type rg > /dev/null; then
	  eval "$(rg --generate=complete-zsh)"
  fi
```

I assume other people prefer it this way too, which is why projects like [fzf](https://github.com/junegunn/fzf?tab=readme-ov-file#setting-up-shell-integration) suggest it by default.  That is why I suggested it in the doc.

If you disagree, then I can remove this bit 🙂 

---

_@vegerot reviewed on 2024-12-31 00:26_

---

_Review comment by @vegerot on `crates/core/flags/complete/rg.zsh`:438 on 2024-12-31 00:26_

I originally wrote this as 

```zsh
if [[ "$funcstack[1]" = "_rg" ]]; then
  _rg "$@"
elif type compdef >/dev/null; then
    compdef _rg rg
fi
```

But `ci/test-complete` didn't like that.  ~~I feel like the test is wrong~~ (nvm I am wrong!), because realistically you will either have `compdef` or will run the completion as `_rg`.  I stared at the test for a few seconds and didn't want to spend more time digging into it.  Instead, I worked around it by making it


```zsh
if [[ "$funcstack[1]" = "_rg" ]]; then
  _rg "$@"
elif type compdef >/dev/null; then
    compdef _rg rg
else
  _rg "$@"
fi
```

To be clear, this code is fine, but feels redundant 🤷🏼‍♀️

---

_@okdana reviewed on 2024-12-31 00:42_

---

_Review comment by @okdana on `crates/core/flags/complete/rg.zsh`:438 on 2024-12-31 00:42_

the test script isn't wrong. the function is designed to be sourced by it without requiring anything from the completion system. we could have the script load `compdef` but it would still do the wrong thing with that check. i would suggest this as more idiomatic (and faster) than the `type` method:

```zsh
if [[ $funcstack[1] == _rg ]] || (( ! $+functions[compdef] )); then
```

---

_@okdana reviewed on 2024-12-31 00:45_

---

_Review comment by @okdana on `FAQ.md`:122 on 2024-12-31 00:45_

obv this is a pre-existing issue but these instructions don't really do anything as written. i would suggest we add something like 'then add `$dir` to your `fpath`'. if you'd rather not do this here i can make another pr

---

_@okdana reviewed on 2024-12-31 00:51_

---

_Review comment by @okdana on `FAQ.md`:129 on 2024-12-31 00:51_

i don't know for sure but i'd guess that other projects suggest this `source` method just because it's self-contained and it works regardless of whatever else might be in the user's zsh profile. creating a personal directory for completion functions and adding it to `fpath` is more idiomatic, more extensible, and (as the comment here mentions) faster at start-up, but harder to explain. i don't have a strong opinion on whether they should both be mentioned in the faq

eta: another benefit is that the function is self-updating. maybe that's a more compelling reason

---

_@vegerot reviewed on 2024-12-31 00:57_

---

_Review comment by @vegerot on `FAQ.md`:122 on 2024-12-31 00:57_

I can do that.

...orr, we could suggest the easier way I'm adding in this commit.

> If you installed ripgrep with a package manager, you should have shell completions automatically (Brew users see [here](https://docs.brew.sh/Shell-Completion))
> However, if completions aren't working, then add to your `zshrc`
> ```zsh
> source <(rg --generate=complete-zsh)
> ```

I like the idea of hoping their package manager does the right thing, but if doesn't then suggesting a simple one-liner instead of a multi-step process of creating directories, files, and adding things to your `fpath`.  On my machine (granted, it's over $1k) it only adds 4ms of overhead which is acceptable imo.

---

_Review comment by @vegerot on `crates/core/flags/complete/rg.zsh`:438 on 2024-12-31 01:01_

Thanks! Done

---

_@vegerot reviewed on 2024-12-31 01:01_

---

_@vegerot reviewed on 2024-12-31 01:57_

---

_Review comment by @vegerot on `FAQ.md`:129 on 2024-12-31 01:57_

Based on how difficult it is to explain (see other thread), I think it makes more sense to show new users the easiest method by default, and if they want the more "idiomatic" / performant (slightly) option we can show it as an alternative.  The FAQ is going to be read by people who may not want to create directories, new files, and update their fpath.

---

_Review comment by @vegerot on `FAQ.md`:129 on 2024-12-31 01:58_

@okdana @BurntSushi how about I remove the FAQ patch, and we can discuss how to improve the FAQ in a new issue? (continued from https://github.com/BurntSushi/ripgrep/pull/2957#discussion_r1899864739 )

---

_@vegerot reviewed on 2024-12-31 01:58_

---

_Comment by @BurntSushi on 2024-12-31 13:11_

I've left it in the FAQ and fixed up the wording by adding an appropriate caveat emptor. I guess since other projects have found it useful to suggest this method it's probably not too big of a deal for ripgrep to do it too. But the text now makes it clear that the "generate and source" approach is slower. 4ms might not seem like much, and indeed, I cannot perceive a 4ms lag, but if you accrue 10 of those kinds of things, it starts to pile up into something that is noticeable.

---

_Merged by @BurntSushi on 2024-12-31 13:23_

---

_Closed by @BurntSushi on 2024-12-31 13:23_

---

_Comment by @vegerot on 2025-01-03 04:01_

@BurntSushi I'm using a self-built version now, but jooc when will the next release be so others can use it?

---

_Comment by @BurntSushi on 2025-01-03 04:29_

I don't know, sorry. See: https://github.com/BurntSushi/ripgrep/blob/master/FAQ.md#when-is-the-next-release

---
