```yaml
number: 2926
title: "(?<!pattern) zero-width negative lookbehind assertion error"
type: issue
state: closed
author: ItsFated
labels:
  - invalid
assignees: []
created_at: 2024-11-08T09:14:44Z
updated_at: 2024-11-08T12:33:57Z
url: https://github.com/BurntSushi/ripgrep/issues/2926
synced_at: 2026-01-12T16:13:25Z
```

# (?<!pattern) zero-width negative lookbehind assertion error

---

_@ItsFated_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

ripgrep 14.1.1 (rev 4649aa9700)

features:+pcre2
simd(compile):+SSE2,-SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,+AVX2

PCRE2 10.43 is available (JIT is available)

### How did you install ripgrep?

Download from https://github.com/BurntSushi/ripgrep/releases/download/14.1.1/ripgrep-14.1.1-x86_64-pc-windows-msvc.zip

### What operating system are you using ripgrep on?

Windows 10 版本21H2(操作系统内部版本19044.2673)
![image](https://github.com/user-attachments/assets/5b41091e-b992-4c68-9824-aa8d9813f4fa)

### Describe your bug.

When I execute the following command:
`rg -iP "(? <! (ACodec|NuPlayerDecoder).*)(buffer)" .\audio_config_main_20241108163659.log`
You will get the following error message: 
`rg: PCRE2: error compiling pattern at offset 3: length of lookbehind assertion is not limited`

### What are the steps to reproduce the behavior?

**Just as described.**
When I execute the following command:
`rg -iP "(? <! (ACodec|NuPlayerDecoder).*)(buffer)" .\audio_config_main_20241108163659.log`
You will get the following error message: 
`rg: PCRE2: error compiling pattern at offset 3: length of lookbehind assertion is not limited`

### What is the actual behavior?

```
rg --debug -iP "(?<!(ACodec|NuPlayerDecoder).*)(buffer)" .\audio_config_main_20241108163659.log
rg: DEBUG|rg::flags::parse|crates/core\flags\parse.rs:97: no extra arguments found from configuration file
rg: DEBUG|rg::flags::hiargs|crates/core\flags\hiargs.rs:1083: number of paths given to search: 1
rg: DEBUG|rg::flags::hiargs|crates/core\flags\hiargs.rs:1094: is_one_file? true
rg: DEBUG|rg::flags::hiargs|crates/core\flags\hiargs.rs:1269: found hostname for hyperlink configuration: WIN-76NEEBL0FS5
rg: DEBUG|rg::flags::hiargs|crates/core\flags\hiargs.rs:1279: hyperlink format: ""
rg: DEBUG|rg::flags::hiargs|crates/core\flags\hiargs.rs:174: using 1 thread(s)
rg: PCRE2: error compiling pattern at offset 3: length of lookbehind assertion is not limited
```

### What is the expected behavior?

Just like the matching of the two strings shown in the following picture:
![image](https://github.com/user-attachments/assets/5e09ce77-3bad-41d1-a135-9fada16a255f)

---

_Comment by @ltrzesniewski on 2024-11-08 10:11_

I suppose you want to search for `(?<!(ACodec|NuPlayerDecoder).*)(buffer)` like you did with `--debug` instead of that thing which begins with `(? <! (`.

There's a `.*` in the lookbehind which makes its size variable, and PCRE2 doesn't support that. All the alternatives in a lookbehind must have a constant size.

You may change the regex to work aroud this limitation. I don't know if this is what you're looking for (your explanation is very unclear), but try this:

```
(?:ACodec|NuPlayerDecoder).*(*SKIP)(*FAIL)|buffer
```


---

_Comment by @ItsFated on 2024-11-08 10:49_

> I suppose you want to search for `(?<!(ACodec|NuPlayerDecoder).*)(buffer)` like you did with `--debug` instead of that thing which begins with `(? <! (`.
> 
> There's a `.*` in the lookbehind which makes its size variable, and PCRE2 doesn't support that. All the alternatives in a lookbehind must have a constant size.
> 
> You may change the regex to work aroud this limitation. I don't know if this is what you're looking for (your explanation is very unclear), but try this:
> 
> ```
> (?:ACodec|NuPlayerDecoder).*(*SKIP)(*FAIL)|buffer
> ```

Thanks for your reply, the following picture is what I want to do: find the string "buffer" that does not start with ACodec or NuPlayerDecoder.
![image](https://github.com/user-attachments/assets/5e09ce77-3bad-41d1-a135-9fada16a255f)
Then through your reply I have the following alternative method:
`rg -iP "(?<!(ACodec|NuPlayerDecoder).{0,99})buffer"`
![image](https://github.com/user-attachments/assets/d1f3798b-447f-4ae1-a64c-795a9963dc50)


---

_Comment by @ItsFated on 2024-11-08 11:17_

> > I suppose you want to search for `(?<!(ACodec|NuPlayerDecoder).*)(buffer)` like you did with `--debug` instead of that thing which begins with `(? <! (`.
> > There's a `.*` in the lookbehind which makes its size variable, and PCRE2 doesn't support that. All the alternatives in a lookbehind must have a constant size.
> > You may change the regex to work aroud this limitation. I don't know if this is what you're looking for (your explanation is very unclear), but try this:
> > ```
> > (?:ACodec|NuPlayerDecoder).*(*SKIP)(*FAIL)|buffer
> > ```
> 
> Thanks for your reply, the following picture is what I want to do: find the string "buffer" that does not start with ACodec or NuPlayerDecoder. ![image](https://private-user-images.githubusercontent.com/13051752/384299626-5e09ce77-3bad-41d1-a135-9fada16a255f.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MzEwNjQ3OTYsIm5iZiI6MTczMTA2NDQ5NiwicGF0aCI6Ii8xMzA1MTc1Mi8zODQyOTk2MjYtNWUwOWNlNzctM2JhZC00MWQxLWExMzUtOWZhZGExNmEyNTVmLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDExMDglMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQxMTA4VDExMTQ1NlomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWM2Mzc5NTI2YzNmMzZhZjc3NWExNWQ5YTA4MWE5MzVkMDU1NzM5ZjllMTIwOGVmNGMwNmM4OGJjNTc0NTMwNzMmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.5WDgHJ9r7Fafn8E1K-kuDURlCyOaLI-M0bEN8MGDxQY) Then through your reply I have the following alternative method: `rg -iP "(?<!(ACodec|NuPlayerDecoder).{0,99})buffer"` ![image](https://private-user-images.githubusercontent.com/13051752/384339219-d1f3798b-447f-4ae1-a64c-795a9963dc50.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MzEwNjQ3OTYsIm5iZiI6MTczMTA2NDQ5NiwicGF0aCI6Ii8xMzA1MTc1Mi8zODQzMzkyMTktZDFmMzc5OGItNDQ3Zi00YWUxLWE2NGMtNzk1YTk5NjNkYzUwLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDExMDglMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQxMTA4VDExMTQ1NlomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTc5NGNhYWJmOGU5ZTVhMjA0MGYxYTQ1YTlkMjhjOGI4MTM1YzYxMTU1NmZiMDZmODBjYTg1ODNmNjg3N2E2MDUmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.1QDmVeTehYCOO287v7htI5iYmhBx2YIQ31vRO1B6ocA)

I test the length is 255, length(ACodec) + 249 = 255
![image](https://github.com/user-attachments/assets/ae11781d-3e18-497e-9c48-af48b6390604)

---

_Closed by @ItsFated on 2024-11-08 11:19_

---

_Label `invalid` added by @BurntSushi on 2024-11-08 12:33_

---
