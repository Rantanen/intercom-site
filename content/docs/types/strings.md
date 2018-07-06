<!-- vim: tw=80
+++
title = "Strings"
description = ""
date = 2018-07-04T05:42:14Z
weight = 200
draft = false
bref = ""
toc = false
+++
-->

# Strings

For most use cases, Intercom APIs should use Rust's `std::String` type, which
will produce interfaces that are compatible with the standard string types in
other languages. This is handled by the language specific libraries.

However for performance critical applications, it might be necessary for the
user to have a better understanding of the underlying string types and how they
are handled by Intercom.

## Strings in different languages

On the surface strings are a very basic data type available in some form in
most programming languages. However the simple types that programming languages
provide hide a great deal of complexity underneath. From Intercom's point of
view there are two major aspects to strings that Intercom has to deal with:
Memory management and memory representation.

Memory representation is a rather easy issue to solve. The ubiquitous C-strings
are widely supported in different languages - even languages that do not use
zero terminated strings often have some kind of support for the C-strings. For
this reason it makes sense for Intercom to use C-strings as the preferred string
format as well. For compatibility reasons Intercom also supports the BSTR string
type.

The memory management issue affects any type that depends on dynamically
allocated memory, which includes strings. The issue is caused by the need to use
the same heap when allocating and deallocating the memory. There is a full
section dedicated to the [memory allocation rules](../memory-allocation/) in
Intercom.

## Intercom's two type systems

Intercom supports two type systems: Raw types and Automation types. The
difference between these type systems and the reason for their existence is
described in the [type systems](../type-systems/) section.

The way this relates to strings is the need to support two string types. The raw
type system uses C-strings, which are zero terminated strings consisting of
8-bit characters. In Intercom's case these are UTF-8 encoded. The Automation
type system uses BSTR strings. This is a bit more special string type with a
length prefix, 16-bit characters and zero termination.

Neither of these strings is memory compatible with Rust's `std::String` - not
the least because the memory layout of Rust's `std::String` isn't public. As a
result both of these types require the string to be re-allocated when
constructing a `std::String`.

## Avoiding re-allocation

Now that we understand Intercom's underlying string representation, we can
discuss how to avoid the re-allocation involved in constructing the
`std::String`. Since the string representation differs between the raw type
system and the automation type system, the re-allocation needs differ between
these as well.

The raw type system uses C-strings. These are almost compatible with Rust's
`ffi::CString`, but fail in memory management rules that Intercom requires.
Intercom provides `intercom::CString` type instead, which can handle the
incoming C-string pointer in a way that is compatible with Intercom's memory
management rules. This type can also be used to acquire the normal `&ffi::CStr`
reference as the reference-type does not suffer from the memory management rule
issue.

The automation type system uses BSTR strings. Unlike C-strings mentioned
above, the BSTR strings do not have an equivalent type in the Rust standard
libraries. They can be handled in Rust using the `intercom::BStr` type, but the
supported operations are quite limited and in most cases the string needs to be
converted into `std::String` or a similar type anyway.

Of course Intercom's support for both type systems means that neither of these
string types avoids re-allocation in all the cases. If the Intercom library is
being used only, for example, from C++ code using the Intercom C++ library, then
using `intercom::CString` as the string type in the APIs will avoid
re-allocation. However for those cases where the Intercom library is going to be
used from multiple different languages - or it is not known from which language
it will be used, the strings may be specified using the `intercom::String` type.

The `intercom::String` is an enum type that may contain the strings in either of
the concrete string types depending on which type system the caller is using.
Eventually the `intercom::String` needs to be converted to a concrete string
type for any meaningful use, but for example in cases where the string doesn't
end up used at all, the `intercom::String` type can be used to avoid
re-allocations.
