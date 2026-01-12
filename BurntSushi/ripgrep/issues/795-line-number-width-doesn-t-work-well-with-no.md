```yaml
number: 795
title: "--line-number-width doesn't work well with --no-heading"
type: issue
state: closed
author: timotheecour
labels:
  - bug
  - wontfix
assignees: []
created_at: 2018-02-13T09:49:44Z
updated_at: 2018-04-23T23:57:30Z
url: https://github.com/BurntSushi/ripgrep/issues/795
synced_at: 2026-01-12T16:13:22Z
```

# --line-number-width doesn't work well with --no-heading

---

_@timotheecour_

#### What version of ripgrep are you using?

ripgrep 0.8.0 (rev )
-SIMD -AVX

#### What operating system are you using ripgrep on?

osx


### what i'm observing:

rg --no-heading --line-number-width=10 packages

```
source/dubregistry/dbcontroller.d:       289:           m_packages.update(["_id": packId], ["$set": ["stats": stats]]);
source/dubregistry/dbcontroller.d:       345:                   auto res = m_packages.aggregate(["$group": group], ["$project": project]);
source/dubregistry/dbcontroller.d:       371:           foreach( bp; m_packages.find() ){
source/dubregistry/dbcontroller.d:       380:                           m_packages.update(["_id": p._id], ["$set": ["versions": newversions]]);
source/dubregistry/web.d:        46:            DbPackage[] m_packages;
source/dubregistry/web.d:        66:                    Package[] packages;
source/dubregistry/web.d:        77:            DbPackage[] packages;
```

### what i'd expect

