```yaml
number: 1352
title: How to Show Line-Excerpt Near the Match?
type: issue
state: open
author: johnyradio
labels:
  - enhancement
  - question
assignees: []
created_at: 2019-08-17T19:39:27Z
updated_at: 2024-02-22T09:48:36Z
url: https://github.com/BurntSushi/ripgrep/issues/1352
synced_at: 2026-01-12T16:13:23Z
```

# How to Show Line-Excerpt Near the Match?

---

_@johnyradio_

#### version
% rg --version
ripgrep 11.0.2
-SIMD -AVX (compiled)
+SIMD -AVX (runtime)

#### install ripgrep?
pacman -S ripgrep

#### operating system
% hostnamectl 
  Operating System: Arch Linux
            Kernel: Linux 5.2.8-arch1-1-ARCH
      Architecture: x86-64

#### question

On very long lines, how can i return a limited number of characters before and after the match?
--max-columns-preview seems to preview the _beginning_ of the line, which may not contain the match, so that's less useful. 

i hoped combining -o with --max-columns-preview might give the desired result. Maybe that's a feature request?

thx!

---

_Renamed from "How to Show Excerpt Near the Match?" to "How to Show Line-Excerpt Near the Match?" by @johnyradio on 2019-08-17 19:41_

---

_Comment by @BurntSushi on 2019-08-18 20:17_

> On very long lines, how can i return a limited number of characters before and after the match?

You can't.

> Maybe that's a feature request?

Could you please provide a more complex specification for how this feature is supposed to work? If it helps, write it as if you were adding documentation for the new feature to the man page.

---

_Label `enhancement` added by @BurntSushi on 2019-08-18 20:17_

---

_Label `question` added by @BurntSushi on 2019-08-18 20:17_

---

_Comment by @johnyradio on 2019-08-19 15:27_

Use -o with --max-columns-preview to display the matching text in context. 

If the original text is:

Every year since 1938, the organization presents Stern Grove Festival, a free concert series, in Sigmund Stern Grove, a beautiful outdoor amphitheater located at 19th Avenue and Sloat Boulevard in San Francisco.


Then this
rg  -o -M 10 --max-columns-preview Grove

will return
und Stern Grove, a beauti

There might be more correct options than how i've done , but this shows the basic idea. 

---

_Comment by @BurntSushi on 2019-08-19 15:28_

Thanks, but that's not good enough. `-o -M10 --max-columns-preview` already has a defined behavior.

I don't see a way to do this without an additional flag.

---

_Comment by @borekb on 2020-01-24 12:04_

I'd also love this feature (minified JS in my case...), it's probably true that it cannot be done without an additional flag. I imagine having something like this in my `.ripgreprc`:

```
--max-columns=150
--max-columns-preview
--max-columns-preview-type=contextual
```

This hypothetical `--max-columns-preview-type` option would support two values:

- `beginning-of-line` (current behavior)
- `contextual`

---

_Comment by @rod-stuchi on 2020-06-19 00:34_

This would be a really wanted feature, like borekb said to look in minified JS files.

I think this is like Context, flags `-A, -B and -C` but instead of lines, will be applied in columns.

New flags (like context in lines) should be the way to go, imho.


---

_Comment by @BurntSushi on 2020-06-19 00:43_

Plus-1 comments aren't going to move this forward. A specification, as requested above, would be more helpful.

---

_Comment by @rod-stuchi on 2020-06-19 01:06_

ok fair, maybe this would help? Like a documentation for the new feature in `man rg`?

```
--after-context-column NUM
           Show NUM columns after each match in one line.

           This overrides the --context-column flag.

-- context-column NUM
           Show NUM columns before and after each match in one line. This is equivalent to providing both the
           --before-context-column and --after-context-column flags with the same value.

           This overrides both the -before-context-column and --after-context-column.

--before-context-column
           Show NUM columns before each match in one line.

           This overrides the --context-column flag.
```


As an example, please consider this piece of text:
```
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Pellentesque vel hendrerit est, vel interdum turpis. Proin pharetra nulla eu nulla aliquet, nec consequat felis bibendum. Vestibulum augue odio, eleifend non pellentesque ac, dapibus in urna. Donec at vehicula risus. Nullam consectetur vitae felis mollis fringilla. Ut tincidunt magna in enim tincidunt lacinia. Orci varius natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. Cras lectus dui, ullamcorper accumsan consectetur sed, porta sed justo. Etiam massa est, ornare commodo porttitor ut, bibendum quis mauris. Etiam eget laoreet ante. Fusce molestie leo vitae dolor dignissim gravida.
```

running
```console
rg --context-column 5 dolor
```
will return something like:
> 1: psum **dolor** sit 
> 1: itae **dolor** dign



---

_Comment by @kkocdko on 2023-12-09 18:22_

Here's an workaround, according to [this comment](https://github.com/BurntSushi/ripgrep/issues/2449#issuecomment-1464979702):

```sh
rgnc(){ rg -p --colors match:none -o ".{0,50}$1.{0,50}" | rg -C 9 "$1" ;}
rgnc "=require\("
```

But overlapping problem still exists. `--pcre2` and look-ahead maybe helpful.

And a more magical maybe with awk if anyone want, LOL.

```sh
~/misc/apps/mold -run cargo run -- --color always --vimgrep '_OMG_' | awk '{
    split($0,f,":");
    sub(/^([^:]+:)/,"",$0);
    sub(/^([^:]+:)/,"",$0);
    sub(/^([^:]+:)/,"",$0);
    print f[1]":"f[2]":"f[3]":"substr($0, substr(f[3], 5) - 2, 50);
}'
```

---

_Comment by @kkocdko on 2023-12-11 14:35_

My advice is to implement the `--max-columns-preview-before 10` to make `--max-columns-preview` starts printing from "10 chars before **first match**". Then we can use the existing `config.per_match` to output one line per match.

This simplifies the implementation a lot, and it's still acceptable for most use cases. @BurntSushi what you think about this design? If you agree, I'll continue to implement the #2683 , update docs and tests.

However I found that `config.per_match` field is not exposed directly to cli args, only when `--vimgrep`. Let's consider making a change to it?

https://github.com/BurntSushi/ripgrep/blob/56c7ad175ac1326e8d36a880061ca3b0e67ad5ea/crates/core/flags/hiargs.rs#L615-L617

---

_Comment by @ssendev on 2024-02-22 09:48_

I came here to request the same feature also dealing with minified js and imagined it working like https://github.com/BurntSushi/ripgrep/issues/1352#issuecomment-646379743 described
 
Here is an improved interim solution which seems pretty usable

```bash
rgnc(){ Q="$1"; shift; rg --pretty --colors match:none -o ".{0,50}$Q.{0,50}" "$@" | rg --passthru "$Q" ;}
```

---
