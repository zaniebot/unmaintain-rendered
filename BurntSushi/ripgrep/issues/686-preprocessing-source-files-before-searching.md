```yaml
number: 686
title: Preprocessing source files before searching
type: issue
state: closed
author: ghost
labels:
  - enhancement
  - question
  - wontfix
assignees: []
created_at: 2017-11-17T23:26:14Z
updated_at: 2018-06-14T11:16:49Z
url: https://github.com/BurntSushi/ripgrep/issues/686
synced_at: 2026-01-12T16:13:22Z
```

# Preprocessing source files before searching

---

_@ghost_

I think that it would be useful to be able to preprocess source files, and search the preprocessed output.

The exact nature of preprocessing would depend on the language. In Java, for example, Unicode escape sequences in source code are translated to the corresponding Unicode characters (see §3.2 Lexical Translations of the Java Language Specification); thus, the following is a valid Java compilation unit:

```java
public class UnicodeEscapesTest {\u000a public static void main(String[] args) {
System.out.println\u0028\u0022Hello\u002c world!\u0022\u0029;
} }
```

For C and C++, it could be useful to allow full preprocessing to be performed, although this might be rather complicated to support because there would need to be a way to specify the preprocessing binary to use, preprocessor options, and ideally parse the line number information emitted by the preprocessor so that the correct source file line numbers are printed by ripgrep.

As a lighter option to full preprocessing, one possible preprocessing operation would be merging of string literals. C and C++ compilers will merge adjacent string literals into one (e.g. `"abc" "def"` is translated to `"abcdef"`). Java's compiler evaluates String-type constant expressions.

**EDIT** In a way, this is related to #225 Support decompression on the fly.

---

_Comment by @BurntSushi on 2017-11-18 02:25_

Sorry, but I have no idea what you're suggesting here. Could you show a simple example with input and output corresponding to the preprocessing you want?

Please consider showing motivating examples. Corner cases of language specifications aren't convincing.

---

_Comment by @ghost on 2017-11-18 19:26_

I opened this enhancement request for searching within Java files in particular, although I anticipate that this could be highly useful for C and C++ source code as well, given that the preprocessor can drastically affect how source code expands to the actually-compiled code.

Suppose that I wanted to find the text "Hello, world!" within Java source files. This might be represented as the sequence of literal Unicode characters "Hello, world!", as with:
```java
public class UnicodeEscapesTest { public static void main(String[] args) {
System.out.println("Hello, world!");
} }
```

Or, because of the ability to use Unicode escape sequences, it can be "obfuscated". For example, `\u002c` represents the comma character, as does `\u002C`. Reading the Java 9 Language Spec this morning, I found out that one can even use arbitrarily many 'u's, as in `\uuuuuuu002c`:
```java
public class UnicodeEscapesTest { public static void main(String[] args) {
System.out.println("Hello\uuuuuuuuu002c world!");
} }
```

So, in order to not miss finding the occurrence of "Hello, world!" in the above file, I would need to adjust the regexp to allow the equivalent Unicode escape sequence for each character:

<pre>
rg -tjava '(H|\\u+0048)(e|\\u+0065)(l|\\u+006[Cc])(l|\\u+006[Cc])(o|\\u+006[Ff])(,|\\u+002[Cc])( |\\u+0020)(w|\\u+0077)(o|\\u+006[Ff])(r|\\u0072)(l|\\u+006[Cc])(d|\\u+0064)(!|\\u+0021)'
</pre>

Searching for "Hello, world!" case insensitively would be a bit more challenging.

Note that the Unicode escape sequence translation is not limited to string literals; Unicode escape sequences can be utilized in any part of the source file.

If ripgrep could preprocess Unicode escape sequences by replacing them with the corresponding character(s), then it would eliminate the pitfall of not finding an actual occurrence of a regexp match just because the match was obfuscated via Unicode escape sequences. Such obfuscation is not necessarily malicious; I once used this feature of the Java language to work around a limitation of some software that I was using.

To give an example of searching through C code, suppose one encounters a multiple definition linker error for a symbol `IMPL_someFunction` and one wants to know where this function is actually defined. It might be that a macro invocation is used that expands to that identifier, making the task of finding the definitions of this symbol more difficult.

---

_Comment by @BurntSushi on 2017-11-18 20:43_

Thanks for elaborating, but I consider this feature request *well* out of scope for ripgrep.

---

_Closed by @BurntSushi on 2017-11-18 20:43_

---

_Label `wontfix` added by @BurntSushi on 2017-11-18 20:44_

---

_Label `question` added by @BurntSushi on 2017-11-18 20:44_

---

_Label `enhancement` added by @BurntSushi on 2017-11-18 20:44_

---

_Comment by @agguser on 2018-06-14 08:09_

Is there any chance that `rg` will support pre-processing sources? This is for normalizing inputs, instead of creating a complex pattern to match, or generating all normalized sources first that take up disk space and will be out-of-sync if sources change.

Or is there a quicker version of `for f in **/*; do echo "$f"; sed … "$f" | rg …; done`?

---

_Comment by @BurntSushi on 2018-06-14 11:16_

> Is there any chance that rg will support pre-processing sources?

I don't see it happening, no.

---
