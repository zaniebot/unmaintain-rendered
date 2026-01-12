```yaml
number: 2874
title: Add .svelte file type to default type list
type: issue
state: closed
author: josbra-attensi
labels:
  - rollup
assignees: []
created_at: 2024-08-15T13:30:10Z
updated_at: 2025-09-20T01:08:35Z
url: https://github.com/BurntSushi/ripgrep/issues/2874
synced_at: 2026-01-12T16:13:25Z
```

# Add .svelte file type to default type list

---

_@josbra-attensi_

#### Describe your feature request

Svelte is popular enough with it's own file types .svelte and .svelte.ts (the latter would still be picked up by searching `-tts` but would probably make sense to add to the `.svelte` glob.

I would like to be able to simply run `rg "my query" -tsvelte` and not add the `--type-add` flag each time. I think this makes sense because it is now an established file type, and not my own made up filetype.

Maybe add to the existing list:

```
svelte: *.svelte, *.svelte.ts
```
or similar? :)


---

_Comment by @BurntSushi on 2025-07-26 15:51_

I can't really make heads or tails of whether this is appropriate or not. Are `.svelte.ts` files actually svelte? It seems like they are primarily TypeScript files and thus could be surprising to appear in a search restricted to svelte.

> I would like to be able to simply run rg "my query" -tsvelte and not add the --type-add flag each time.

This could easily be flipped around: if #2909 were merged, then what would folks do if they _didn't_ want `.svelte.ts` files to appear in a `-tsvelte` search? I understand that's always possible, but I don't know how to judge whether it's common enough to be annoying.

(You can add `--type-add` in an alias or in a ripgrep config file.)

---

_Label `question` added by @BurntSushi on 2025-07-26 15:51_

---

_Comment by @josbra-attensi on 2025-07-28 11:22_

I can understand that, from a purely filetype perspective then it wouldn't make sense as this is just a typescript file, but you can use runes within them. So I assume the Svelte compiler does treat `.svelte.ts` files differently to `.ts` files to get that reactivity we would normally only be able to use in `.svelte` files.

Svelte developers move a lot of state management that as of Svelte 5, primarily relies on runes, to `x.svelte.ts` files. So I personally believe from a UX perspective it would make sense to combine them. Though I understand ripgrep is a tool that isn't in particular about any framework or technology, I know that if you add `-tts` you will also get `.tsx` results which are an extended typescript format.

So I defer it to your better judgement, though thank you for the last tip - I was unaware and think that doing that could be good enough. When I made this issue, I did it mostly because my intuition when using rg was to think that it would work for `.svelte.ts` simply by covering `.svelte`.

---

_Comment by @BurntSushi on 2025-07-28 11:57_

Yeah basically, I just don't have the domain expertise to adjudicate this. The fact that Svelte stuff can be "inside" `.svelte.ts` makes sense, but that's true of a ton of stuff. For example, you can find Markdown inside of `*.rs` files. There's all sorts of examples of file types that can "contain" other file types.

I absolutely could be wrong here.

In particular, one thing that strikes me as different here is that `svelte` does actually appear in the file extension. Maybe that should move the needle?

So how about we do this provisionally. If this change ends up annoying a bunch of people, then I'll very likely back it out.

---

_Label `question` removed by @BurntSushi on 2025-07-28 12:01_

---

_Label `rollup` added by @BurntSushi on 2025-07-28 12:01_

---

_Comment by @josbra-attensi on 2025-07-28 12:08_

That sounds sensible, thanks for picking this one up. I'll have zero feelings hurt if you decide to revert it. I do think the fact that it's in the filename and the Svelte compiler physically processes `.svelte.ts` files is a good rationale for it.

---

_Closed by @BurntSushi on 2025-09-20 01:08_

---
