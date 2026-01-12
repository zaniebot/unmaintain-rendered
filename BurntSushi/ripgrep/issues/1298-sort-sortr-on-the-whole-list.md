```yaml
number: 1298
title: "--sort/--sortr on the whole list"
type: issue
state: closed
author: unhammer
labels:
  - wontfix
assignees: []
created_at: 2019-06-12T13:32:23Z
updated_at: 2019-06-17T07:39:48Z
url: https://github.com/BurntSushi/ripgrep/issues/1298
synced_at: 2026-01-12T16:13:23Z
```

# --sort/--sortr on the whole list

---

_@unhammer_

#### What version of ripgrep are you using?

ripgrep 11.0.1 (rev 1f1cd9b467)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

dpkg -i the github release

#### What operating system are you using ripgrep on?

Xubuntu 18.10

#### Describe your feature request

It seems like `--sort/sortr` only have an effect on files within the same folders. I'd like the whole result list sorted. I realise this means having to wait for the whole result list to come, but it's probably faster than my current workaround `rg "$@" | while IFS=: read  -r   f m; do  printf '%s:%s\n' "$(stat --printf '%Y\t%n' "$f")" "$m"; done | sort -nr | cut -f2-`. 

The reason I want the whole list sorted is that I work in a big git repo where there are some files that are nearly never changed and which are fairly irrelevant most of the time (but not always), but I keep getting their hits at the top of my list when I use [https://oremacs.com/2017/08/04/ripgrep/](counsel-rg) to get a grip on the project.



---

_Comment by @BurntSushi on 2019-06-16 22:27_

I think `--sort path` already behaves as you're requesting. `--sortr path` will not, since as you've noticed, files are sorted in each directory. For sorting in lexicographic ascending order, this works fine, but can produce something that isn't quite the descending order when sorting in reverse.

I don't think it's worth supporting this feature. I'd recommend either continuing your work-around, or even better, use a custom `.ignore` file with the `--ignore-file` flag to filter out files you don't want to search most of the time (but not always).

---

_Closed by @BurntSushi on 2019-06-16 22:27_

---

_Label `wontfix` added by @BurntSushi on 2019-06-16 22:27_

---

_Comment by @unhammer on 2019-06-17 07:39_

Fair enough :)

---
