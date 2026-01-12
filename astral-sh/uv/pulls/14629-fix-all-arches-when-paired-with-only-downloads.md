```yaml
number: 14629
title: Fix --all-arches when paired with --only-downloads
type: pull_request
state: merged
author: alexprengere
labels:
  - bug
assignees: []
merged: true
base: main
head: fix_only_downloads_with_all_arches
created_at: 2025-07-15T15:57:14Z
updated_at: 2025-07-15T17:20:50Z
url: https://github.com/astral-sh/uv/pull/14629
synced_at: 2026-01-12T16:11:19Z
```

# Fix --all-arches when paired with --only-downloads

---

_@alexprengere_

## Summary

On current main, and on the latest released version 0.7.21, I have:

```
$ uv python list --only-downloads --all-arches
cpython-3.14.0b4-linux-x86_64-gnu                 <download available>
cpython-3.14.0b4+freethreaded-linux-x86_64-gnu    <download available>
cpython-3.13.5-linux-x86_64-gnu                   <download available>
cpython-3.13.5+freethreaded-linux-x86_64-gnu      <download available>
cpython-3.12.11-linux-x86_64-gnu                  <download available>
cpython-3.11.13-linux-x86_64-gnu                  <download available>
cpython-3.10.18-linux-x86_64-gnu                  <download available>
cpython-3.9.23-linux-x86_64-gnu                   <download available>
cpython-3.8.20-linux-x86_64-gnu                   <download available>
pypy-3.11.13-linux-x86_64-gnu                     <download available>
pypy-3.10.16-linux-x86_64-gnu                     <download available>
pypy-3.9.19-linux-x86_64-gnu                      <download available>
pypy-3.8.16-linux-x86_64-gnu                      <download available>
graalpy-3.11.0-linux-x86_64-gnu                   <download available>
graalpy-3.10.0-linux-x86_64-gnu                   <download available>
graalpy-3.8.5-linux-x86_64-gnu                    <download available>
```

As you can see, `--all-arches` is not respected here.

## Test Plan

With the patch:

