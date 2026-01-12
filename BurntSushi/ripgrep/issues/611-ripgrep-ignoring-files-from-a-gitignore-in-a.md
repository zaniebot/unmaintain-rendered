```yaml
number: 611
title: RipGrep ignoring files from a .gitignore in a parent directory
type: issue
state: closed
author: samsondav
labels:
  - question
assignees: []
created_at: 2017-09-23T11:19:07Z
updated_at: 2017-09-24T11:19:37Z
url: https://github.com/BurntSushi/ripgrep/issues/611
synced_at: 2026-01-12T16:13:22Z
```

# RipGrep ignoring files from a .gitignore in a parent directory

---

_@samsondav_

I have a node app for which all of the useful application code is ignored by the `.dockerignore` since we only copy the compiled files to docker.

The compiled files are useless for searching and are ignored by the `.gitignore` file, which is correct.

I want to "ignore" the `.dockerignore` file. The docs don't seem to specify how to do that, is there a way?

---

_Comment by @BurntSushi on 2017-09-23 11:29_

Could you please describe what you want in more precise terms? It would help if you gave an example. Namely, I don't know what you mean by "ignoring" `.dockerignore`. It is a hidden file, so it is automatically ignored by default.

---

_Label `question` added by @BurntSushi on 2017-09-23 11:30_

---

_Comment by @samsondav on 2017-09-23 17:50_

@BurntSushi 

Sorry I wasn't clear.

I mean that the `.dockerignore` contains some ignore rules that I don't want ripgrep to use. So how can I configure it not to load ignore rules from this file.

---

_Comment by @BurntSushi on 2017-09-23 18:42_

ripgrep doesn't load ignore rules from `.dockerignore` files. Please
provide a reproducible example so that I can help you figure out the actual
problem.

On Sep 23, 2017 1:51 PM, "Sam Davies" <notifications@github.com> wrote:

> @BurntSushi <https://github.com/burntsushi>
>
> Sorry I wasn't clear.
>
> I mean that the .dockerignore contains some ignore rules that I don't
> want ripgrep to use. So how can I configure it not to load ignore rules
> from this file.
>
> —
> You are receiving this because you were mentioned.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/611#issuecomment-331658049>,
> or mute the thread
> <https://github.com/notifications/unsubscribe-auth/AAb34kWg5SJvOeKcuJDM3T9dcSKYIvA2ks5slUUEgaJpZM4PhhiC>
> .
>


---

_Comment by @samsondav on 2017-09-24 07:23_

Ahh, you are quite right. It only loads ignore matches from the `.gitignore` file. The `.dockerignore` was a red herring.

I ran RipGrep again with `--debug` and found this output:

```
$ pwd
/Users/sam/code/nested/local-stack/app-react
$ rg mypattern
...
DEBUG:ignore::walk: ignoring ./app: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/sam/code/nested/local-stack/.gitignore"), original: "app", actual: "**/app", is_whitelist: false, is_only_dir: false })))
...
```

I have a directory called `app` in the directory above the current one. This is gitignored by a `.gitignore` file in the directory above.

It seems that the rules from the `.gitignore` in the directory above are being applied incorrectly to this directory.

---

_Comment by @samsondav on 2017-09-24 07:25_

The directory format looks like this:

```
project
\-app
\-.gitignore # contains `app`
\-app-react
  \-app # I want to search inside this directory but rg is ignoring it even if I run from app-react
```

---

_Renamed from "Can't configure Ripgrep to "ignore" a particular "*ignore" file." to "RipGrep ignoring files from a .gitignore in a parent directory" by @samsondav on 2017-09-24 07:25_

---

_Comment by @okdana on 2017-09-24 07:31_

How is it incorrect? The [`gitignore` documentation](https://git-scm.com/docs/gitignore) states:

>When deciding whether to ignore a path, Git normally checks gitignore patterns from multiple sources, with the following order of precedence, from highest to lowest (within one level of precedence, the last matching pattern decides the outcome):
>
>... Patterns read from a .gitignore file in the same directory as the path, or **in any parent directory**, with patterns in the higher level files (up to the toplevel of the work tree) being overridden by those in lower level files down to the directory containing the file. These patterns match relative to the location of the .gitignore file. A project normally includes such .gitignore files in its repository, containing patterns for files generated as part of the project build.

You can use the `--no-ignore-parent` option to disable this behaviour:

> --no-ignore-parent
> Don't respect ignore files (.gitignore, .ignore, etc.) in parent directories.

---

_Comment by @samsondav on 2017-09-24 07:44_

I dug into this further.

Turns out it was a weird issue to do with submodules. Not a fault of RipGrep.

This issue can be closed. Thanks for your help! ❤️ 💛 💙 💚 💜 



---

_Comment by @BurntSushi on 2017-09-24 11:18_

Also, if your gitignore file contains an `app` rule, then that will cause*all* child directories and files named `app` to be ignored. If you only want to ignore the top level app directory, then you can use `/app` instead.

---

_Closed by @BurntSushi on 2017-09-24 11:18_

---

_Comment by @BurntSushi on 2017-09-24 11:19_

In the future, obese start by trying to recreate a minimal reproducible example. That will cut through most misunderstandings like a hot knife through butter. :-)

---
