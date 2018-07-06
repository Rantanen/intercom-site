<!-- vim: tw=80
+++
title = "Documentation"
+++
-->

# Documentation

This is the official Intercom framework documentation. While Intercom is a Rust
project, it is not a usual Rust project with a discoverable API. For this reason
the usual approach of just providing an API reference documentation doesn't
really fit the documentation needs.

Instead this documentation focuses mostly on the practical and technological
aspects of Intercom.

The [getting started](./getting-started/) section contains instructions for
setting up an Intercom project, writing a component in Rust and using that
component from projects written in other languages.

The [types](./types/) section describes the various types supported by Intercom,
how these types are handled in the underlying binary layer and how this affects
their use from other languages.

The [binary layer](./binary-layer/) section explains the basic COM binary layer
as used by Intercom. This section is aimed for the people who are implementing
Intercom support for new languages or who for some other reason want to use
the raw COM layer without the help of any language specific support libraries.
