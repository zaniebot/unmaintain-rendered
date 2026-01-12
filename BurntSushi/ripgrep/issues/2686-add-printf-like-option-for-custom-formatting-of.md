```yaml
number: 2686
title: Add printf-like option for custom formatting of filepaths
type: issue
state: closed
author: cohml
labels:
  - wontfix
assignees: []
created_at: 2023-12-13T18:36:08Z
updated_at: 2023-12-13T20:20:24Z
url: https://github.com/BurntSushi/ripgrep/issues/2686
synced_at: 2026-01-12T16:13:24Z
```

# Add printf-like option for custom formatting of filepaths

---

_@cohml_

When ripgrepping across multiple files, the path to the parent file is shown alongside each match. This is problematic when the filepaths get long. For example, when using absolute filepaths.

A nice addition would therefore be some way to customize the way that paths that are shown.

Probably the most useful example would be to replace whatever `$HOME` expands to with `~`. But one can imagine something like adding entries into `.ripgreprc` mapping commonly used paths to informative yet shorter strings, e.g.

```
/Users/me/ -> ~/
/Users/me/dev/my_project1/src/ -> mypj1/
/Users/me/dev/my_project1/src/ -> mypj2/
[...]
```

This would ensure that even when ripgrepping using absolute paths, the output won't eat up needless horizontal space. It would also allow users to tailor ripgrep to their bespoke formatting needs, e.g., #665.

---

_Comment by @BurntSushi on 2023-12-13 18:44_

#665 is already done FWIW, and it needed a lot more than a simple `printf` option like you're asking for here. Actually, I'm not sure exactly what you're asking for here. The issue title mentions `printf`, but your actual request seems to suggest wanting something that permits hard-coding paths to replace with shorter strings.

With all that said, I do think you made the specific _problem_ you care about pretty clear, but I think there's already a pretty standard solution to that:

```
$ rg is_accel /home/andrew/rust/ripgrep/
/home/andrew/rust/ripgrep/crates/regex/src/literal.rs
73:        if re.is_accelerated() {
$ (cd /home/andrew/rust/ripgrep && rg is_accel)
crates/regex/src/literal.rs
73:        if re.is_accelerated() {
```

That is, ripgrep already lets you shorten the file paths it prints. You just need to change ripgrep's current working directory and provide a relative path.

---

_Closed by @BurntSushi on 2023-12-13 18:44_

---

_Label `wontfix` added by @BurntSushi on 2023-12-13 18:44_

---

_Comment by @cohml on 2023-12-13 19:16_

Thanks for the prompt reply.

> #665 is already done FWIW

Sorry, guess I missed that part. I skimmed the issue and it sounded like you weren't into the proposal.

> Actually, I'm not sure exactly what you're asking for here. The issue title mentions `printf`, but your actual request seems to suggest wanting something that permits hard-coding paths to replace with shorter strings.

Sorry for the confusion. I initially had `find`'s `-printf` option in mind when I drafted the title, which can do exactly what I said but also so much more. After drafting the body I debated changing the title too, but just rolled with it.

My `.ripgreprc` proposal does involve hardcoding paths, but I left the door open to other ideas in case there was a more elegant solution.

> ripgrep already lets you shorten the file paths it prints. You just need to change ripgrep's current working directory and provide a relative path.

With unix tools, there is almost always a way. However, this approach will only work if you are executing ripgrep directly.

But what about if it runs inside a function that gets executed by keybinding? For example, [this amazing fzf + ripgrep example](https://github.com/junegunn/fzf/blob/master/ADVANCED.md#using-fzf-as-the-secondary-filter), which I have a couple variants of mapped to different keybindings.

That is just one specific example, but emblematic of a more general use case that would be solved by a dedicated argument or behavior configurable in `.ripgreprc`.

---

_Comment by @BurntSushi on 2023-12-13 19:27_

There's still a way even then if you feel strongly enough. You can use symlinks to shorten a path. Not the prettiest answer and has downsides of its own, but it's there if you really need it.

`find`'s printf flag is an _enormous_ surface area. I'm not too keen on adding that.

I think for fzf, it would make more sense for the thing doing the display to try to trim down the output when possible.

I understand the request, but this is the kind of scope increase that I'd prefer not to chase. It also doesn't help that this is exactly the kind of thing that begets more features. As soon as something like this added, folks are going to want to customize it in more ways. It's too heavy of a maintenance burden and the benefit isn't big enough to justify it IMO.

---

_Comment by @cohml on 2023-12-13 20:20_

Makes sense! Not the outcome I was hoping for but your justifications are totally reasonable. Thanks anyway for discussing it!

Should you ever change your mind, something similar is being considered for `fd` that perhaps you could check out for inspiration (see sharkdp/fd#533 and sharkdp/fd#1043). Admittedly this functionality is less out of scope for `fd` than `ripgrep`, but anyway, it's a precedent 😄

---
