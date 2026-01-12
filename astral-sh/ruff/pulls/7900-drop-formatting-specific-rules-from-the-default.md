```yaml
number: 7900
title: Drop formatting specific rules from the default set
type: pull_request
state: merged
author: zanieb
labels:
  - breaking
assignees: []
merged: true
base: main
head: zanie/default-rules
created_at: 2023-10-10T17:21:00Z
updated_at: 2023-11-14T19:52:40Z
url: https://github.com/astral-sh/ruff/pull/7900
synced_at: 2026-01-12T15:55:25Z
```

# Drop formatting specific rules from the default set

---

_@zanieb_

Closes https://github.com/astral-sh/ruff/issues/7572

Drops formatting specific rules from the default rule set as they conflict with formatters in general (and in particular, conflict with our formatter). Most of these rules are in preview, but the removal of `line-too-long` and `mixed-spaces-and-tabs` is a change to the stable rule set.

## Example

The following no longer raises `E501`
```
echo "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx = 1" | ruff check -
```

---

_Label `breaking` added by @zanieb on 2023-10-10 17:21_

---

_Review comment by @zanieb on `crates/flake8_to_ruff/src/converter.rs`:32 on 2023-10-10 17:25_

Unclear if the flake8 converter needs a different default rule set than Ruff?

---

_@zanieb reviewed on 2023-10-10 17:25_

---

_Comment by @codspeed-hq[bot] on 2023-10-10 17:37_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/zanie/default-rules)

### Merging #7900 will **improve performances by 9.75%**

<sub>Comparing <code>zanie/default-rules</code> (cd53a45) with <code>main</code> (090c1a4)</sub>



### Summary

