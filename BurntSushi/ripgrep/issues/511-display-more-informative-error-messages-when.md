```yaml
number: 511
title: Display more informative error messages when attempting to read files
type: issue
state: closed
author: MichaelAquilina
labels:
  - bug
assignees: []
created_at: 2017-06-12T11:07:02Z
updated_at: 2018-01-09T23:08:20Z
url: https://github.com/BurntSushi/ripgrep/issues/511
synced_at: 2026-01-12T16:13:22Z
```

# Display more informative error messages when attempting to read files

---

_@MichaelAquilina_

When searching for files which the current user does not have access to, a very cryptic error message is shown:

```
$ rg --files -g "virtualenvwrapper.sh" /
Invalid argument (os error 22)
Invalid argument (os error 22)
Invalid argument (os error 22)
```

This seems to be because the user does not have permission to view the file. But it does not specify what the file was or what the error code even means.

Redirecting stderr helps filter out the noise at least:
```
$ rg --files -g "virtualenvwrapper.sh" / 2>/dev/null
/usr/share/virtualenvwrapper/virtualenvwrapper.sh
```

---

_Comment by @MichaelAquilina on 2017-06-12 11:08_

Btw, I am happy to try attempt solving this if I am giving some indicators :) Been meaning to dive into rust after reading the learn rust book.

---

_Comment by @BurntSushi on 2017-06-12 11:34_

@MichaelAquilina Hmm, I'm happy to mentor this! Basically, it's going to require auditing the codebase for where errors can occur. It's something that is relatively easy to do if you know the codebase, but if you don't, it will take some careful sleuthing. If you're feeling up to the challenge, then I'd be happy to give you some pointers.

For example, if I had to *guess* at where the error is coming from, I'd say it's wherever we're trying to open a file to search it. That would be here: https://github.com/BurntSushi/ripgrep/blob/8bbe58d623db78a32b04eabff9a69667ad23ff7b/src/worker.rs#L221 --- As you can see though, there is already logic for printing a nicer error message with the file name. That means it's likely somewhere else, perhaps in the directory traversal which is in the `ignore` crate.

(There may be other smaller issues you might want to start with. I believe they're all tagged with "help wanted.")

---

_Label `bug` added by @BurntSushi on 2017-06-12 11:34_

---

_Comment by @MichaelAquilina on 2017-06-12 13:11_

Thanks @BurntSushi I will probably attempt to do this some time this week in the evening or during the Weekend :)

---

_Assigned to @MichaelAquilina by @BurntSushi on 2017-06-12 13:12_

---

_Comment by @BurntSushi on 2017-06-12 13:13_

@MichaelAquilina Awesome! I've assigned it to you. :-) I'm on the various Rust IRC channels and happy to be pinged if you want any help! I'm typically available in the evening EST time during the week, and unpredictably available on the weekends.

---

_Unassigned @MichaelAquilina by @BurntSushi on 2017-09-24 12:40_

---

_Comment by @MichaelAquilina on 2018-01-09 23:08_

Revisiting this, and found that OS error is "EINVAL 22 Invalid argument". 

I suspect this might have been a previous issue that is now fixed in the latest version as I cannot reproduce it. 

Closing this issue for this reason. 

---

_Closed by @MichaelAquilina on 2018-01-09 23:08_

---
