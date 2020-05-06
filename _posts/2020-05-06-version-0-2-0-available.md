---
layout: post
title:  "Rebar 0.2.0 is now available"
date:   2020-05-06 00:30:00 -0500
categories: rebar
---

Version 0.2.0 of the Rebar addon is now [available to download](...) and install! Note: starting with version 0.2.0, Rebar targets **[LabVIEW NXG 5.0](https://www.ni.com/en-us/shop/labview/labview-nxg.html)** (including [Community Edition](https://www.ni.com/en-us/support/downloads/software-products/download.labview-nxg-community.html#342195), which is free for non-commercial, non-academic use).

Reminder that incomplete features must generally be enabled with feature toggles.
* [How to enable Rebar feature toggles](https://github.com/rebarlang/rebar/wiki/EnableFeatureToggles)

## What's new in this release:

# Stable features
* The LLVM compiler no longer requires a feature toggle. The old bytecode compiler/runtime is removed.
* Support for LabVIEW NXG 5.0: Going forward, releases of Rebar will support LabVIEW NXG 5.0, unless otherwise noted.

# Incomplete features
* **Option Pattern Structure:** destructure Option values with Some and None cases.
* **Parameters and calls:** Added the ability to call one .rfn from another, with in or out Int32, Bool, or String parameters. To call a .rfn, drag it from the Files pane or the Software palette onto a Function diagram.
* Added the **Type Diagram (.td) document** for defining named types:
  * The Type Diagram supports primitive types Int32, Bool, and String.
  * The Type Diagram supports defining struct types with anonymous fields.
  * Drag a .td file from the Files pane or the Software palette onto a Function diagram to get a **Constructor** node, which creates an instance of the .td type.
  * The **Struct Fields** node takes an immutable/mutable reference to a struct type value, and outputs immutable/mutable references to one or more fields of that value.
* **String type:**
  * **String Slice To String Split Iterator** creates a StringSplitIterator from a string slice reference. A StringSplitIterator splits the input string slice by spaces and yields the separated string slices without spaces.
  * String now properly supports Clone.
* **Vector and slice types:**
  * **Vector Initialize** will initialize a Vector<T> with n copies of an input T value.
  * **Vector Append** appends a T to a Vector<T> (by mutable reference).
  * **Vector Remove Last** removes and returns the last T value from a Vector<T> (by mutable reference).
  * **Vector To Slice** converts an immutable/mutable reference to a Vector<T> to an immutable/mutable T slice reference.
  * **Slice Index** gets a reference to the nth element in a T slice (by immutable/mutable reference).
  * **Vector Create** is now generic in its output type.
  * Vector now properly supports Clone and Drop.
* **Shared type:**
  * Renamed from NonLockingCell.
  * **Create Shared** creates a Shared<T> from a T value.
  * **Get Value From Shared** gets an immutable reference to the inner T value from an immutable Shared<T> reference.
  * Shared implements Clone and Drop.
* **Async functions:**
  * **Yield** passes through an immutable reference to any type of value and yields back to the scheduler.
  * The **NotifierReader**/**NotifierWriter** types allow passing a single value asynchronously between two parallel parts of a program (a one-shot channel).
    * **Create Notifier** creates a linked NotifierReader<T>/NotifierWriter<T> pair.
	* **Get Notifier Value** takes a NotifierReader and asynchronously waits for the corresponding NotifierWriter to be written to or dropped; it outputs Some(x) if the value x was written, and None if the NotifierWriter was dropped.
	* **Set Notifier Value** takes a NotifierWriter<T> and a T value, and writes the value to be read by the corresponding NotifierReader.
* **Panics:**
  * **Unwrap Option** takes an Option<T>; on Some(x), it outputs x, and on None it panics.
  * Add support for unwinding through calling Functions that may panic.
  
See also the release notes for previous versions: [0.1.3](https://benleedom.github.io/blog/version-0-1-3-available), [0.1.1](https://benleedom.github.io/blog/version-0-1-1-available)