`⚡ 4` improvements
`✅ 21` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `zanie/default-rules` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/default-rules[unicode/pypinyin.py]` | 6.3 ms | 6.1 ms | +4.74% |
| ⚡ | `linter/default-rules[numpy/globals.py]` | 2.2 ms | 2 ms | +9.75% |
| ⚡ | `linter/default-rules[numpy/ctypeslib.py]` | 18.6 ms | 17.4 ms | +6.99% |
| ⚡ | `linter/default-rules[pydantic/types.py]` | 38.8 ms | 36.8 ms | +5.58% |


---

_Comment by @github-actions[bot] on 2023-10-10 17:37_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+0, -88, 0 error(s))

<details><summary>setuptools (+0, -88)</summary>
<p>

<pre>
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/__init__.py#L109'>pkg_resources/_vendor/platformdirs/__init__.py:109:89:</a> E501 Line too long (115 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/__init__.py#L125'>pkg_resources/_vendor/platformdirs/__init__.py:125:89:</a> E501 Line too long (110 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/__init__.py#L141'>pkg_resources/_vendor/platformdirs/__init__.py:141:89:</a> E501 Line too long (110 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/__init__.py#L157'>pkg_resources/_vendor/platformdirs/__init__.py:157:89:</a> E501 Line too long (108 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/__init__.py#L180'>pkg_resources/_vendor/platformdirs/__init__.py:180:89:</a> E501 Line too long (112 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/__init__.py#L196'>pkg_resources/_vendor/platformdirs/__init__.py:196:89:</a> E501 Line too long (110 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/__init__.py#L212'>pkg_resources/_vendor/platformdirs/__init__.py:212:89:</a> E501 Line too long (114 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/__init__.py#L228'>pkg_resources/_vendor/platformdirs/__init__.py:228:89:</a> E501 Line too long (112 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/__init__.py#L244'>pkg_resources/_vendor/platformdirs/__init__.py:244:89:</a> E501 Line too long (116 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/__init__.py#L260'>pkg_resources/_vendor/platformdirs/__init__.py:260:89:</a> E501 Line too long (111 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/__init__.py#L276'>pkg_resources/_vendor/platformdirs/__init__.py:276:89:</a> E501 Line too long (111 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/__init__.py#L292'>pkg_resources/_vendor/platformdirs/__init__.py:292:89:</a> E501 Line too long (109 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/__init__.py#L2'>pkg_resources/_vendor/platformdirs/__init__.py:2:89:</a> E501 Line too long (119 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/__init__.py#L315'>pkg_resources/_vendor/platformdirs/__init__.py:315:89:</a> E501 Line too long (113 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/__init__.py#L61'>pkg_resources/_vendor/platformdirs/__init__.py:61:89:</a> E501 Line too long (109 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/__init__.py#L77'>pkg_resources/_vendor/platformdirs/__init__.py:77:89:</a> E501 Line too long (113 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/__init__.py#L93'>pkg_resources/_vendor/platformdirs/__init__.py:93:89:</a> E501 Line too long (111 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/android.py#L111'>pkg_resources/_vendor/platformdirs/android.py:111:89:</a> E501 Line too long (107 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/android.py#L14'>pkg_resources/_vendor/platformdirs/android.py:14:89:</a> E501 Line too long (100 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/android.py#L21'>pkg_resources/_vendor/platformdirs/android.py:21:89:</a> E501 Line too long (114 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/android.py#L32'>pkg_resources/_vendor/platformdirs/android.py:32:89:</a> E501 Line too long (117 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/android.py#L34'>pkg_resources/_vendor/platformdirs/android.py:34:89:</a> E501 Line too long (94 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/android.py#L43'>pkg_resources/_vendor/platformdirs/android.py:43:89:</a> E501 Line too long (120 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/android.py#L54'>pkg_resources/_vendor/platformdirs/android.py:54:89:</a> E501 Line too long (112 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/android.py#L65'>pkg_resources/_vendor/platformdirs/android.py:65:89:</a> E501 Line too long (92 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/android.py#L72'>pkg_resources/_vendor/platformdirs/android.py:72:89:</a> E501 Line too long (116 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/api.py#L39'>pkg_resources/_vendor/platformdirs/api.py:39:89:</a> E501 Line too long (119 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/api.py#L44'>pkg_resources/_vendor/platformdirs/api.py:44:89:</a> E501 Line too long (120 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/api.py#L45'>pkg_resources/_vendor/platformdirs/api.py:45:89:</a> E501 Line too long (106 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/api.py#L49'>pkg_resources/_vendor/platformdirs/api.py:49:89:</a> E501 Line too long (117 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/api.py#L55'>pkg_resources/_vendor/platformdirs/api.py:55:89:</a> E501 Line too long (119 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/macos.py#L10'>pkg_resources/_vendor/platformdirs/macos.py:10:89:</a> E501 Line too long (103 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/macos.py#L18'>pkg_resources/_vendor/platformdirs/macos.py:18:89:</a> E501 Line too long (112 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/macos.py#L19'>pkg_resources/_vendor/platformdirs/macos.py:19:89:</a> E501 Line too long (102 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/macos.py#L23'>pkg_resources/_vendor/platformdirs/macos.py:23:89:</a> E501 Line too long (110 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/macos.py#L28'>pkg_resources/_vendor/platformdirs/macos.py:28:89:</a> E501 Line too long (106 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/macos.py#L29'>pkg_resources/_vendor/platformdirs/macos.py:29:89:</a> E501 Line too long (94 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/macos.py#L33'>pkg_resources/_vendor/platformdirs/macos.py:33:89:</a> E501 Line too long (99 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/macos.py#L38'>pkg_resources/_vendor/platformdirs/macos.py:38:89:</a> E501 Line too long (100 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/macos.py#L48'>pkg_resources/_vendor/platformdirs/macos.py:48:89:</a> E501 Line too long (96 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/macos.py#L58'>pkg_resources/_vendor/platformdirs/macos.py:58:89:</a> E501 Line too long (117 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/macos.py#L59'>pkg_resources/_vendor/platformdirs/macos.py:59:89:</a> E501 Line too long (103 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/unix.py#L10'>pkg_resources/_vendor/platformdirs/unix.py:10:89:</a> E501 Line too long (104 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/unix.py#L110'>pkg_resources/_vendor/platformdirs/unix.py:110:89:</a> E501 Line too long (111 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/unix.py#L133'>pkg_resources/_vendor/platformdirs/unix.py:133:89:</a> E501 Line too long (101 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/unix.py#L143'>pkg_resources/_vendor/platformdirs/unix.py:143:89:</a> E501 Line too long (114 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/unix.py#L148'>pkg_resources/_vendor/platformdirs/unix.py:148:89:</a> E501 Line too long (120 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/unix.py#L21'>pkg_resources/_vendor/platformdirs/unix.py:21:89:</a> E501 Line too long (119 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/unix.py#L22'>pkg_resources/_vendor/platformdirs/unix.py:22:89:</a> E501 Line too long (118 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/unix.py#L33'>pkg_resources/_vendor/platformdirs/unix.py:33:89:</a> E501 Line too long (94 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/unix.py#L44'>pkg_resources/_vendor/platformdirs/unix.py:44:89:</a> E501 Line too long (113 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/unix.py#L45'>pkg_resources/_vendor/platformdirs/unix.py:45:89:</a> E501 Line too long (115 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/unix.py#L46'>pkg_resources/_vendor/platformdirs/unix.py:46:89:</a> E501 Line too long (105 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/unix.py#L58'>pkg_resources/_vendor/platformdirs/unix.py:58:89:</a> E501 Line too long (97 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/unix.py#L64'>pkg_resources/_vendor/platformdirs/unix.py:64:89:</a> E501 Line too long (91 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/unix.py#L75'>pkg_resources/_vendor/platformdirs/unix.py:75:89:</a> E501 Line too long (112 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/unix.py#L76'>pkg_resources/_vendor/platformdirs/unix.py:76:89:</a> E501 Line too long (118 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/unix.py#L88'>pkg_resources/_vendor/platformdirs/unix.py:88:89:</a> E501 Line too long (89 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/unix.py#L99'>pkg_resources/_vendor/platformdirs/unix.py:99:89:</a> E501 Line too long (95 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/windows.py#L136'>pkg_resources/_vendor/platformdirs/windows.py:136:89:</a> E501 Line too long (112 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/windows.py#L140'>pkg_resources/_vendor/platformdirs/windows.py:140:89:</a> E501 Line too long (119 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/windows.py#L48'>pkg_resources/_vendor/platformdirs/windows.py:48:89:</a> E501 Line too long (101 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/windows.py#L65'>pkg_resources/_vendor/platformdirs/windows.py:65:89:</a> E501 Line too long (113 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/windows.py#L79'>pkg_resources/_vendor/platformdirs/windows.py:79:89:</a> E501 Line too long (111 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/platformdirs/windows.py#L99'>pkg_resources/_vendor/platformdirs/windows.py:99:89:</a> E501 Line too long (92 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/typing_extensions.py#L1080'>pkg_resources/_vendor/typing_extensions.py:1080:89:</a> E501 Line too long (89 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/typing_extensions.py#L1139'>pkg_resources/_vendor/typing_extensions.py:1139:89:</a> E501 Line too long (89 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/typing_extensions.py#L1410'>pkg_resources/_vendor/typing_extensions.py:1410:89:</a> E501 Line too long (89 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/typing_extensions.py#L162'>pkg_resources/_vendor/typing_extensions.py:162:89:</a> E501 Line too long (89 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/typing_extensions.py#L1726'>pkg_resources/_vendor/typing_extensions.py:1726:89:</a> E501 Line too long (90 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/typing_extensions.py#L1743'>pkg_resources/_vendor/typing_extensions.py:1743:89:</a> E501 Line too long (90 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/typing_extensions.py#L1811'>pkg_resources/_vendor/typing_extensions.py:1811:89:</a> E501 Line too long (90 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/typing_extensions.py#L2149'>pkg_resources/_vendor/typing_extensions.py:2149:89:</a> E501 Line too long (89 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/typing_extensions.py#L2178'>pkg_resources/_vendor/typing_extensions.py:2178:89:</a> E501 Line too long (89 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/typing_extensions.py#L2182'>pkg_resources/_vendor/typing_extensions.py:2182:89:</a> E501 Line too long (89 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/typing_extensions.py#L629'>pkg_resources/_vendor/typing_extensions.py:629:89:</a> E501 Line too long (89 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/typing_extensions.py#L689'>pkg_resources/_vendor/typing_extensions.py:689:89:</a> E501 Line too long (89 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/pkg_resources/_vendor/typing_extensions.py#L968'>pkg_resources/_vendor/typing_extensions.py:968:89:</a> E501 Line too long (89 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/setuptools/_vendor/more_itertools/more.py#L3456'>setuptools/_vendor/more_itertools/more.py:3456:89:</a> E501 Line too long (99 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/setuptools/_vendor/typing_extensions.py#L1006'>setuptools/_vendor/typing_extensions.py:1006:89:</a> E501 Line too long (89 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/setuptools/_vendor/typing_extensions.py#L1155'>setuptools/_vendor/typing_extensions.py:1155:89:</a> E501 Line too long (89 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/setuptools/_vendor/typing_extensions.py#L1512'>setuptools/_vendor/typing_extensions.py:1512:89:</a> E501 Line too long (89 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/setuptools/_vendor/typing_extensions.py#L1755'>setuptools/_vendor/typing_extensions.py:1755:89:</a> E501 Line too long (89 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/setuptools/_vendor/typing_extensions.py#L752'>setuptools/_vendor/typing_extensions.py:752:89:</a> E501 Line too long (90 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/setuptools/_vendor/typing_extensions.py#L951'>setuptools/_vendor/typing_extensions.py:951:89:</a> E501 Line too long (89 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/setuptools/config/_validate_pyproject/fastjsonschema_exceptions.py#L17'>setuptools/config/_validate_pyproject/fastjsonschema_exceptions.py:17:89:</a> E501 Line too long (139 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/setuptools/config/_validate_pyproject/fastjsonschema_exceptions.py#L20'>setuptools/config/_validate_pyproject/fastjsonschema_exceptions.py:20:89:</a> E501 Line too long (91 > 88 characters)
- <a href='https://github.com/pypa/setuptools/blob/fbe0d7962822c2a1fdde8dd179f2f8b8c8bf8892/setuptools/config/_validate_pyproject/fastjsonschema_exceptions.py#L21'>setuptools/config/_validate_pyproject/fastjsonschema_exceptions.py:21:89:</a> E501 Line too long (111 > 88 characters)
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| E501 | 88 | 0 | 88 |



---

_Comment by @zanieb on 2023-10-10 17:57_

The performance win is an exciting upside.

---

_Review requested from @charliermarsh by @zanieb on 2023-10-10 19:57_

---

_@zanieb reviewed on 2023-10-10 19:58_

---

_Review comment by @zanieb on `README.md`:242 on 2023-10-10 19:58_

Hey we already say we won't include these

---

_@charliermarsh approved on 2023-10-10 20:13_

---

_Review comment by @konstin on `README.md`:242 on 2023-10-11 12:08_

The first and second sentence in the paragraph sound contradictory to me

---

_@konstin approved on 2023-10-11 12:09_

---

_@zanieb reviewed on 2023-10-11 16:29_

---

_Review comment by @zanieb on `README.md`:242 on 2023-10-11 16:29_

How so?

---

_Merged by @zanieb on 2023-10-11 16:29_

---

_Closed by @zanieb on 2023-10-11 16:29_

---

_Branch deleted on 2023-10-11 16:29_

---

_Comment by @MichaReiser on 2023-10-15 23:21_

Nice. Thank you for landing this change!

---

_Comment by @g3rv4 on 2023-11-14 19:36_

hi there :) I was trying to understand why after v0.1.0 I'm no longer seeing E501 errors on long lines, and based on the description this seems to be why.

is the idea not to raise E501 anymore using ruff? or am I missing some extra configuration to bring it back? I've been reading the documentation and couldn't find anything that has changed.

I'd expect

```
echo 'print({"a": 1, "b":2, "c":3, "d": 4, "e": 5, "f": 6, "g": 7, "h": 8, "i": 9, "j": 10, "k": 11, "l": 12})' | ruff check -
```

to raise E501

---

_Comment by @charliermarsh on 2023-11-14 19:49_

@g3rv4 -- You can still enable E501! It's just not part of the _default_ rule set. If you're using `pyproject.toml`:

```toml
[tool.ruff]
extend-select = ["E501"]
```

Or, to re-enable all of the `E`-class rules:

```toml
[tool.ruff]
extend-select = ["E"]
```


---

_Comment by @g3rv4 on 2023-11-14 19:52_

@charliermarsh thanks! now I understand the change on the README :)

---
