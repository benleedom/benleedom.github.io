# Version 0.1.3 is now available

Version 0.1.3 of the Rebar addon is now [available to download](https://github.com/ni/rebar/releases/tag/v0.1.3-alpha) and install!

Reminder that incomplete features must be enabled with feature toggles.
* [How to enable Rebar feature toggles](https://github.com/ni/rebar/wiki/EnableFeatureToggles)

What's new in this release:

* The Option data type is now stable, so it no longer requires a feature toggle to enable.
  * Added the None constructor node, which outputs Option<T>::None values. The output type of None is determined through type inference, so it generally must be combined with some other downstream node.
  * Loop output tunnels that receive inputs of type T now output Option<T>; the output value will be Option<T>::Some(x), where x was the last value output, if the loop executes at least one iteration, and Option<T>::None otherwise.
* Added the Modulus node for integer types.
* Added comparison (Equal, Not Equal, Greater Than, Greater Than or Equal, Less Than, Less Than or Equal) for integer types.
* The compiler now inserts Drop calls for types that require it: String and FileHandle (see below), Option<T> where T: Drop
* Incomplete features:
  * The Rebar compile target now supports an LLVM-based backend; this will eventually replace the current backend, and some features below are only implemented on the LLVM backend.
  * String data type: 
    * string constants (currently outputs a String; will output a static string slice later)
    * String From Slice: given a string slice reference (&str), creates a new String by copying the slice.
    * String To Slice: creates a &str that references the entirety of the input &String.
    * Append To String: appends the contents of a &str to the end of a mutable &String.
    * Concat Strings: creates a new String from the concatenation of two &strs.
  * Vectors and Slices: Insert Into Vector node
  * File Handle data type:
    * Open File Handle: Given a &str path, attempts to open a handle to the file at that path; outputs Some(FileHandle) if this was successful and None otherwise.
    * Write String To File Handle: Given a mutable &FileHandle and a &str, writes the contents of the &str to the end of the file.
  * Output node: now supports Boolean and String inputs.
  * Support for all integer types (signed/unsigned 8-, 16-, 32-, and 64-bit integers)
  * Support for visualizing variable identities as wire colors