<!-- vim: tw=80
+++
title = "Lists"
description = ""
date = 2018-07-04T05:42:14Z
weight = 300
draft = false
bref = ""
toc = false
+++
-->


# Lists

- SAFEARRAY(TItem) for Automation type system.
- `{ count : usize, data : *const TItem }` for raw types.
  - This should work as a `Array<T> { size_t count; T* data }` in C++?
  - We don't even need to come up with an IDL for this since the IDLs are only
    "required" for the automation API. The rest is all aobut Intercom
    conventions, etc. and can be hard coded into whatever library providers we
    write - such as the C++ one.
