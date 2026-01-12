```yaml
number: 2136
title: "search-zip: include .jar files"
type: issue
state: closed
author: simon04
labels:
  - wontfix
assignees: []
created_at: 2022-01-31T16:26:53Z
updated_at: 2022-05-24T13:08:04Z
url: https://github.com/BurntSushi/ripgrep/issues/2136
synced_at: 2026-01-12T16:13:24Z
```

# search-zip: include .jar files

---

_@simon04_

#### Describe your feature request

I would like to `rg --search-zip` to search in .jar files

```
$ file *jar
foo.jar: Zip archive data, at least v1.0 to extract, compression method=deflate
bar.jar: Zip archive data, at least v2.0 to extract, compression method=deflate
```

Plenty of test files can be obtained from https://repo1.maven.org/maven2/, for instance https://repo1.maven.org/maven2/org/slf4j/slf4j-api/1.7.35/slf4j-api-1.7.35.jar


---

_Label `question` added by @BurntSushi on 2022-01-31 17:00_

---

_Comment by @BurntSushi on 2022-01-31 17:00_

I'm not sure this is exactly expected behavior to be honest. You can work around it with the `--pre` and `--pre-glob` flags.

---

_Comment by @garoto on 2022-01-31 19:56_

`--search-zip` is searching inside actual zip files currently?

---

_Comment by @BurntSushi on 2022-01-31 20:15_

No, it isn't. Because zips are archives. Good point. So this is an easy "no."

---

_Closed by @BurntSushi on 2022-01-31 20:15_

---

_Label `question` removed by @BurntSushi on 2022-01-31 20:15_

---

_Label `wontfix` added by @BurntSushi on 2022-01-31 20:15_

---

_Comment by @jumarko on 2022-05-20 07:44_

@BurntSushi can you elaborate more on why this isn't supported.
I thought using `rg -z pattern my.zip` (or `my.jar`) would work 
If `-z` doesn't search inside compressed files what it is for?

---

_Comment by @BurntSushi on 2022-05-20 11:06_

I'm not sure what else there is to elaborate on. Zips and tar files are archives. Gzip and xz and what not are compression formats. ripgrep searches the latter with this flag, not the former.

---

_Comment by @jumarko on 2022-05-24 12:30_

I'm sorry for my confusion. I should have known this but to casual users I guess this difference is quite nuanced.
At least, Wikipedia says, for ZIP: https://en.wikipedia.org/wiki/ZIP_(file_format)
> ZIP is an [archive file format](https://en.wikipedia.org/wiki/Archive_file_format) that supports [lossless data **compression**](https://en.wikipedia.org/wiki/Lossless_compression).

Regardless of the terminology, it would be fantastic if ripgrep could search _archives_ too.

---

_Comment by @BurntSushi on 2022-05-24 13:08_

See: #1112, #974, #1479, #1060, #918.

---
