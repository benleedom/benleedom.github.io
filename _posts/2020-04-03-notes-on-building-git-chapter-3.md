---
layout: post
title:  "Notes on Building Git, Chapter 3"
date:   2020-04-03 17:30:00 -0500
categories: jekyll update
---

I recently bought and started reading Building Git by James Coglan, which is a deep dive into the internals of the git version control system by way of teaching you how to build a working clone ("jit"). Chapter 3, the first chapter to introduce code, focuses on getting to a minimal self-hosting version of the clone, which is enough to create a valid commit of its own source code. I successfully implemented that version in C# on Windows, so I wanted to share some notes from that here.

The initial commit is [on github](https://github.com/benleedom/BuildingGit/commit/4239cb36563d56a8316314424ef658502ecdac33).

This is not the cleanest or best-factored C# I would ever write; the intent was translate the Unix/Ruby code in the book as well as to stick to the goal of providing a minimal self-hosting implementation. I deliberately held off on some refactorings I wanted to do until the code was capable of creating its own history. Hopefully it is readable if you're somewhat familiar with C#.

The code implements the `jit init` and `jit commit` commands. `commit` can only create root commits (commits without parents), and only supports files in the top-level directory of the repository; it also assumes the current working directory is the top-level of the repository.

Some things that stood out to me on getting this to work in C# in a Windows environment:

Git database objects are stored as a header followed by the object content itself, compressed using the DEFLATE algorithm. .NET provides an implementation of DEFLATE in `System.IO.Compression.DeflateStream`, which since .NET Framework 4.5 is based on zlib. However, git itself uses zlib directly, and it stores objects in zlib's own format, where the compressed data is preceded by some configuration bits and followed by a checksum. `DeflateStream`, on the other hand, just writes out the data compressed by DEFLATE. To write out objects in a form that git itself would recognize, I switched to using the zlib.NET NuGet.

Git database objects are indexed by a key that is the SHA-1 hash of the uncompressed object content (not including the header). .NET provides an implementation of SHA-1 in `System.Security.Cryptography.SHA1`.

Git tree objects are made of entries that describe a snapshot of the filesystem entries in the repository when the tree was created. Git requires the entries in a tree to be sorted by path; it wasn't entirely clear in the book, but they specifically need to be sorted case-sensitively, i.e., `StringComparer.Ordinal`. (The default .NET string comparison is case insensitive.)

Some of the content of git database objects comes from strings, but some of it comes directly from binary data; for instance, tree objects contain the SHA-1 hash for each of the file objects they reference, stored directly as 20 bytes (rather than as a 40-digit hex string, as the SHA-1 has appears in other places). Because of this, I opted to translate much of the object data into byte arrays early, using `Encoding.ASCII.GetBytes`.

C# is a compiled language that produces lots of non-source files in order to run, and Visual Studio (the environment I'm using) also generates or copies various files near the source code in order to build it. Since the goal is for the source code to self-host, and also because of the restriction to a single directory, I used various tricks to get the source files into a single directory by themselves (which is all that's in the commit linked above):
* Moved the auto-generated AssemblyInfo.cs file up next to BuildingGit.csproj; deleted the Properties Folder and ItemGroup from the .csproj.
* Moved BuildingGit.csproj and everything it references next to BuildingGit.sln.
* Changed the location where MSBuild creates the final executable for BuildingGit.csproj to ..\bin. You can change this in the project settings editor.
* Changed the location where MSBuild create intermediate object files for BuildingGit.csproj to ..\obj. This isn't in the project settings editor; you have to add a `<BaseIntermediateOutputPath>` MSBuild property to the .csproj file.
* Changed the location where NuGet packages are installed to ..\packages by adding a nuget.config file; see the commit. (Found at [https://siderite.dev/blog/setting-nuget-package-folder-location.html/#at2169015847](https://siderite.dev/blog/setting-nuget-package-folder-location.html/#at2169015847))

Visual Studio auto-generates a .vs folder next to the solution file containing user settings. There doesn't seem to be any way to change the location where it generates this folder, but you can also just delete the folder before creating a commit without any harm.

I'm hoping to continue working through the book and updating the github repository above.