that the line numbers would align WRT left margin (ie that they'd all lign up), not that they'd align WRT rightmost character of path (which has variable position due to different path lengths)


---

_Comment by @BurntSushi on 2018-02-13 12:12_

Sorry, but this will never work. The whole point of `--line-number-width` is to specify a fixed padding. There is no intelligent "alignment" going on. We can either let this bug stand or mark `--no-heading` as a conflicting flag with `--line-number-width`.

---

_Label `bug` added by @BurntSushi on 2018-02-13 12:12_

---

_Label `wontfix` added by @BurntSushi on 2018-02-13 12:12_

---

_Comment by @erikrw on 2018-02-13 12:15_

Perhaps I'm overlooking something, but could the padded line number be
printed before the file path on each line?

On Tue, Feb 13, 2018 at 1:12 PM, Andrew Gallant <notifications@github.com>
wrote:

> Sorry, but this will never work. The whole point of --line-number-width
> is to specify a fixed padding. There is no intelligent "alignment" going
> on. We can either let this bug stand or mark --no-heading as a
> conflicting flag with --line-number-width.
>
> —
> You are receiving this because you are subscribed to this thread.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/795#issuecomment-365249322>,
> or mute the thread
> <https://github.com/notifications/unsubscribe-auth/ABwFepYMXPeSewHv2OI-lpDFByyT27zGks5tUXxDgaJpZM4SDeeA>
> .
>


---

_Comment by @BurntSushi on 2018-02-13 12:22_

@erikrw It could, but it won't happen. That's not the standard grep output format.

---

_Comment by @BurntSushi on 2018-02-13 12:24_

I'm regretting ever adding this flag. I wish I considered this output format, because if I had, I would have never let this flag be a thing.

This is absolutely not something that ripgrep will ever fix because we are not going to get into the business of "aligning" output. I thought we could get away with something simple, but I didn't think carefully about the interaction with `--no-heading`. So this flag is either going to get removed in the next release or the flags get marked as conflicting (resulting in a ripgrep error). *sigh* But the `--no-heading` format is automatically enabled when ripgrep has its output redirected. This is a mess.

---

_Comment by @timotheecour on 2018-02-13 20:31_

i can live without this but what would be controversial about the following:

(requires monospace font, the | are aligned)

```
source/dubregistry/dbcontroller.d:371: | foreach( bp; m_packages.find() ){
source/dubregistry/dbcontroller.d:380: |   m_packages.update(["_id": p._id], newversions]]);
source/dubregistry/web.d:46:           | DbPackage[] m_packages;
```
NOTE: user specifies minimum distance between | and left margin, same as using printf padding:
`printf("|%-10s|", "filename+line+col");`


---

_Comment by @BurntSushi on 2018-02-13 21:26_

@timotheecour That doesn't align the line numbers, which I thought was the point of `--line-number-width`. Moreover, file paths are variable length, so what happens if you have a path like `x/y` in your case?

---

_Comment by @timotheecour on 2018-02-13 21:51_

> That doesn't align the line numbers, which I thought was the point of --line-number-width.

well maybe the flag could be renamed to something more general, like `--location-width` which would work with both just heading and no heading cases.

The intent i had in mind is to have aligned output in essentially 2 columns: one with file/line/col, the other with the data

> Moreover, file paths are variable length, so what happens if you have a path like x/y in your case?

not sure I understand that 2nd point, but the point is to align in 2 columns (analog to how sql output is aligned in columns, eg
https://cloud.google.com/bigquery/docs/reference/standard-sql/query-syntax)

```
< -------------------------------------> #specified (minimum) width in --location-width
source/dubregistry/dbcontroller.d:371: | foreach( bp; m_packages.find() ){
source/dubregistry/dbcontroller.d:380: |   m_packages.update(["_id": p._id], newversions]]);
source/dubregistry/web.d:46:           | DbPackage[] m_packages;
x/y:46:                                | DbPackage[] m_packages;
```


---

_Comment by @BurntSushi on 2018-02-13 21:58_

What value do you give to `--location-width` to get that output? What happens when the output includes a shorter file name? Or a longer one?

---

_Comment by @timotheecour on 2018-02-13 22:36_

> What happens when the output includes a shorter file name? Or a longer one?

the location of each `|` will be at max(left_hand_side.length, locationWidth) so results in rg default behavior when locationWidth=0
(where left_hand_side could contain file, line, column depending on options)

> What value do you give to --location-width to get that output? 

user responsability to give a big enough one; if too small, not a big deal (see above point).

Unless `rg` has an option where it stores all results in memory before printing to stdout, in which case it could compute locationWidth to be the max of all left_hand_side. Note, this would be also useful for sorting outputs before dumping to stdout. very useful in the common case where number of generated output lines is small enough to fit well in memory (could always have a --max-stored-lines to avoid consuming too much memory)


Again, that's just an option...


---

_Comment by @BurntSushi on 2018-02-13 23:23_

@timotheecour Thanks for explaining! But yeah basically ripgrep isn't in the business of aligning things since it won't buffer all output into memory.

I basically am now of the opinion that these flags are too error prone and don't fit well into ripgrep.

---

_Comment by @timotheecour on 2018-02-14 00:29_

sure. I'm really curious though (and sorry if it was already explained, if it is, thanks for the link) but what is downside of buffering output as an option? (orthogonal to this issue)
eg:
```
rg --max-stored-lines=1000 options...
```
each thread would collect output into its own thread local buffer (up to max-stored-lines/num_threads, and maybe optionally dump to stdout if it exceeds buffer size)
then main thread merges each TLS buffer, does final processing (eg sort or align or whatever depending on other options) and dumps to stdout.

For all the queries I've ever done, output size is always fits in memory (and if it exceeds passed in buffer size it can just output to stdout)

enabling use cases:
* --sort-files with parallelism enabled
* --location-width (as discussed)
* multiline search (for simple cases at least) (cf https://github.com/BurntSushi/ripgrep/issues/360)

---

_Comment by @BurntSushi on 2018-02-14 00:34_

> but what is downside of buffering output as an option?

Because I don't think every possible thing should be an option. I care about UX and maintainability, and part of that is managing the number of flags ripgrep supports.

It needs to be OK for me to say No to some things. If you care about output format this much, then it would be better to parse the results and re-display them in a format of your liking. That way, you can make the trade offs that make sense for your specific usage. (There is a ticket for enabling ripgrep to output results in a more structured format.)

---

_Comment by @timotheecour on 2018-02-14 00:37_

> There is a ticket for enabling ripgrep to output results in a more structured format.

sructured output format would indeed solve all the use cases, ok thx (https://github.com/BurntSushi/ripgrep/issues/359  + other i guess)

---

_Comment by @BurntSushi on 2018-02-14 01:04_

Yes, that's the one!

On Feb 13, 2018 7:37 PM, "Timothee Cour" <notifications@github.com> wrote:

> There is a ticket for enabling ripgrep to output results in a more
> structured format.
>
> sructured output format would indeed solve all the use cases, ok thx (
> #244 <https://github.com/BurntSushi/ripgrep/issues/244> ?)
>
> —
> You are receiving this because you commented.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/795#issuecomment-365455102>,
> or mute the thread
> <https://github.com/notifications/unsubscribe-auth/AAb34kmQI6b2xhap7AzUo27Fk-xMEKpbks5tUirggaJpZM4SDeeA>
> .
>


---

_Closed by @BurntSushi on 2018-04-23 23:57_

---
