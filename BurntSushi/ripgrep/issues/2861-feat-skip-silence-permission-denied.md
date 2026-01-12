```yaml
number: 2861
title: "[feat] skip/silence permission denied"
type: issue
state: closed
author: mazunki
labels: []
assignees: []
created_at: 2024-07-29T17:02:15Z
updated_at: 2024-07-29T17:58:30Z
url: https://github.com/BurntSushi/ripgrep/issues/2861
synced_at: 2026-01-12T16:13:25Z
```

# [feat] skip/silence permission denied

---

_@mazunki_

I often find myself grepping for something on my whole system as a user, and while I can often just run `rg` as root to match files which I don't have permission to read, I know for a fact there's no point looking in files which I don't have access to.

Because of this, I would like to add something like `--suppress-error=nopermission` to my default ripgrep alias. I can silence all errors with `2>&-`, which returns errno=2 telling me something went wrong. I don't want to suppress all errors, just those related to permission issues. 

Is this a feature which is currently possible, and I just missed it? If it doesn't exist, is this a feature worth considering? If this is the case, I'd be willing to give Rust a go.

If this is something we want to add, what should the flag and its options look like? Should we have separate options for just silencing the errors (exit code stays 2), vs skipping the files entirely (exit successfully)?

---

_Comment by @BurntSushi on 2024-07-29 17:13_

I think you probably want `--no-messages`, although that will suppress more than permission errors.

I don't think I want ripgrep to go down the rabbit hole of trying to classify errors and then building a system on top of that classification for whitelisting/blacklisting each kind of error. Like, I realize that "I just want to suppress permission errors" is in and of itself pretty simple. And it wouldn't be that absurd to add one single flag for this one single use case like, `--suppress-permission-errors`. But it's a little odd, very bespoke and likely difficult to discover. And the alternative of adding a fuller system is probably a much bigger increase in scope than you are picturing here.

Instead, there are a few different paths for you to take, not all of which present a perfect solution:

* Do a very crude filtering step by redirecting stderr to stdout, and then filtering out any lines containing things that look like permission errors.
* Redirect stderr to a separate log file and search it after the fact for any errors that aren't permission errors.
* Develop a `.rgignore` file that prevents ripgrep from descending into most of the directories that you don't have access to. I'm willing to bet that the [Pareto principle](https://en.wikipedia.org/wiki/Pareto_principle) applies here, and that you'll be able to get the error messages down to tolerable levels without a lot of work.
* Use `--no-messages` and live with the fact that some errors won't be surfaced. Note that `--no-messages` is likely better for you than `&>/dev/null`. Compare, for example, the outputs of `rg '(' &> /dev/null` and `rg '(' --no-messages`.

---

_Closed by @BurntSushi on 2024-07-29 17:13_

---

_Comment by @mazunki on 2024-07-29 17:56_

Thanks for the reply, I will probably build up the ignore file as you suggested.

I understand the concerns of classifying errors. I haven't checked the code, so I have no clue how that's set up. I kind of assumed the errors had their own types, but I guess that's why errno=2 includes both syntax errors and permission errors, for instance. I will definitely use `--no-messages` instead from now on :)

---

_Comment by @BurntSushi on 2024-07-29 17:58_

Aye. And yeah, errors do not have their own types. Most errors are treated the same: they're just chained together and bubbled up be emitted to stderr. There are some very broad exceptions to this. Errors related to gitignore parsing are treated differently. And errors related to a broken pipe will cause ripgrep to exit gracefully, as is the custom with Unix CLI tools.

---
