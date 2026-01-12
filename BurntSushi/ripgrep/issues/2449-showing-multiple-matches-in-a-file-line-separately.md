```yaml
number: 2449
title: Showing multiple matches in a file line separately
type: issue
state: closed
author: manikantag
labels:
  - duplicate
assignees: []
created_at: 2023-03-11T18:37:27Z
updated_at: 2023-03-11T18:58:04Z
url: https://github.com/BurntSushi/ripgrep/issues/2449
synced_at: 2026-01-12T16:13:24Z
```

# Showing multiple matches in a file line separately

---

_@manikantag_

This may be duplicate of https://github.com/BurntSushi/ripgrep/issues/365, or with slightly different experience requirements.

Is there any flag to **change the output to include separate entry per each match in the line** of a file with some characters before & after. This is particularly useful in displaying matched results in very lengthy lines, like minified js files. 

Simply put: Each match should be displayed in separate output line (if files has total 10 matches, then I want to display in 10 different output lines irrespective of where those 10 matches are)

Ex: 
```javascript
/*3000 chars...;*/ function1(); console.log('ripgrep'); function2(); /*..... 1000 more chars...*/ console.log('ripgrep'); function3(); /*....more chars....*/
```
Given a file with a line like above, when searching for `ripgrep` word, I want to show something line below:

```
1:3028   ion1(); console.log('ripgrep'); functio
                              -------
1:4032   ... console.log('ripgrep'); functio
                          -------
```

For example, VSCode, Chrome Devtools display like this.

![image](https://user-images.githubusercontent.com/317299/224505531-36e9acc8-c8e4-46f5-9fe6-24c977f3f28e.png)

IMHO, this way it will be better user experience than existing `--max-columns`  & `--max-columns-preview` options.

---

_Comment by @BurntSushi on 2023-03-11 18:43_

The standard way to do this would be `-o/--only-matching` coupled with some bounded repeats. So for example, `rg -o '.{0,10}TestString.{0,10}`. Although ripgrep doesn't do overlapping search, so if another `TestString` occurs within ten characters of another, then the one occurring second wouldn't be found as a match.

Otherwise, this looks like a duplicate of #1352.

---

_Label `duplicate` added by @BurntSushi on 2023-03-11 18:43_

---

_Closed by @BurntSushi on 2023-03-11 18:43_

---

_Comment by @manikantag on 2023-03-11 18:47_

That is pretty quick!

I tried that too. But then I don't see the match in different color.

![image](https://user-images.githubusercontent.com/317299/224506228-fad370c9-6e7a-4ed2-88f6-9cb98032c3c9.png)


---

_Comment by @manikantag on 2023-03-11 18:48_

My main requirement is not to display in a console, but to build search UI from ripgrep JSON (using `--json`)

---

_Comment by @manikantag on 2023-03-11 18:53_

I see `--max-columns` is not considered when `--json` is used. I see way more in the JSON.

![image](https://user-images.githubusercontent.com/317299/224506478-ae878a66-db26-48ec-8796-9916275af848.png)

Is it the standard behavior? 

---

_Comment by @BurntSushi on 2023-03-11 18:55_

Right. The `.{0,10}` becomes part of the match. But it _does_ give you the surrounding context.

> My main requirement is not to display in a console, but to build search UI from ripgrep JSON (using `--json`)

That's very relevant information. Please definitely include the _actual problem_ you're trying to solve in issues.

In this case, I don't think you need anything from ripgrep here. ripgrep already gives you the match offsets. So you can pluck them out of the JSON representation and add whatever windowing logic you need. There's no need for ripgrep to do it.

> I see `--max-columns` is not considered when `--json` is used. I see way more in the JSON.

Yes. As the docs for the `--json` flag say:

> Other flags that control aspects of the standard output such as --only-matching, --heading, --replace, --max-columns, etc., have no effect when --json is set.

Options like `--max-columns` are _output format_ settings. They don't make sense in the `--json` context. If you're writing a program to read the JSON, then you have the _full espressive power_ of a programming language to do whatever you want to massage the output into whatever kind of format you need. ripgrep gives you the stuff you can't easily get otherwise, but the rest is up to you.

---

_Comment by @manikantag on 2023-03-11 18:58_

Makes perfect sense. Once I saw the _almost_ entire line in JSON, I thought getting required string out of the string and highlight in HTML.

Thanks

---
