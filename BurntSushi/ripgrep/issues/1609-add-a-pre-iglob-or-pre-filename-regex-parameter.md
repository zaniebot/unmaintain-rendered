```yaml
number: 1609
title: Add a --pre-iglob or --pre-filename-regex parameter
type: issue
state: open
author: phiresky
labels:
  - enhancement
assignees: []
created_at: 2020-06-09T09:20:43Z
updated_at: 2020-06-09T11:46:16Z
url: https://github.com/BurntSushi/ripgrep/issues/1609
synced_at: 2026-01-12T16:13:23Z
```

# Add a --pre-iglob or --pre-filename-regex parameter

---

_@phiresky_

Currently, --pre-glob is case sensitive. Since I (and I guess most people) want case insensitive matching, my command line looks like this:

`"rg" "--no-line-number" "--smart-case" "--pre" "rga-preproc" "--pre-glob" "*.{mkv,MKV,mp4,MP4,avi,AVI,epub,EPUB,odt,ODT,docx,DOCX,fb2,FB2,ipynb,IPYNB,pdf,PDF,zip,ZIP,tgz,TGZ,tbz,TBZ,tbz2,TBZ2,gz,GZ,bz2,BZ2,xz,XZ,zst,ZST,tar,TAR,db,DB,db3,DB3,sqlite,SQLITE,sqlite3,SQLITE3}" "foo"`

which is kinda unweildly and will only get worse the more adapters I add - it also misses `.Zip` files which might be relevant for people.

 I don't think there's really a common use case where this case sensitivity is wanted, but it's probably too late to change now because of backwards compatibility.

So it would be great if there was either of these options:

1. `--pre-regex` to allow supplying a rust regex for extensions. This would be the most flexible.
2. `--pre-iglob` akin to `--iglob` does exactly what it says. 
3. `--pre-glob-case-insensitive` boolean flag


---

_Comment by @BurntSushi on 2020-06-09 11:46_

I think adding `--pre-iglob` is probably the simplest option, so let's go with that for now.

---

_Label `enhancement` added by @BurntSushi on 2020-06-09 11:46_

---
