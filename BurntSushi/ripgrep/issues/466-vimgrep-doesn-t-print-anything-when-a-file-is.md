```yaml
number: 466
title: "--vimgrep doesn't print anything when a file is specified"
type: issue
state: closed
author: elirnm
labels:
  - question
assignees: []
created_at: 2017-04-27T23:48:37Z
updated_at: 2018-01-31T22:41:50Z
url: https://github.com/BurntSushi/ripgrep/issues/466
synced_at: 2026-01-12T16:13:22Z
```

# --vimgrep doesn't print anything when a file is specified

---

_@elirnm_

When a specific file is specified for a search, running rg with `--vimgrep` results in nothing being printed. The `--vimgrep` option works fine if a specific file isn't specified.

This is with rg 0.5.1

Specifying a specific file (doesn't work):
![capture](https://cloud.githubusercontent.com/assets/19817004/25508720/f79a5a8c-2b68-11e7-967b-556e2c320935.PNG)

Not specifying a specific file (works):
![capt2ure](https://cloud.githubusercontent.com/assets/19817004/25508756/225f2270-2b69-11e7-8b7b-7c11e2ffba1b.PNG)


---

_Comment by @BurntSushi on 2017-04-28 11:04_

I can't reproduce this problem:

```
[andrew@Serval ripgrep-466] echo Haunting > titles.txt
[andrew@Serval ripgrep-466] rg Haunting
titles.txt
1:Haunting
[andrew@Serval ripgrep-466] rg Haunting titles.txt
1:Haunting
[andrew@Serval ripgrep-466] rg --vimgrep Haunting titles.txt
1:1:Haunting
```

---

_Comment by @kpp on 2017-04-30 12:27_

I have reproduced this problem only on **Windows**. Linux is fine.

![rg](https://cloud.githubusercontent.com/assets/467709/25564322/846ce00e-2db9-11e7-9756-3eb376286185.png)


---

_Comment by @kpp on 2017-04-30 12:41_

Test it with code on `Windows`:

```
// See: https://github.com/BurntSushi/ripgrep/issues/466
#[test]
fn regression_466_vimgrep_fails_on_windows() {
    let wd = WorkDir::new("regression_466_vimgrep_fails_on_windows");
    let path = "titles.txt";
    wd.create(path, "Haunting\n");

    let mut cmd = wd.command();
    cmd.arg("--vimgrep").arg("Haunting").arg(path);
    let lines: String = wd.stdout(&mut cmd);

    let expected = "\
1:1:Haunting
";

    assert_eq!(lines, expected);
}
```

---

_Label `question` added by @BurntSushi on 2017-05-08 21:28_

---

_Comment by @Taverius on 2017-05-29 19:33_

I was noticing this too.

Just to be quadruply sure, I tried all variants of `rg` v0.5.2 provided: x86/x64, gnu/msvc

So hey, at least its not a compiler issue?

---

_Comment by @elirnm on 2017-06-02 10:45_

I did some combination testing on the arguments that `--vimgrep` is a combination of, and the issue seems to lie with `--color`. When a file is specified to search, setting either `--color never` or `--color ansi` will cause nothing to be printed, while the same command with `--color always` or `--color auto` will print the matches.

Everything behaves as expected if a filepath isn't specified, even if that exact path is given as the value to a `-g` argument.

So:
`rg blah test.txt --color <never|ansi>` fails
`rg blah test.txt --color <auto|always>` succeeds
`rg blah -g test.txt --color <never|ansi|auto|always>` succeeds

---

_Comment by @andyleejordan on 2017-07-06 18:31_

To add to this confusing conversation: using rg 0.5.2, I'm getting invisible output, here's a copy paste:

```
$ rg -tcmake group.cpp
src\CMakeLists.txt
471:  zookeeper/group.cpp
$ rg -tcmake group.cpp | echo
src\CMakeLists.txt:  zookeeper/group.cpp
```

And here's a screenshot:

![ripgrep](https://user-images.githubusercontent.com/2226434/27926796-858c71ca-623e-11e7-8ca6-93cfb4d4e4b8.PNG)

The result `src\CMakeLists.txt` is output, since it copied and pasted into here, but it's not rendering!

---

_Comment by @elirnm on 2017-07-06 20:39_

That is a colorization issue. See #342.

________________________________
From: Andrew Schwartzmeyer <notifications@github.com>
Sent: Thursday, July 6, 2017 11:31:18 AM
To: BurntSushi/ripgrep
Cc: elirnm; Author
Subject: Re: [BurntSushi/ripgrep] --vimgrep doesn't print anything when a file is specified (#466)


To add to this confusing conversation: using rg 0.5.2, I'm getting invisible output, here's a copy paste:

$ rg -tcmake group.cpp
src\CMakeLists.txt
471:  zookeeper/group.cpp
$ rg -tcmake group.cpp | echo
src\CMakeLists.txt:  zookeeper/group.cpp


And here's a screenshot:

[ripgrep]<https://user-images.githubusercontent.com/2226434/27926796-858c71ca-623e-11e7-8ca6-93cfb4d4e4b8.PNG>

The result src\CMakeLists.txt is output, since it copied and pasted into here, but it's not rendering!

—
You are receiving this because you authored the thread.
Reply to this email directly, view it on GitHub<https://github.com/BurntSushi/ripgrep/issues/466#issuecomment-313481076>, or mute the thread<https://github.com/notifications/unsubscribe-auth/AS5iLGb5zQTLijd_I5VZeoVutyeAQTeEks5sLSf2gaJpZM4NK5dA>.


---

_Comment by @andyleejordan on 2017-07-06 21:34_

@elirnm thank you a million! This has been aching me since I upgraded.

---

_Comment by @BurntSushi on 2017-10-22 01:41_

I'm not able to reproduce this on Windows in either cygwin or PowerShell. For others, is this bug still happening with the latest version of ripgrep?

---

_Comment by @elirnm on 2017-10-25 01:55_

No, this is working for me now with 0.7.1.

---

_Comment by @kpp on 2017-10-28 17:18_

This is working for me now with 0.7.1 under Windows & Linux. Would you please add my test from https://github.com/BurntSushi/ripgrep/issues/466#issuecomment-298229947 so we could catch the bug before releasing the code?

---

_Comment by @BurntSushi on 2018-01-31 02:40_

I can't reproduce this. Closing.

(I'd encourage folks that want to add tests to submit PRs. :-))

---

_Closed by @BurntSushi on 2018-01-31 02:40_

---

_Comment by @kpp on 2018-01-31 22:41_

I failed to write regression test. When I call rg from win console, there is no output `1:1:Haunting`, but when I run test, it passes:

```
...
test regression_428_unrecognized_style ... ok
test regression_466_vimgrep_fails_on_windows ... ok
test regression_451_only_matching ... ok
...
```

```
// See: https://github.com/BurntSushi/ripgrep/issues/466
#[test]
fn regression_466_vimgrep_fails_on_windows() {
    let wd = WorkDir::new("regression_466_vimgrep_fails_on_windows");
    let path = "titles.txt";
    wd.create(path, "Haunting\n");

    let mut cmd = wd.command();
    cmd.arg("--vimgrep").arg("Haunting").arg(path);
    let lines: String = wd.stdout(&mut cmd);

    let expected = "\
1:1:Haunting
";

    assert_eq!(lines, expected);
}
```


---