```
$ cargo run python list --only-downloads --all-arches
cpython-3.14.0b4-linux-x86_64-gnu                      <download available>
cpython-3.14.0b4+freethreaded-linux-x86_64-gnu         <download available>
cpython-3.14.0b4-linux-x86_64_v2-gnu                   <download available>
cpython-3.14.0b4+freethreaded-linux-x86_64_v2-gnu      <download available>
cpython-3.14.0b4-linux-x86_64_v3-gnu                   <download available>
cpython-3.14.0b4+freethreaded-linux-x86_64_v3-gnu      <download available>
cpython-3.14.0b4-linux-x86_64_v4-gnu                   <download available>
cpython-3.14.0b4+freethreaded-linux-x86_64_v4-gnu      <download available>
cpython-3.14.0b4-linux-aarch64-gnu                     <download available>
cpython-3.14.0b4+freethreaded-linux-aarch64-gnu        <download available>
cpython-3.14.0b4-linux-powerpc64le-gnu                 <download available>
cpython-3.14.0b4+freethreaded-linux-powerpc64le-gnu    <download available>
cpython-3.14.0b4-linux-riscv64gc-gnu                   <download available>
cpython-3.14.0b4+freethreaded-linux-riscv64gc-gnu      <download available>
cpython-3.14.0b4-linux-s390x-gnu                       <download available>
cpython-3.14.0b4+freethreaded-linux-s390x-gnu          <download available>
cpython-3.13.5-linux-x86_64-gnu                        <download available>
cpython-3.13.5+freethreaded-linux-x86_64-gnu           <download available>
cpython-3.13.5-linux-x86_64_v2-gnu                     <download available>
cpython-3.13.5+freethreaded-linux-x86_64_v2-gnu        <download available>
cpython-3.13.5-linux-x86_64_v3-gnu                     <download available>
cpython-3.13.5+freethreaded-linux-x86_64_v3-gnu        <download available>
cpython-3.13.5-linux-x86_64_v4-gnu                     <download available>
cpython-3.13.5+freethreaded-linux-x86_64_v4-gnu        <download available>
cpython-3.13.5-linux-aarch64-gnu                       <download available>
cpython-3.13.5+freethreaded-linux-aarch64-gnu          <download available>
cpython-3.13.5-linux-powerpc64le-gnu                   <download available>
cpython-3.13.5+freethreaded-linux-powerpc64le-gnu      <download available>
cpython-3.13.5-linux-riscv64gc-gnu                     <download available>
cpython-3.13.5+freethreaded-linux-riscv64gc-gnu        <download available>
cpython-3.13.5-linux-s390x-gnu                         <download available>
cpython-3.13.5+freethreaded-linux-s390x-gnu            <download available>
cpython-3.12.11-linux-x86_64-gnu                       <download available>
cpython-3.12.11-linux-x86_64_v2-gnu                    <download available>
cpython-3.12.11-linux-x86_64_v3-gnu                    <download available>
cpython-3.12.11-linux-x86_64_v4-gnu                    <download available>
cpython-3.12.11-linux-aarch64-gnu                      <download available>
cpython-3.12.11-linux-powerpc64le-gnu                  <download available>
cpython-3.12.11-linux-riscv64gc-gnu                    <download available>
cpython-3.12.11-linux-s390x-gnu                        <download available>
cpython-3.11.13-linux-x86_64-gnu                       <download available>
cpython-3.11.13-linux-x86_64_v2-gnu                    <download available>
cpython-3.11.13-linux-x86_64_v3-gnu                    <download available>
cpython-3.11.13-linux-x86_64_v4-gnu                    <download available>
cpython-3.11.13-linux-aarch64-gnu                      <download available>
cpython-3.11.13-linux-powerpc64le-gnu                  <download available>
cpython-3.11.13-linux-riscv64gc-gnu                    <download available>
cpython-3.11.13-linux-s390x-gnu                        <download available>
cpython-3.11.5-linux-x86-gnu                           <download available>
cpython-3.10.18-linux-x86_64-gnu                       <download available>
cpython-3.10.18-linux-x86_64_v2-gnu                    <download available>
cpython-3.10.18-linux-x86_64_v3-gnu                    <download available>
cpython-3.10.18-linux-x86_64_v4-gnu                    <download available>
cpython-3.10.18-linux-aarch64-gnu                      <download available>
cpython-3.10.18-linux-powerpc64le-gnu                  <download available>
cpython-3.10.18-linux-riscv64gc-gnu                    <download available>
cpython-3.10.18-linux-s390x-gnu                        <download available>
cpython-3.10.13-linux-x86-gnu                          <download available>
cpython-3.9.23-linux-x86_64-gnu                        <download available>
cpython-3.9.23-linux-x86_64_v2-gnu                     <download available>
cpython-3.9.23-linux-x86_64_v3-gnu                     <download available>
cpython-3.9.23-linux-x86_64_v4-gnu                     <download available>
cpython-3.9.23-linux-aarch64-gnu                       <download available>
cpython-3.9.23-linux-powerpc64le-gnu                   <download available>
cpython-3.9.23-linux-riscv64gc-gnu                     <download available>
cpython-3.9.23-linux-s390x-gnu                         <download available>
cpython-3.9.18-linux-x86-gnu                           <download available>
cpython-3.8.20-linux-x86_64-gnu                        <download available>
cpython-3.8.20-linux-aarch64-gnu                       <download available>
cpython-3.8.17-linux-x86-gnu                           <download available>
pypy-3.11.13-linux-x86_64-gnu                          <download available>
pypy-3.11.13-linux-aarch64-gnu                         <download available>
pypy-3.11.13-linux-x86-gnu                             <download available>
pypy-3.10.16-linux-x86_64-gnu                          <download available>
pypy-3.10.16-linux-aarch64-gnu                         <download available>
pypy-3.10.16-linux-x86-gnu                             <download available>
pypy-3.10.14-linux-s390x-gnu                           <download available>
pypy-3.9.19-linux-x86_64-gnu                           <download available>
pypy-3.9.19-linux-aarch64-gnu                          <download available>
pypy-3.9.19-linux-x86-gnu                              <download available>
pypy-3.9.19-linux-s390x-gnu                            <download available>
pypy-3.8.16-linux-x86_64-gnu                           <download available>
pypy-3.8.16-linux-aarch64-gnu                          <download available>
pypy-3.8.16-linux-x86-gnu                              <download available>
pypy-3.8.16-linux-s390x-gnu                            <download available>
graalpy-3.11.0-linux-x86_64-gnu                        <download available>
graalpy-3.11.0-linux-aarch64-gnu                       <download available>
graalpy-3.10.0-linux-x86_64-gnu                        <download available>
graalpy-3.10.0-linux-aarch64-gnu                       <download available>
graalpy-3.8.5-linux-x86_64-gnu                         <download available>
graalpy-3.8.5-linux-aarch64-gnu                        <download available>
```

---

_@zanieb approved on 2025-07-15 17:02_

Thanks!

---

_Merged by @zanieb on 2025-07-15 17:03_

---

_Closed by @zanieb on 2025-07-15 17:03_

---

_Label `bug` added by @zanieb on 2025-07-15 17:03_

---

_Branch deleted on 2025-07-15 17:20_

---
