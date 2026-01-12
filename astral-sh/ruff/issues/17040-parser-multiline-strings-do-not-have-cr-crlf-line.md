```yaml
number: 17040
title: "[parser] Multiline strings do not have CR/CRLF line endings normalized to LF"
type: issue
state: closed
author: coolreader18
labels:
  - parser
assignees: []
created_at: 2025-03-28T17:16:10Z
updated_at: 2025-03-28T17:50:57Z
url: https://github.com/astral-sh/ruff/issues/17040
synced_at: 2026-01-12T15:54:55Z
```

# [parser] Multiline strings do not have CR/CRLF line endings normalized to LF

---

_@coolreader18_

### Summary

We caught this in RustPython in CPython's regression tests when switching to ruff's parser. Python behavior:

```sh
$ python -c $'print(repr("""\r\n"""))' 
'\n'
$ python -c $'print(repr(b"""\r\n"""))'
b'\n'
$ python -c $'print(repr(r"""\r\n"""))'
'\n'
$ python -c $'print(repr("""\r"""))' 
'\n'
$ python -c $'print(repr(b"""\r"""))'
b'\n'
$ python -c $'print(repr(r"""\r"""))'
'\n'
```

(the `$` means that the shell is expanding the escapes, not python)

A repro for ruff is that these tests I wrote to verify the issue fail (or, the second one fails): 

```rust
    fn string_parser_multiline_eol(eol: &str) {
        let source = format!(r#""""abc{eol}def""""#);
        let suite = parse_suite(&source).unwrap();
        let str_val = suite[0]
            .as_expr_stmt()
            .unwrap()
            .value
            .as_string_literal_expr()
            .unwrap()
            .as_single_part_string()
            .unwrap()
            .as_str();
        assert_eq!(str_val, "abc\ndef");
    }

    #[test]
    fn string_parser_multiline_unix_eol() {
        string_parser_multiline_eol("\n");
    }

    #[test]
    fn string_parser_multiline_windows_eol() {
        string_parser_multiline_eol("\r\n");
    }
```

Not sure whether it would make more sense to fix this in the lexer or the string parser; I found #5976, which seems to imply that this used to be handled in the lexer, but I feel like the string parser may make more sense.

### Version

master

---

_Comment by @coolreader18 on 2025-03-28 17:19_

(our current workaround for rustpython is to just call `.replace("\r\n", "\n")` before handing it to the parser, which does seems to work perfectly, but I thought this might be something ruff would want to handle)

---

_Comment by @charliermarsh on 2025-03-28 17:21_

Hmm, I think it's intentional that we don't normalize these, since we want to preserve them when modifying user code (unlike in an interpreter, where you want to evaluate them down to whatever CPython would produce).

---

_Comment by @MichaReiser on 2025-03-28 17:50_

We were very excited when we saw that RustPython now uses Ruff's parser! 

I quickly checked what Python's AST module does and it doesn't normalize the new lines:

```
Module(
   body=[
      Expr(
         value=Call(
            func=Name(id='print', ctx=Load()),
            args=[
               Call(
                  func=Name(id='repr', ctx=Load()),
                  args=[
                     Constant(value='\r\n')],
                  keywords=[])],
            keywords=[]))],
   type_ignores=[])
```

This suggests that Python normalizes the new lines during the compile phase and isn't something we should change in Ruff's parser.


---

_Closed by @MichaReiser on 2025-03-28 17:50_

---

_Label `parser` added by @MichaReiser on 2025-03-28 17:50_

---
