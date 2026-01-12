```yaml
number: 474
title: ripgrep not getting all occurrences in huge file
type: issue
state: closed
author: grench
labels: []
assignees: []
created_at: 2017-05-05T11:35:09Z
updated_at: 2017-05-08T21:27:31Z
url: https://github.com/BurntSushi/ripgrep/issues/474
synced_at: 2026-01-12T16:13:22Z
```

# ripgrep not getting all occurrences in huge file

---

_@grench_

I'm trying to get all occurrences in huge file (37GB). But it gives me not all results. How to fix it in ripgrep search?
```
wc -l < file_name.txt
1181551380
```
```
rg 'drive' file_name.txt  --debug -c
DEBUG:grep::search: regex ast:
Literal {
    chars: [
        'd',
        'r',
        'i',
        'v',
        'e'
    ],
    casei: false
}
DEBUG:grep::literals: literal prefixes detected: Literals { lits: [Complete(drive)], limit_size: 250, limit_class: 10 }
DEBUG:globset: built glob set; 0 literals, 1 basenames, 0 extensions, 0 prefixes, 1 suffixes, 0 required extensions, 0 regexes
5673
```
```
grep "drive" file_name.txt -c
342894
```
OS: macOS 10.12.4
ripgrep 0.5.1


---

_Comment by @BurntSushi on 2017-05-05 11:38_

Thanks for reporting this issue. As I said on StackOverflow, please provide enough data so that *others can reproduce the problem.* If I can't reproduce it, then I can't fix it. 37GB is too large, so perhaps you could shrink it down or find another file.

---

_Comment by @BurntSushi on 2017-05-05 11:53_

The results of searching [OpenSubtitles2016.raw.en (9.3GB uncompressed)](http://opus.lingfil.uu.se/OpenSubtitles2016/mono/OpenSubtitles2016.raw.en.gz):

```
$ rg drive -c OpenSubtitles2016.raw.en --mmap
416327
$ rg drive -c OpenSubtitles2016.raw.en --no-mmap
416327
$ grep drive -c OpenSubtitles2016.raw.en
416327
$ sift drive -c OpenSubtitles2016.raw.en
416327
$ pt drive -c OpenSubtitles2016.raw.en
OpenSubtitles2016.raw.en:416327
$ ag drive -c OpenSubtitles2016.raw.en
475635
$ ucg drive OpenSubtitles2016.raw.en | wc -l
102088
```

Notice that `ag` and `ucg` have problems, but the rest of the tools agree.

---

_Comment by @grench on 2017-05-05 12:08_

Thanks!!! --mmap helped :) It gave correct result.  But 165 sec (sift did the same in 57 sec). I just wanted to compare.
```
rg 'drive' emails.txt -c --no-mmap
5673
rg 'drive' emails.txt -c --mmap
342894
```


---

_Comment by @BurntSushi on 2017-05-05 12:12_

@grench **Can you please provide a file that I can search that reproduces the problem?**

The timings on a 37GB file are hard to measure. Unless you have a ton of RAM, it's going to involve reading from disk, so you need to be very careful with your benchmarking.

---

_Comment by @grench on 2017-05-05 12:25_

Sorry ... I can-not give this file (
Physical memory: 8GB

---

_Comment by @BurntSushi on 2017-05-05 12:27_

@grench I don't need *that* file, I need *a* file the reproduces the problem. If I can't reproduce this, then I can't fix it.

---

_Comment by @grench on 2017-05-05 12:40_

I'm using only one this big file for testing with data that I can not to upload for you to reproduce the problem ( 
Even if I'll mix each line in file and zip it ... I cann't upload so many data because of payable limited internet (

---

_Comment by @BurntSushi on 2017-05-05 12:45_

I'll try to be clearer. I don't need the whole file. I just need something that reproduces the problem. For example, if you can find just a couple of kilobytes of this file that causes inconsistencies between ripgrep and grep, then that should be good enough. For example:

```
$ head -n 1000 emails.txt > emails-sample.txt
$ rg drive -c emails-sample.txt
$ grep drive -c emails-sample.txt
```

If that doesn't reproduce the problem, then maybe try a different section of the file that has a cluster of matches found by grep. I'm asking you to be a bit creative here.

---

_Comment by @grench on 2017-05-05 16:37_

`<NUL>` in txt stop search - rg returns nothing
https://www.dropbox.com/s/b9sqgidcdr7dru9/file_name.txt?dl=0
<img width="321" alt="test" src="https://cloud.githubusercontent.com/assets/1156141/25755070/cb20edd6-31c1-11e7-8242-71f6e2f5883b.png">


---

_Comment by @BurntSushi on 2017-05-05 16:45_

Well, yeah, but it does for other searchers too. `<NUL>` causes most of these tools to detect the file as a binary file. For example:

```
[andrew@Serval ~] rg com tmp/file_name.txt
[andrew@Serval ~] sift com tmp/file_name.txt 
Binary file matches: tmp/file_name.txt
[andrew@Serval ~] grep com tmp/file_name.txt 
Binary file tmp/file_name.txt matches
```

ripgrep is a bit more silent, but if you use the `-a` flag, you'll get all your matches:

```
$ rg -a com tmp/file_name.txt 
1:1234567890@gmail.com
2:1234567890@yahoo.comm
3:1234567890@yahoo.comm
```

---

_Comment by @grench on 2017-05-05 19:22_

Thanks a lot!!! 👍 

---

_Closed by @BurntSushi on 2017-05-08 21:27_

---
