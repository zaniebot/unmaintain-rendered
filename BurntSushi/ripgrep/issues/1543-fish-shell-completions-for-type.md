```yaml
number: 1543
title: "fish-shell completions for `--type`"
type: issue
state: closed
author: thomcc
labels: []
assignees: []
created_at: 2020-04-06T07:14:44Z
updated_at: 2020-04-06T15:37:35Z
url: https://github.com/BurntSushi/ripgrep/issues/1543
synced_at: 2026-01-12T16:13:23Z
```

# fish-shell completions for `--type`

---

_@thomcc_

#### What version of ripgrep are you using?

```
ripgrep 12.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

Pretty sure it was `cargo install`, but I also downloaded the completion set from the releases.

#### What operating system are you using ripgrep on?

macOS 10.14.6. Also, using fish-shell version 3.0.2.

#### Describe your question, feature request, or bug.

Feature request: fish autocompletions for `--type` / `-t` argument. 

This can be done by appending the following to the generated fish completions. I added this for myself (because I have to constantly look up what the valid types are) and figured I'd upstream it rather than have it blown away each time I update ripgrep (and y'know, so that someone else can benefit).

```fish
complete -xc rg -n "__fish_use_subcommand" -s t -l type -a '(__rg_types)'

function __rg_types
    for item in (rg --type-list)
        if set -l match (string match -r '^(.*?)\:\s*(.*?)$' -- "$item")
            set -l name $match[2]
            set -l val $match[3]
            set -l spl (string split ', ' -- "$val")
            # If there are more than 4 example extensions take the
            # first 3 and end with ellipsis.
            if test (count $spl) -gt 4
                # Note: in fish indexing starts at 1 and ranges
                # are inclusive by default
                set val (string join ', ' -- $spl[1..3])
                set val "$val, ..."
            end
            echo "$name"\t"$val"
        end
    end
end
```

I'm not sure if you're interested in this. I could try and get the completions upstreamed into fish-shell/fish-shell instead if you aren't.

If you are interested, I can move this to a PR. That doesn't sound too bad (use https://docs.rs/clap/2.33.0/clap/struct.App.html#method.gen_completions_to and append this data to it. I guess I'd just put this as a raw string literal? Or would you prefer I include_str it or something -- my uncertainty here is part of why I didn't just submit a PR).

For reference, with this the completions on my machine look like:

<img width="996" alt="Screen Shot 2020-04-06 at 12 07 58 AM" src="https://user-images.githubusercontent.com/860665/78531966-4802ca00-779b-11ea-8c12-075b48477068.png">

---

_Comment by @BurntSushi on 2020-04-06 12:35_

I think I'd prefer that this sort of thing get upstreamed to fish shell completions. I think it would be hard for me to maintain this myself. If it wasn't for clap's autogeneration of completion files, I probably wouldn't offer them at all. (Other than zsh, which are hand written, tested and dogfooded by me.)

---

_Comment by @thomcc on 2020-04-06 15:37_

Okay, that's fine. Assuming that lands I'll try to keep it up to date with rg releases.

---

_Closed by @thomcc on 2020-04-06 15:37_

---
