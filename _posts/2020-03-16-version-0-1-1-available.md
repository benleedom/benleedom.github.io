---
layout: post
title:  "Version 0.1.1 is now available"
date:   2019-03-20 20:00:00 -0500
categories: jekyll update
permalink: /blog/version-0-1-1-available
---

Version 0.1.1 of the Rebar addon is now [available to download](https://github.com/ni/rebar/releases/tag/v0.1.1-alpha) and install!

For the time being, I'm planning to use the following release strategy:
* Time-based releases every six weeks
* Incomplete features are available, but must be enabled with feature toggles
  * [How to enable Rebar feature toggles](https://github.com/ni/rebar/wiki/EnableFeatureToggles)

What's new in this release:
* The Option and Cell data types present in 0.1.0 have been moved behind feature toggles
* New incomplete features:
  * Rebar Target, which allows executing Rebar Functions from within the editor
    * [Creating and running a function on the Rebar target](https://github.com/ni/rebar/wiki/CreateAndRunFunctionOnRebarTarget)
  * Output Node, for debug-printing values
  * Vectors and Slices (very incomplete)
  * Visualize Variable Identity (incomplete; allows coloring wires with unique colors per variable)
* Select Reference now outputs a mutable reference when both selected-from inputs are mutable; it no longer passes through references in the immutable case
* Various bug fixes
