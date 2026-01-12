```yaml
number: 385
title: Add Max Filesize Option
type: pull_request
state: merged
author: tiehuis
labels: []
assignees: []
merged: true
base: master
head: master
created_at: 2017-02-28T05:07:58Z
updated_at: 2017-03-08T15:21:43Z
url: https://github.com/BurntSushi/ripgrep/pull/385
synced_at: 2026-01-12T18:23:12Z
```

# Add Max Filesize Option

---

_@tiehuis_

See #369.

There is now a build dependency on `regex`/ This is due to using it to validate the expect input format for the new option. I did seriously consider just doing this manually since it isn't very complicated but since there is a very similar function when actual parsing the arguments, I've opted for minimality.

---

_Renamed from "Max Filesize Option" to "Add Max Filesize Option" by @tiehuis on 2017-02-28 05:09_

---

_Review comment by @BurntSushi on `ignore/src/walk.rs` on 2017-02-28 12:02_

This function cannot accept a `Metadata` because that implies *every* file is stat'd regardless of whether a max filesize option is set or not. You need to move the metadata access to occur only after you know there is a file size filter.

---

_Review comment by @BurntSushi on `src/app.rs` on 2017-02-28 12:04_

`\d` is Unicode aware, which includes a lot more than `[0-9]`, so I think you want to change this to `[0-9]`.

---

_Review comment by @BurntSushi on `src/app.rs` on 2017-02-28 12:05_

I don't think you need the heredoc syntax either, since you aren't using any `"` inside the regex. `r"^([0-9]+)([KMG])?$"` will suffice.

---

_Review comment by @BurntSushi on `src/app.rs` on 2017-02-28 12:06_

You don't need this match. You can unwrap the `value` since it is guaranteed to match if the entire regex matched. e.g., `try!(caps[1].parse::<u64>().map_err(|err| err.to_string()))`.

---

_Review comment by @BurntSushi on `src/args.rs`:786 on 2017-02-28 12:07_

This duplication isn't acceptable, sorry.

Please just get rid of the clap validator. The error can be reported from here. That will also get rid of the build dep on regex.

---

_Review comment by @BurntSushi on `src/args.rs` on 2017-02-28 12:08_

See comments above about the regex itself and handling the captures after-the-fact.

---

_Review comment by @BurntSushi on `tests/tests.rs`:446 on 2017-02-28 12:09_

Can you add a test that checks the functionality of `--max-filesize` itself? I realize you have some unit tests, but checking that it works in an integration test will cover some important pieces of code, like the filesize parser and whether the limit is actually passed to the directory iterator.

---

_@BurntSushi requested changes on 2017-02-28 12:12_

@tiehuis Thanks! Overall this looks pretty good, but the duplication of the parsing routine ain't gunna fly. We really don't need the clap validator if we're also going to report an error when extracting the arguments in `args.rs`. I left a few other nits as well, but the other important change is that we cannot call `metadata` unconditionally on every single file. It would slow down iteration way too much even when you don't use the `--max-filesize` flag.

---

_Comment by @tiehuis on 2017-03-01 05:50_

Thanks for the quick response. Here is an updated version.

A note about the `skip_filesize` function. I've currently just added appropriate checks prior to calling to ensure that it is necessary to even call. The two places it is called have slightly differing types `walkdir::DirEntry` and `walk::DirEntry` with no common impl for the metadata function. For the moment this seems the cleanest way to me and I've added a note to the function regarding the performance concern in actually calling it.

---

_@BurntSushi approved on 2017-03-08 15:17_

Fantastic. Thanks so much! Great work. :D

---

_Merged by @BurntSushi on 2017-03-08 15:17_

---

_Closed by @BurntSushi on 2017-03-08 15:17_

---

_Comment by @BurntSushi on 2017-03-08 15:17_

@tiehuis I will see if I can sort out the `skip_filesize` issue you mention next time I'm in that code. For now it looks OK. Thanks. :-)

---

_Comment by @BurntSushi on 2017-03-08 15:21_

Urk. I missed that this hadn't been squashed. In the future, I will try to do better to notice and remind folks.

---
