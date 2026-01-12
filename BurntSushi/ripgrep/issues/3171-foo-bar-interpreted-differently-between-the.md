```yaml
number: 3171
title: "`foo**/bar` interpreted differently between the ignore crate and Git"
type: issue
state: closed
author: tomokinakamaru
labels:
  - wontfix
assignees: []
created_at: 2025-10-08T02:07:43Z
updated_at: 2025-10-11T02:50:03Z
url: https://github.com/BurntSushi/ripgrep/issues/3171
synced_at: 2026-01-12T16:13:25Z
```

# `foo**/bar` interpreted differently between the ignore crate and Git

---

_@tomokinakamaru_

(I chose the blank issue template because I was not sure if this is a bug or not.)

I've noticed that the gitignore pattern `foo**/bar` is interpreted differently between the ignore crate and Git. When `.gitignore` contains `foo**/bar`,

- Git ignores a file named `foobar`, while
- the ignore crate does __not__ ignore `foobar`.[^test]

Git's interpretation is unintuitive, but given the wide use of the ignore crate, I wanted to report this difference for clarification. I would be happy if the ignore crate follows Git perfectly, but I understand if it behaves differently against this edge case.

Is this difference expected or a known issue?

[^test]: https://github.com/tomokinakamaru/ripgrep/blob/71c409aab31132d726ed2b98402eeaf1f3118df3/crates/ignore/tests/gitignore_matched_path_or_any_parents_tests.gitignore#L218 and https://github.com/tomokinakamaru/ripgrep/blob/71c409aab31132d726ed2b98402eeaf1f3118df3/crates/ignore/tests/gitignore_matched_path_or_any_parents_tests.rs#L25-L31

---

_Comment by @BurntSushi on 2025-10-08 11:14_

Please provide an MRE.

---

_Renamed from "`foo**/bar` interpreted between the ignore crate and Git" to "`foo**/bar` interpreted differently between the ignore crate and Git" by @tomokinakamaru on 2025-10-10 06:32_

---

_Comment by @tomokinakamaru on 2025-10-10 06:48_

Sorry for not providing an MRE earlier. I've attached a tar.gz file to help reproduce the issue. The archive includes the following files and directories:

- `mre-3171`
- `mre-3171/foobar`
- `mre-3171/.gitignore`
- `mre-3171/.git/`

To reproduce the issue,

1. Unarchive the tar.gz file.
2. Move to the directory `mre-3171`
3. Run `git status -uall -s` to list Git-tracked files. This will print only `?? .gitignore`.
4. Run `rg --files` to list search targets. This will print `foobar`, which is ignored by Git.

[mre-3171.tar.gz](https://github.com/user-attachments/files/22840683/mre-3171.tar.gz)

If you're uncomfortable downloading and opening the tar.gz file for security reasons, you can use the following commands instead.

```sh
mkdir mre-3171
cd mre-3171
git init
echo 'foo**/bar' > .gitignore
touch foobar
git status -uall -s  # prints "?? .gitignore"
rg --files  # prints "foobar"
```

---

_Comment by @BurntSushi on 2025-10-11 00:36_

Thanks for the MRE! The tar archive isn't necessary. The shell commands are perfect. :-)

From what I can tell, this is a bug in `git`, not in ripgrep. It's either in git's behavior or in its docs, as they seem inconsistent for this particular pattern. From `man gitignore`:

```
       Two consecutive asterisks ("**") in patterns matched against full pathname may have special meaning:

       •   A leading "**" followed by a slash means match in all directories. For example, "**/foo" matches file or directory "foo" anywhere, the same as pattern "foo". "**/foo/bar" matches file or directory "bar" anywhere that is
           directly under directory "foo".

       •   A trailing "/**" matches everything inside. For example, "abc/**" matches all files inside directory "abc", relative to the location of the .gitignore file, with infinite depth.

       •   A slash followed by two consecutive asterisks then a slash matches zero or more directories. For example, "a/**/b" matches "a/b", "a/x/b", "a/x/y/b" and so on.

       •   Other consecutive asterisks are considered regular asterisks and will match according to the previous rules.
```

So my interpretation here is that `foo**/bar` falls into the fourth bullet point above. And thus referencing the meaning of `*`:

```
       •   An asterisk "*" matches anything except a slash. The character "?" matches any one character except "/". The range notation, e.g. [a-zA-Z], can be used to match one of the characters in a range. See fnmatch(3) and the
           FNM_PATHNAME flag for a more detailed description.
```

I really don't get how `foo**/bar` could match `foobar` to be honest. I think it either has to be interpreted as `foo/**/bar` or `foo*/bar`. And neither of those (according to git) match `foobar`.

So this might be worth filing with `git`. Feel free to re-open this if they fix this on their end and it requires a change in ripgrep.

(I do generally want to match git's behavior and there are some cases where we don't today. This is one of them, but I can't tell if git's behavior is intentional here or not.)

---

_Closed by @BurntSushi on 2025-10-11 00:36_

---

_Label `wontfix` added by @BurntSushi on 2025-10-11 00:36_

---

_Comment by @tomokinakamaru on 2025-10-11 02:50_

Thank you for taking the time to investigate them! I agree with your assessment. The patterns should be interpreted as you described. (I thought I should at least report and discuss them with the maintainer just in case, which is why I opened this issue.)

Thanks again for your time!

---
