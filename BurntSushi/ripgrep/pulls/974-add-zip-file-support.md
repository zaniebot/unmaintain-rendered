```yaml
number: 974
title: add zip file support
type: pull_request
state: closed
author: derekmorr
labels: []
assignees: []
base: master
head: zip
created_at: 2018-07-09T20:51:27Z
updated_at: 2023-01-17T23:10:48Z
url: https://github.com/BurntSushi/ripgrep/pull/974
synced_at: 2026-01-12T18:23:13Z
```

# add zip file support

---

_@derekmorr_

This adds support for searching in zip files for the `-z` option. It partially addresses #918.

I'm not sure if we should use the `-p` or `-c` option to unzip. I'd appreciate feedback on that.

---

_Comment by @okdana on 2018-07-09 21:05_

`-p` definitely seems preferable to `-c`. It's probably also worth mentioning that there are at least two or three different implementations of the `unzip` tool — but AFAIK they all support `-p`, so that's probably fine

---

_Comment by @derekmorr on 2018-07-09 22:00_

Do I need to update the ignore crate and add the unzip package to travis? I assume unzip is pretty common, but it might not be a bad idea to explicitly list it.

---

_Comment by @BurntSushi on 2018-07-10 11:08_

@derekmorr Thanks for this PR! Unfortunately, I'm not sure it's a good fit. Namely, as I understand it, `zip` is an _archive format_, which means it may contain multiple files. As far as I can see, this PR provides an implementation that always searches zip archives as if they contained a single file. I'm not convinced that this is actually what we want, and in particular, I worry that it could be quite confusing to end users.

Interestingly, #918 is slightly different than this. Namely, the file extensions used (`tar` and `gz`) decouple the notion of archive from compression, such that decompression can be done without unpacking the archive and the archive can be searched directly. In this instance, there is no confusion presented to the end user because the archive isn't being unpacked; it's just searching a `.tar` like any other file.

I'm honestly not sure how to proceed here. My inclination is to draw line and say that ripgrep should not be responsible for unpacking archive formats.

> Do I need to update the ignore crate and add the unzip package to travis? 

@derekmorr I'm not sure what you mean with respect to the `ignore` crate, but we should make sure the test will run on Windows CI. Unfortunately, Cargo's output won't tell you anything here, and as far as it's concerned, the test ran and passed even though for all intents and purposes, it might have been skipped. It might be wise to remove the skipping logic and see what happens.

---

_Comment by @derekmorr on 2018-07-10 20:32_

I see what you mean. It is possible that a user would only have a single file in a zip archive, but it's also possible they wouldn't. 

FWIW, I think there are valid use cases for wanting to search inside the contents of a .tar.gz or a zip file. I've certainly seen plenty of questions about how to do that online. There is a tool that greps inside the full content of zip archives -- zipgrep -- but it sounds like you don't want to incorporate that functionality into ripgrep. That's a reasonable position to take.

My question about the ignore crate was that `DEFAULT_TYPES` doesn't have an entry for "zip" and does it need to have one?


---

_Comment by @BurntSushi on 2018-07-10 20:56_

Yeah I think I'd rather not have ripgrep get in the business of reading archives, at least, not at this point.

> My question about the ignore crate was that DEFAULT_TYPES doesn't have an entry for "zip" and does it need to have one?

It's not necessary, no. But probably a good idea.

---

_Comment by @derekmorr on 2018-07-11 13:20_

Ok, fair enough. I'll close this PR.

---

_Closed by @derekmorr on 2018-07-11 13:20_

---

_Comment by @edsu on 2022-01-20 21:05_

I'm just noting that this actually would have been useful to me just now.

---

_Comment by @danie-dejager on 2023-01-17 19:45_

It would've been useful if ripgrep can search inside a zip and list the files and string matched in the zip but that would mean that ripgrep would need to extract the data from the zip first. a helper tool like `rga` does that using rg already.
https://github.com/phiresky/ripgrep-all


---

_Comment by @garoto on 2023-01-17 23:07_

Workarounds and third-party tools for zip searching has been posted on the issue tracker many times. This is a four-year-old closed PR. Avoid commenting on closed PRs.

---
